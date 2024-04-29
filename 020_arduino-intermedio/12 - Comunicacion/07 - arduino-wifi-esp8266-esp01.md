> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/arduino-wifi-esp8266-esp01/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

## Primera prueba
```cpp
// La velocidad depende del modelo de ESP-01
// siendo habituales 9600 y 115200
const int baudRate = 9600;

#include "SoftwareSerial.h"
SoftwareSerial softSerial(2, 3); // RX, TX

void setup()
{
  Serial.begin(baudRate);
  softSerial.begin(baudRate);
}

void loop()
// enviar los datos de la consola serial al ESP-01, 
// y mostrar lo enviado por el ESP-01 a nuestra consola
{
  if (softSerial.available())
  {
    Serial.print((char)softSerial.read());
  }
  if (Serial.available())
  {
    softSerial.print((char)Serial.read());
  }
}
```


## Listado comandos AT
```cpp
//**** GENERAL ****
// Acknowlege, se recive "ok"
AT

// Reset, reinicia el módudo. 
// Se recive una cadena de texto que depende del modelo y fabricante y, al final, "READY"
AT+RST

//**** CONFIGURACION ****
// Obtener la velocidad de transmision
AT+CIOBAUD?

// Cambiar la velocidad de transmision (en el ejemplo a 9600)
// Velocidades validas 9600, 19200, 38400, 74880, 115200, 230400, 460800 y 921600
AT+CIOBAUD=9600
AT+IPR=9600

// Obtener el modo de funcionamiento
// 1 Station
// 2 SoftAp
// 3 Station + SoftAp
AT+CWMODE?

// Cambia el modo de funcionamiento a 1, 2 o 3
// Lo normal es AT+CWMODE=3, por que es el más versátil
// Tras el cambio es necesario AT+RST
AT+CWMODE=mode

//**** UNISER A UNA RED WIFI ****
// List Access Point
// Muestra una lista de las redes wifi disponibles
AT+CWLAP

// Join Access Point
// Unirse a una red wifi existente
AT+CWJAP=you ssid, password

// Check if connected successfully, or use AT+CWJAP=?, or use AT+CIFSR to find your ip address
AT+CWJAP?

// Obtener la IP del módulo
AT+CIFSR

//**** CREAR UNA RED WIFI ****

// Crear una red wifi
AT+CWSAP="ssid","password",3,0

// Listar los dispositivos conectados a la red generada
AT+CWLIF

//**** TCP SERVER ****
// Configura un servidor TCP en el puerto 80 (1 significa activado)
AT+CIPSERVER=1,80

//**** TCP CLIENT ****
// Activar multiples conexiones
AT+CIPMUX=1

// Conectar con el servidor remoto 192.168.1.100 en el puerto 80
AT+CIPSTART=4,"TCP","192,168.1.100",80

// Configurar el modo de transmisión
AT+CIPMODE=1

// Enviar data por el canal 4 (en el ejemplo 5 bytes)
AT+CIPSEND=4,5
```


## Ejemplos AT
```cpp
// Listar las redes WiFi y conectar a una de ella
// sustituir SSID y PASSWORD por los parametros de la red
AT+CWLAP
AT+CWJAP=SSID,PASSWORD

// Establecer conexión como cliente
AT+CWJAP=SSID,PASSWORD
AT+CIPMUX=1
AT+CIPSTART=4,"TCP","google.com",80

// Establecer una conexión como servidor
realizar un servidor
AT+CWJAP=SSID,PASSWORD
AT+CIPMUX=1
AT+CIPSERVER=1,80
```

```cpp
#include "SoftwareSerial.h"
SoftwareSerial softSerial(2, 3); // RX, TX

const int baudRate = 9600;
char* SSDI = "tuWifi";
char* password = "tuPassword";

void setup()
{
  Serial.begin(baudRate);
  softSerial.begin(baudRate);
  delay(1000);

  softSerial.write("AT+CWJAP=\"");
  softSerial.write(SSDI);
  softSerial.write("\",\"");
  softSerial.write(password);
  softSerial.write("\"\r\n");

  delay(4000);
  softSerial.write("AT+CIPMUX=1\r\n");
  delay(2000);
  softSerial.write("AT+CIPSERVER=1,80\r\n");
}

void loop()
{
  if (softSerial.available())
  {
    Serial.print((char)softSerial.read());
  }
  if (Serial.available())
  {
    softSerial.print((char)Serial.read());
  }
}
```


