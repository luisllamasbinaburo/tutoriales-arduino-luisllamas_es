> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/medir-tension-intensidad-y-potencia-con-arduino-y-ina219/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa


## Ejemplos de código
```cpp
#include <Wire.h>
#include <Adafruit_INA219.h>

Adafruit_INA219 ina219;

void setup(void) 
{
  Serial.begin(115200);
  uint32_t currentFrequency;
  
  // Iniciar el INA219
  ina219.begin();  //por defecto, inicia a 32V y 2A

  // Opcionalmente, cambiar la sensibilidad del sensor
  //ina219.setCalibration_32V_1A();
  //ina219.setCalibration_16V_400mA();

  Serial.println("INA219 iniciado...");
}

void loop(void) 
{
  float shuntvoltage = 0;
  float busvoltage = 0;
  float current_mA = 0;
  float loadvoltage = 0;
  float power_mW = 0;

  // Obtener mediciones
  shuntvoltage = ina219.getShuntVoltage_mV();
  busvoltage = ina219.getBusVoltage_V();
  current_mA = ina219.getCurrent_mA();
  power_mW = ina219.getPower_mW();
  loadvoltage = busvoltage + (shuntvoltage / 1000);
  
  // Mostrar mediciones
  Serial.print("Bus Voltaje:   "); Serial.print(busvoltage); Serial.println(" V");
  Serial.print("Shunt Voltaje: "); Serial.print(shuntvoltage); Serial.println(" mV");
  Serial.print("Load Voltaje:  "); Serial.print(loadvoltage); Serial.println(" V");
  Serial.print("Corriente:       "); Serial.print(current_mA); Serial.println(" mA");
  Serial.print("Potencia:         "); Serial.print(power_mW); Serial.println(" mW");
  Serial.println("");

  delay(2000);
}
```


