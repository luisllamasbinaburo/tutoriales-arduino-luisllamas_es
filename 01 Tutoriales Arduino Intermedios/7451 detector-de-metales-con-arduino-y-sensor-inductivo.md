> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/detector-de-metales-con-arduino-y-sensor-inductivo/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

```csharp
const int sensorPin = 9;

void setup()
{
   Serial.begin(9600);
}

void loop()
{
   bool state = digitalRead(sensorPin);

   //mandar mensaje a puerto serie en función del valor leido
   if (state == HIGH)
   {
      Serial.println("Detección");   
      //aquí se ejecutarían las acciones
   }
   delay(1000);
}
```