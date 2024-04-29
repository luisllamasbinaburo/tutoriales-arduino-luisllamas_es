> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/enviar-recibir-numeros-puerto-serie-arduino/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa


## Recibir un único dígito
```cpp
#define DEBUG(a) Serial.println(a);

void setup()
{
   Serial.begin(9600);
}

void loop()
{
   if (Serial.available())
   {
      char data = Serial.read();

      if (data >= '0' && data <= '9')
      {
         data -= '0';
         DEBUG((int)data);
      }
   }
}
```



## Recibir con clase Serial
```cpp
#define DEBUG(a) Serial.println(a);

void setup()
{
   Serial.begin(9600);
   Serial.setTimeout(50);
}

void loop()
{
   if (Serial.available())
   {
      int data = Serial.parseInt();
      DEBUG((int)data);
   }
}
```

```cpp
#define DEBUG(a) Serial.println(a);

void setup()
{
   Serial.begin(9600);
   Serial.setTimeout(50);
}

void loop()
{
   if (Serial.available())
   {
      float data = Serial.parseFloat();
      DEBUG(data);
   }
}
```



## Recibir con clase String
```cpp
#define DEBUG(a) Serial.println(a);

void setup()
{
   Serial.begin(9600);
}

void loop()
{

   if (Serial.available() > 0)
   {
      String str = Serial.readStringUntil('\n');
      int data = str.toInt();
      DEBUG(data);
   }
}
```

```cpp
#define DEBUG(a) Serial.println(a);

void setup()
{
   Serial.begin(9600);
}

void loop()
{
   if (Serial.available() > 0)
   {
      String str = Serial.readStringUntil('\n');
      float data = str.toFloat();
      DEBUG(data);
   }
}
```



## Recibir en un char array
```cpp
#define DEBUG(a) Serial.println(a);

void setup()
{
   Serial.begin(9600);
   Serial.setTimeout(50);
}

void loop()
{
   if (Serial.available())
   {
      char buffer[7];
      Serial.readBytesUntil('\n', buffer, 7);
      int data = atoi(buffer);
      DEBUG(data);
   }
}
```

```cpp
#define DEBUG(a) Serial.println(a);

void setup()
{
   Serial.begin(9600);
   Serial.setTimeout(50);
}

void loop()
{
   if (Serial.available())
   {
      char buffer[7];
      Serial.readBytesUntil('\n', buffer, 7);
      int data = atof(buffer);
      DEBUG(data);
   }
}
```



## Recibir con método naive
```cpp
#define DEBUG(a) Serial.println(a);

void setup()
{
   Serial.begin(9600);
}

int data = 0;
bool isNegative = false;

void loop()
{
   while (Serial.available())
   {
      char incomingChar = Serial.read();
    
      if (incomingChar >= '0' && incomingChar <= '9')
         data = (data * 10) + (incomingChar - '0');
   
      else if (incomingChar == '-')
         isNegative = true;
   
      else if(incomingChar == '\n')
      {
         data = isNegative? -data : data;
         DEBUG(data);
         data = 0;
         isNegative = false;
      }
   }
}
```

```cpp
#define DEBUG(a) Serial.println(a);

void setup()
{
   Serial.begin(9600);
}

float data = 0;
int dataReal = 0;
int dataDecimal = 0;
int dataPow = 1;
bool isDecimalStage = false;
bool isNegative = false;

void loop()
{
   while (Serial.available())
   {
      char incomingChar = Serial.read();
    
    if (incomingChar == '-')
         isNegative = -1;
    
    else if (incomingChar == '.' || incomingChar == ',')
         isDecimalStage = true;

   else if (incomingChar >= '0' && incomingChar <= '9')
      {
         if (isDecimalStage == false)
            dataReal = (dataReal * 10) + (incomingChar - '0');
         else
         {
            dataDecimal = (dataDecimal * 10) + (incomingChar - '0');
            dataPow *= 10;
         }
      }
      else if (incomingChar == '\n')
      {
     data = (float)dataReal + (float)dataDecimal / dataPow;
         data = isNegative ? -data : data;
         WATCH_STOP;
         DEBUG(data);
         dataReal = 0;
         dataDecimal = 0;
         dataPow = 1;
         isDecimalStage = false;
         sign = 1;
      }
   }
}
```


