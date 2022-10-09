> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/arduino-checksum/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

```cpp
uint8_t XORChecksum8(const byte *data, size_t dataLength)
{
  uint8_t value = 0;
  for (size_t i = 0; i < dataLength; i++)
  {
    value ^= (uint8_t)data[i];
  }
  return ~value;
}
```

```cpp
uint16_t XORChecksum16(const byte *data, size_t dataLength)
{
  uint16_t value = 0;
  for (size_t i = 0; i < dataLength / 2; i++)
  {
    value ^= data[2*i] + (data[2*i+1] << 8);
  }
  if(dataLength%2) value ^= data[dataLength - 1];
  return ~value;
}
```

```cpp
uint8_t TwoComplementChecksum8(const byte *data, size_t dataLength)
{
  uint8_t value = 0;
  for (size_t i = 0; i < dataLength; i++)
  {
    value += (uint8_t)data[i];
  }
  return ~value;
}
```

```cpp
uint16_t TwoComplementChecksum16(const byte *data, size_t dataLength)
{
  uint16_t value = 0;
  for (size_t i = 0; i < dataLength / 2; i++)
  {
    value += data[2*i] + (data[2*i+1] << 8);
  }
  if(dataLength%2) value += data[dataLength - 1];
  return value;
}
```

```cpp
uint8_t OneComplementChecksum8(const byte *data, size_t dataLength)
{
  uint16_t value = 0;
  for (size_t i = 0; i < dataLength; i++)
  {
    value += (uint8_t)data[i];
  }
  
   while (sum>>8)
    sum = (sum & 0xF) + (sum >> 8);

  return (uint8_t)~sum;
}
```

```cpp
uint16_t OneComplementChecksum16(const byte *data, size_t dataLength)
{
  uint32_t value = 0;
  for (size_t i = 0; i < dataLength / 2; i++)
  {
    value += data[2*i] + (data[2*i+1] << 8);
  }
  if(dataLength%2) value += data[dataLength - 1];
  
  while (sum>>16)
    sum = (sum & 0xFF) + (sum >> 16);
	
  return sum;
}
```

```cpp
uint16_t ChecksumFletcher16(byte *data, size_t count )
{
   uint8_t sum1 = 0;
   uint8_t sum2 = 0;

   for(size_t index = 0; index < count; ++index )
   {
      sum1 = sum1 + (uint8_t)data[index];
      sum2 = sum2 + sum1;
   }
   return (sum2 << 8) | sum1;
}
```

```cpp
uint16_t ChecksumFletcher16(byte *data, size_t count )
{
   uint16_t sum1 = 0;
   uint16_t sum2 = 0;

   for(size_t index = 0; index < count; ++index )
   {
    sum1 = sum1 + (uint8_t)data[index];
	while (sum1>>8) 
		sum1 = (sum1 & 0xF) + (sum1 >> 8);
	
    sum2 = sum2 + sum1;
	while (sum2>>8)
		sum2 = (sum2 & 0xF) + (sum2 >> 8);
   }
   return (sum2 << 8) | sum1;
}
```

```cpp
//CRC-8 - based on the CRC8 formulas by Dallas/Maxim
//code released under the therms of the GNU GPL 3.0 license
byte CRC8(const byte *data, size_t dataLength)
{
  byte crc = 0x00;
  while (dataLength--) 
  {
    byte extract = *data++;
    for (byte tempI = 8; tempI; tempI--)
	{
      byte sum = (crc ^ extract) & 0x01;
      crc >>= 1;
      if (sum)
	  {
        crc ^= 0x8C;
      }
      extract >>= 1;
    }
  }
  return crc;
}
```