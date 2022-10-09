> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/que-son-y-como-usar-interrupciones-en-arduino/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

```csharp
attachInterrupt(interrupt, ISR, mode);
```

```csharp
attachInterrupt(digitalPinToInterrupt(pin), ISR, mode);
```

```csharp
const int emuPin = 10;

const int LEDPin = 13;
const int intPin = 2;
volatile int state = LOW;

void setup() {
	pinMode(emuPin, OUTPUT);
	pinMode(LEDPin, OUTPUT);
	pinMode(intPin, INPUT_PULLUP);
	attachInterrupt(digitalPinToInterrupt(intPin), blink, CHANGE);
}

void loop() {
	//esta parte es para emular la salida
	digitalWrite(emuPin, HIGH);
	delay(150);
	digitalWrite(emuPin, LOW);
	delay(150);
}

void blink() {
	state = !state;
	digitalWrite(LEDPin, state);
}
```

```csharp
const int emuPin = 10;

const int intPin = 2;
volatile int ISRCounter = 0;
int counter = 0;


void setup()
{
	pinMode(emuPin, OUTPUT);
	
	pinMode(intPin, INPUT_PULLUP);
	Serial.begin(9600);
	attachInterrupt(digitalPinToInterrupt(intPin), interruptCount, LOW);
}

void loop()
{
	//esta parte es para emular la salida
	digitalWrite(emuPin, HIGH);
	delay(1000);
	digitalWrite(emuPin, LOW);
	delay(1000);


	if (counter != ISRCounter)
	{
		counter = ISRCounter;
		Serial.println(counter);
	}
}

void interruptCount()
{
	ISRCounter++;
	timeCounter = millis();
}
```