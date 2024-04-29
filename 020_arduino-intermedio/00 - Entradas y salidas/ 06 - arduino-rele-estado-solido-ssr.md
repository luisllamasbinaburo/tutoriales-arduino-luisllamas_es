> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/arduino-rele-estado-solido-ssr/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa


## Ejemplos de código
```cpp
const int pin = 9;
 
void setup()
{
  Serial.begin(9600);    //iniciar puerto serie
  pinMode(pin, OUTPUT);  //definir pin como salida
}
 
void loop()
{
  digitalWrite(pin, HIGH);   // poner el Pin en HIGH
  delay(10000);               // esperar diez segundos
  digitalWrite(pin, LOW);    // poner el Pin en LOW
  delay(10000);               // esperar diez segundos
}
```


