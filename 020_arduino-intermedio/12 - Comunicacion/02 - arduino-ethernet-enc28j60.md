> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/arduino-ethernet-enc28j60/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

## Cliente Ethernet – Leer páginas web
```cpp
#include <EtherCard.h>

static byte mymac[] = { 0xDD, 0xDD, 0xDD, 0x00, 0x01, 0x05 };
static byte myip[] = { 192, 168, 1, 177 };

byte Ethernet::buffer[700];
static uint32_t timer;

const char website[] PROGMEM = "www.pasted.co";
const char dataLocationC[] = "/2434bc64";

// called when the client request is complete
static void my_callback (byte status, word off, word len) {
  Serial.println(">>>");
  Ethernet::buffer[off+300] = 0;
  Serial.print((const char*) Ethernet::buffer + off);
  Serial.println("...");
}

void setup () {
  Serial.begin(57600);
  Serial.println(F("\n[webClient]"));

  if (ether.begin(sizeof Ethernet::buffer, mymac) == 0) 
    Serial.println(F("Failed to access Ethernet controller"));
  if (!ether.dhcpSetup())
    Serial.println(F("DHCP failed"));

  ether.printIp("IP:  ", ether.myip);
  ether.printIp("GW:  ", ether.gwip);  
  ether.printIp("DNS: ", ether.dnsip);  

  if (!ether.dnsLookup(website))
    Serial.println("DNS failed");
    
  ether.printIp("SRV: ", ether.hisip);
}

void loop () {
  ether.packetLoop(ether.packetReceive());
  
  if (millis() > timer) {
    timer = millis() + 5000;
    Serial.println();
    Serial.print("<<< REQ ");

    // ether.browseUrl(PSTR(dataLocationC), "", website, my_callback); // No funciona en IDE Standard
    ether.browseUrl(PSTR("/2434bc64"), "", website, my_callback);  // En entorno IDE
  }
}
```


## Servidor Ethernet – Visualizar entradas
```cpp
#include <EtherCard.h>

static byte mymac[] = { 0xDD, 0xDD, 0xDD, 0x00, 0x01, 0x05 };
static byte myip[] = { 192, 168, 1, 177 };
byte Ethernet::buffer[700];

void setup() {

  Serial.begin(9600);

  if (!ether.begin(sizeof Ethernet::buffer, mymac, 10))
    Serial.println("No se ha podido acceder a la controlador Ethernet");
  else
    Serial.println("Controlador Ethernet inicializado");

  if (!ether.staticSetup(myip))
    Serial.println("No se pudo establecer la dirección IP");
  Serial.println();
}

static word mainPage()
{
  BufferFiller bfill = ether.tcpOffset();
  bfill.emit_p(PSTR("HTTP/1.0 200 OKrn"
    "Content-Type: text/htmlrnPragma: no-cachernRefresh: 5rnrn"
    "<html><head><title>Luis Llamas</title></head>"
    "<body>"
    "<div style='text-align:center;'>"
    "<h1>Entradas digitales</h1>"
    "Tiempo transcurrido : $L s"
    "<br /><br />D00: $D"
    "<br /><br />D01: $D"
    "<br /><br />D02: $D"
    "<br /><br />D03: $D"
    "<br /><br />D04: $D"
    "<br /><br />D05: $D"
    "<br /><br />D06: $D"
    "<br /><br />D07: $D"
    "<br /><br />D08: $D"
    "<br /><br />D09: $D"
    "<br /><br />D10: $D"
    "<br /><br />D11: $D"
    "<br /><br />D12: $D"
    "<br /><br />D13: $D"

    "<h1>Entradas analogicas</h1>"
    "<br /><br />AN0: $D"
    "<br /><br />AN1: $D"
    "<br /><br />AN2: $D"
    "<br /><br />AN3: $D"
    "<br /><br />AN4: $D"
    "<br /><br />AN5: $D"
    "<br /><br />AN6: $D"
    "<br /><br />"
    "</body></html>"), 
    millis() / 1000, 
    digitalRead(0),
    digitalRead(1),
    digitalRead(2),
    digitalRead(3),
    digitalRead(4),
    digitalRead(5),
    digitalRead(6),
    digitalRead(7),
    digitalRead(8),
    digitalRead(9),
    digitalRead(10),
    digitalRead(11),
    digitalRead(12),
    digitalRead(13), 
    analogRead(0),
    analogRead(1),
    analogRead(2),
    analogRead(3),
    analogRead(4),
    analogRead(5),
    analogRead(6));

  return bfill.position();
}

void loop() 
{
  // wait for an incoming TCP packet, but ignore its contents
  if (ether.packetLoop(ether.packetReceive())) 
  {
    ether.httpServerReply(mainPage());
  }
}
```


