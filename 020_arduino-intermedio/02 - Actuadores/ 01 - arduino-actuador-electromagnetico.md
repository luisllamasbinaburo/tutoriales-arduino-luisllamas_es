> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/arduino-actuador-electromagnetico/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa


## Ejemplos de código
```cpp


void setup()
{
  pinMode(pin, OUTPUT);  //definir pin como salida
}
 
void loop()
{
  digitalWrite(pin, HIGH);   // poner el Pin en HIGH
  delay(10000);               // esperar 10 segundos
  digitalWrite(pin, LOW);    // poner el Pin en LOW
  delay(10000);               // esperar 10 segundos
}


