/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/arduino-mpr121/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

```cpp
#include <Wire.h>
#include "Adafruit_MPR121.h"

Adafruit_MPR121 mpr121 = Adafruit_MPR121();
uint8_t mpr121Address = 0x5A;

uint16_t lasttouched = 0;
uint16_t currtouched = 0;

void setup() {
	Serial.begin(9600);

	// Iniciar el sensor
	if (!mpr121.begin(mpr121Address)) {
		Serial.println("MPR121 no encontrado");
		while (1);
	}
	Serial.println("MPR121 iniciado");
}

// Devuelve true si el pad se ha tocado
bool isPressed(uint8_t channel)
{
	return bitRead(currtouched, channel) && !bitRead(lasttouched, channel)
}

// Devuelve true si el pad se ha dejado de tocar
bool isReleased(uint8_t channel)
{
	return !bitRead(currtouched, channel) && bitRead(lasttouched, channel)
}

void loop()
{
	// Obtener los datos del sensor
	mpr121.updateTouchData();
	currtouched = mpr121.touched();

	// Comprobamos los cambios en todos los canales
	for (uint8_t sensorChannel  = 0; sensorChannel < 12; i++) 
	{
		if (isPressed(sensorChannel) ) 
		{
			Serial.print(sensorChannel ); 
			Serial.println(" tocado");
		}

		if (isReleased(sensorChannel))
		{
			Serial.print(sensorChannel); 
			Serial.println(" soltado");
		}
	}

	lasttouched = currtouched;
}
```