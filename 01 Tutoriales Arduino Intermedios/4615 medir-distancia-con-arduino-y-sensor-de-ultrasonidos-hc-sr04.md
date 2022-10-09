/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/medir-distancia-con-arduino-y-sensor-de-ultrasonidos-hc-sr04/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

const int EchoPin = 5;
const int TriggerPin = 6;

void setup() {
	Serial.begin(9600);
	pinMode(TriggerPin, OUTPUT);
	pinMode(EchoPin, INPUT);
}

void loop() {
	int cm = ping(TriggerPin, EchoPin);
	Serial.print("Distancia: ");
	Serial.println(cm);
	delay(1000);
}

int ping(int TriggerPin, int EchoPin) {
	long duration, distanceCm;
	
	digitalWrite(TriggerPin, LOW);  //para generar un pulso limpio ponemos a LOW 4us
	delayMicroseconds(4);
	digitalWrite(TriggerPin, HIGH);  //generamos Trigger (disparo) de 10us
	delayMicroseconds(10);
	digitalWrite(TriggerPin, LOW);
	
	duration = pulseIn(EchoPin, HIGH);  //medimos el tiempo entre pulsos, en microsegundos
	
	distanceCm = duration * 10 / 292/ 2;   //convertimos a distancia, en cm
	return distanceCm;
}


#include <newping.h>

const int UltrasonicPin = 5;
const int MaxDistance = 200;

NewPing sonar(UltrasonicPin, UltrasonicPin, MaxDistance);

void setup() {
  Serial.begin(9600);
}

void loop() {
  delay(50);                      // esperar 50ms entre pings (29ms como minimo)
  Serial.print(sonar.ping_cm()); // obtener el valor en cm (0 = fuera de rango)
  Serial.println("cm");
}
</newping.h>