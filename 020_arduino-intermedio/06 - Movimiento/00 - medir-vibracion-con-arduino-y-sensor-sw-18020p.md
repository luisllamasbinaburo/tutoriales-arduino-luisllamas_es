> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/medir-vibracion-con-arduino-y-sensor-sw-18020p/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

## Ejemplo de código
```cpp
const int sensorPin = 2;
const int ledPin = 13;

int tiltSensorPreviousValue = 0;
int tiltSensorCurrentValue = 0;
long lastTimeMoved = 0;
int shakeTime = 50;

void setup() {
    pinMode(sensorPin, INPUT);
    digitalWrite(sensorPin, HIGH);  //activamos la resistencia interna PULL UP
    pinMode(ledPin, OUTPUT);
}

void loop() {
    tiltSensorCurrentValue = digitalRead(sensorPin);
    if (tiltSensorPreviousValue != tiltSensorCurrentValue) {
        lastTimeMoved = millis();
        tiltSensorPreviousValue = tiltSensorCurrentValue;
    }
    if (millis() - lastTimeMoved < shakeTime) {
        digitalWrite(ledPin, HIGH);
    }
    else {
        digitalWrite(ledPin, LOW);
    }
}
```


