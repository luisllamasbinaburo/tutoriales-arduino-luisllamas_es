/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/motores-paso-paso-arduino-driver-a4988-drv8825/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

```csharp
const int dirPin = 8;
const int stepPin = 9;

const int steps = 200;
int stepDelay;

void setup() {
	// Marcar los pines como salida
	pinMode(dirPin, OUTPUT);
	pinMode(stepPin, OUTPUT);
}

void loop() {
	//Activar una direccion y fijar la velocidad con stepDelay
	digitalWrite(dirPin, HIGH);
	stepDelay = 250;
	// Giramos 200 pulsos para hacer una vuelta completa
	for (int x = 0; x < steps * 1; x++) {
		digitalWrite(stepPin, HIGH);
		delayMicroseconds(stepDelay);
		digitalWrite(stepPin, LOW);
		delayMicroseconds(stepDelay);
	}
	delay(1000);

	//Cambiamos la direccion y aumentamos la velocidad
	digitalWrite(dirPin, LOW);
	stepDelay = 150;
	// Giramos 400 pulsos para hacer dos vueltas completas
	for (int x = 0; x < steps * 2; x++) {
		digitalWrite(stepPin, HIGH);
		delayMicroseconds(stepDelay);
		digitalWrite(stepPin, LOW);
		delayMicroseconds(stepDelay);
	}
	delay(1000);
}
```