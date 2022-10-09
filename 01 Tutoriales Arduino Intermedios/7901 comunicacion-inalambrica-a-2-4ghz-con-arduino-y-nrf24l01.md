/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/comunicacion-inalambrica-a-2-4ghz-con-arduino-y-nrf24l01/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

#include <nrf24l01.h>
#include <rf24.h>
#include <rf24_config.h>
#include <spi.h>

const int pinCE = 9;
const int pinCSN = 10;
RF24 radio(pinCE, pinCSN);

// Single radio pipe address for the 2 nodes to communicate.
const uint64_t pipe = 0xE8E8F0F0E1LL;
 
char data[16]="Hola mundo" ;

void setup(void)
{
	radio.begin();
	radio.openWritingPipe(pipe);
}
 
void loop(void)
{
	radio.write(data, sizeof data);
	delay(1000);
}
</spi.h></rf24_config.h></rf24.h></nrf24l01.h>

#include <nrf24l01.h>
#include <rf24.h>
#include <rf24_config.h>
#include <spi.h>

const int pinCE = 9;
const int pinCSN = 10;
RF24 radio(pinCE, pinCSN);

// Single radio pipe address for the 2 nodes to communicate.
const uint64_t pipe = 0xE8E8F0F0E1LL;

char data[16];

void setup(void)
{
	Serial.begin(9600);
	radio.begin();
	radio.openReadingPipe(1,pipe);
	radio.startListening();
}
 
void loop(void)
{
	if (radio.available())
	{
		radio.read(data, sizeof data); 
		Serial.println(data);
	}
}
</spi.h></rf24_config.h></rf24.h></nrf24l01.h>

#include <spi.h>
#include <nrf24l01.h>
#include <rf24.h>
 
const int pinCE = 9;
const int pinCSN = 10;
RF24 radio(pinCE, pinCSN);

// Single radio pipe address for the 2 nodes to communicate.
const uint64_t pipe = 0xE8E8F0F0E1LL;

float data[2];

void setup()
{
	radio.begin();
	radio.openWritingPipe(pipe);
}
 
void loop()
{ 
	data[0]= 3.14;
	data[1] = millis()/1000.0;
	
	radio.write(data, sizeof data);
	delay(1000);
}
</rf24.h></nrf24l01.h></spi.h>

#include <spi.h>
#include <nrf24l01.h>
#include <rf24.h>
 
const int pinCE = 9;
const int pinCSN = 10;
RF24 radio(pinCE, pinCSN);
 
// Single radio pipe address for the 2 nodes to communicate.
const uint64_t pipe = 0xE8E8F0F0E1LL;

float data[3];

void setup()
{
	radio.begin();
	Serial.begin(9600); 
	radio.openReadingPipe(1, pipe);
	radio.startListening();
}
 
void loop()
{
	if (radio.available())
	{    
		radio.read(data, sizeof data);
		Serial.print("Dato0= " );
		Serial.print(data[0]);
		Serial.print("Dato1= " );
		Serial.print(data[1]);
		Serial.println("");
	}
	delay(1000);
}
</rf24.h></nrf24l01.h></spi.h>