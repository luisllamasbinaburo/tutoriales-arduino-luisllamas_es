> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/interruptor-touchless-con-arduino-y-sensor-capacitivo/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

```cpp
const int sensorPin = 9;

void setup()
{
   Serial.begin(9600);
   pinMode(sensorPin, INPUT);
}

void loop()
{
   int estado = digitalRead(sensorPin);

   //mandar mensaje a puerto serie en función del valor leido
   if (estado == HIGH)
   {
      Serial.println("Contacto detectado");   
      //aquí se ejecutarían las acciones
   }
   delay(1000);
}
```