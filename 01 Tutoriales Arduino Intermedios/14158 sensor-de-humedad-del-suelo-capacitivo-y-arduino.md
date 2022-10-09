> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/sensor-de-humedad-del-suelo-capacitivo-y-arduino/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

```cpp
const int sensorPin = A0;

void setup()
{
 Serial.begin(9600);
}
void loop()
{
 int humedad = analogRead(sensorPin);
 Serial.print(humedad);
 delay(1000);
}
```

```cpp
const int sensorPin = A0;
const int humedadAire = 550;
const int humedadAgua = 250;

void setup() 
{
  Serial.begin(9600);
}

void loop() 
{
   int humedad = analogRead(sensorPin);
   Serial.println(humedad);
   
   porcentajeHumedad = map(humedad, humedadAire, humedadAgua, 0, 100);
   if(porcentajeHumedad > 100) porcentajeHumedad = 100;

   Serial.print(porcentajeHumedad);
   Serial.println("%"); 
}
```