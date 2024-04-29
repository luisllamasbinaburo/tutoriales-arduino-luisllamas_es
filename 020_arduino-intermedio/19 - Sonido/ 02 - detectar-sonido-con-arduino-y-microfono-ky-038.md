> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/detectar-sonido-con-arduino-y-microfono-ky-038/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa


## Ejemplos de código
```cpp
const int pinLED = 13;
const int pinMicrophone = 9;

void setup ()
{
  pinMode (pinLED, OUTPUT);
  pinMode (pinMicrophone, INPUT);
}
 
void loop ()
{
  bool soundDetected = digitalRead(pinMicrophone);
  if (soundDetected)
  {
    digitalWrite (pinLED, HIGH);
    delay(1000);
  }
  else
  {
    digitalWrite (pinLED, LOW);
    delay(10);
  }
}
```

```cpp
const int pinLED = 13; 
const int pinMicrophone = 9;
bool state;

void setup()
{
  pinMode(pinLED, OUTPUT);
  pinMode(pinMicrophone, INPUT_PULLUP);
}

void loop()
{
  bool soundDetected = digitalRead(pinMicrophone); 
  if (soundDetected == true)
  {
    state = ! state;    
    digitalWrite(pinLED , state);
    delay (1000);
  }
  delay(10);
}
```


