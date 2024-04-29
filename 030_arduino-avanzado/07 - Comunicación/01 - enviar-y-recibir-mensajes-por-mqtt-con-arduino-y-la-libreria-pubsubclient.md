> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/enviar-y-recibir-mensajes-por-mqtt-con-arduino-y-la-libreria-pubsubclient/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

## Constructores
```csharp
PubSubClient ()
PubSubClient (client)
PubSubClient (server, port, [callback], client, [stream])
```


## Suscribirse a un topic
```csharp
mqttClient.subscribe("hello/world");
```


## Enviar un mensaje
```csharp
mqttClient.publish("hello/world", (char*)payload.c_str());
```


## Funciones importantes
```csharp
mqttClient.loop();
```

```csharp
boolean connect(const char* id);
boolean connect(const char* id, const char* user, const char* pass);
void disconnect();

boolean publish(const char* topic, const char* payload);

boolean subscribe(const char* topic);
boolean subscribe(const char* topic, uint8_t qos); // qos es 0 o 1, por defecto 0
boolean unsubscribe(const char* topic);

PubSubClient& setServer(IPAddress ip, uint16_t port);
PubSubClient& setCallback(MQTT_CALLBACK_SIGNATURE);
PubSubClient& setClient(Client& client);
PubSubClient& setStream(Stream& stream);
PubSubClient& setKeepAlive(uint16_t keepAlive);
PubSubClient& setSocketTimeout(uint16_t timeout);

// prototipo de función callback
void OnMqttReceived(char* topic, byte* payload, unsigned int length)
```


## Ejemplo de código
```csharp
#include <Ethernet.h>

#include <PubSubClient.h>

// direcciones IP, servidor, y MAC
// necesarias para Ethernet Shield
IPAddress ip(172, 16, 0, 100);
IPAddress server(172, 16, 0, 2);
byte mac[] = {
    0xDE,
    0xED,
    0xBA,
    0xFE,
    0xFE,
    0xED};

// instanciar objetos
EthernetClient ethClient;
PubSubClient client(ethClient);

// constantes del MQTT
// direccion broker, puerto, y nombre cliente
const char *MQTT_BROKER_ADRESS = "192.168.1.150";
const uint16_t MQTT_PORT = 1883;
const char *MQTT_CLIENT_NAME = "ArduinoClient_1";

// realiza las suscripción a los topic
// en este ejemplo, solo a 'hello/world'
void SuscribeMqtt()
{
    mqttClient.subscribe("hello/world");
}

// callback a ejecutar cuando se recibe un mensaje
// en este ejemplo, muestra por serial el mensaje recibido
void OnMqttReceived(char *topic, byte *payload, unsigned int length)
{
    Serial.print("Received on ");
    Serial.print(topic);
    Serial.print(": ");

    String content = "";
    for (size_t i = 0; i < length; i++)
    {
        content.concat((char)payload[i]);
    }
    Serial.print(content);
    Serial.println();
}

// inicia la comunicacion MQTT
// inicia establece el servidor y el callback al recibir un mensaje
void InitMqtt()
{
    mqttClient.setServer(MQTT_BROKER_ADRESS, MQTT_PORT);
    mqttClient.setCallback(OnMqttReceived);
}

// conecta o reconecta al MQTT
// consigue conectar -> suscribe a topic y publica un mensaje
// no -> espera 5 segundos
void ConnectMqtt()
{
    Serial.print("Starting MQTT connection...");
    if (mqttClient.connect(MQTT_CLIENT_NAME))
    {
        SuscribeMqtt();
        client.publish("connected","hello/world");
    }
    else
    {
        Serial.print("Failed MQTT connection, rc=");
        Serial.print(mqttClient.state());
        Serial.println(" try again in 5 seconds");

        delay(5000);
    }
}

// gestiona la comunicación MQTT
// comprueba que el cliente está conectado
// no -> intenta reconectar
// si -> llama al MQTT loop
void HandleMqtt()
{
    if (!mqttClient.connected())
    {
        ConnectMqtt();
    }
    mqttClient.loop();
}

// únicamente inicia seria, ethernet y MQTT
void setup()
{
    Serial.begin(9600);

    Ethernet.begin(mac, ip);
    delay(1500);
    InitMqtt();
}

// únicamente llama a HandleMqtt
void loop()
{
    HandleMqtt();
}
```


