/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/copiar-un-mando-inalambrico-315-433mhz-con-arduino/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

#include <rcswitch.h>

RCSwitch mySwitch = RCSwitch();

// Sustituir con tu código
unsigned long code = XXXXXXXX;

void setup()
{
  Serial.begin(9600);
  mySwitch.enableTransmit(2);
}

void loop()
{
  mySwitch.send(code, 24);
  delay(200000);
}
</rcswitch.h>