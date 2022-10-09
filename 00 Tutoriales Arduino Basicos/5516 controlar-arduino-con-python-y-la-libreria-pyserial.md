/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/controlar-arduino-con-python-y-la-libreria-pyserial/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

python -m pip install PySerial


void setup() {
Serial.begin(9600);
}

void loop() {
Serial.println("Hola mundo");
delay(1000);
}


import serial, time
arduino = serial.Serial('COM4', 9600)
time.sleep(2)
rawString = arduino.readline()
print(rawString)
arduino.close()


const int pinLED = 13;

void setup() 
{
	Serial.begin(9600);
	pinMode(pinLED, OUTPUT);
}

void loop()
{
	if (Serial.available()>0) 
	{
		char option = Serial.read();
		if (option >= '1' && option <= '9')
		{
			option -= '0';
			for (int i = 0;i<option;i++) {="" digitalwrite(pinled,="" high);="" delay(100);="" digitalwrite(pinled,="" low);="" delay(200);="" }="" }="" }="" }=""></option;i++)>

import serial, time
arduino = serial.Serial("COM4", 9600)
time.sleep(2)
arduino.write(b'9')
arduino.close()