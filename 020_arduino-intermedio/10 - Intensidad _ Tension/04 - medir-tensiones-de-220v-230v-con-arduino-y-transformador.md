> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/medir-tensiones-de-220v-230v-con-arduino-y-transformador/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

## Montaje FZ0430 y ADS1115
```cpp
#include <Wire.h>
#include <Adafruit_ADS1015.h>

Adafruit_ADS1115 ads;

// const float multiplier = 0.1875F; // Transformador de 15V
const float multiplier = 0.1255F; // Transformador de 12V

void setup()
{
  Serial.begin(115200);

  // ads.setGain(GAIN_TWOTHIRDS);  // Transformador de 15V
  ads.setGain(GAIN_ONE);       // Transformador de 12V
  ads.begin();
}

void loop()
{
  while(1)
  {
    Serial.print(ads.readADC_Differential_0_1() * multiplier); 
  }
}
```


## Montaje con resistencias y punto medio
```cpp
const int sensorPin = A0;
int sensorValue;
float value; 
 
void setup() 
{
  Serial.begin(9600);
}
 
void loop(
{
 sensorValue = analogRead(sensorPin);  
 value = fmap(sensorValue, 0, 1023, -426.2080, +426.2080);
 
 delay(1);
}
 
// cambio de escala entre floats
float fmap(float x, float in_min, float in_max, float out_min, float out_max)
{
 return (x - in_min) * (out_max - out_min) / (in_max - in_min) + out_min;
}
```


