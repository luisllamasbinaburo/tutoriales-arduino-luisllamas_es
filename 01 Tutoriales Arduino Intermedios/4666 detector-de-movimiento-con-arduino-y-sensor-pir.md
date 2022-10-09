/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/detector-de-movimiento-con-arduino-y-sensor-pir/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

const int LEDPin= 13;
const int PIRPin= 2;

void setup()
{
  pinMode(LEDPin, OUTPUT);
  pinMode(PIRPin, INPUT);
}

void loop()
{
  int value= digitalRead(PIRPin);
 
  if (value == HIGH)
  {
    digitalWrite(LEDPin, HIGH);
    delay(50);
    digitalWrite(LEDPin, LOW);
    delay(50);
  }
  else
  {
    digitalWrite(LEDPin, LOW);
  }
}


const int LEDPin = 13;        // pin para el LED
const int PIRPin = 2;         // pin de entrada (for PIR sensor)

int pirState = LOW;           // de inicio no hay movimiento
int val = 0;                  // estado del pin

void setup() 
{
	pinMode(LEDPin, OUTPUT); 
	pinMode(PIRPin, INPUT);
	Serial.begin(9600);
}

void loop()
{
	val = digitalRead(PIRPin);
	if (val == HIGH)   //si está activado
	{ 
		digitalWrite(LEDPin, HIGH);  //LED ON
		if (pirState == LOW)  //si previamente estaba apagado
		{
		  Serial.println("Sensor activado");
		  pirState = HIGH;
		}
	} 
	else   //si esta desactivado
	{
		digitalWrite(LEDPin, LOW); // LED OFF
		if (pirState == HIGH)  //si previamente estaba encendido
		{
		  Serial.println("Sensor parado");
		  pirState = LOW;
		}
	}
}