> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/arduino-nfc-pn532/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa


## Mostrar datos y UID
```cpp
#include <Wire.h>
#include <SPI.h>
#include <Adafruit_PN532.h>


#define PN532_IRQ   (2)

#define PN532_RESET (3)  // Not connected by default on the NFC Shield

Adafruit_PN532 nfc(PN532_IRQ, PN532_RESET);

void setup(void) {
  Serial.begin(115200);
 
  nfc.begin();

  uint32_t versiondata = nfc.getFirmwareVersion();
  if (! versiondata) {
    Serial.print("PN53x no encontrado");
    while (1); // halt
  }

  // Mostrar datos del sensor
  Serial.print("Found chip PN5"); Serial.println((versiondata>>24) & 0xFF, HEX); 
  Serial.print("Firmware ver. "); Serial.print((versiondata>>16) & 0xFF, DEC); 
  Serial.print('.'); Serial.println((versiondata>>8) & 0xFF, DEC);
  
  // Configurar para leer etiquetas RFID
  nfc.setPassiveActivationRetries(0xFF);
  nfc.SAMConfig();
  
  Serial.println("Esperando tarjeta ISO14443A");
}

// Funcion auxiliar para mostrar el buffer
void printArray(byte *buffer, byte bufferSize) {
   for (byte i = 0; i < bufferSize; i++) {
      Serial.print(buffer[i] < 0x10 ? " 0" : " ");
      Serial.print(buffer[i], HEX);
   }
}
 

void loop(void) {
  boolean success;
  uint8_t uid[] = { 0, 0, 0, 0, 0, 0, 0 };
  uint8_t uidLength;

  success = nfc.readPassiveTargetID(PN532_MIFARE_ISO14443A, &uid[0], &uidLength);
  
  if (success) {
    Serial.println("Tarjeta encontrada");
    Serial.print("UID Longitud: ");Serial.print(uidLength, DEC);Serial.println(" bytes");
    Serial.print("UID: "); printArray(uid, uidLength);
    Serial.println("");
  
    delay(1000);
  }
  else
  {    
    Serial.println("Tarjeta no encontrada");
  }
}
```



## Grabar datos
```cpp
#include <Wire.h>
#include <SPI.h>
#include <Adafruit_PN532.h>


#define PN532_IRQ   (2)

#define PN532_RESET (3)

Adafruit_PN532 nfc(PN532_IRQ, PN532_RESET);

void setup(void) 
{
  Serial.begin(115200);

  // Configurar para leer etiquetas RFID
  nfc.begin();
  nfc.setPassiveActivationRetries(0xFF);
  nfc.SAMConfig();
  
  Serial.println("Esperando tarjeta");
}

void loop(void) 
{
  uint8_t success;
  uint8_t uid[] = { 0, 0, 0, 0, 0, 0, 0 };
  uint8_t uidLength;
    
  success = nfc.readPassiveTargetID(PN532_MIFARE_ISO14443A, uid, &uidLength);  
  if (success) {
      Serial.println("Intentando autentificar bloque 4 con clave KEYA");
      uint8_t keya[6] = { 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF };

      success = nfc.mifareclassic_AuthenticateBlock(uid, uidLength, 4, 0, keya);   
      if (success)
      {
        Serial.println("Sector 1 (Bloques 4 a 7) autentificados");
        uint8_t data[16];
 
        memcpy(data, (const uint8_t[]){ 'l', 'u', 'i', 's', 'l', 'l', 'a', 'm', 'a', 's', '.', 'e', 's', 0, 0, 0 }, sizeof data);
        success = nfc.mifareclassic_WriteDataBlock (4, data);
    
        if (success)
        {          
          Serial.println("Datos escritos en bloque 4");
          delay(10000);           
        }
        else
        {
          Serial.println("Fallo al escribir tarjeta");
          delay(1000);   
        }
      }
      else
      {
        Serial.println("Fallo autentificar tarjeta");
        delay(1000); 
      }
    }
}
```



