> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/arduino-mando-a-distancia-infrarrojo/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

```cpp
#include <IRremote.h>

const int RECV_PIN = 9;

IRrecv irrecv(RECV_PIN);
decode_results results;

void setup()
{
	Serial.begin(9600);
	irrecv.enableIRIn();
}

void loop()
{
	if (irrecv.decode(&results))
	{
		Serial.println(results.value, HEX);
		irrecv.resume();
	}
}
```

```cpp
#include <IRremote.h>

const int KEY_UP = 0xFF629D;
const int KEY_LEFT = 0xFF22DD;
const int KEY_OK = 0xFF02FD;
const int KEY_RIGHT = 0xFFC23D;
const int KEY_DOWN = 0xFFA857;
const int KEY_1 = 0xFF6897;
const int KEY_2 = 0xFF9867;
const int KEY_3 = 0xFFB04F;
const int KEY_4 = 0xFF30CF;
const int KEY_5 = 0xFF18E7;
const int KEY_6 = 0xFF7A85;
const int KEY_7 = 0xFF10EF;
const int KEY_8 = 0xFF38C7;
const int KEY_9 = 0xFF5AA5;
const int KEY_0 = 0xFF4AB5;
const int KEY_ASTERISK = 0xFF42BD;
const int KEY_POUND = 0xFF52AD;
const int KEY_REPEAT = 0xFFFFFFFF;

const int RECV_PIN = 9;

IRrecv irrecv(RECV_PIN);
decode_results results;

void setup()
{
	Serial.begin(9600);
	irrecv.enableIRIn();
}

void loop()
{
	if (irrecv.decode(&results))
	{
		switch (results.value)
		{
		case KEY_UP:
			Serial.println("KEY_UP");
			break;
		case KEY_LEFT:
			Serial.println("KEY_LEFT");
			break;
		case KEY_OK:
			Serial.println("KEY_OK");
			break;
		case KEY_RIGHT:
			Serial.println("KEY_RIGHT");
			break;
		case KEY_DOWN:
			Serial.println("KEY_DOWN");
			break;
		case KEY_1:
			Serial.println("KEY_1");
			break;
		case KEY_2:
			Serial.println("KEY_2");
			break;
		case KEY_3:
			Serial.println("KEY_3");
			break;
		case KEY_4:
			Serial.println("KEY_4");
			break;
		case KEY_5:
			Serial.println("KEY_5");
			break;
		case KEY_6:
			Serial.println("KEY_6");
			break;
		case KEY_7:
			Serial.println("KEY_7");
			break;
		case KEY_8:
			Serial.println("KEY_8");
			break;
		case KEY_9:
			Serial.println("KEY_9");
			break;
		case KEY_0:
			Serial.println("KEY_0");
			break;
		case KEY_ASTERISK:
			Serial.println("KEY_ASTERISK");
			break;
		case KEY_POUND:
			Serial.println("KEY_POUND");
			break;
		case KEY_REPEAT:
			Serial.println("KEY_REPEAT");
			break;
		}
		irrecv.resume();
	}
}
```