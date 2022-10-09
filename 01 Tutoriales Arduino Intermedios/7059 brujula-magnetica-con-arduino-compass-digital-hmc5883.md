> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/brujula-magnetica-con-arduino-compass-digital-hmc5883/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

```cpp
//GND - GND
//VCC - VCC
//SDA - Pin A4
//SCL - Pin A5

#include "Wire.h"
#include "I2Cdev.h"
#include "HMC5883L.h"

HMC5883L compass;

int16_t mx, my, mz;

void setup() 
{
    
    Serial.begin(9600);
    Wire.begin();
    compass.initialize();
}

void loop()
{
    //Obtener componentes del campo magnético
    compass.getHeading(&mx, &my, &mz);
    
    Serial.print("mx:");
    Serial.print(mx); 
    Serial.print("tmy:");
    Serial.print(my);
    Serial.print("tmz:");
    Serial.println(mz);
    delay(100);
}
```

```cpp
//GND - GND
//VCC - VCC
//SDA - Pin A4
//SCL - Pin A5

#include "Wire.h"
#include "I2Cdev.h"
#include "HMC5883L.h"

HMC5883L compass;

int16_t mx, my, mz;

//declinacion en grados en tu posición
const float declinacion = 0.12; 

void setup()
{
    Serial.begin(9600);
    Wire.begin();
    compass .initialize();
}

void loop() {
    //Obtener componentes del campo magnético
    compass .getHeading(&mx, &my, &mz);

    //Calcular ángulo el ángulo del eje X respecto al norte
    float angulo = atan2(my, mx);
    angulo = angulo * RAD_TO_DEG;
    angulo = angulo - declinacion;
    
    if(angulo < 0) angulo = angulo + 360;
    
    Serial.print("N:");
    Serial.println(angulo,0);  
}
```