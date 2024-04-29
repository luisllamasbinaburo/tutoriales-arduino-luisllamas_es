> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/medir-inclinacion-con-arduino-y-sensor-tilt-sw-520d/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

## Ejemplo de código
```cpp
const int SensorPin = 2;
const int LEDPin = 13;

void setup() {
    pinMode(SensorPin , INPUT);
    digitalWrite(SensorPin , HIGH);    //activamos la resistencia interna PULL UP
    pinMode(LEDPin, OUTPUT);
}

void loop() {
    if (digitalRead(SensorPin))  {
        digitalWrite(LEDPin, HIGH);
    }  else  {
        digitalWrite(LEDPin, LOW);
    }
}
```


