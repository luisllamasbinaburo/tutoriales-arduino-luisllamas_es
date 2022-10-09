/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/electroiman-arduino/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

const int pin = 9;

void setup() {
  pinMode(pin, OUTPUT);  //definir pin como salida
}
 
void loop(){
  digitalWrite(pin, HIGH);   // poner el Pin en HIGH
  delay(10000);               // esperar un segundo
  digitalWrite(pin, LOW);    // poner el Pin en LOW
  delay(10000);               // esperar un segundo
}