## Uso del ESP8266 con librería
```cpp
#define ESP8266_USE_SOFTWARE_SERIAL
```

```bash
\arduino\hardware\arduino\avr\cores\arduino\HardwareSerial
```

```bash
\arduino\hardware\arduino\avr\cores\arduino\SoftwareSerial
```

```cpp
// En HardwareSerial.h

#define SERIAL_BUFFER_SIZE 64

// En SoftwareSerial.h

#define _SS_MAX_RX_BUFF 64 // RX buffer size
```


## Cliente Wifi - Leer páginas web
```cpp
#include "ESP8266.h"
#include <SoftwareSerial.h>

const char* SSID = "myssid";
const char* PASSWORD = "mypassword";
const char* HOST_NAME = "www.pasted.co";
const int HOST_PORT = 80;

SoftwareSerial softSerial(2, 3); // RX, TX
ESP8266 wifi(softSerial);

void setup(void)
{
  Serial.begin(9600);

  if (wifi.setOprToStationSoftAP()) {
    Serial.print("to station + softap ok\r\n");
  }
  else {
    Serial.print("to station + softap err\r\n");
  }

  if (wifi.joinAP(SSID, PASSWORD)) {
    Serial.print("Join AP success\r\n");
    Serial.print("IP:");
    Serial.println(wifi.getLocalIP().c_str());
  }
  else {
    Serial.print("Join AP failure\r\n");
  }

  if (wifi.disableMUX()) {
    Serial.print("single ok\r\n");
  }
  else {
    Serial.print("single err\r\n");
  }

  Serial.print("setup end\r\n");
}

void loop(void)
{
  uint8_t buffer[800] = { 0 };

  if (wifi.createTCP(HOST_NAME, HOST_PORT)) {
    Serial.print("create tcp ok\r\n");
  }
  else {
    Serial.print("create tcp err\r\n");
  }

  char *request = "GET /2434bc64 HTTP/1.1\r\nHost: www.pasted.co\r\nConnection: close\r\n\r\n";
  wifi.send((const uint8_t*)request, strlen(request));

  uint32_t len = wifi.recv(buffer, sizeof(buffer), 10000);
  if (len > 0) 
  {
    Serial.print("Received:\r\n");
    for (uint32_t i = 0; i < len; i++) 
    {
      char c = (char)buffer[i];
      if (c == '~')
      {
        for (uint32_t j = i + 1; j < len; j++)
        {
          c = (char)buffer[j];
          if (c == '~') break;
          Serial.print(c);
        }
        break;
      }
    }
    Serial.print("\r\n");
  }

  while (1) delay(1000);
}
```


## Servidor Wifi - Atender peticiones
```cpp
#include "ESP8266.h"
#include <SoftwareSerial.h>

const char* SSID = "myssid";
const char* PASSWORD = "mypassword";

SoftwareSerial softSerial(2, 3); // RX, TX
ESP8266 wifi(softSerial);

void setup(void)
{
  Serial.begin(9600);
  Serial.print("setup begin\r\n");
  
  wifi.restart();
  delay(500);

  if (wifi.setOprToStationSoftAP()) {
    Serial.print("to station + softap ok\r\n");
  }
  else {
    Serial.print("to station + softap err\r\n");
  }

  if (wifi.joinAP(SSID, PASSWORD)) {
    Serial.print("Join AP success\r\n");
    Serial.print("IP: ");
    Serial.println(wifi.getLocalIP().c_str());
  }
  else {
    Serial.print("Join AP failure\r\n");
  }

  if (wifi.enableMUX()) {
    Serial.print("multiple ok\r\n");
  }
  else {
    Serial.print("multiple err\r\n");
  }

  if (wifi.startTCPServer(80)) {
    Serial.print("start tcp server ok\r\n");
  }
  else {
    Serial.print("start tcp server err\r\n");
  }

  if (wifi.setTCPServerTimeout(10)) {
    Serial.print("set tcp server timout 10 seconds\r\n");
  }
  else {
    Serial.print("set tcp server timout err\r\n");
  }

  Serial.println("setup end\r\n");
}

void loop(void)
{
  uint8_t buffer[128] = { 0 };
  uint8_t mux_id;

  uint32_t len = wifi.recv(&mux_id, buffer, sizeof(buffer), 100);
  if (len > 0) {
    Serial.print("Received from: ");
    Serial.print(mux_id);
    Serial.print("\r\n");
    for (uint32_t i = 0; i < len; i++) {
      Serial.print((char)buffer[i]);
    }
    Serial.print("\r\n");
    
    if (wifi.releaseTCP(mux_id)) {
      Serial.print("release tcp ");
      Serial.print(mux_id);
      Serial.println(" ok");
    }
    else {
      Serial.print("release tcp");
      Serial.print(mux_id);
      Serial.println(" err");
    }

    Serial.print("Status: ");
    Serial.print(wifi.getIPStatus().c_str());
    Serial.println();
  }
}
```


