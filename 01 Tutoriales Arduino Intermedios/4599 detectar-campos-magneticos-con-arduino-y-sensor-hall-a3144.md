/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/detectar-campos-magneticos-con-arduino-y-sensor-hall-a3144/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

const int HALLPin = 5;
const int LEDPin = 13;

void setup() {
  pinMode(LEDPin, OUTPUT);
  pinMode(HALLPin, INPUT);
}

void loop() {
  if(digitalRead(HALLPin)==HIGH)
  {
    digitalWrite(LEDPin, HIGH);   
  }
  else
  {
    digitalWrite(LEDPin, LOW);
  }
}