## Leer datos
```cpp
#include <Wire.h>
#include <SPI.h>
#include <Adafruit_PN532.h>


#define PN532_IRQ   (2)

#define PN532_RESET (3)

Adafruit_PN532 nfc(PN532_IRQ, PN532_RESET);

void setup(void) 
{
  Serial.begin(115200);

  // Configurar para leer etiquetas RFID
  nfc.begin();
  nfc.setPassiveActivationRetries(0xFF);
  nfc.SAMConfig();
  
  Serial.println("Esperando tarjeta");
}

void loop(void) 
{
  uint8_t success;
  uint8_t uid[] = { 0, 0, 0, 0, 0, 0, 0 };
  uint8_t uidLength;
    
  success = nfc.readPassiveTargetID(PN532_MIFARE_ISO14443A, uid, &uidLength);  
  if (success) 
  {
      Serial.println("Intentando autentificar bloque 4 con clave KEYA");
      uint8_t keya[6] = { 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF };

      success = nfc.mifareclassic_AuthenticateBlock(uid, uidLength, 4, 0, keya);   
      if (success)
      {
        Serial.println("Sector 1 (Bloques 4 a 7) autentificados");
        uint8_t data[16];
          
        success = nfc.mifareclassic_ReadDataBlock(4, data);    
        if (success)
        {          
      Serial.println("Datos leidos de sector 4:");
          nfc.PrintHexChar(data, 16);
          Serial.println("");
          delay(5000);               
        }
        else
        {
          Serial.println("Fallo al leer tarjeta");
        }
      }
      else
      {
        Serial.println("Fallo autentificar tarjeta");
      }
    }
}
```



## Comprobar UID
```cpp
#include <Wire.h>
#include <SPI.h>
#include <Adafruit_PN532.h>


#define PN532_IRQ   (2)

#define PN532_RESET (3)

Adafruit_PN532 nfc(PN532_IRQ, PN532_RESET);

void setup(void) {
  Serial.begin(115200);

  // Configurar para leer etiquetas RFID
  nfc.begin();
  nfc.setPassiveActivationRetries(0xFF);
  nfc.SAMConfig();

  Serial.println("Esperando tarjeta");
}

const uint8_t validUID[4] = { 0xC8, 0x3E, 0xE7, 0x59 };  // Ejemplo de UID valido

//Función para comparar dos vectores
bool isEqualArray(uint8_t* arrayA, uint8_t* arrayB, uint8_t length)
{
  for (uint8_t index = 0; index < length; index++)
  {
    if (arrayA[index] != arrayB[index]) return false;
  }
  return true;
}

void loop(void)
{
  uint8_t success;
  uint8_t uid[] = { 0, 0, 0, 0, 0, 0, 0 };
  uint8_t uidLength;

  success = nfc.readPassiveTargetID(PN532_MIFARE_ISO14443A, uid, &uidLength);
  if (success)
  {
    uint8_t keya[6] = { 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF };

    if (isEqualArray(uid, validUID, uidLength))
    {
      Serial.println("Tarjeta valida");
      delay(5000);
    }
    else
    {
      Serial.println("Tarjeta invalida");
      delay(5000);
    }
  }
}
```



## Comprobar datos
```cpp
#include <Wire.h>
#include <SPI.h>
#include <Adafruit_PN532.h>


#define PN532_IRQ   (2)

#define PN532_RESET (3)

Adafruit_PN532 nfc(PN532_IRQ, PN532_RESET);

void setup(void) {
  Serial.begin(115200);

  // Configurar para leer etiquetas RFID
  nfc.begin();
  nfc.setPassiveActivationRetries(0xFF);
  nfc.SAMConfig();

  Serial.println("Esperando tarjeta");
}

const uint8_t validData[16] = { 'l', 'u', 'i', 's', 'l', 'l', 'a', 'm', 'a', 's', '.', 'e', 's', 0, 0, 0 };  // Ejemplo de clave valida

//Función para comparar dos vectores
bool isEqualArray(uint8_t* arrayA, uint8_t* arrayB, uint8_t length)
{
  for (uint8_t index = 0; index < length; index++)
  {
    if (arrayA[index] != arrayB[index]) return false;
  }
  return true;
}

void loop(void) {
  uint8_t success;
  uint8_t uid[] = { 0, 0, 0, 0, 0, 0, 0 };
  uint8_t uidLength;

  success = nfc.readPassiveTargetID(PN532_MIFARE_ISO14443A, uid, &uidLength);
  if (success)
  {
    uint8_t keya[6] = { 0xFF, 0xFF, 0xFF, 0xFF, 0xFF, 0xFF };

    success = nfc.mifareclassic_AuthenticateBlock(uid, uidLength, 4, 0, keya);
    if (success)
    {
      uint8_t data[16];

      success = nfc.mifareclassic_ReadDataBlock(4, data);
      if (success)
      {
        if (isEqualArray(data, validData, sizeof validData))
        {
          Serial.println("Tarjeta valida");
          delay(5000);
        }
        else
        {
          Serial.println("Tarjeta invalida");
          delay(5000);
        }
      }
    }
    else
    {
      Serial.println("Fallo autentificar tarjeta");
      delay(5000);
    }
  }
}
```


