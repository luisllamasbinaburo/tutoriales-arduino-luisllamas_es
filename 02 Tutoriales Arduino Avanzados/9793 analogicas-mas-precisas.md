> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/analogicas-mas-precisas/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

```cpp
long readVcc() {
	ADMUX = _BV(REFS0) | _BV(MUX3) | _BV(MUX2) | _BV(MUX1);
	delay(2);
	ADCSRA |= _BV(ADSC);
	while (bit_is_set(ADCSRA, ADSC));
	
	long result = ADCL;
	result |= ADCH << 8;
	result = 1126400L / result; // Back-calculate AVcc in mV
	return result;
}

void setup() {
	Serial.begin(9600);
}

void loop() {
	Serial.println(readVcc(), DEC);
	delay(1000);
}
```

```cpp
long readVcc()
{
	ADMUX = _BV(REFS0) | _BV(MUX3) | _BV(MUX2) | _BV(MUX1);
	delay(2);
	ADCSRA |= _BV(ADSC);
	while (bit_is_set(ADCSRA, ADSC));
	
	long result = ADCL;
	result |= ADCH << 8;
	result = 1126400L / result; // Back-calculate AVcc in mV
	return result;
}

void setup()
{
	Serial.begin(9600);
}

void loop()
{
	float vcc = readVcc() / 1000.0;
	float voltage = (analogRead(A0) / 1024.0) * Vcc;

	Serial.println(voltage);
	delay(1000);
}

```