/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/arduino-bytes-puerto-serie/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

```cpp
const byte data[] = {0, 50, 100, 150, 200, 250};
const size_t dataLength = sizeof(data) / sizeof(data[0]);

void setup()
{
  Serial.begin(9600);
  Serial.write(data, dataLength);
}

void loop() 
{
}
```

```cpp
const int NUM_BYTES = 3;
byte data[NUM_BYTES];

bool TryGetSerialData(const byte* data, uint8_t dataLength)
{
	if (Serial.available() >= dataLength)
	{
		Serial.readBytes(data, dataLength);
		return true;
	}
	return false;
}

void OkAction()
{
}

void setup()
{
	Serial.begin(9600);
}

void loop()
{
	if(TryGetSerialData(data, NUM_BYTES))
	{
		OkAction();
	}
}
```

```cpp
void sendBytes(uint8_t value)
{
  Serial.write(value);
}

void sendBytes(uint16_t value)
{
  Serial.write(highByte(value));
  Serial.write(lowByte(value));
}

void sendBytes(uint32_t value)
{
  int temp = value & 0xFFFF;
  sendBytes(temp);
  temp = value >> 16;
  sendBytes(temp);
}
```

```cpp
uint8_t byteToInt(byte byte1)
{
	return (uint8_t)byte1;
}

uint16_t byteToInt(byte byte1, byte byte2)
{
	return (uint16_t)byte1 << 8 | (uint16_t)byte2;
}

uint32_t byteToLong(byte byte1, byte byte2, byte byte3, byte byte4)
{
	return byte1 << 24 
           | (uint32_t)byte2 << 16
           | (uint32_t)byte3 << 8
           | (uint32_t)byte4;
}
```