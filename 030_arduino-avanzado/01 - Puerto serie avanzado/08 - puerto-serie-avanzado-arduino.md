> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/puerto-serie-avanzado-arduino/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

## puerto-serie-avanzado-arduino
```cpp
const char STX = '\x002';
const char ETX = '\x003';
const char ACK = '\x006';
const char NAK = '\x015';
const int TimeOut = 100;

enum SerialResult
{
  OK,
  ERROR,
  NO_RESPONSE,
};

struct DataMessage
{
   int value;
};
 
struct DataMessage message;

void SendMessage(byte *structurePointer, int structureLength)
{
  Serial.write(STX);
    Serial.write(structurePointer, structureLength);
  
  uint16_t checksum = ChecksumFletcher16(structurePointer, structureLength);
  Serial.write((byte)checksum);
  Serial.write((byte)checksum >> 8);
  Serial.write(STX);
}

uint16_t ChecksumFletcher16(const byte *data, int dataLength)
{
  uint8_t sum1 = 0;
  uint8_t sum2 = 0;

  for (int index = 0; index < dataLength; ++index)
  {
    sum1 = sum1 + data[index];
    sum2 = sum2 + sum1;
  }
  return (sum2 << 8) | sum1;
}

int TryGetACK(int timeOut)
{
  unsigned long startTime = millis();

  while (!Serial.available() && (millis() - startTime) < timeOut)
  {
  }

  if (Serial.available())
  {
    if (Serial.read() == ACK) return OK;
  if (Serial.read() == NAK) return ERROR;
  }
  return NO_RESPONSE;
}

int ProcessACK(const int timeOut, 
       void (*okCallBack)(), 
       void (*errorCallBack)())
{
  int rst = TryGetACK(timeOut);
  if (rst == OK)
  {
    if(okCallBack != nullptr) okCallBack();
  }
  else
  {
    if(errorCallBack != nullptr) errorCallBack();
  }
  return rst;
}

void okAction(byte *data, const uint8_t dataLength)
{
}

void errorAction(byte *data, const uint8_t dataLength)
{
}

void setup()
{
  Serial.begin(9600);
  
  message.value = 10;  

  SendMessage((byte*)&message, sizeof(message));
  ProcessACK(TimeOut, okAction, errorAction);
}

void loop() 
{
}
```

```cpp
const char STX = '\x002';
const char ETX = '\x003';
const char ACK = '\x006';
const char NAK = '\x015';
const int TimeOut = 100;

struct DataMessage
{
   int value;
};
 
struct DataMessage message;

enum SerialResult
{
  OK,
  ERROR,
  NO_RESPONSE,
};

uint16_t ChecksumFletcher16(const byte *data, int dataLength)
{
  uint8_t sum1 = 0;
  uint8_t sum2 = 0;

  for (int index = 0; index < dataLength; ++index)
  {
    sum1 = sum1 + data[index];
    sum2 = sum2 + sum1;
  }
  return (sum2 << 8) | sum1;
}

int TryGetSerialData(byte *data, uint8_t dataLength, int timeOut)
{
  unsigned long startTime = millis();

  while (Serial.available() < (dataLength + 4) && (millis() - startTime) < timeOut)
  {
  }

  if (Serial.available() >= dataLength + 4)
  {
    if (Serial.read() == STX)
    {
      for (int i = 0; i < dataLength; i++)
      {
        data[i] = Serial.read();
      }

      if (Serial.read() == ETX && Serial.read() | Serial.read() << 8 == ChecksumFletcher16(data, dataLength))
      {
        return OK;
      }
      return ERROR;
    }
  }
  return NO_RESPONSE;
}

int ProcessSerialData(byte *data, const uint8_t dataLength, const int timeOut, 
       void (*okCallBack)(byte *, const uint8_t ), 
       void (*errorCallBack)(byte *, const uint8_t ))
{
  int rst = TryGetSerialData(data, dataLength, timeOut);
  if (rst == OK)
  {
    Serial.print(ACK);
  if(okCallBack != nullptr) okCallBack(data, dataLength);
  }
  else
  {
  Serial.print(NAK);
  if(errorCallBack != nullptr) errorCallBack(data, dataLength);
  }
  return rst;
}

void okAction(byte *data, const uint8_t dataLength)
{
}

void errorAction(byte *data, const uint8_t dataLength)
{
}

void setup()
{
  Serial.begin(9600);
}

void loop()
{
  ProcessSerialData((byte*)&message, sizeof(message), TimeOut, okAction, errorAction);
}
```


