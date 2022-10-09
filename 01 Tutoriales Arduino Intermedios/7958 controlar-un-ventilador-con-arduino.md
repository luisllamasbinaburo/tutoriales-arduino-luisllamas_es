> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/controlar-un-ventilador-con-arduino/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

```cpp
const int pin = 9;

void setup() {
  pinMode(pin, OUTPUT);  //definir pin como salida
}
 
void loop(){
  digitalWrite(pin, HIGH);   // poner el Pin en HIGH
  delay(10000);               // esperar 10 segundos
  digitalWrite(pin, LOW);    // poner el Pin en LOW
  delay(10000);               // esperar 10 segundos

}
```

```cpp
const int pin = 9;

const float thresholdLOW = 20.0;
const float thresholdHIGH= 30.0;

bool state = false; /// ventilador activo o inactivo

float GetTemperature()
{
  return 20.0;  //sustituir en función del sensor empleado
}

void setup() {
  pinMode(pin, OUTPUT);  //definir pin como salida
}
 
void loop(){
  float currentTemperature = GetTemperature();

  if(state == false && currentTemperature > thresholdHIGH)
  {
      state = true;
      digitalWrite(pin, HIGH);   // encender ventilador
  }
  if(state == true && currentTemperature < thresholdLOW)
  {
      state = false;
      digitalWrite(pin, LOW);   // apagar ventilador
  }

   delay(5000);  // esperar 5 segundos entre mediciones
 }
```