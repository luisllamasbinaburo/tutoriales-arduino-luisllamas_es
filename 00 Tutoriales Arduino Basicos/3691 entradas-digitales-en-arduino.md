/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/entradas-digitales-en-arduino/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

int pin = 2;
int value = 0;

void setup() {
  Serial.begin(9600);   //iniciar puerto serie
  pinMode(pin, INPUT);  //definir pin como entrada
}

void loop(){
  value = digitalRead(pin);  //lectura digital de pin

  //mandar mensaje a puerto serie en función del valor leido
  if (value == HIGH) {
      Serial.println("Encendido");
  }
  else {
      Serial.println("Apagado");
  }
  delay(1000);
}