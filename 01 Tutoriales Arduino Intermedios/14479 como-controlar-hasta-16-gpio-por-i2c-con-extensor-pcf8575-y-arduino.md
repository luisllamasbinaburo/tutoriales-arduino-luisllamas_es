/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/como-controlar-hasta-16-gpio-por-i2c-con-extensor-pcf8575-y-arduino/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

```csharp
#include "PCF8575.h"

// Iniciar el PCF8575 en la dirección 0x20
PCF8575 pcf8575(0x20); 

void setup()
{
	Serial.begin(115200);

	// Establecer el pin P0 como salida
	pcf8575.pinMode(P0, OUTPUT);

	pcf8575.begin();
}

void loop()
{
	pcf8575.digitalWrite(P0, HIGH);
	delay(1000);
	pcf8575.digitalWrite(P0, LOW);
	delay(1000);
}
```