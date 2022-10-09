> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/estructuras-puerto-serie-arduino/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

```cpp
void sendStructure(byte *structurePointer, int structureLength)
{
    Serial.write(structurePointer, structureLength);
}
sendStructure((byte*)&myStruct, sizeof(myStruct));
```

```cpp
void recieveStructure(byte *structurePointer, int structureLength)
{
    Serial.readBytes(structurePointer, structureLength);
}
recieveStructure((byte*)&myStruct, sizeof(myStruct));
```

```cpp
struct servoMoveMessage
{
   int  servoNum;
   int positionGoal;
   float interval;
};
 
struct servoMoveMessage message;

void sendStructure(byte *structurePointer, int structureLength)
{
    Serial.write(structurePointer, structureLength);
}

void setup()
{
  Serial.begin(9600);
  
  message.servoNum = 10;
  message.positionGoal = 1200;
  message.interval = 2.5;  

  sendStructure((byte*)&message, sizeof(message));
}

void loop() 
{
}
```

```cpp
struct servoMoveMessage
{
   int  servoNum;
   int positionGoal;
   float interval;
};
 
struct servoMoveMessage message;

void recieveStructure(byte *structurePointer, int structureLength)
{
  if(Serial.available() < sizeof(message)) return;
  Serial.readBytes(structurePointer, structureLength);
}

void setup()
{
  Serial.begin(9600);
  recieveStructure((byte*)&message, sizeof(message));
}

void loop() 
{
}
```