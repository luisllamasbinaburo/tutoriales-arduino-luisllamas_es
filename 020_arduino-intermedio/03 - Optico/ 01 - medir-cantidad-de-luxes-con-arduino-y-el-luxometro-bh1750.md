> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/medir-cantidad-de-luxes-con-arduino-y-el-luxometro-bh1750/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa


## Mostrar nivel de lux
```cpp
#include <Wire.h>
#include <BH1750.h>

BH1750 luxometro;

const byte luxMode = BH1750_CONTINUOUS_HIGH_RES_MODE;
// BH1750_CONTINUOUS_HIGH_RES_MODE
// BH1750_CONTINUOUS_HIGH_RES_MODE_2
// BH1750_CONTINUOUS_LOW_RES_MODE
// BH1750_ONE_TIME_HIGH_RES_MODE
// BH1750_ONE_TIME_HIGH_RES_MODE_2
// BH1750_ONE_TIME_LOW_RES_MODE

void setup() {
  Serial.begin(9600);
  Serial.println(F("Inicializando sensor..."));
  luxometro.begin(luxMode); // Inicializar BH1750
}

void loop() {
  uint16_t lux = luxometro.readLightLevel(); // Lectura del BH1750
  Serial.print(F("Iluminancia:  "));
  Serial.print(lux);
  Serial.println(" lx");
  delay(500);
}
```



## Encender un dispositivo con el luxómetro
```cpp
#include <Wire.h>
#include <BH1750.h>

BH1750 luxometro;;
const byte luxMode = BH1750_CONTINUOUS_HIGH_RES_MODE;

const uint16_t lowThreshold = 20;
const uint16_t highThreshold = 50;

const int pinOut = LED_BUILTIN;

void setup() {
  Serial.begin(9600);
  Serial.println(F("Inicializando sensor..."));
  luxometro.begin(luxMode); // Iniciar BH1750
}

void setup() {
  Serial.begin(9600);
  Serial.println(F("Inicializando sensor..."));
  luxometro.begin(BH1750_CONTINUOUS_HIGH_RES_MODE); // Iniciar sens el sensor
  pinMode(pinOut, OUTPUT);
  digitalWrite(pinOut, LOW);
}

void loop() {
  uint16_t lux = luxometro.readLightLevel();  // Lectura iluminancia
  
  if (lux < lowThreshold)
  {
    digitalWrite(pinOut, HIGH);
  }
  else if (lux > highThreshold)
  {
    digitalWrite(pinOut, LOW);
  }
  delay(500);
}
```


