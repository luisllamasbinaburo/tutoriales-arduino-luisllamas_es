> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/arduino-detector-movimiento-rcwl-0516/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa


## Ejemplos de código
```cpp
const int LEDPin = 13;
const int RadarPin = 2;
 
void setup()
{
  pinMode(LEDPin, OUTPUT);
  pinMode(RadarPin, INPUT);
}
 
void loop()
{
  int value= digitalRead(RadarPin);
 
  if (value == HIGH)
  {
    digitalWrite(LEDPin, HIGH);
    delay(50);
    digitalWrite(LEDPin, LOW);
    delay(50);
  }
  else
  {
    digitalWrite(LEDPin, LOW);
  }
}
```


