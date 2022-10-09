> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/medir-temperatura-con-arduino-y-termistor-mf52/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

```cpp
#include <math.h>

const int Rc = 10000; //valor de la resistencia
const int Vcc = 5;
const int SensorPIN = A0;

float A = 1.11492089e-3;
float B = 2.372075385e-4;
float C = 6.954079529e-8;

float K = 2.5; //factor de disipacion en mW/C

void setup()
{
  Serial.begin(9600);
}

void loop() 
{
  float raw = analogRead(SensorPIN);
  float V =  raw / 1024 * Vcc;

  float R = (Rc * V ) / (Vcc - V);
  

  float logR  = log(R);
  float R_th = 1.0 / (A + B * logR + C * logR * logR * logR );

  float kelvin = R_th - V*V/(K * R)*1000;
  float celsius = kelvin - 273.15;

  Serial.print("T = ");
  Serial.print(celsius);
  Serial.print("C\n");
  delay(2500);
}
```