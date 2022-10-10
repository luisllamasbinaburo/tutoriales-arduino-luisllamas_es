> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/obtener-orientacion-y-altitud-ahrs-con-imu-9dof-y-rtimulib-arduino/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

```cpp
#include <Wire.h>
#include "I2Cdev.h"
#include "RTIMUSettings.h"
#include "RTIMU.h"
#include "CalLib.h"
#include <EEPROM.h>

RTIMU *imu;                                           // the IMU object
RTIMUSettings settings;                               // the settings object
CALLIB_DATA calData;                                  // the calibration data

//  SERIAL_PORT_SPEED defines the speed to use for the debug serial port

#define  SERIAL_PORT_SPEED  115200

void setup()
{
  calLibRead(0, &calData);                           // pick up existing mag data if there   

  calData.magValid = false;
  for (int i = 0; i < 3; i++) {
    calData.magMin[i] = 10000000;                    // init mag cal data
    calData.magMax[i] = -10000000;
  }
   
  Serial.begin(SERIAL_PORT_SPEED);
  Serial.println("ArduinoMagCal starting");
  Serial.println("Enter s to save current data to EEPROM");
  Wire.begin();
   
  imu = RTIMU::createIMU(&settings);                 // create the imu object
  imu->IMUInit();
  imu->setCalibrationMode(true);                     // make sure we get raw data
  Serial.print("ArduinoIMU calibrating device "); Serial.println(imu->IMUName());
}

void loop()
{  
  boolean changed;
  RTVector3 mag;
  
  if (imu->IMURead()) {                                 // get the latest data
    changed = false;
    mag = imu->getCompass();
    for (int i = 0; i < 3; i++) {
      if (mag.data(i) < calData.magMin[i]) {
        calData.magMin[i] = mag.data(i);
        changed = true;
      }
      if (mag.data(i) > calData.magMax[i]) {
        calData.magMax[i] = mag.data(i);
        changed = true;
      }
    }
 
    if (changed) {
      Serial.println("-------");
      Serial.print("minX: "); Serial.print(calData.magMin[0]);
      Serial.print(" maxX: "); Serial.print(calData.magMax[0]); Serial.println();
      Serial.print("minY: "); Serial.print(calData.magMin[1]);
      Serial.print(" maxY: "); Serial.print(calData.magMax[1]); Serial.println();
      Serial.print("minZ: "); Serial.print(calData.magMin[2]);
      Serial.print(" maxZ: "); Serial.print(calData.magMax[2]); Serial.println();
    }
  }
  
  if (Serial.available()) {
    if (Serial.read() == 's') {                  // save the data
      calData.magValid = true;
      calLibWrite(0, &calData);
      Serial.print("Mag cal data saved for device "); Serial.println(imu->IMUName());
    }
  }
}
```

```cpp
#include <Wire.h>
#include "I2Cdev.h"
#include "RTIMUSettings.h"
#include "RTIMU.h"
#include "RTFusionRTQF.h" 
#include "CalLib.h"
#include <EEPROM.h>

RTIMU *imu;                                           // the IMU object
RTFusionRTQF fusion;                                  // the fusion object
RTIMUSettings settings;                               // the settings object

#define DISPLAY_INTERVAL 100
#define SERIAL_PORT_SPEED 115200

unsigned long lastDisplay;

void setup()
{
    Serial.begin(SERIAL_PORT_SPEED);
    Wire.begin();
    imu = RTIMU::createIMU(&settings);
  
    Serial.print("ArduinoIMU starting using device "); 
	Serial.println(imu->IMUName());
	int errcode;
    if ((errcode = imu->IMUInit()) < 0) {
        Serial.print("Failed to init IMU: "); Serial.println(errcode);
    }
  
    if (imu->getCalibrationValid())
        Serial.println("Using compass calibration");
    else
        Serial.println("No valid compass calibration data");

    fusion.setSlerpPower(0.02);
    fusion.setGyroEnable(true);
    fusion.setAccelEnable(true);
    fusion.setCompassEnable(true);
}

void loop()
{  
    unsigned long now = millis();
    unsigned long delta;
   
	int loopCount = 1;
    while (imu->IMURead()) {
       
        if (++loopCount >= 10)  // this flushes remaining data in case we are falling behind
            continue;
        fusion.newIMUData(imu->getGyro(), imu->getAccel(), imu->getCompass(), imu->getTimestamp());
        if ((now - lastDisplay) >= DISPLAY_INTERVAL) {
            lastDisplay = now;

			Serial.print(((RTVector3&)fusion.getFusionPose()).y() * RTMATH_RAD_TO_DEGREE);
			Serial.print(';');
		    Serial.print(((RTVector3&)fusion.getFusionPose()).x() * RTMATH_RAD_TO_DEGREE);
			Serial.print(';');
			Serial.println(((RTVector3&)fusion.getFusionPose()).z() * RTMATH_RAD_TO_DEGREE);
        }
    }
}
```

```cpp
import processing.serial.*;
import java.awt.event.KeyEvent;
import java.io.IOException;

Serial arduinoPort;
float roll, pitch, yaw;

void setup()
{
  size (1280, 768, P3D);
  arduinoPort = new Serial(this, "COM4", 115200);
  arduinoPort.bufferUntil('\n');
  }

void draw()
{
  drawBackground();
    
  rotateY(radians(-yaw)); 
  rotateX(radians(-pitch));
  rotateZ(radians(-roll));

  drawCube();
}

void drawBackground() {
  translate(width/2, height/2, 0);
  background(#374046);
  fill(200, 200, 200); 
  textSize(32);
  text("Roll: " + int(roll) + "     Pitch: " + int(pitch) + "     Yaw: " + int(yaw) , -200, -320);
}

void drawCube() {
  strokeWeight(1);
  stroke(255, 255, 255);
  fill(#607D8B); 
  box (500, 40, 300);
  
  strokeWeight(4);
  stroke(#E81E63);
  line(0, 0, 0, 300, 0, 0);
  stroke(#2196F2);
  line(0, 0, 0, 0, -70, 0);
  stroke(#8BC24A);
  line(0, 0, 0, 0, 0, 200);
}

void serialEvent (Serial myPort) { 
  String data = myPort.readStringUntil('\n');
  if (data != null) {
    data = trim(data);
    String items[] = split(data, ';');
    if (items.length > 1) {
      roll = float(items[0]);
      pitch = float(items[1]);
      yaw = float(items[2]);
    }
  }
}
```