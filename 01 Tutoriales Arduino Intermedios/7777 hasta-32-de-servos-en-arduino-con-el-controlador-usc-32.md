/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/hasta-32-de-servos-en-arduino-con-el-controlador-usc-32/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

uint8_t ServoMinMs = 500; // ancho de pulso en ms para pocicion 0°
uint8_t ServoMaxMs = 2500; // ancho de pulso en ms para la pocicion 180°

void setup()
{
	Serial.begin(9600);
}

void loop() 
{
	for (uint8_t servo = 0; servo < 32; servo++)
	{
		for (uint16_t servoPos = ServoMinMs; servoPos < ServoMaxMs; servoPos++)
		{
			moveServo(servo, servoPos, 500);
		}
	}
	delay(750
	for (uint8_t servo = 0; servo < 32; servo++)
	{
		for (uint16_t servoPos = ServoMaxMs; servoPos > ServoMinMs; servoPos--)
		{
			moveServo(servo, servoPos, 500);
		}
	}
	delay(750);
}

void moveServo(uint8_t servo, uint16_t position, uint16_t time)
{
	Serial.print("#");
	Serial.print(servo);
	Serial.print("P");
	Serial.print(position);
	Serial.print("T");
	Serial.println(time);
}