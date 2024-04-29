> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/detector-llama-arduino/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa


## Ejemplos de código
```csharp
const int sensorPin = 9;

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
      Serial.println("Detección");   
      //aquí se ejecutarían las acciones
   }
   delay(1000);
}
```

```csharp
const int sensorPin = A0;

void setup() {
   Serial.begin(9600);
}

void loop() 
{
   int measure = analogRead(sensorPin);
   Serial.println(measure);
  
   if(measure < 500)
   {
      Serial.println("Detección");  
      //hacer las acciones necesarias
   }
   delay(1000);
}
```


