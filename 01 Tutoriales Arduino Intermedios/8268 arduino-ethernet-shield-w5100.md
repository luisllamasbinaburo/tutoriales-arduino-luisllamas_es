/***************************************************
Códigos de ejemplo de los tutoriales de www.luisllamas.es
Enlace entrada: https://www.luisllamas.es/arduino-ethernet-shield-w5100/
Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa
****************************************************/

```cpp
#include <SPI.h>
#include <Ethernet.h>

byte mac[] = { 0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0xED };
IPAddress ip(192, 168, 1, 177);
EthernetClient client;

char serverC[] = "www.pasted.co";
char dataLocationC[] = "/2434bc64 HTTP / 1.1";

void setup()
{
   Serial.begin(9600);
   
   if (Ethernet.begin(mac) == 0)
   {
      Serial.println("Failed to configure Ethernet using DHCP");
      Ethernet.begin(mac, ip);
   }

   delay(1000);
   Serial.println("connecting...");

   if (client.connect(serverC, 80))
   {
      Serial.println("connected");
      client.print("GET ");
      client.println(dataLocationC);
      client.print("Host: ");
      client.println(serverC);
      client.println();
   }
   else 
   {
      Serial.println("connection failed");
   }
}

void loop()
{
   if (client.available())
   {
      char c = client.read();
      if (c == '~')
      {
         while (client.available())
         {
            c = client.read();
            if (c == '~')
            {
               client.stop();
               client.flush();
               break;
            }
            Serial.print(c);
         }
         
      }
   }

   if (!client.connected())
   {
      Serial.println();
      Serial.println("disconnecting.");
      client.stop();

      // do nothing
      while (true);
   }
}
```

```cpp
#include <SPI.h>
#include <Ethernet.h>

byte mac[] = { 0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0xED };
IPAddress ip(192, 168, 1, 177);
EthernetServer server(80);

void setup()
{
  Serial.begin(9600);

  Ethernet.begin(mac, ip);
  server.begin();

  Serial.print("server is at ");
  Serial.println(Ethernet.localIP());
}

void loop()
{
  EthernetClient client = server.available(); 
  if (client)
  {
    Serial.println("new client");
    bool currentLineIsBlank = true;
    String cadena = "";
    while (client.connected()) 
    {
      if (client.available()) 
      {
        char c = client.read();
        Serial.write(c);
        
        // Al recibir linea en blanco, servir página a cliente
        if (c == '\n' && currentLineIsBlank)
        {
          client.println(F("HTTP/1.1 200 OK\nContent-Type: text/html"));
          client.println();

          client.println(F("<html>\n<head>\n<title>Luis Llamas</title>\n</head>\n<body>"));
          client.println(F("<div style='text-align:center;'>"));

          client.println(F("<h2>Entradas digitales</h2>"));
          for (int i = 0; i < 13; i++)
          {
            client.print("D");
            client.print(i);
            client.print(" = ");
            client.print(digitalRead(i));
            client.println(F("<br />"));
          }
          client.println("<br /><br />");

          client.println(F("<h2>Entradas analogicas</h2>"));
          for (int i = 0; i < 7; i++)
          {
            client.println("A");
            client.println(i);
            client.println(" = ");
            client.println(analogRead(i));
            client.println(F("<br />"));
          }

          client.println(F("<a href='http://192.168.1.177'>Refrescar</a>"));
          client.println(F("</div>\n</body></html>"));
          break;
        }
        if (c == '\n') 
        {
          currentLineIsBlank = true;
        }
        else if (c != '\r') 
        {
          currentLineIsBlank = false;
        }
      }
    }

    delay(1);
    client.stop();
  }
}
```

```cpp
#include <SPI.h>
#include <Ethernet.h>

byte mac[] = { 0xDE, 0xAD, 0xBE, 0xEF, 0xFE, 0xED };
IPAddress ip(192, 168, 1, 177);
EthernetServer server(80);

const int pinLed1 = A0;
const int pinLed2 = 3;

void setup()
{
  Serial.begin(9600);

  Ethernet.begin(mac, ip);
  server.begin();

  Serial.print("server is at ");
  Serial.println(Ethernet.localIP());

  pinMode(pinLed1, OUTPUT);
  pinMode(pinLed2, OUTPUT);
  digitalWrite(pinLed1, LOW);
  digitalWrite(pinLed2, LOW);
}

void loop()
{
  EthernetClient client = server.available(); 
  if (client)
  {
    Serial.println("new client");
    bool currentLineIsBlank = true;
    String cadena = "";
    while (client.connected()) 
    {
      if (client.available()) 
      {
        char c = client.read();
        Serial.write(c);

        if (cadena.length()<50)
        {
          cadena.concat(c);

           // Buscar campo data
          int posicion = cadena.indexOf("data");
          String command = cadena.substring(posicion);

          if (command == "data1=0")
          {
            digitalWrite(pinLed1, HIGH);
          }
          else if (command == "data1=1")
          {
            digitalWrite(pinLed1, LOW);
          }
          else if (command == "data2=0")
          {
            digitalWrite(pinLed2, LOW);
          }
          else if (command == "data2=1")
          {
            digitalWrite(pinLed2, HIGH);
          }
        }

        // Al recibir linea en blanco, servir página a cliente
        if (c == '\n' && currentLineIsBlank)
        {
          client.println(F("HTTP/1.1 200 OK\nContent-Type: text/html"));
          client.println();

          client.println(F("<html>\n<head>\n<title>Luis Llamas</title>\n</head>\n<body>"));
          client.println(F("<div style='text-align:center;'>"));
          client.println(F("<h2>Salidas digitales</h2>"));
          
          client.print(F("Estado LED 1 = "));
          client.println(digitalRead(pinLed1) == LOW ? "OFF" : "ON");
          client.println(F("<br/>"));
          client.println(F("<button onClick=location.href='./?data1=0'>ON</button>"));
          client.println(F("<button onClick=location.href='./?data1=1'>OFF</button>"));
          client.println(F("<br/><br/>"));

          client.print(F("Estado LED 2 = "));
          client.println(digitalRead(pinLed2) == LOW ? "OFF" : "ON");
          client.println(F("<br/>"));
          client.println(F("<button onClick=location.href='./?data2=0'>ON</button>"));
          client.println(F("<button onClick=location.href='./?data2=1'>OFF</button>"));
          client.println(F("<br/>"));

          client.println(F("<a href='http://192.168.1.177'>Refrescar</a>"));
          client.println(F("</div>\n</body></html>"));
          break;
        }
        if (c == '\n') 
        {
          currentLineIsBlank = true;
        }
        else if (c != '\r') 
        {
          currentLineIsBlank = false;
        }
      }
    }

    delay(1);
    client.stop();
  }
}
```