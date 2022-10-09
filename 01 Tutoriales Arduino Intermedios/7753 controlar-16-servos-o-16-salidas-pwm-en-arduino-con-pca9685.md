/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/controlar-16-servos-o-16-salidas-pwm-en-arduino-con-pca9685/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

```cpp
//SCL -> A5
//SDA -> A4

#include <Wire.h>
#include <Adafruit_PWMServoDriver.h>

Adafruit_PWMServoDriver pwm = Adafruit_PWMServoDriver(0x40);

void setup() 
{
	pwm.begin();

	// Ajustamos la frecuencia al máximo (1600Hz)
	pwm.setPWMFreq(1600); 
}

void loop() 
{
	for (uint16_t duty = 0; duty < 4096; duty += 8)
	{
		for (uint8_t pwmNum = 0; pwmNum < 16; pwmNum++)
		{
			// Ajustar PWM con ON en tick=0 y OFF en tick=duty
			pwm.setPWM(pwmNum, 0, duty);
		}
	}
}
```

```cpp
//SCL -> A5
//SDA -> A4

#include <Wire.h>
#include <Adafruit_PWMServoDriver.h>

Adafruit_PWMServoDriver servoController = Adafruit_PWMServoDriver(0x40); 

const uint8_t frequency = 50;    // Frecuencia PWM de 50Hz o T=20ms
const uint16_t ServoMinTicks = 102; // ancho de pulso en ticks para pocicion 0°
const uint16_t ServoMaxTicks = 512; // ancho de pulso en ticks para la pocicion 180°

void setup()
{
	servoController.begin();
	servoController.setPWMFreq(frequency );
}

void loop()
{
	for (uint16_t duty = ServoMinTicks; duty < ServoMaxTicks; duty++)
	{
		for (uint8_t n = 0; n<16; n++)
		{
			servoController.setPWM(n, 0, duty);
		}
	}
	delay(1000);

	for (uint16_t duty = ServoMaxTicks; duty > ServoMinTicks; duty++)
	{
		for (uint8_t n = 0; n<16; n++)
		{
			servoController.setPWM(n, 0, duty);
		}
	}
	delay(1000);
}

```

```cpp

//SCL -> A5
//SDA -> A4

#include <Wire.h>
#include <Adafruit_PWMServoDriver.h>

Adafruit_PWMServoDriver servoController = Adafruit_PWMServoDriver(0x40);

const uint8_t frequency = 50;    // Frecuencia PWM de 50Hz o T=20ms
const uint16_t ServoMinMs = 500;  // Ancho de pulso en ms para pocicion 0°
const uint16_t ServoMaxMs = 2500; // Ancho de pulso en ms para la pocicion 180°

void setup()
{
	servoController.begin();
	servoController.setPWMFreq(frequency);
}


void setServoPulse(uint8_t n, double pulseMs)
{
	double bitlength;
	// 1,000,000 us per second / frequency / 4096 (12 bits)
	bitlength = 1000000 / frequency / 4096;
	double duty = pulseMs / bitlength;
	servoController.setPWM(n, 0, pulseMs);
}

void loop()
{
	for (uint16_t pulseWidth = ServoMinMs; pulseWidth < ServoMaxMs; pulseWidth = pulseWidth + 10)
	{
		for (uint8_t n = 0; n<16; n++)
		{
			setServoPulse(n, pulseWidth);
		}
	}
	delay(1000);

	for (uint16_t pulseWidth = ServoMaxMs; pulseWidth > ServoMinMs; pulseWidth = pulseWidth - 10)
	{
		for (uint8_t n = 0; n<16; n++)
		{
			setServoPulse(n, pulseWidth);
		}
	}
	delay(1000);
}
```