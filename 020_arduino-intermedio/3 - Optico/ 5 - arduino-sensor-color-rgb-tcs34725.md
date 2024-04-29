> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/arduino-sensor-color-rgb-tcs34725/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa


## Leer valores RGB
```cpp
#include <Wire.h>
#include "Adafruit_TCS34725.h"
   
/* Initialise with default values (int time = 2.4ms, gain = 1x) */
// Adafruit_TCS34725 tcs = Adafruit_TCS34725();

Adafruit_TCS34725 tcs = Adafruit_TCS34725(TCS34725_INTEGRATIONTIME_700MS, TCS34725_GAIN_1X);

void setup(void) {
  Serial.begin(9600);
  
  if (!tcs.begin()) 
  {
    Serial.println("Error al iniciar TCS34725");
    while (1) delay(1000);
  }

}

void loop(void) {
  uint16_t r, g, b, c, colorTemp, lux;
  
  tcs.getRawData(&r, &g, &b, &c);
  colorTemp = tcs.calculateColorTemperature(r, g, b);
  lux = tcs.calculateLux(r, g, b);
  
  Serial.print("Temperatura color: "); Serial.print(colorTemp, DEC); Serial.println(" K");
  Serial.print("Lux : "); Serial.println(lux, DEC);
  Serial.print("Rojo: "); Serial.println(r, DEC);
  Serial.print("Verde: "); Serial.println(g, DEC);
  Serial.print("Azul: "); Serial.println(b, DEC);
  Serial.print("Clear: "); Serial.println(c, DEC);
  Serial.println(" ");
  delay(1000);
}
```



## Clasificar colores
```cpp
#include <Wire.h>
#include "Adafruit_TCS34725.h"
#include "RGBConverterLib.h"

Adafruit_TCS34725 tcs = Adafruit_TCS34725(TCS34725_INTEGRATIONTIME_50MS, TCS34725_GAIN_1X);

void setup()
{
  Serial.begin(9600);

  if (!tcs.begin())
  {
    Serial.println("Error al iniciar TCS34725");
    while (1) delay(1000);
  }
}

void loop()
{
  uint16_t clear, red, green, blue;

  tcs.setInterrupt(false);
  delay(60); // Cuesta 50ms capturar el color
  tcs.getRawData(&red, &green, &blue, &clear);
  tcs.setInterrupt(true);

  // Hacer rgb medición relativa
  uint32_t sum = clear;
  float r, g, b;
  r = red; r /= sum;
  g = green; g /= sum;
  b = blue; b /= sum;

  // Escalar rgb a bytes
  r *= 256; g *= 256; b *= 256;

  // Convertir a hue, saturation, value
  double hue, saturation, value;
  RGBConverterLib::RgbToHsv(static_cast<uint8_t>(r), static_cast<uint8_t>(g), static_cast<uint8_t>(b), hue, saturation, value);

  // Mostrar nombre de color
  printColorName(hue * 360);

  delay(1000);
}

void printColorName(double hue)
{
  if (hue < 15)
  {
    Serial.println("Rojo");
  }
  else if (hue < 45)
  {
    Serial.println("Naranja");
  }
  else if (hue < 90)
  {
    Serial.println("Amarillo");
  }
  else if (hue < 150)
  {
    Serial.println("Verde");
  }
  else if (hue < 210)
  {
    Serial.println("Cyan");
  }
  else if (hue < 270)
  {
    Serial.println("Azul");
  }
  else if (hue < 330)
  {
    Serial.println("Magenta");
  }
  else
  {
    Serial.println("Rojo");
  }
}
```



## Clonar color en tira WS2812b
```cpp
#include <Wire.h>
#include "Adafruit_TCS34725.h"
#include "FastLED.h"
FASTLED_USING_NAMESPACE


#define DATA_PIN    6

#define LED_TYPE    WS2812

#define COLOR_ORDER GRB

#define BRIGHTNESS    250

#define FRAMES_PER_SECOND 250

Adafruit_TCS34725 tcs = Adafruit_TCS34725(TCS34725_INTEGRATIONTIME_50MS, TCS34725_GAIN_1X);

const unsigned long INTERVAL = 2000;
const int NUM_LEDS = 16;
CRGB leds[NUM_LEDS];

byte gammatable[256];

float r, g, b;

void setup()
{
  Serial.begin(9600);

  if (!tcs.begin())
  {
    Serial.println("Error al iniciar TCS34725");
    while (1) delay(1000);
  }
  
  delay(1000);
  FastLED.addLeds<LED_TYPE, DATA_PIN, COLOR_ORDER>(leds, NUM_LEDS);
  for (int i=0; i<256; i++)
  {
      float x = i;
      x /= 255;
      x = pow(x, 2.5);
      x *= 255;
      gammatable[i] = x;  
  }
}

void loop()
{
  readColor();
  calculateLeds(); 
  FastLED.show();
  FastLED.delay(1000 / FRAMES_PER_SECOND);
}

void readColor()
{
  uint16_t clear, red, green, blue;

  tcs.setInterrupt(false);
  delay(60);
  tcs.getRawData(&red, &green, &blue, &clear);
  tcs.setInterrupt(true);

  uint32_t sum = clear;
  r = red; r /= sum;
  g = green; g /= sum;
  b = blue; b /= sum;

  r *= 256;
  g *= 256;
  b *= 256;
}

void calculateLeds()
{
  fadeToBlackBy(leds, NUM_LEDS, 20);

  uint8_t pixel;
  unsigned long tCurrent = millis();
  pixel = (tCurrent % INTERVAL) * NUM_LEDS / INTERVAL;
  leds[pixel] = CRGB(gammatable[(int)r], gammatable[(int)g], gammatable[(int)b]);
}
```


