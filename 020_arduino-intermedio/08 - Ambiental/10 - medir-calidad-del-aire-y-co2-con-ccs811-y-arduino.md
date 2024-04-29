> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/medir-calidad-del-aire-y-co2-con-ccs811-y-arduino/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

## Ejemplos de código
```cpp
#include "Adafruit_CCS811.h"

Adafruit_CCS811 ccs;

void setup() {
  Serial.begin(9600);

  Serial.println("CCS811 test");

  if(!ccs.begin()){
    Serial.println("Failed to start sensor! Please check your wiring.");
    while(1);
  }

  // Wait for the sensor to be ready
  while(!ccs.available());
}

void loop() {
  if(ccs.available()){
    if(!ccs.readData()){
    Serial.println(ccs.calculateTemperature(););
    Serial.print("ºC, CO2: ");
      Serial.print(ccs.geteCO2());
      Serial.print("ppm, TVOC: ");
      Serial.println(ccs.getTVOC());
  }   
    else{
      Serial.println("ERROR!");
      while(1);
    }
  }
  delay(500);
}
```


