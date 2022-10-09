/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/encender-un-led-con-arduino/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

```cpp
const int ledPIN = 9;
 
void setup() {
  Serial.begin(9600);    //iniciar puerto serie
  pinMode(ledPIN , OUTPUT);  //definir pin como salida
}
 
void loop(){
  digitalWrite(ledPIN , HIGH);   // poner el Pin en HIGH
  delay(1000);                   // esperar un segundo
  digitalWrite(ledPIN , LOW);    // poner el Pin en LOW
  delay(1000);                   // esperar un segundo
}
```

```csharp
const int ledPIN = 9;

int option;

void setup(){
  Serial.begin(9600);
  pinMode(ledPIN , OUTPUT); 
}

void loop(){
  //si existe información pendiente
  if (Serial.available()>0){
    //leeemos la opcion
    char option = Serial.read();
    //si la opcion esta entre '1' y '9'
    if (option >= '1' && option <= '9')
    {
      //restamos el valor '0' para obtener el numero enviado
      option -= '0';
      for(int i=0;i<option;i++){
         digitalWrite(ledPIN , HIGH);
         delay(100);
         digitalWrite(ledPIN , LOW);
         delay(200);
      }
    }
  }
}
```

```cpp
const int ledPIN = 9;

byte outputValue = 0;  

void setup()
{  
   Serial.begin(9600);         // Iniciar puerto serie
   pinMode(ledPIN , OUTPUT); 
}

void loop() 
{
   if (Serial.available()>0)  // Si hay datos disponibles
   {
      if(outputValue >= '0' && outputValue <= '9')
      {
         outputValue = Serial.read();	// Leemos la opción
         outputValue -= '0';		// Restamos '0' para convertir a un número
         outputValue *= 25;		// Multiplicamos x25 para pasar a una escala 0 a 250
         analogWrite(ledPIN , outputValue);
      }
   }
}  
```