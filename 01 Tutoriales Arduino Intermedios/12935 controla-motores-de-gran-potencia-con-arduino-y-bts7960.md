/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/controla-motores-de-gran-potencia-con-arduino-y-bts7960/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

#include "BTS7960.h"

const uint8_t EN = 8;
const uint8_t L_PWM = 9;
const uint8_t R_PWM = 10;

BTS7960 motorController(EN, L_PWM, R_PWM);

void setup() 
{
}

void loop() 
{
  motorController.Enable();

  for(int speed = 0 ; speed < 255; speed+=10)
  {
	motorController.TurnLeft(speed);
	delay(100);
  }  

  motorController.Stop();
  
  for(int speed = 255 ; speed > 0; speed-=10)
  {
	motorController.TurnLeft(speed);
	delay(100);
  }  
  motorController.Stop();
  motorController.Disable();
  delay(5000);
}