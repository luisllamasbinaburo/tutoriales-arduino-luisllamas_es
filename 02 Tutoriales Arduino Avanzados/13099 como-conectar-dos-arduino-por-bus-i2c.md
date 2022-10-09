/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/como-conectar-dos-arduino-por-bus-i2c/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

<pre class="EnlighterJSRAW" data-enlighter-language="cpp">
#include "Wire.h"

const byte I2C_SLAVE_ADDR = 0x20;

long data = 100;
long response = 0;

void setup()
{
	Serial.begin(115200);
	Wire.begin();
}

void loop()
{
	sendToSlave();
	requestToSlave();
	delay(2000);
}

void sendToSlave()
{
	Wire.beginTransmission(I2C_SLAVE_ADDR);
	Wire.write((byte*)&data, sizeof(data));
	Wire.endTransmission();
}

void requestToSlave()
{
	response = 0;
	Wire.requestFrom(I2C_SLAVE_ADDR, sizeof(response));

	uint8_t index = 0;
	byte* pointer = (byte*)&response;
	while (Wire.available())
	{
		*(pointer + index) = (byte)Wire.read();
		index++;
	}

	Serial.println(response);
}
```

<pre class="EnlighterJSRAW" data-enlighter-language="cpp">
#include "Wire.h"

const byte I2C_SLAVE_ADDR = 0x20;

void setup()
{
	Serial.begin(115200);

	Wire.begin(I2C_SLAVE_ADDR);
	Wire.onReceive(receiveEvent);
	Wire.onRequest(requestEvent);
}

long data = 0;
long response = 200;

void receiveEvent(int bytes)
{
	data = 0;
	uint8_t index = 0;
	while (Wire.available())
	{
		byte* pointer = (byte*)&data;
		*(pointer + index) = (byte)Wire.read();
		index++;
	}
}

void requestEvent()
{
	Wire.write((byte*)&response, sizeof(response));
}


void loop() {

	if (data != 0)
	{
		Serial.println(data);
		data = 0;
	}
}
```