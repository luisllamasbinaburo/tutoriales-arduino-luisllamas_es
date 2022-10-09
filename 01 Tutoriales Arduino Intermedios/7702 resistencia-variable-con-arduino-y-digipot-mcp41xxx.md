> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/resistencia-variable-con-arduino-y-digipot-mcp41xxx/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

```cpp
//SCK > D13
//SI  > D11
//CS  > D10

#include <SPI.h>
const byte address = 0x11;
const int CS = 10;

int digitalPotWrite(int value)
{
	digitalWrite(CS, LOW);
	SPI.transfer(address);
	SPI.transfer(value);
	digitalWrite(CS, HIGH);
}

void setup()
{
	pinMode(CS, OUTPUT);
	SPI.begin();
	
	// Resistencia maxima
	digitalPotWrite(0);
	delay(5000);

	// Resistencia intermedia
	digitalPotWrite(126);
	delay(5000);

	// Resistencia inferior
	digitalPotWrite(255);
	delay(5000);
}

void loop()
{
	for (int i = 0; i <= 255; i++)
	{
		digitalPotWrite(i);
		delay(100);
	}
	delay(2000);
	for (int i = 255; i >= 0; i--)
	{
		digitalPotWrite(i);
		delay(100);
	}
	delay(2000);
}
```