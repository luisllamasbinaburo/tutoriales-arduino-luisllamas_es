/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/arduino-serial-plotter/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

void setup() {
  Serial.begin(9600);
}

void loop() {

  float value;
  value = random(100,300)/100.0;

  Serial.println(value);

  delay(100);
}