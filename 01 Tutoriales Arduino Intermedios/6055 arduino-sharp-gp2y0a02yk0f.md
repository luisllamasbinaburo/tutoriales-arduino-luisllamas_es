/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/arduino-sharp-gp2y0a02yk0f/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

```csharp
const int sensorPin = A0;
const long referenceMv = 5000;

void setup() {
	Serial.begin(9600);
	pinMode(ledPin, OUTPUT);
}

void loop() {
	//lectura de la tensión
	int val = analogRead(sensorPin);
	int mV = (val * referenceMv) / 1023;
	int cm = getDistance(mV);

	
	//mostrar valores por pantalla
	Serial.print(mV);
	Serial.print(",");
	Serial.println(cm);

	delay(1000);
}

//interpolación de la distancia a intervalos de 250mV
const int TABLE_ENTRIES = 12;
const int INTERVAL  = 250;
static int distance[TABLE_ENTRIES] = {150,140,130,100,60,50,40,35,30,25,20,15};

int getDistance(int mV) {
	if (mV > INTERVAL * TABLE_ENTRIES - 1)      return distance[TABLE_ENTRIES - 1];
	else {
		int index = mV / INTERVAL;
		float frac = (mV % 250) / (float)INTERVAL;
		return distance[index] - ((distance[index] - distance[index + 1]) * frac);
	}
}
```