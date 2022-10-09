/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/tarjeta-micro-sd-arduino/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

```cpp
// Iniciar la SD
SD.begin(cspin);

// Comprobar si existe un fichero (devuelve true si existe, falso en caso contrario)
SD.exists(filename);

// Borrar un fichero
SD.remove(filename);

// Abrir un fichero
// Mode: FILE_READ para sólo lectura
// 		 FILE_WRITE para lectura y escritura
SD.open(filepath, mode);

// Crear un directorio
SD.mkdir(directory);

// Eliminar un directorio
SD.rmdir(dirname);
```

```cpp
// Obtener el tamaño de un fichero
file.size()

// Comprobar si quedan bytes por leer
file.available()

// Leer un byte del fichero
file.read()

// Escribir un byte en el fichero
file.write(data)

// Escribir una variable en un fichero (en forma similar a Serial.Print)
file.print(data)

// Obtener el punto de lectura/escritura actual
file.position()

// Mover el punto de lectura/escritura actual
// Pos: Debe estar entre 0 y file.size()
file.seek(pos)

// Cerrar el fichero
file.close()
```

```cpp
#include <SD.h>

File dataFile;

void setup()
{
  Serial.begin(9600);
  Serial.print(F("Iniciando SD ..."));
  if (!SD.begin(9))
  {
    Serial.println(F("Error al iniciar"));
    return;
  }
  Serial.println(F("Iniciado correctamente"));
 
  // Abrir fichero y mostrar el resultado
  dataFile = SD.open("datalog.txt"); 
  if (dataLine)
  {
	string dataLine;
    while (dataFile.available())
	{
		dataLine = dataFile.read(); 
    	Serial.write(dataLine);  // En un caso real se realizarían las acciones oportunas
    }
    dataFile.close();
  }
  else 
  {
    Serial.println(F("Error al abrir el archivo"));
  }
}

void loop()
{
}
```

```cpp
#include <SD.h>

File logFile;

void setup()
{
  Serial.begin(9600);
  Serial.print(F("Iniciando SD ..."));
  if (!SD.begin(9))
  {
    Serial.println(F("Error al iniciar"));
    return;
  }
  Serial.println(F("Iniciado correctamente"));
}


// Funcion que simula la lectura de un sensor
int readSensor()
{
   return 0;
}

void loop()
{
  // Abrir archivo y escribir valor
  logFile = SD.open("datalog.txt", FILE_WRITE);
  
  if (logFile) { 
        int value = readSensor;
        logFile.print("Time(ms)=");
        logFile.print(millis());
        logFile.print(", value=");
        logFile.println(value);
        
        logFile.close();
  
  } 
  else {
    Serial.println("Error al abrir el archivo");
  }
  delay(500);
}
```