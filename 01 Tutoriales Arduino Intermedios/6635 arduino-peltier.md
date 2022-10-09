> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/arduino-peltier/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

```cpp
const int pin = 9;

void setup() {
  pinMode(pin, OUTPUT);  //definir pin como salida
}
 
void loop(){
  digitalWrite(pin, HIGH);   // poner el Pin en HIGH
  delay(5000);               // esperar 5 segundos
  digitalWrite(pin, LOW);    // poner el Pin en LOW
  delay(10000);               // esperar 10 segundos
}
```

```cpp
const int pin = 9;

const float thresholdLOW = 20.0;
const float thresholdHIGH= 30.0;

bool state = 0; //placa Peltier desacivada o desactivada

float GetTemperature()
{
  return 20.0;  //sustituir en función del sensor empleado
}

void setup() {
  pinMode(pin, OUTPUT);  //definir pin como salida
}
 
void loop(){
  float currentTemperature = GetTemperature();

  if(state == 0 && currentTemperature > thresholdHIGH)
  {
      state = 1;
      digitalWrite(pin, HIGH);   // encender la placa Peltier
  }
  if(state == 1 && currentTemperature < thresholdLOW)
  {
      state == 0;
      digitalWrite(pin, LOW);   // apagar la placa Peltier
  }

   delay(5000);  // esperar 5 segundos entre medicioens
 }
```