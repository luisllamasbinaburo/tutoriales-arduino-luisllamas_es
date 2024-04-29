> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/cadenas-de-texto-puerto-serie-arduino/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa


## Leer un carácter
```cpp


void setup()
{
   Serial.begin(9600);
   Serial.setTimeout(50);
}

void loop()
{
   if (Serial.available())
   {
      char data = Serial.read();
      if ((data >= 'A'  && data <= 'Z') || (data >= 'a' && data <= 'z'))
      {
         DEBUG((char)data);
      }
   }
}




## Leer una cadena de texto con clase String
```cpp


void setup()
{
   Serial.begin(9600);
}

void loop()
{
   if (Serial.available())
   {
      String data = Serial.readStringUntil('\n');
      DEBUG(data);
   }
}




## Leer cadena de texto con char array
```cpp


void setup()
{
   Serial.begin(9600);
   Serial.setTimeout(50);
}

void loop()
{
   if (Serial.available())
   {
      char data[20];
      size_t count = Serial.readBytesUntil('\n', data, 20);
      DEBUG(data, count)
   }
}




## Leer cadena con método Naive
```cpp


String data = "";

void setup()
{
  Serial.begin(9600);
}

void loop()
{
  while (Serial.available())
  {
    char character = Serial.read();
    if (character != '\n')
    {
      data.concat(character);
    }
    else
    {
      Serial.println(data);
      data = "";
    }
  }
}





char data[20];
size_t dataIndex = 0;

void setup()
{
  Serial.begin(9600);
}

void loop()
{
  while (Serial.available())
  {
    char character = Serial.read();
    if (character != '\n')
    {
      data[dataIndex] = character;
      dataindex ++;
    }
    else
    {
      Serial.println(data);
      dataIndex = 0;
    }
  }
}


