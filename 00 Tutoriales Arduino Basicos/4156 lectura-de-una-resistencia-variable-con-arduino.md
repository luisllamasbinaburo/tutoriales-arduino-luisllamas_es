/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/lectura-de-una-resistencia-variable-con-arduino/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

```cpp
const int sensorPin = A0;
const int Rc = 1500;  // valor de la resistencia de calibración

int V;				  // almacena el valor medido
long Rsensor;		  // almacena la resistencia calculada

void setup() {
}

void loop() {
	V = analogRead(sensorPin);		//realizar la lectura
	Rsensor = 1024L * Rc / V - Rc;   //calcular el valor de la resistencia

	//...haríamos lo que quisieramos por Rsensor

	delay(1000);
}
```