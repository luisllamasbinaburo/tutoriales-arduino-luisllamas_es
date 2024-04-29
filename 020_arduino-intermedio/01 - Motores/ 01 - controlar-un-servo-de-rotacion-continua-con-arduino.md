> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/controlar-un-servo-de-rotacion-continua-con-arduino/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa


## Ejemplos de código
```csharp
#include <Servo.h>

Servo myservo;  // crea el objeto servo

int vel = 0;    // velocidad del servo

void setup() {
  myservo.attach(9);  // vincula el servo al pin digital 9
}

void loop() {
  //servo parado (equivalente a angulo 90º)
  vel = 90;
  myservo.write(vel);              
  delay(1500);    

  //servo 100% CW (equivalente a angulo 180º)
  vel = 180;
  myservo.write(vel);              
  delay(1500); 

  //servo 100% CCW (equivalente a angulo 0º)
  vel = 0;
  myservo.write(vel);              
  delay(1500); 
}
```


