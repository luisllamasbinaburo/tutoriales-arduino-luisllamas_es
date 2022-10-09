/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/comunicacion-inalambrica-a-2-4ghz-con-arduino-y-nrf24l01/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

```cpp
#include <nRF24L01.h>
#include <RF24.h>
#include <RF24_config.h>
#include <SPI.h>

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
```

```cpp
#include <nRF24L01.h>
#include <RF24.h>
#include <RF24_config.h>
#include <SPI.h>

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
```

```cpp
#include <SPI.h>
#include <nRF24L01.h>
#include <RF24.h>
 
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
```

```cpp
#include <SPI.h>
#include <nRF24L01.h>
#include <RF24.h>
 
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
```