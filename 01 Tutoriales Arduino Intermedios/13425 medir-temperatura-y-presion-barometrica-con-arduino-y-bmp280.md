/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/medir-temperatura-y-presion-barometrica-con-arduino-y-bmp280/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

Adafruit_BMP280 bmp;

void setup() {
  Serial.begin(9600);
  Serial.println(F("BMP280 test"));

  if (!bmp.begin()) {
    Serial.println(F("Could not find a valid BMP280 sensor, check wiring!"));
    while (1);
  }

  bmp.setSampling(Adafruit_BMP280::MODE_NORMAL,     /* Modo de operación */
                  Adafruit_BMP280::SAMPLING_X2,     /* Temp. oversampling */
                  Adafruit_BMP280::SAMPLING_X16,    /* Presion oversampling */
                  Adafruit_BMP280::FILTER_X16,      /* Filtrado. */
                  Adafruit_BMP280::STANDBY_MS_500); /* Tiempo Standby. */
}

void loop() 
{
    Serial.print(F("Temperatura = "));
    Serial.print(bmp.readTemperature());
    Serial.println(" *C");

    Serial.print(F("Presión = "));
    Serial.print(bmp.readPressure());
    Serial.println(" Pa");

    Serial.print(F("Altitud = "));
    Serial.print(bmp.readAltitude(1013.25));
    Serial.println(" m");

    Serial.println();
    delay(2000);
}