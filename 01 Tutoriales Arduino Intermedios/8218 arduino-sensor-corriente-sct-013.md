/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/arduino-sensor-corriente-sct-013/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

#define ADS1115_CONVERSIONDELAY         (8)


    #define ADS1115_CONVERSIONDELAY         (1)


#include <wire.h>
#include <adafruit_ads1015.h>
 
Adafruit_ADS1115 ads;
  
const float FACTOR = 30; //30A/1V

const float multiplier = 0.0625F;
 
void setup()
{
  Serial.begin(9600);
 
  ads.setGain(GAIN_TWO);        // ±2.048V  1 bit = 0.0625mV
  ads.begin();
}

void printMeasure(String prefix, float value, String postfix)
{
 Serial.print(prefix);
 Serial.print(value, 3);
 Serial.println(postfix);
}
 
void loop()
{
 float currentRMS = getCorriente();
 float power = 230.0 * currentRMS;
 
 printMeasure("Irms: ", currentRMS, "A ,");
 printMeasure("Potencia: ", power, "W");
 delay(1000);
}
 
float getCorriente()
{
 float voltage;
 float corriente;
 float sum = 0;
 long tiempo = millis();
 int counter = 0;
 
 while (millis() - tiempo < 1000)
 {
   voltage = ads.readADC_Differential_0_1() * multiplier;
   corriente = voltage * FACTOR;
   corriente /= 1000.0;
 
   sum += sq(corriente);
   counter = counter + 1;
  }
 
 corriente = sqrt(sum / counter);
 return(corriente);
}
</adafruit_ads1015.h></wire.h>

float getCorriente()
{
 long tiempo = millis();
 long rawAdc = ads.readADC_Differential_0_1();
 long minRaw = rawAdc;
 long maxRaw = rawAdc;
 while (millis() - tiempo < 1000)
 {
   rawAdc = ads.readADC_Differential_0_1();
   maxRaw = maxRaw > rawAdc ? maxRaw : rawAdc;
   minRaw = minRaw < rawAdc ? minRaw : rawAdc;
 }

  maxRaw = maxRaw > -minRaw ? maxRaw : -minRaw;
  float voltagePeak = maxRaw * multiplier / 1000;
  float voltageRMS = voltagePeak * 0.70710678118;
  float currentRMS = voltageRMS * FACTOR;
  return(currentRMS);
}


const float FACTOR = 30; //30A/1V

const float VMIN = 1.08;
const float VMAX = 3.92;

const float ADCV = 5.0;  //Para Vcc
//const float ADCV = 1.1; //Para referencia interna  


void setup()
{
	Serial.begin(9600);
	//analogReference(INTERNAL);
}

void printMeasure(String prefix, float value, String postfix)
{
	Serial.print(prefix);
	Serial.print(value, 3);
	Serial.println(postfix);
}

void loop()
{
	float currentRMS = getCorriente();
	float power = 230.0 * currentRMS;

	printMeasure("Irms: ", currentRMS, "A ,");
	printMeasure("Potencia: ", power, "W");
	delay(1000);
}

float getCorriente()
{
	float voltage;
	float corriente;
	float sum = 0;
	long tiempo = millis();
	int counter = 0;

	while (millis() - tiempo < 500)
	{
		voltage = analogRead(A0) * ADCV / 1023.0;
		corriente = fmap(voltage, VMIN, VMAX, -FACTOR, FACTOR);

		sum += sq(corriente);
		counter = counter + 1;
		delay(1);
	}

	corriente = sqrt(sum / counter);
	return(corriente);
}

// cambio de escala entre floats
float fmap(float x, float in_min, float in_max, float out_min, float out_max)
{
 return (x - in_min) * (out_max - out_min) / (in_max - in_min) + out_min;
}