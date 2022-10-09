/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/arduino-lcd-i2c/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

```cpp
#include <Wire.h> 
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x27, 16, 2);  // Inicia el LCD en la dirección 0x27, con 16 caracteres y 2 líneas

void setup()
{
	lcd.begin();                      
	lcd.backlight();
	lcd.setCursor(0, 0);
	lcd.print("Linea 1");
	lcd.setCursor(0, 1);
	lcd.print("Linea 2");
	delay(2500);
	lcd.clear();
}

void loop()
{
}
```