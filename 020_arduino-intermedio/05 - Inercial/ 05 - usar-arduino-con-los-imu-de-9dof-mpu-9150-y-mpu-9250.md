> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/usar-arduino-con-los-imu-de-9dof-mpu-9150-y-mpu-9250/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa


## Ejemplos de código
```cpp
//GND - GND
//VCC - VCC
//SDA - Pin A4
//SCL - Pin A5

#include <Wire.h>


#define    MPU9250_ADDRESS            0x68

#define    MAG_ADDRESS                0x0C


#define    GYRO_FULL_SCALE_250_DPS    0x00  

#define    GYRO_FULL_SCALE_500_DPS    0x08

#define    GYRO_FULL_SCALE_1000_DPS   0x10

#define    GYRO_FULL_SCALE_2000_DPS   0x18


#define    ACC_FULL_SCALE_2_G        0x00  

#define    ACC_FULL_SCALE_4_G        0x08

#define    ACC_FULL_SCALE_8_G        0x10

#define    ACC_FULL_SCALE_16_G       0x18

//Funcion auxiliar lectura
void I2Cread(uint8_t Address, uint8_t Register, uint8_t Nbytes, uint8_t* Data)
{
  Wire.beginTransmission(Address);
  Wire.write(Register);
  Wire.endTransmission();

  Wire.requestFrom(Address, Nbytes);
  uint8_t index = 0;
  while (Wire.available())
    Data[index++] = Wire.read();
}

// Funcion auxiliar de escritura
void I2CwriteByte(uint8_t Address, uint8_t Register, uint8_t Data)
{
  Wire.beginTransmission(Address);
  Wire.write(Register);
  Wire.write(Data);
  Wire.endTransmission();
}

void setup()
{
  Wire.begin();
  Serial.begin(115200);

  // Configurar acelerometro
  I2CwriteByte(MPU9250_ADDRESS, 28, ACC_FULL_SCALE_16_G);
  // Configurar giroscopio
  I2CwriteByte(MPU9250_ADDRESS, 27, GYRO_FULL_SCALE_2000_DPS);
  // Configurar magnetometro
  I2CwriteByte(MPU9250_ADDRESS, 0x37, 0x02);
  I2CwriteByte(MAG_ADDRESS, 0x0A, 0x01);
}

void loop()
{
  // ---  Lectura acelerometro y giroscopio --- 
  uint8_t Buf[14];
  I2Cread(MPU9250_ADDRESS, 0x3B, 14, Buf);

  // Convertir registros acelerometro
  int16_t ax = -(Buf[0] << 8 | Buf[1]);
  int16_t ay = -(Buf[2] << 8 | Buf[3]);
  int16_t az = Buf[4] << 8 | Buf[5];

  // Convertir registros giroscopio
  int16_t gx = -(Buf[8] << 8 | Buf[9]);
  int16_t gy = -(Buf[10] << 8 | Buf[11]);
  int16_t gz = Buf[12] << 8 | Buf[13];

  // ---  Lectura del magnetometro --- 
  uint8_t ST1;
  do
  {
    I2Cread(MAG_ADDRESS, 0x02, 1, &ST1);
  } while (!(ST1 & 0x01));

  uint8_t Mag[7];
  I2Cread(MAG_ADDRESS, 0x03, 7, Mag);

  // Convertir registros magnetometro
  int16_t mx = -(Mag[3] << 8 | Mag[2]);
  int16_t my = -(Mag[1] << 8 | Mag[0]);
  int16_t mz = -(Mag[5] << 8 | Mag[4]);

  // --- Mostrar valores ---

  // Acelerometro
  Serial.print(ax, DEC);
  Serial.print("\t");
  Serial.print(ay, DEC);
  Serial.print("\t");
  Serial.print(az, DEC);
  Serial.print("\t");

  // Giroscopio
  Serial.print(gx, DEC);
  Serial.print("\t");
  Serial.print(gy, DEC);
  Serial.print("\t");
  Serial.print(gz, DEC);
  Serial.print("\t");

  // Magnetometro
  Serial.print(mx + 200, DEC);
  Serial.print("\t");
  Serial.print(my - 70, DEC);
  Serial.print("\t");
  Serial.print(mz - 700, DEC);
  Serial.print("\t");
  
  // Fin medicion
  Serial.println("");
  
  delay(10);    
}
```


