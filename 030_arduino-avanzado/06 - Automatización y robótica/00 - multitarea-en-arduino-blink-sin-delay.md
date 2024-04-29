> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/multitarea-en-arduino-blink-sin-delay/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

## Blink con delay
```cpp
void setup() 
{
   pinMode(LED_BUILTIN, OUTPUT);
}
 
void loop() 
{
   digitalWrite(LED_BUILTIN, HIGH);
   delay(1000);
   digitalWrite(LED_BUILTIN, LOW);
   delay(1000);
}
```


## Blink sin delay
```cpp
bool state = false;
// Muestra valores para el debug
void debug(char* text)
{
  Serial.print(millis());
  Serial.print('\t');
  Serial.println(text);
}

void setup()
{
  Serial.begin(115200);
}

void loop()
{
  toggleLed()
  delay(1000);
}

// Cambia el estado del Led
void toggleLed()
{
  state = !state;
  if (state) debug("ON");
  if (!state) debug("OFF");
}
```

```cpp
//Blink without delay
unsigned long interval = 1000;
unsigned long previousMillis;

bool state = false;

void debug(String text)
{
  Serial.print(millis());
  Serial.print('\t');
  Serial.println(text);
}

void setup() 
{
  Serial.begin(115200);
  
  // Capturar el primer millis
  previousMillis = millis();
}

void loop()
 {
  // Esta es la parte importante del ejemplo
  unsigned long currentMillis = millis();
  if ((unsigned long)(currentMillis - previousMillis) >= interval)
  {
    switchLed();
    previousMillis = millis();
  }
}

void switchLed()
{
  state = !state;
  if (state) debug("ON");
  if (!state) debug("OFF");
}
```


## Multitarea en Arduino
```cpp
//Blink without delay multitarea
unsigned long interval1 = 1000;
unsigned long interval2 = 800;
unsigned long previousMillis1;
unsigned long previousMillis2;

void debug(String text)
{
  Serial.print(millis());
  Serial.print('\t');
  Serial.println(text);
}

void setup() {
  Serial.begin(115200);
  previousMillis1 = millis();
  previousMillis2 = millis();
}

void loop() {
  unsigned long currentMillis = millis();

  // Gestionar el desbordamiento
  if ((unsigned long)(currentMillis - previousMillis1) >= interval1)
  {
    action1();
    previousMillis1 = millis();
  }
  if ((unsigned long)(currentMillis - previousMillis2) >= interval2)
  {
    action2();
    previousMillis2 = millis();
  }
}

void action1()
{
  debug("Action1");
}

void action2()
{
  debug("Action2");
}
```


## Multitarea en una clase
```cpp
#include "AsyncTaskLib.h"

AsyncTask asyncTask1(1000, []() { digitalWrite(LED_BUILTIN, HIGH); });
AsyncTask asyncTask2(2000, []() { digitalWrite(LED_BUILTIN, LOW); });

void setup()
{
  pinMode(LED_BUILTIN, OUTPUT);

  asyncTask1.Start();
}

void loop()
{
  asyncTask1.Update(asyncTask2);
  asyncTask2.Update(asyncTask1);
}
```


