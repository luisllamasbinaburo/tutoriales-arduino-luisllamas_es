> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/localizacion-gps-con-arduino-y-los-modulos-gps-neo-6/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa


## Obtener lectura NMEA
```cpp
#include <SoftwareSerial.h>

const int RX = 4;
const int TX = 3;

SoftwareSerial gps(RX, TX);

void setup()
{
  Serial.begin(115200);
  gps.begin(9600);
}

void loop()
{
  if (gps.available())
  {
    char data;
    data = gps.read();
    Serial.print(data);
  }
}
```

```bash
$GPRMC,hhmmss.ss,A,llll.ll,a,yyyyy.yy,a,x.x,x.x,ddmmyy,x.x,a\*hh
```

```bash
$GPRMC,225446,A,4916.45,N,12311.12,W,000.5,054.7,191194,020.3,E\*68
```



## Obtener coordenadas con TinyGPS
```cpp
#include <SoftwareSerial.h>
#include <TinyGPS.h>

TinyGPS gps;
SoftwareSerial softSerial(4, 3);

void setup()
{
  Serial.begin(115200);
  softSerial.begin(9600);
}

void loop()
{
  bool newData = false;
  unsigned long chars;
  unsigned short sentences, failed;
  
  // Intentar recibir secuencia durante un segundo
  for (unsigned long start = millis(); millis() - start < 1000;)
  {
    while (softSerial.available())
    {
      char c = softSerial.read();
      if (gps.encode(c)) // Nueva secuencia recibida
        newData = true;
    }
  }

  if (newData)
  {
    float flat, flon;
    unsigned long age;
    gps.f_get_position(&flat, &flon, &age);
    Serial.print("LAT=");
    Serial.print(flat == TinyGPS::GPS_INVALID_F_ANGLE ? 0.0 : flat, 6);
    Serial.print(" LON=");
    Serial.print(flon == TinyGPS::GPS_INVALID_F_ANGLE ? 0.0 : flon, 6);
    Serial.print(" SAT=");
    Serial.print(gps.satellites() == TinyGPS::GPS_INVALID_SATELLITES ? 0 : gps.satellites());
    Serial.print(" PREC=");
    Serial.print(gps.hdop() == TinyGPS::GPS_INVALID_HDOP ? 0 : gps.hdop());
  }

  gps.stats(&chars, &sentences, &failed);
  Serial.print(" CHARS=");
  Serial.print(chars);
  Serial.print(" SENTENCES=");
  Serial.print(sentences);
  Serial.print(" CSUM ERR=");
  Serial.println(failed);
}
```