## Servidor WiFI - Controlar salidas digitales
```cpp
#include "ESP8266.h"
#include <SoftwareSerial.h>

const char* SSID = "myssid";
const char* PASSWORD = "mypassword";

SoftwareSerial softSerial(2, 3); // RX, TX
ESP8266 wifi(softSerial);

void setup(void)
{
  pinMode(LED_BUILTIN, OUTPUT);

  Serial.begin(9600);
  Serial.print("setup begin\r\n");
  
  wifi.restart();
  delay(500);
  if (wifi.setOprToStationSoftAP()) {
    Serial.print("to station + softap ok\r\n");
  }
  else {
    Serial.print("to station + softap err\r\n");
  }

  if (wifi.joinAP(SSID, PASSWORD)) {
    Serial.print("Join AP success\r\n");
    Serial.print("IP: ");
    Serial.println(wifi.getLocalIP().c_str());
  }
  else {
    Serial.print("Join AP failure\r\n");
  }

  if (wifi.enableMUX()) {
    Serial.print("multiple ok\r\n");
  }
  else {
    Serial.print("multiple err\r\n");
  }

  if (wifi.startTCPServer(80)) {
    Serial.print("start tcp server ok\r\n");
  }
  else {
    Serial.print("start tcp server err\r\n");
  }

  if (wifi.setTCPServerTimeout(20)) {
    Serial.print("set tcp server timout 20 seconds\r\n");
  }
  else {
    Serial.print("set tcp server timout err\r\n");
  }

  Serial.println("setup end\r\n");
}


#define wifiWrite(A) wifi.send(mux_id, (uint8_t*) A, sizeof(A) - 1);
void loop(void)
{
  uint8_t buffer[128] = { 0 };
  uint8_t mux_id;

  uint32_t len = wifi.recv(&mux_id, buffer, sizeof(buffer), 100);
  if (len > 0) {
    Serial.print("Received from: ");
    Serial.print(mux_id);
    Serial.print("\r\n");

    wifiWrite("HTTP/1.1 200 OK\r\nnContent-Type: /html\r\nConnection: close\r\n\r\n");
    wifiWrite("<html>\n<head>\n<title>Luis Llamas</title>\n</head>\n<body>");
    wifiWrite("<h2>Salidas digitales</h2>");
    wifiWrite("<button onClick=location.href='./?data=0'>ON</button>");
    wifiWrite("<button onClick=location.href='./?data=1'>OFF</button>");
    wifiWrite("</body></html>");

    Serial.println("Send finish");

    for (uint32_t i = 0; i < len; i++) {
      char c = (char)buffer[i];
      if (c == '?')
      {
        if ((char)buffer[i + 6] == '1')
        {
          digitalWrite(LED_BUILTIN, HIGH);
          Serial.println("LED ON");
        }
        else
        {
          digitalWrite(LED_BUILTIN, LOW);
          Serial.println("LED OFF");
        }

        break;
      }
    }
  }
}
```


