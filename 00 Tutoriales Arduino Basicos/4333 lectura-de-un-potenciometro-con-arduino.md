> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/lectura-de-un-potenciometro-con-arduino/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

```cpp
const int analogPin = A0;
int value;      //variable que almacena la lectura analógica raw
int position;   //posicion del potenciometro en tanto por ciento

void setup() {
}

void loop() {
	value = analogRead(analogPin);			 // realizar la lectura analógica raw
	position = map(value, 0, 1023, 0, 100);  // convertir a porcentaje

	//...hacer lo que se quiera, con el valor de posición medido

	delay(1000);
}
```