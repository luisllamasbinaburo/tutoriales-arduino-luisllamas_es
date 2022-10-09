/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/medir-presion-del-aire-y-altitud-con-arduino-y-barometro-bmp180/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

#include <sfe_bmp180.h>
#include <wire.h>

SFE_BMP180 bmp180;

void setup()
{
  Serial.begin(9600);

  if (bmp180.begin())
    Serial.println("BMP180 iniciado");
  else
  {
    Serial.println("Error al iniciar BMP180");
    while(1); // bucle infinito
  }
}

void loop()
{
  char status;
  double T,P;

  status = bmp180.startTemperature(); //Inicio de lectura de temperatura
  if (status != 0)
  {   
    delay(status); //Pausa para que finalice la lectura
    status = bmp180.getTemperature(T); //Obtener la temperatura
    if (status != 0)
    {
      status = bmp180.startPressure(3); //Inicio lectura de presión
      if (status != 0)
      {        
        delay(status); //Pausa para que finalice la lectura        
        status = bmp180.getPressure(P,T); //Obtenemos la presión
        if (status != 0)
        {                  
          Serial.print("Temperatura: ");
          Serial.print(T,2);
          Serial.print(" *C , ");
          Serial.print("Presion: ");
          Serial.print(P,2);
          Serial.println(" mb");          
        }      
      }      
    }   
  } 
  delay(1000);
}
</wire.h></sfe_bmp180.h>

#include <sfe_bmp180.h>
#include <wire.h>

SFE_BMP180 bmp180;

double PresionNivelMar = 1013.25; //presion sobre el nivel del mar en mbar

void setup()
{
  Serial.begin(9600);

  if (bmp180.begin())
    Serial.println("BMP180 iniciado");
  else
  {
    Serial.println("Error al iniciar el BMP180");
    while(1);
  }
}

void loop()
{
  char status;
  double T,P,A;
  
  status = bmp180.startTemperature(); //Inicio de lectura de temperatura
  if (status != 0)
  {   
    delay(status); //Pausa para que finalice la lectura
    status = bmp180.getTemperature(T); //Obtener la temperatura
    if (status != 0)
    {
      status = bmp180.startPressure(3); //Inicio lectura de presión
      if (status != 0)
      {        
        delay(status); //Pausa para que finalice la lectura        
        status = bmp180.getPressure(P,T); //Obtener la presión
        if (status != 0)
        {                  
          Serial.print("Temperatura: ");
          Serial.print(T);
          Serial.print(" *C , ");
          Serial.print("Presion: ");
          Serial.print(P);
          Serial.print(" mb , ");     
          
          A= bmp180.altitude(P,PresionNivelMar); //Calcular altura
          Serial.print("Altitud: ");
          Serial.print(A);
          Serial.println(" m");    
        }      
      }      
    }   
  } 
  delay(1000);
}
</wire.h></sfe_bmp180.h>

#include <sfe_bmp180.h>
#include <wire.h>

SFE_BMP180 bmp180;

double Po; //presion del punto inicial para h=0;
char status;
double T,P,A;
void setup()
{
  Serial.begin(9600);

  if (bmp180.begin())
  {
    Serial.println("BMP180 iniciado");
    status = bmp180.startTemperature(); //Inicio de lectura de temperatura
    if (status != 0)
    {   
      delay(status); //Pausa para que finalice la lectura
      status = bmp180.getTemperature(T);//Obtener la temperatura
      if (status != 0)
      {
        status = bmp180.startPressure(3); //Inicio lectura de presión
        if (status != 0)
        {        
          delay(status); //Pausa para que finalice la lectura        
          status = bmp180.getPressure(P,T); //Obtener la presión
          if (status != 0)
          {                  
            Po=P; //Asignamos el valor de presión como punto de referencia
            Serial.println("Punto de referencia establecido: h=0");  
          }      
        }      
      }   
    }
    
  }
  else
  {
    Serial.println("Error al iniciar el BMP180");
    while(1);
  }
}

void loop()
{
  status = bmp180.startTemperature(); //Inicio de lectura de temperatura
  if (status != 0)
  {   
    delay(status); //Pausa para que finalice la lectura
    status = bmp180.getTemperature(T); //Obtener la temperatura
    if (status != 0)
    {
      status = bmp180.startPressure(3); //Inicio lectura de presión
      if (status != 0)
      {        
        delay(status); //Pausa para que finalice la lectura        
        status = bmp180.getPressure(P,T); //Obtener la presión
        if (status != 0)
        {                    
          A= bmp180.altitude(P,Po); //Calcular altura con respecto al punto de referencia
          Serial.print("h=");
          Serial.print(A);
          Serial.println(" m");    
        }      
      }      
    }   
  } 
  delay(1000);
}

</wire.h></sfe_bmp180.h>