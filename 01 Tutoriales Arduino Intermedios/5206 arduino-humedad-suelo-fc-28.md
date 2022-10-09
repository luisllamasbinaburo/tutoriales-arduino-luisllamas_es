/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/arduino-humedad-suelo-fc-28/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

```csharp
const int sensorPin = A0;

void setup() {
   Serial.begin(9600);
}

void loop() 
{
   int humedad = analogRead(sensorPin);
   Serial.print(humedad);
  
   if(humedad < 500)
   {
      Serial.println("Encendido");  
      //hacer las acciones necesarias
   }
   delay(1000);
}
```

```csharp
const int sensorPin = 10;

void setup()
{
   Serial.begin(9600);
   pinMode(sensorPin, INPUT);
}

void loop()
{
   int humedad = digitalRead(sensorPin);

   //mandar mensaje a puerto serie en función del valor leido
   if (humedad == HIGH)
   {
      Serial.println("Encendido");   
      //aquí se ejecutarían las acciones
   }
   delay(1000);
}
```