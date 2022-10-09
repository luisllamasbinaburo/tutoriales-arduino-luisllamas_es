> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/arduino-sensor-distancia-vl53l0x/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

```cpp
#include "Adafruit_VL53L0X.h"

Adafruit_VL53L0X lox = Adafruit_VL53L0X();

void setup() {
  Serial.begin(9600);

  // Iniciar sensor
  Serial.println("VL53L0X test");
  if (!lox.begin()) {
    Serial.println(F("Error al iniciar VL53L0X"));
    while(1);
  }
}


void loop() {
  VL53L0X_RangingMeasurementData_t measure;
    
  Serial.print("Leyendo sensor... ");
  lox.rangingTest(&measure, false); // si se pasa true como parametro, muestra por puerto serie datos de debug

  if (measure.RangeStatus != 4)
  {
    Serial.print("Distancia (mm): ");
	Serial.println(measure.RangeMilliMeter);
  } 
  else
  {
    Serial.println("  Fuera de rango ");
  }
    
  delay(100);
}

```