/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/arduino-acelerometro-mma7455/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

```cpp
#include <Wire.h>
#include <MMA_7455.h>

MMA_7455 accel = MMA_7455(i2c_protocol);
float xg, yg, zg;

void setup()
{ 
  Serial.begin(9600);
  accel.begin();
  accel.setSensitivity(2);   //Definir el rango, valores 2, 4 o 8
  accel.setMode(measure);
  accel.setAxisOffset(0, 0, 0);
}

void loop()
{
  //leer los valores e imprimirlos
  xg  = accel.readAxis10g('x');
  yg  = accel.readAxis10g('y');
  zg  = accel.readAxis10g('z');

  Serial.print("\tXg: "); Serial.print(xg, DEC);
  Serial.print("\tYg: "); Serial.print(yg, DEC);
  Serial.print("\tZg: "); Serial.print(zg, DEC);
  Serial.println();
  delay(500);
}
```