> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/arduino-y-mando-a-distancia-de-4-canales-315mhz-ic2262/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

```cpp
// D0 - 8
// D1 - 9
// D2 - 10
// D3 - 11

const int pinB = 8;
const int pinD = 9;
const int pinA = 10;
const int pinC = 11;

void setup() 
{
	Serial.begin(9600);
}

void loop() 
{
	if (digitalRead(pinA) == HIGH)
	{
		Serial.println("A"); 
		delay(1500);
	}

	if (digitalRead(pinB) == HIGH)
	{
		Serial.println("B");
		delay(1500);
	}

	if (digitalRead(pinC) == HIGH)
	{
		Serial.println("C");
		delay(1500);
	}

	if (digitalRead(pinD) == HIGH)
	{
		Serial.println("D");
		delay(1500);
	}
	delay(10);

```