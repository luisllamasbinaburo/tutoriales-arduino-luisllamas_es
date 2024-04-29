> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/sensor-de-calidad-ambiental-con-arduino-y-bme680/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

## Librería Adafruit
```cpp
#include <Wire.h>
#include <SPI.h>
#include <Adafruit_Sensor.h>
#include "Adafruit_BME680.h"

const int BME_SCK = 13;
const int BME_MISO = 12;
const int BME_MOSI = 11;
const int BME_CS = 10;

const int SEALEVELPRESSURE_HPA = 1013.25;

Adafruit_BME680 bme;

void setup() {
    Serial.begin(9600);
    while (!Serial);
    Serial.println(F("BME680 test"));

    if (!bme.begin()) {
    Serial.println("Could not find a valid BME680 sensor, check wiring!");
    while (1);
    }

    // Set up oversampling and filter initialization
    bme.setTemperatureOversampling(BME680_OS_8X);
    bme.setHumidityOversampling(BME680_OS_2X);
    bme.setPressureOversampling(BME680_OS_4X);
    bme.setIIRFilterSize(BME680_FILTER_SIZE_3);
    bme.setGasHeater(320, 150); // 320*C for 150 ms
}

void loop() {
    if (! bme.performReading()) {
    Serial.println("Failed to perform reading :(");
    return;
    }
    Serial.print("Temperature = ");
    Serial.print(bme.temperature);
    Serial.println(" *C");

    Serial.print("Pressure = ");
    Serial.print(bme.pressure / 100.0);
    Serial.println(" hPa");

    Serial.print("Humidity = ");
    Serial.print(bme.humidity);
    Serial.println(" %");

    Serial.print("Gas = ");
    Serial.print(bme.gas_resistance / 1000.0);
    Serial.println(" KOhms");

    Serial.print("Approx. Altitude = ");
    Serial.print(bme.readAltitude(SEALEVELPRESSURE_HPA));
    Serial.println(" m");

    Serial.println();
    delay(2000);
}
```


## Librería Bosch
```csharp
#include "bsec.h"

    // Helper functions declarations
    void checkIaqSensorStatus(void);
    void errLeds(void);
    
    // Create an object of the class Bsec
    Bsec iaqSensor;
    
    String output;
    
    // Entry point for the example
    void setup(void)
    {
      Serial.begin(115200);
      Wire.begin();
    
      iaqSensor.begin(BME680_I2C_ADDR_PRIMARY, Wire);
      output = "\nBSEC library version " + String(iaqSensor.version.major) + "." + String(iaqSensor.version.minor) + "." + String(iaqSensor.version.major_bugfix) + "." + String(iaqSensor.version.minor_bugfix);
      Serial.println(output);
      checkIaqSensorStatus();
    
      bsec_virtual_sensor_t sensorList[10] = {
        BSEC_OUTPUT_RAW_TEMPERATURE,
        BSEC_OUTPUT_RAW_PRESSURE,
        BSEC_OUTPUT_RAW_HUMIDITY,
        BSEC_OUTPUT_RAW_GAS,
        BSEC_OUTPUT_IAQ,
        BSEC_OUTPUT_STATIC_IAQ,
        BSEC_OUTPUT_CO2_EQUIVALENT,
        BSEC_OUTPUT_BREATH_VOC_EQUIVALENT,
        BSEC_OUTPUT_SENSOR_HEAT_COMPENSATED_TEMPERATURE,
        BSEC_OUTPUT_SENSOR_HEAT_COMPENSATED_HUMIDITY,
      };
    
      iaqSensor.updateSubscription(sensorList, 10, BSEC_SAMPLE_RATE_LP);
      checkIaqSensorStatus();
    
      // Print the header
      output = "Timestamp [ms], raw temperature [°C], pressure [hPa], raw relative humidity [%], gas [Ohm], IAQ, IAQ accuracy, temperature [°C], relative humidity [%], Static IAQ, CO2 equivalent, breath VOC equivalent";
      Serial.println(output);
    }
    
    // Function that is looped forever
    void loop(void)
    {
      unsigned long time_trigger = millis();
      if (iaqSensor.run()) { // If new data is available
        output = String(time_trigger);
        output += ", " + String(iaqSensor.rawTemperature);
        output += ", " + String(iaqSensor.pressure);
        output += ", " + String(iaqSensor.rawHumidity);
        output += ", " + String(iaqSensor.gasResistance);
        output += ", " + String(iaqSensor.iaq);
        output += ", " + String(iaqSensor.iaqAccuracy);
        output += ", " + String(iaqSensor.temperature);
        output += ", " + String(iaqSensor.humidity);
        output += ", " + String(iaqSensor.staticIaq);
        output += ", " + String(iaqSensor.co2Equivalent);
        output += ", " + String(iaqSensor.breathVocEquivalent);
        Serial.println(output);
      } else {
        checkIaqSensorStatus();
      }
    }
    
    // Helper function definitions
    void checkIaqSensorStatus(void)
    {
      if (iaqSensor.status != BSEC_OK) {
        if (iaqSensor.status < BSEC_OK) {
          output = "BSEC error code : " + String(iaqSensor.status);
          Serial.println(output);
          for (;;)
            errLeds(); /* Halt in case of failure */
        } else {
          output = "BSEC warning code : " + String(iaqSensor.status);
          Serial.println(output);
        }
      }
    
      if (iaqSensor.bme680Status != BME680_OK) {
        if (iaqSensor.bme680Status < BME680_OK) {
          output = "BME680 error code : " + String(iaqSensor.bme680Status);
          Serial.println(output);
          for (;;)
            errLeds(); /* Halt in case of failure */
        } else {
          output = "BME680 warning code : " + String(iaqSensor.bme680Status);
          Serial.println(output);
        }
      }
    }
    
    void errLeds(void)
    {
      pinMode(LED_BUILTIN, OUTPUT);
      digitalWrite(LED_BUILTIN, HIGH);
      delay(100);
      digitalWrite(LED_BUILTIN, LOW);
      delay(100);
    }
```


