/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/arduino-i2c-json/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

#include "Wire.h"
#include <arduinojson.hpp>
#include <arduinojson.h>

const byte I2C_SLAVE_ADDR = 0x20;

String response;
StaticJsonDocument<300> doc;

void setup()
{
	Serial.begin(115200);
	Wire.begin();
}

void loop()
{
	askSlave();
	if(response != "") DeserializeResponse();

}

const char ASK_FOR_LENGTH = 'L';
const char ASK_FOR_DATA = 'D';
void askSlave()
{
	response = "";

	unsigned int responseLenght = askForLength();
	if (responseLenght == 0) return;

	askForData(responseLenght);
	delay(2000);
}

unsigned int askForLength()
{
	Wire.beginTransmission(I2C_SLAVE_ADDR);
	Wire.write(ASK_FOR_LENGTH);
	Wire.endTransmission();

	Wire.requestFrom(I2C_SLAVE_ADDR, 1);
	unsigned int responseLenght = Wire.read();
	return responseLenght;
}

void askForData(unsigned int responseLenght)
{
	Wire.beginTransmission(I2C_SLAVE_ADDR);
	Wire.write(ASK_FOR_DATA);
	Wire.endTransmission();

	for (int requestIndex = 0; requestIndex <= (responseLenght / 32); requestIndex++)
	{
		Wire.requestFrom(I2C_SLAVE_ADDR, requestIndex < (responseLenght / 32) ? 32 : responseLenght % 32);
		while (Wire.available())
		{
			response += (char)Wire.read();
		}
	}
}

char*  text;
int id;
bool stat;
float value;
void DeserializeResponse()
{
	DeserializationError error = deserializeJson(doc, response);
    if (error) { return; }
 
    text = doc["text"];
    id = doc["id"];
    stat = doc["status"];
    value = doc["value"];
 
    Serial.println(text);
    Serial.println(id);
    Serial.println(stat);
    Serial.println(value);
}
</arduinojson.h></arduinojson.hpp>

#include "Wire.h"
#include <arduinojson.hpp>
#include <arduinojson.h>
 
String json;
StaticJsonDocument<300> doc;
void SerializeObject()
{
    doc["text"] = "myText";
    doc["id"] = 10;
    doc["status"] = true;
    doc["value"] = 3.14;
 
    serializeJson(doc, json);
}

const byte I2C_SLAVE_ADDR = 0x20;


void setup()
{
	Serial.begin(115200);

	SerializeObject();

	Wire.begin(I2C_SLAVE_ADDR);
	Wire.onRequest(requestEvent);
	Wire.onReceive(receiveEvent);
}

const char ASK_FOR_LENGTH = 'L';
const char ASK_FOR_DATA = 'D';


char request = ' ';
char requestIndex = 0;

void receiveEvent(int bytes)
{
	while (Wire.available())
	{
	  request = (char)Wire.read();
	}
}

void requestEvent()
{
	if(request == ASK_FOR_LENGTH)
	{
		Wire.write(json.length());
		char requestIndex = 0;
	}
	if(request == ASK_FOR_DATA)
	{
		if(requestIndex < (json.length() / 32)) 
		{
			Wire.write(json.c_str() + requestIndex * 32, 32);
			requestIndex ++;
		}
		else
		{
			Wire.write(json.c_str() + requestIndex * 32, (json.length() % 32));
			requestIndex = 0;
		}
	}

}

void loop() 
{
}
</arduinojson.h></arduinojson.hpp>