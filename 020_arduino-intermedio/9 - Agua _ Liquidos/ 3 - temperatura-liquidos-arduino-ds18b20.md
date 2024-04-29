> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/temperatura-liquidos-arduino-ds18b20/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa


## Ejemplos de código
```csharp
#include <OneWire.h>
#include <DallasTemperature.h>

const int oneWirePin = 5;

OneWire oneWireBus(oneWirePin);
DallasTemperature sensor(&oneWireBus);

void setup() {
  Serial.begin(9600);
  sensor.begin(); 
}

void loop() {
    Serial.println("Leyendo temperaturas: ");
  sensor.requestTemperatures();

  Serial.print("Temperatura en sensor 0: ");
  Serial.print(sensor.getTempCByIndex(0));
  Serial.println(" ºC");

  delay(1000); 
}
```

```csharp
#include <OneWire.h>

const int oneWirePin = 5;
OneWire oneWireBus(oneWirePin);

void setup(void) {
  Serial.begin(9600);
  discoverOneWireDevices();
}

void discoverOneWireDevices(void) {
  byte i;
  byte present = 0;
  byte data[12];
  byte addr[8];
  
  Serial.println("Buscando dispositivos 1-Wire");
  while(oneWireBus.search(addr)) {
    Serial.println("Encontrado dispositivo 1-Wire en direccion");
    for( i = 0; i < 8; i++) {
      Serial.print("0x");
      if (addr[i] < 16) {
        Serial.print('0');
      }
      Serial.print(addr[i], HEX);
      if (i < 7) {
        Serial.print(", ");
      }
    }
    if ( OneWire::crc8( addr, 7) != addr[7]) {
        Serial.print("Error en dispositivo, CRC invalido!\n");
        return;
    }
  }
  Serial.println("Búsqueda finalizada");
  oneWireBus.reset_search();
  return;
}

void loop(void) {
  // nada que hacer aqui
}
```

```csharp
#include <OneWire.h>
#include <DallasTemperature.h>

const int oneWirePin = 5;

OneWire oneWireBus(oneWirePin);
DallasTemperature sensors(&oneWireBus);

DeviceAddress insideThermometer = { 0x28, 0x94, 0xE2, 0xDF, 0x02, 0x00, 0x00, 0xFE };
DeviceAddress outsideThermometer = { 0x28, 0x6B, 0xDF, 0xDF, 0x02, 0x00, 0x00, 0xC0 };

void setup(void)
{
  Serial.begin(9600);
  sensors.begin();
  sensors.setResolution(insideThermometer, 10);
  sensors.setResolution(outsideThermometer, 10);
}

void printTemperature(DeviceAddress deviceAddress)
{
  float tempC = sensors.getTempC(deviceAddress);
  if (tempC == -127.00) {
    Serial.print("Error getting temperature");
  } else {
    Serial.print(tempC);
  Serial.println(" ºC");
  }
}

void loop(void)
{ 

  Serial.println("Leyendo temperaturas");
  sensors.requestTemperatures();
  
  Serial.print("Temperatura interior: ");
  printTemperature(insideThermometer);
  Serial.print("Temperatura exterior: ");
  printTemperature(outsideThermometer);
  Serial.println("-------");
  
  delay(2000);
}
```


