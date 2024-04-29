> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/arduino-eeprom-externa-i2c-at24c256/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

## Ejemplos de código
```cpp
//Vcc - 5V
//GND - GND
//SDA - A4
//SCL - A5

#include <Wire.h>
const int eepromAddress = 0x50;

// Funciones de lectura basadas en el trabajo de 
// Author: hkhijhe
// Date : 01 / 10 / 2010
void i2c_eeprom_write_byte(int deviceaddress, unsigned int eeaddress, byte data) {
  int rdata = data;
  Wire.beginTransmission(deviceaddress);
  Wire.write((int)(eeaddress >> 8)); // MSB
  Wire.write((int)(eeaddress & 0xFF)); // LSB
  Wire.write(rdata);
  Wire.endTransmission();
}

// WARNING: address is a page address, 6-bit end will wrap around
// also, data can be maximum of about 30 bytes, because the Wire library has a buffer of 32 bytes
void i2c_eeprom_write_page(int deviceaddress, unsigned int eeaddresspage, byte* data, byte length) {
  Wire.beginTransmission(deviceaddress);
  Wire.write((int)(eeaddresspage >> 8)); // MSB
  Wire.write((int)(eeaddresspage & 0xFF)); // LSB
  byte c;
  for (c = 0; c < length; c++)
    Wire.write(data[c]);
  Wire.endTransmission();
}

byte i2c_eeprom_read_byte(int deviceaddress, unsigned int eeaddress) {
  byte rdata = 0xFF;
  Wire.beginTransmission(deviceaddress);
  Wire.write((int)(eeaddress >> 8)); // MSB
  Wire.write((int)(eeaddress & 0xFF)); // LSB
  Wire.endTransmission();
  Wire.requestFrom(deviceaddress, 1);
  if (Wire.available()) rdata = Wire.read();
  return rdata;
}

// maybe let's not read more than 30 or 32 bytes at a time!
void i2c_eeprom_read_buffer(int deviceaddress, unsigned int eeaddress, byte *buffer, int length) {
  Wire.beginTransmission(deviceaddress);
  Wire.write((int)(eeaddress >> 8)); // MSB
  Wire.write((int)(eeaddress & 0xFF)); // LSB
  Wire.endTransmission();
  Wire.requestFrom(deviceaddress, length);
  int c = 0;
  for (c = 0; c < length; c++)
    if (Wire.available()) buffer[c] = Wire.read();
}

void setup()
{
  Wire.begin();
  Serial.begin(9600);

  // Datos de ejemplo
  char somedata[] = "datos grabados en la EEPROM";

  // Escribir ejemplo en memoria
  i2c_eeprom_write_page(eepromAddress, 0, (byte *)somedata, sizeof(somedata)); 
  delay(10);

  Serial.println(F("Escritura inicial completada"));
}

void loop()
{
  // Leer el primer byte de la memoria
  int addr = 0;
  byte value = i2c_eeprom_read_byte(eepromAddress, addr);

  // Leer e imprimir el caracter mientras sea distinto de '0'
  while (value != 0)
  {
    Serial.print((char)value);
    addr++;
    value = i2c_eeprom_read_byte(0x50, addr);
  }
  Serial.println(" ");
  delay(20000);
}
```