## Servidor Ethernet – Controlar salidas
```cpp
#include <EtherCard.h>

static byte mymac[] = { 0xDD, 0xDD, 0xDD, 0x00, 0x01, 0x05 };
static byte myip[] = { 192, 168, 1, 177 };
byte Ethernet::buffer[700];

const int pinLed1 = 13;
const int pinLed2 = A0;
char* statusLed1 = "OFF";
char* statusLed2 = "OFF";

void setup() {

  Serial.begin(9600);

  if (!ether.begin(sizeof Ethernet::buffer, mymac, 10))
    Serial.println("No se ha podido acceder a la controlador Ethernet");
  else
    Serial.println("Controlador Ethernet inicializado");

  if (!ether.staticSetup(myip))
    Serial.println("No se pudo establecer la dirección IP");
  Serial.println();

  pinMode(pinLed1, OUTPUT);
  pinMode(pinLed2, OUTPUT);
  digitalWrite(pinLed1, LOW);
  digitalWrite(pinLed2, LOW);
}

static word mainPage()
{
  BufferFiller bfill = ether.tcpOffset();
  bfill.emit_p(PSTR("HTTP/1.0 200 OKrn"
    "Content-Type: text/htmlrnPragma: no-cachernRefresh: 5rnrn"
    "<html><head><title>Luis Llamas</title></head>"
    "<body>"
    "<div style='text-align:center;'>"
    "<h1>Salidas digitales</h1>"
    "<br /><br />Estado LED 1 = $S<br />"
    "<a href='./?data1=0'><input type='button' value='OFF'></a>"
    "<a href='./?data1=1'><input type='button' value='ON'></a>"
    "<br /><br />Estado LED 2 = $S<br />"
    "<a href='./?data2=0'><input type='button' value='OFF'></a>"
    "<a href='./?data2=1'><input type='button' value='ON'></a>"
    "<br /></div>\n</body></html>"), statusLed1, statusLed2);

  return bfill.position();
}

void loop() 
{
  word len = ether.packetReceive();
  word pos = ether.packetLoop(len);

  if (pos) 
  {
    if (strstr((char *)Ethernet::buffer + pos, "GET /?data1=0") != 0) {
      Serial.println("Led1 OFF");
      digitalWrite(pinLed1, LOW);
      statusLed1 = "OFF";
    }

    if (strstr((char *)Ethernet::buffer + pos, "GET /?data1=1") != 0) {
      Serial.println("Led1 ON");
      digitalWrite(pinLed1, HIGH);
      statusLed1 = "ON";
    }

    if (strstr((char *)Ethernet::buffer + pos, "GET /?data2=0") != 0) {
      Serial.println("Led2 OFF recieved");
      digitalWrite(pinLed2, LOW);
      statusLed2 = "OFF";
    }

    if (strstr((char *)Ethernet::buffer + pos, "GET /?data2=1") != 0) {
      Serial.println("Led2 ON");
      digitalWrite(pinLed2, HIGH);
      statusLed2 = "ON";
    }

    ether.httpServerReply(mainPage());
  }
}
```


