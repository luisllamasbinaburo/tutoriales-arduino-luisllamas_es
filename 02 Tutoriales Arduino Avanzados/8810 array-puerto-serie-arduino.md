/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/array-puerto-serie-arduino/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

const size_t dataLength = 10;
int data[dataLength];
 
void setup()
{
   Serial.begin(9600);
  
   for(int n = 0; n < dataLength; n++)
   {
      sendBytes(data[n]);
   }
} 
 
void loop() 
{ 
}
 
void sendBytes(uint16_t value)
{
  Serial.write(highByte(value));
  Serial.write(lowByte(value));
}


const size_t dataLength = 10;
int data[dataLength];
int dataIndex = 0;
 
void setup()
{
  Serial.begin(9600);
} 
 
 
void loop()
{   
  if(Serial.available() >= sizeof(data[0])
  {
    byte byte1 = Serial.read();
    byte byte2 = Serial.read();
    data[dataIndex] = byteToInt(byte1, byte2);
 
    dataIndex++;
    if(dataIndex >= dataLength)
    {
      dataIndex = 0;
    }
  } 
} 

uint16_t byteToInt(byte byte1, byte byte2)
{
   return (uint16_t)byte1 << 8 | (uint16_t)byte2;
}


const size_t dataLength = 10;
int data[dataLength];

void setup()
{
  Serial.begin(9600);
  Serial.write((byte*)data, dataLength * sizeof(data[0]));
} 

void loop()
{
}


const size_t dataLength = 10;
int data[dataLength];
 
void setup()
{
  Serial.begin(9600);
} 

void loop()
{   
  if(Serial.available() >= dataLength * sizeof(data[0]))
  {
    Serial.readBytes((byte*)data, dataLength * sizeof(data[0]));
  } 
}