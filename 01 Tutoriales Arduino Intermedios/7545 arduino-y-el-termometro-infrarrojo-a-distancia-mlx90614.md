/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/arduino-y-el-termometro-infrarrojo-a-distancia-mlx90614/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

#include <wire.h>
#include <adafruit_mlx90614.h>

Adafruit_MLX90614 mlx = Adafruit_MLX90614();

void setup() {
  Serial.begin(9600);

  mlx.begin();  
}

void loop() {
  Serial.print("Ambiente = ");
  Serial.print(mlx.readAmbientTempC()); 
  Serial.print("ºC\tObjeto = "); 
  Serial.print(mlx.readObjectTempC()); 
  Serial.println("ºC");
  delay(500);
}
</adafruit_mlx90614.h></wire.h>