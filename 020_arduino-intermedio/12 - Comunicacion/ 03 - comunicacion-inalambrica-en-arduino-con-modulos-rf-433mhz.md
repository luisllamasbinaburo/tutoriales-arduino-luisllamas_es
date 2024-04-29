> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/comunicacion-inalambrica-en-arduino-con-modulos-rf-433mhz/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa


## Encender un LED a distancia
```cpp
#include <VirtualWire.h>

const int dataPin = 9;

void setup()
{
    Serial.begin(9600);    
    vw_setup(2000);
    vw_set_tx_pin(dataPin);
}

void loop()
{
    while (Serial.available() > 0) 
    {
      char data[1];
      data[0] = Serial.read();
      vw_send((uint8_t*)data,sizeof(data));
      vw_wait_tx();         
    }
    delay(200);
}
```

```cpp
#include <VirtualWire.h>

const int dataPin = 9;
const int ledPin = 13;

void setup()
{
    vw_setup(2000);
    vw_set_rx_pin(dataPin);
    vw_rx_start();
    
    pinMode(ledPin, OUTPUT);
    digitalWrite(ledPin, false);
}

void loop()
{
    uint8_t data;
    uint8_t dataLength=1;

    if (vw_get_message(&data,&dataLength))
    {
        if((char)data=='a')
        {
            digitalWrite(ledPin, true);
        }
        else if((char)data=='b')
        {
            digitalWrite(ledPin, false);
        }            
    }
}
```



## Enviar un string
```cpp
#include <VirtualWire.h>

const int dataPin = 9;
const int ledPin = 13;

void setup()
{
    vw_setup(2000);
    vw_set_tx_pin(dataPin);
}

void loop()
{
    const char *msg = "Hola mundo";

    digitalWrite(ledPin, true);
    vw_send((uint8_t *)msg, strlen(msg));
    vw_wait_tx();
    digitalWrite(ledPin, false);
    delay(200);
}
```

```cpp
#include <VirtualWire.h>

const int dataPin = 9;
const int ledPin = 13;

void setup()
{
    Serial.begin(9600);
    vw_setup(2000);
    vw_set_rx_pin(dataPin);
    vw_rx_start();
}

void loop()
{
    uint8_t buf[VW_MAX_MESSAGE_LEN];
    uint8_t buflen = VW_MAX_MESSAGE_LEN;
    
    if (vw_get_message(buf, &buflen)) 
    {
    digitalWrite(ledPin, true);
    Serial.print("Mensaje: ");
    for (int i = 0; i < buflen; i++)
    {
      Serial.print((char)buf[i]);
    }
    Serial.println("");
        digitalWrite(ledPin, false);
    }
}
```



## Enviar una variable integer y float
```cpp
#include <VirtualWire.h>

const int dataPin = 9;
  
void setup()
{
    vw_setup(2000);
    vw_set_tx_pin(dataPin);
}

void loop()
{
  String str;  
    char buf[VW_MAX_MESSAGE_LEN];
  
  // Ejemplo de envío int
  int dataInt = millis() / 1000;
    str = "i" + String(dataInt); /// Convertir a string
    str.toCharArray(buf,sizeof(buf)); // Convertir a char array
    vw_send((uint8_t *)buf, strlen(buf)); // Enviar array
    vw_wait_tx(); // Esperar envio
    
  // Ejemplo de envío float
  float dataFloat = 3.14;
    str = "f" + String(dataFloat); // Convertir a string
    str.toCharArray(buf,sizeof(buf)); // Convertir a char array
    vw_send((uint8_t *)buf, strlen(buf)); // Enviar array
    vw_wait_tx(); // Esperar envio
    
    delay(200);
}
```

```cpp
#include <VirtualWire.h>

const int dataPin = 9;

void setup()
{
    Serial.begin(9600);
    vw_setup(2000);
    vw_set_rx_pin(dataPin);
    vw_rx_start();
}

void loop()
{
    uint8_t buf[VW_MAX_MESSAGE_LEN];
    uint8_t buflen = VW_MAX_MESSAGE_LEN;

  // Recibir dato
    if (vw_get_message((uint8_t *)buf,&buflen))
    {
    String dataString;
        if((char)buf[0]=='i')
        {
            for (int i = 1; i < buflen; i++)
            {
        dataString.concat((char)buf[i]);
            }
            int dataInt = dataString.toInt();  // Convertir a int
            Serial.print("Int: ");
            Serial.println(dataInt);
        }
        else if((char)buf[0]=='f')
        {
            for (int i = 1; i < buflen; i++)
            {
        dataString.concat((char)buf[i]);
            }
            float dataFloat = dataString.toFloat();  // Convertir a float
            Serial.print("Float: ");
            Serial.println(dataFloat);
        }
    }
}
```


