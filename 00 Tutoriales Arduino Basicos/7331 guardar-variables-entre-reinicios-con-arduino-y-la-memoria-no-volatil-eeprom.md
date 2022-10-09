/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/guardar-variables-entre-reinicios-con-arduino-y-la-memoria-no-volatil-eeprom/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

```cpp
//Lee un único byte de la dirección address
EEPROM.Read(address) 

//Lee un único byte de la dirección address
EEPROM.Write(address)
```

```cpp
// Funciones para variables completas (tiene en cuenta el tamaño de la variable)
//Lee una variable en la dirección address
EEPROM.Put(address, variable) 

//Lee una variable en la dirección address
EEPROM.Get(address, variable) 

EEPROM.Update(address, variable) //Actualiza el valor de una variable, es decir, primero la lee, y sólo la graba si su valor es distinto del que vamos a guardar. Esto favorece a reducir el número de escrituras, y alargar la vida útil de la memoria.
```

```cpp
#include <EEPROM.h>

void setup(
{
  for( int index = 0 ; index < EEPROM.length() ; index++ )
  {
    EEPROM[ index ] = 0;
  } 
} 

void loop()
{
}
```

```cpp
#include <EEPROM.h>

float sensorValue;
int eeAddress = 0;

//Funcion que simula la lectura de un sensor
float ReadSensor()
{
  return 10.0f;
}

void setup()
{
}

void loop()
{
  sensorValue = ReadSensor(); //Lectura simulada del sensor
  EEPROM.put( eeAddress, sensorValue );  //Grabamos el valor
  eeAddress += sizeof(float);  //Obtener la siguiente posicion para escribir
  if(eeAddress >= EEPROM.length()) eeAddress = 0;  //Comprobar que no hay desbordamiento

  delay(30000); //espera 30 segunos
}
```

```cpp
#include <EEPROM.h>

struct MyStruct{
  float field1;
  byte field2;
  char name[10];
};

void setup()
{
  int eeAddress = 0;
  MyStruct customVar = {
    3.14f,
    65,
    "Working!"
  };

  eeAddress += sizeof(MyStruct);
  
  EEPROM.put( eeAddress, customVar ); 
}

void loop()
{
}
```

```cpp
#include <EEPROM.h>

int eeAddress = 0;

void setup()
{
}

void loop()
{
   int val = analogRead(0) / 4;
   EEPROM.update(eeAddress, val);
  
  eeAddress += sizeof(int);
  if(address == EEPROM.length()) eeAddress = 0;

  delay(10000);  //Espera de 10 segundos
}
```

```cpp
#include <EEPROM.h>

struct MyStruct{
  float field1;
  byte field2;
  char name[10];
};

void setup(){
  
  float f;
  int eeAddress = 0; //EEPROM address to start reading from    
  EEPROM.get( eeAddress, f );
  Serial.print( "Float leido: " );
  Serial.println( f, 3 );  
 
  eeAddress += sizeof(float);

  MyStruct customVar;
  EEPROM.get( eeAddress, customVar );  
  Serial.println( "Estructura leida: " );
  Serial.println( customVar.field1 );
  Serial.println( customVar.field2 );
  Serial.println( customVar.name );
}

void loop()
{
}
```