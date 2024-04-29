> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/detectar-gestos-con-arduino-y-sensor-apds-9960/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa


## Mostrar por puerto de serie
```cpp
#include "Adafruit_APDS9960.h"
Adafruit_APDS9960 apds;

void setup() {
  Serial.begin(115200);

  if(!apds.begin()) Serial.println("failed to initialize device! Please check your wiring.");
  apds.enableProximity(true);
  apds.enableGesture(true);
}

void loop() {
    uint8_t gesture = apds.readGesture();
    if(gesture == APDS9960_DOWN) Serial.println("DOWN");
    if(gesture == APDS9960_UP) Serial.println("UP");
    if(gesture == APDS9960_LEFT) Serial.println("LEFT");
    if(gesture == APDS9960_RIGHT) Serial.println("RIGHT");
}
```



## Mostrar por Led integrado
```cpp
#include "Adafruit_APDS9960.h"
Adafruit_APDS9960 apds;

void setup() 
{
  Serial.begin(115200);

  pinMode(LED_BUILTIN, OUTPUT);
  
  if(!apds.begin()) Serial.println("failed to initialize device! Please check your wiring.");
  apds.enableProximity(true);
  apds.enableGesture(true);
}

void loop() 
{
    uint8_t gesture = apds.readGesture();
    if(gesture == APDS9960_DOWN) blink(1);
    if(gesture == APDS9960_UP) blink(2);
    if(gesture == APDS9960_LEFT) blink(3);
    if(gesture == APDS9960_RIGHT) blink(4);
}

void blink(int count)
{
  for(int i = 0; i < count; i++)
  {
    digitalWrite(LED_BUILTIN, HIGH);   
  delay(100);                     
  digitalWrite(LED_BUILTIN, LOW);   
  delay(100);     
  }
}
```


