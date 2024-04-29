> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/mas-pines-digitales-con-arduino-y-el-expansor-es-i2c-pcf8574/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa


## Sin librería
```cpp
#include <Wire.h>

const int pcfAddress = 0x38;

void setup()
{
  Wire.begin();
  Serial.begin(9600);
}

void loop()
{
  for (short channel = 0; channel < 8; channel++)
  {
    // Escribir dato en canal 'channel'
    Wire.beginTransmission(pcfAddress);
    Wire.write(~(1 << channel));
    Wire.endTransmission();
    
    // Leer dato de canal
    delay(500);
  }
}
```

```cpp
#include <Wire.h>

const int pcfAddress = 0x38;

void setup()
{
  Wire.begin();
  Serial.begin(9600);
}

void loop()
{
  short channel = 1;
  byte value = 0;

  // Leer dato de canal 'channel'
  Wire.requestFrom(pcfAddress, 1 << channel);
  if (Wire.available())
  {
    value = Wire.read();
  }
  Wire.endTransmission();

  // Mostrar el valor por puerto serie
  Serial.println(value);
}
```



## Con librería
```cpp
#include <Wire.h>
#include "PCF8574.h"

PCF8574 expander;

void setup() 
{
  Serial.begin(9600);
  
  expander.begin(0x20);
  
  /* Setup some PCF8574 pins for demo */
  expander.pinMode(0, OUTPUT);
  expander.pinMode(1, OUTPUT);
  expander.pinMode(2, OUTPUT);
  expander.pinMode(3, INPUT_PULLUP);

  /* Blink hardware LED for debug */
  digitalWrite(13, HIGH);  
  
  /* Toggle PCF8574 output 0 for demo */
  expander.toggle();
  
  /* Blink hardware LED for debug */
  digitalWrite(13, LOW);
}

void loop() 
{
}
```


