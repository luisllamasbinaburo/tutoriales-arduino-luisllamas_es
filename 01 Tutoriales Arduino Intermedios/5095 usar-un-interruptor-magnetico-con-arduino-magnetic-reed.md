/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/usar-un-interruptor-magnetico-con-arduino-magnetic-reed/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

```cpp
const int pinSensor = 2;
const int pinLED = 13;

void setup() {
  //configurar pin como entrada con resistencia pull-up interna
  pinMode(pinSensor, INPUT_PULLUP);
  pinMode(pinLED, OUTPUT);
}

void loop() {
  int value = digitalRead(pinSensor);

  if (value == LOW) {
    digitalWrite(pinLED, HIGH);
  } else {
    digitalWrite(pinLED, LOW);
  }

  delay(1000);
}
```