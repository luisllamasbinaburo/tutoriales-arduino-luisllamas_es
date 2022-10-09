/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/arduino-acelerometro-adxl345/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

```cpp
// Gnd      -  GND
// 3.3v     -  VCC
// 3.3v     -  CS
// Analog 4 -  SDA
// Analog 5 -  SLC

#include <Wire.h>

//Direccion del dispositivo
const int DEVICE_ADDRESS = (0x53);  

byte _buff[6];

//Direcciones de los registros del ADXL345
char POWER_CTL = 0x2D;
char DATA_FORMAT = 0x31;
char DATAX0 = 0x32;	//X-Axis Data 0
char DATAX1 = 0x33;	//X-Axis Data 1
char DATAY0 = 0x34;	//Y-Axis Data 0
char DATAY1 = 0x35;	//Y-Axis Data 1
char DATAZ0 = 0x36;	//Z-Axis Data 0
char DATAZ1 = 0x37;	//Z-Axis Data 1

void setup()
{
  Serial.begin(57000);
  Serial.print("Iniciado");
  
  Wire.begin();
  writeTo(DEVICE_ADDRESS, DATA_FORMAT, 0x01); //Poner ADXL345 en +- 4G
  writeTo(DEVICE_ADDRESS, POWER_CTL, 0x08);  //Poner el ADXL345 
}

void loop()
{
  readAccel(); //Leer aceleracion x, y, z
  delay(500);
}

void readAccel() {
  //Leer los datos
  uint8_t numBytesToRead = 6;
  readFrom(DEVICE_ADDRESS, DATAX0, numBytesToRead, _buff);

  //Leer los valores del registro y convertir a int (Cada eje tiene 10 bits, en 2 Bytes LSB)
  int x = (((int)_buff[1]) << 8) | _buff[0];   
  int y = (((int)_buff[3]) << 8) | _buff[2];
  int z = (((int)_buff[5]) << 8) | _buff[4];
  Serial.print("x: ");
  Serial.print( x );
  Serial.print(" y: ");
  Serial.print( y );
  Serial.print(" z: ");
  Serial.println( z );
}

//Funcion auxiliar de escritura
void writeTo(int device, byte address, byte val) {
  Wire.beginTransmission(device);
  Wire.write(address);
  Wire.write(val);
  Wire.endTransmission(); 
}

//Funcion auxiliar de lectura
void readFrom(int device, byte address, int num, byte _buff[]) {
  Wire.beginTransmission(device);
  Wire.write(address);
  Wire.endTransmission();

  Wire.beginTransmission(device);
  Wire.requestFrom(device, num);

  int i = 0;
  while(Wire.available())
  { 
    _buff[i] = Wire.read();
    i++;
  }
  Wire.endTransmission();
}
```

```cpp
#include <SPI.h>
#include <Wire.h>
#include <SparkFun_ADXL345.h> 

ADXL345 adxl = ADXL345();

void setup() 
{
	Serial.begin(9600);             
	Serial.println("Iniciar");
	Serial.println();

	adxl.powerOn();            
	adxl.setRangeSetting(16);       //Definir el rango, valores 2, 4, 8 o 16
}

void loop() 
{
	//leer los valores e imprimirlos
	int x, y, z;
	adxl.readAccel(&x, &y, &z);  
	Serial.print(x);
	Serial.print(", ");
	Serial.print(y);
	Serial.print(", ");
	Serial.println(z); 
}


```