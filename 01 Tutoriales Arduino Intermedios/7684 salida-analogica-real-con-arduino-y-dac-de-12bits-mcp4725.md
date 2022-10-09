> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/salida-analogica-real-con-arduino-y-dac-de-12bits-mcp4725/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

```cpp
#include <Wire.h>
#include <Adafruit_MCP4725.h>

Adafruit_MCP4725 dac;

void setup(void) 
{
  dac.begin(0x60);
}

void loop(void) {
    uint32_t counter;
    for (counter = 0; counter < 4095; counter++)
    {
      dac.setVoltage(counter, false);
    }
    for (counter = 4095; counter > 0; counter--)
    {
      dac.setVoltage(counter, false);
    }
}
```