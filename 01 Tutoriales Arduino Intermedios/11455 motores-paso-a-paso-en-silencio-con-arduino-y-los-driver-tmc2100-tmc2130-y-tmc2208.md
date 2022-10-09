/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/motores-paso-a-paso-en-silencio-con-arduino-y-los-driver-tmc2100-tmc2130-y-tmc2208/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

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
	for (int x = 0; x < 200; x++) {
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
	for (int x = 0; x < 400; x++) {
		digitalWrite(stepPin, HIGH);
		delayMicroseconds(stepDelay);
		digitalWrite(stepPin, LOW);
		delayMicroseconds(stepDelay);
	}
	delay(1000);
}


const int enPin = 16;
const int dirPin = 19;
const int stepPin = 18;
const int csPin = 17;

const int steps = 200;
int stepDelay;

#include <tmc2130stepper.h>
TMC2130Stepper TMC2130 = TMC2130Stepper(enPin, dirPin, stepPin, csPin);

void setup() {
	Serial.begin(9600);

	pinMode(stepPin, OUTPUT);
	
	TMC2130.begin(); // Inicializar TMC2130
	TMC2130.SilentStepStick2130(600); // Fijar corriente a 600mA
	TMC2130.stealthChop(1); // Activar modo silencioso
}


void loop() {
	//Activar una direccion y fijar la velocidad con stepDelay
	driver.shaft(true);
	stepDelay = 250;
	// Giramos 200 pulsos para hacer una vuelta completa
	for (int x = 0; x < steps; x++) {
		digitalWrite(stepPin, HIGH);
		delayMicroseconds(stepDelay);
		digitalWrite(stepPin, LOW);
		delayMicroseconds(stepDelay);
	}
	delay(1000);

	driver.shaft(false);
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
</tmc2130stepper.h>

const int stepPin = 9;

const int steps = 200;
int stepDelay;

#include <tmc2208stepper.h>
TMC2208Stepper driver = TMC2208Stepper(&Serial);

void setup() {
	Serial.begin(9600);
	while(!Serial);

	pinMode(dirPin, OUTPUT);
	pinMode(stepPin, OUTPUT);
	
	driver.pdn_disable(1);	// Use PDN/UART pin for communication
	driver.I_scale_analog(0);
	driver.rms_current(500); // Fijar corriente a 500mA
	driver.toff(0x2); // Encender friver
}

void loop() {
	//Activar una direccion y fijar la velocidad con stepDelay
	driver.shaft(true);
	stepDelay = 250;
	// Giramos 200 pulsos para hacer una vuelta completa
	for (int x = 0; x < steps; x++) {
		digitalWrite(stepPin, HIGH);
		delayMicroseconds(stepDelay);
		digitalWrite(stepPin, LOW);
		delayMicroseconds(stepDelay);
	}
	delay(1000);

	driver.shaft(false);
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
</tmc2208stepper.h>