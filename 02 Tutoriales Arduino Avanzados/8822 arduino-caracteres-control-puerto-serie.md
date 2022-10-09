/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/arduino-caracteres-control-puerto-serie/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

```cpp
const char STX = '\x002';
const char ETX = '\x003';

const int data[] = {0, 50, 100, 150, 200, 250};
const size_t dataLength = sizeof(data) / sizeof(data[0]);
const int bytesLength = dataLength * sizeof(data[0]);


void setup()
{
  Serial.begin(9600);
  Serial.write(STX);
  Serial.write((byte*)&data, dataLength);
  Serial.write(ETX);
}

void loop() 
{
}
```

```cpp
const char STX = '\x002';
const char ETX = '\x003';

const int dataLength = 3;
size_t data[dataLength];
const int bytesLength = dataLength * sizeof(data[0]);

void setup()
{
  Serial.begin(9600);
}

void loop()
{
  if (Serial.available() >= bytesLength)
  {
    if (Serial.read() == STX)
    {
      Serial.readBytes((byte*)&data, bytesLength);

      if (Serial.read() == ETX)
      {
        //performAction();
      }
    }
  }
}
```