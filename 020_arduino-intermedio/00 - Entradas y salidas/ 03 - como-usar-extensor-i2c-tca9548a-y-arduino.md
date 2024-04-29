> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/como-usar-extensor-i2c-tca9548a-y-arduino/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa


## Escaner I2C
```csharp
#include "Wire.h"

void scanI2CBus(byte from_addr, byte to_addr, void(*callback)(byte address, byte result) ) 
{
  byte rc;
  byte data = 0;
  for( byte addr = from_addr; addr <= to_addr; addr++ ) {
    rc = twi_writeTo(addr, &data, 0, 1, 0);
    callback( addr, rc );
  }
}

void scanFunc( byte addr, byte result ) {
  Serial.print("addr: ");
  Serial.print(addr,DEC);
  Serial.print( (result==0) ? " Encontrado!":"       ");
  Serial.print( (addr%4) ? "\t":"\n");
}

const byte start_address = 8;
const byte end_address = 119;

void setup()
{
    Wire.begin();

    Serial.begin(9600);
    Serial.print("Escaneando bus I2C...");
    scanI2CBus( start_address, end_address, scanFunc );
    Serial.println("\nTerminado");
}

void loop() 
{
    delay(1000);
}
```



## Escaner I2C para TCA9548A
```csharp
/**
 * TCA9548 I2CScanner.ino -- I2C bus scanner for Arduino
 *
 * Based on https://playground.arduino.cc/Main/I2cScanner/
 *
 */

#include "Wire.h"


#define TCAADDR 0x70

void tcaselect(uint8_t i) {
  if (i > 7) return;
 
  Wire.beginTransmission(TCAADDR);
  Wire.write(1 << i);
  Wire.endTransmission();  
}

void setup()
{
    while (!Serial);
    delay(1000);

    Wire.begin();
    
    Serial.begin(115200);
    Serial.println("\nTCA escaner listo");
    
    for (uint8_t t=0; t<8; t++) {
      tcaselect(t);
      Serial.print("  Escaneando salida "); Serial.println(t);

      for (uint8_t addr = 0; addr<=127; addr++) {
        if (addr == TCAADDR) continue;

        Wire.beginTransmission(addr);
        if (!Wire.endTransmission()) {
          Serial.print("  - Encontrado I2C 0x");  Serial.println(addr,HEX);
        }
      }
    }
    Serial.println("Finalizado");
}

void loop() 
{
}
```


