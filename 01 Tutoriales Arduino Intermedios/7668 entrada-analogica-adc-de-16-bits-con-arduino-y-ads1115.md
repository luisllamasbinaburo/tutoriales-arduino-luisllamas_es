> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/entrada-analogica-adc-de-16-bits-con-arduino-y-ads1115/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

```cpp
#include <Wire.h>
#include <Adafruit_ADS1015.h>

Adafruit_ADS1115 ads;
const float multiplier = 0.1875F;

void setup(void) 
{
  Serial.begin(9600);

  // Descomentar el que interese
  // ads.setGain(GAIN_TWOTHIRDS);  +/- 6.144V  1 bit = 0.1875mV (default)
  // ads.setGain(GAIN_ONE);        +/- 4.096V  1 bit = 0.125mV
  // ads.setGain(GAIN_TWO);        +/- 2.048V  1 bit = 0.0625mV
  // ads.setGain(GAIN_FOUR);       +/- 1.024V  1 bit = 0.03125mV
  // ads.setGain(GAIN_EIGHT);      +/- 0.512V  1 bit = 0.015625mV
  // ads.setGain(GAIN_SIXTEEN);    +/- 0.256V  1 bit = 0.0078125mV 
  ads.begin();
}

void loop(void) 
{
  int16_t adc0, adc1, adc2, adc3;

  
  adc0 = ads.readADC_SingleEnded(0);
  adc1 = ads.readADC_SingleEnded(1);
  adc2 = ads.readADC_SingleEnded(2);
  adc3 = ads.readADC_SingleEnded(3);
  Serial.print("AIN0: "); Serial.println(adc0 * multiplier);
  Serial.print("AIN1: "); Serial.println(adc1 * multiplier);
  Serial.print("AIN2: "); Serial.println(adc2 * multiplier);
  Serial.print("AIN3: "); Serial.println(adc3 * multiplier);
  Serial.println(" ");
  
  delay(1000);
}
```

```cpp
#include <Wire.h>
#include <Adafruit_ADS1015.h>

Adafruit_ADS1115 ads;
const float multiplier = 0.1875F;

void setup(void)
{
  Serial.begin(9600);
 
  ads.begin();
}

void loop(void)
{
  int16_t results = ads.readADC_Differential_0_1();  
 
  Serial.print("Diferencial: "); 
  Serial.print(results); 
  Serial.print(" "); 
  Serial.print(results * multiplier); 
  Serial.println("mV");

  delay(1000);
}
```

```cpp
#include <Wire.h>
#include <Adafruit_ADS1015.h>

Adafruit_ADS1115 ads;

void setup(void) 
{
  Serial.begin(9600);
  
  ads.begin();
  
  // Activar comparador para 2.5V en canal 0 
  // (Donde 13333 = 2500 / 0.1875F)
  ads.startComparator_SingleEnded(0, 13333); 
}

void loop(void) 
{
  int16_t adc0;

  // Actualizar comparadores
  adc0 = ads.getLastConversionResults();
  Serial.print("AIN0: "); Serial.println(adc0);
  
  delay(100);
}
```