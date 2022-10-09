/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/reloj-y-calendario-en-arduino-con-los-rtc-ds1307-y-ds3231/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

#include <wire.h>
#include "RTClib.h"

// RTC_DS1307 rtc;
RTC_DS3231 rtc;

String daysOfTheWeek[7] = { "Domingo", "Lunes", "Martes", "Miercoles", "Jueves", "Viernes", "Sabado" };
String monthsNames[12] = { "Enero", "Febrero", "Marzo", "Abril", "Mayo",  "Junio", "Julio","Agosto","Septiembre","Octubre","Noviembre","Diciembre" };

void setup() {
	Serial.begin(9600);
	delay(1000); 

	if (!rtc.begin()) {
		Serial.println(F("Couldn't find RTC"));
		while (1);
	}

	// Si se ha perdido la corriente, fijar fecha y hora
	if (rtc.lostPower()) {
		// Fijar a fecha y hora de compilacion
		rtc.adjust(DateTime(F(__DATE__), F(__TIME__)));
		
		// Fijar a fecha y hora específica. En el ejemplo, 21 de Enero de 2016 a las 03:00:00
		// rtc.adjust(DateTime(2016, 1, 21, 3, 0, 0));
	}
}

void printDate(DateTime date)
{
	Serial.print(date.year(), DEC);
	Serial.print('/');
	Serial.print(date.month(), DEC);
	Serial.print('/');
	Serial.print(date.day(), DEC);
	Serial.print(" (");
	Serial.print(daysOfTheWeek[date.dayOfTheWeek()]);
	Serial.print(") ");
	Serial.print(date.hour(), DEC);
	Serial.print(':');
	Serial.print(date.minute(), DEC);
	Serial.print(':');
	Serial.print(date.second(), DEC);
	Serial.println();
}

void loop() {
	// Obtener fecha actual y mostrar por Serial
	DateTime now = rtc.now();
	printDate(now);

	delay(3000);
}
</wire.h>

#include <wire.h>
#include "RTClib.h"

const int outputPin = LED_BUILTIN;
bool state = false;

// RTC_DS1307 rtc;
RTC_DS3231 rtc;

void setup() {
	Serial.begin(9600);
	delay(1000);

	if (!rtc.begin()) {
		Serial.println(F("Couldn't find RTC"));
		while (1);
	}

	if (rtc.lostPower()) {
		rtc.adjust(DateTime(F(__DATE__), F(__TIME__)));
	}
}

// Comprobar si esta programado el encendido
bool isScheduledON(DateTime date)
{
	int weekDay = date.dayOfTheWeek();
	float hours = date.hour() + date.minute() / 60.0;

	// De 09:30 a 11:30 y de 21:00 a 23:00
	bool hourCondition = (hours > 9.50 && hours < 11.50) || (hours > 21.00 && hours < 23.00);

	// Miercoles, Sabado o Domingo
	bool dayCondition = (weekDay == 3 || weekDay == 6 || weekDay == 0); 
	if (hourCondition && dayCondition)
	{
		return true;
	}
	return false;
}

void loop() {
	DateTime now = rtc.now();

	if (state == false && isScheduledON(now))		// Apagado y debería estar encendido
	{
		digitalWrite(outputPin, HIGH);
		state = true;
		Serial.print("Activado");
	}
	else if (state == true && !isScheduledON(now))  // Encendido y deberia estar apagado
	{
		digitalWrite(outputPin, LOW);
		state = false;
		Serial.print("Desactivar");
	}

	delay(3000);
}</wire.h>

#include <spi.h>
#include <sd.h>
#include <wire.h>
#include "RTClib.h"

File logFile;

// RTC_DS1307 rtc;
RTC_DS3231 rtc;

void setup()
{
	Serial.begin(9600);
	Serial.print(F("Iniciando SD ..."));
	if (!SD.begin(4))
	{
		Serial.println(F("Error al iniciar"));
		return;
	}
	Serial.println(F("Iniciado correctamente"));
}


// Funcion que simula la lectura de un sensor
int readSensor()
{
	return 0;
}

void logValue(DateTime date, int value)
{
	logFile.print(date.year(), DEC);
	logFile.print('/');
	logFile.print(date.month(), DEC);
	logFile.print('/');
	logFile.print(date.day(), DEC);
	logFile.print(" ");
	logFile.print(date.hour(), DEC);
	logFile.print(':');
	logFile.print(date.minute(), DEC);
	logFile.print(':');
	logFile.print(date.second(), DEC);
	logFile.print(" ");
	logFile.println(value);
}


void loop()
{
	// Abrir archivo y escribir valor
	logFile = SD.open("datalog.txt", FILE_WRITE);

	if (logFile) {
		int value = readSensor();
		DateTime now = rtc.now();

		logValue(now, value);
		logFile.close();

	}
	else {
		Serial.println(F("Error al abrir el archivo"));
	}
	delay(10000);
}
</wire.h></sd.h></spi.h>