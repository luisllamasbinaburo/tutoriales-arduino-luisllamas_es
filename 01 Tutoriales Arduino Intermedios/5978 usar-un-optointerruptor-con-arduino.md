/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/usar-un-optointerruptor-con-arduino/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

```csharp
const int sensorPin = 9;

void setup() {
  Serial.begin(9600);   //iniciar puerto serie
  pinMode(sensorPin , INPUT);  //definir pin como entrada
}
 
void loop(){
  int value = 0;
  value = digitalRead(sensorPin );  //lectura digital de pin
 
  if (value == LOW) {
      Serial.println("Optointerruptor activado");
  }
  delay(1000);
}
```