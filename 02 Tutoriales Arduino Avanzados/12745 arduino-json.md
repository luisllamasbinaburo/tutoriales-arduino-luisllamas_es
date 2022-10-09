> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/arduino-json/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

```json

{
  "id": 12,
  "description": "demoItem",
  "value": 50.4
}
```

```json
[
  "banana",
  12,
  3.1416
]
```

```json

{
  "id": 12,
  "name": "peter",
  "fruits": [
    "banana",
    "orange",
    "raspberry"
  ],
  "house": {
    "address": "white citadel",
    "city": "gondor"
  }
}
```

```cpp
StaticJsonDocument<200> doc;
DynamicJsonDocument doc(1024);
```

```cpp
doc["hola"] = "mundo";
```

```cpp
doc.add("hola mundo");
```

```cpp
JsonObject obj = doc.createNestedObject();
obj["hello"] = "world";
```

```cpp
JsonArray obj = doc.createNestedArray("nested");
obj.add("hola mundo");
```

```cpp
StaticJsonDocument<200> doc;
```

```cpp
DynamicJsonDocument doc(1024);
```

```cpp
serializeJson(const JsonDocument& doc, char* output, size_t outputSize);
serializeJson(const JsonDocument& doc, char output[size]);
serializeJson(const JsonDocument& doc, Print& output);
serializeJson(const JsonDocument& doc, String& output);
serializeJson(const JsonDocument& doc, std::string& output);
serializeJson(const JsonDocument& doc, std::ostream& output);
```

```cpp
DeserializationError deserializeJson(JsonDocument& doc, const char* input);
DeserializationError deserializeJson(JsonDocument& doc, const char* input, size_t inputSize);
DeserializationError deserializeJson(JsonDocument& doc, const __FlashStringHelper* input);
DeserializationError deserializeJson(JsonDocument& doc, const __FlashStringHelper* input, size_t inputSize);
DeserializationError deserializeJson(JsonDocument& doc, const String& input);
DeserializationError deserializeJson(JsonDocument& doc, const std::string& input);
DeserializationError deserializeJson(JsonDocument& doc, Stream& input);
DeserializationError deserializeJson(JsonDocument& doc, std::istream& input);
```

```cpp
#include <ArduinoJson.hpp>
#include <ArduinoJson.h>

void SerializeObject()
{
    String json;
    StaticJsonDocument<300> doc;
    doc["text"] = "myText";
    doc["id"] = 10;
    doc["status"] = true;
    doc["value"] = 3.14;

    serializeJson(doc, json);
    Serial.println(json);
}

void DeserializeObject()
{
    String json = "{\"text\":\"myText\",\"id\":10,\"status\":true,\"value\":3.14}";

    StaticJsonDocument<300> doc;
    DeserializationError error = deserializeJson(doc, json);
    if (error) { return; }

    String text = doc["text"];
    int id = doc["id"];
    bool stat = doc["status"];
    float value = doc["value"];

    Serial.println(text);
    Serial.println(id);
    Serial.println(stat);
    Serial.println(value);
}

void setup()
{
    Serial.begin(115200);

    Serial.println("===== Object Example =====");
    Serial.println("-- Serialize --");
    SerializeObject();
    Serial.println();
    Serial.println("-- Deserialize --");
    DeserializeObject();

}

void loop()
{
}
```

```json
{
    "text": "myText",
    "id": 10,
    "status": true,
    "value": 3.14
}
```

```cpp
#include <ArduinoJson.hpp>
#include <ArduinoJson.h>

void SerializeArray()
{
    String json;
    StaticJsonDocument<300> doc;
    doc.add("B");
    doc.add(45);
    doc.add(2.1728);
    doc.add(true);

    serializeJson(doc, json);
    Serial.println(json);
}

void DeserializeArray()
{
    String json = "[\"B\",45,2.1728,true]";

    StaticJsonDocument<300> doc;
    DeserializationError error = deserializeJson(doc, json);
    if (error) { return; }

    String item0 = doc[0];
    int item1 = doc[1];
    float item2 = doc[2];
    bool item3 = doc[3];

    Serial.println(item0);
    Serial.println(item1);
    Serial.println(item2);
    Serial.println(item3);
}

void setup()
{
    Serial.begin(115200);

    Serial.println("===== Array Example =====");
    Serial.println("-- Serialize --");
    SerializeArray();
    Serial.println();
    Serial.println("-- Deserialize --");
    DeserializeArray();
    Serial.println();
}

void loop()
{
}
```

```json
["B", 45, 2.1728, true]
```

```cpp
#include <ArduinoJson.hpp>
#include <ArduinoJson.h>

void SerializeComplex()
{
    String json;
    StaticJsonDocument<300> doc;
    doc["text"] = "myText";
    doc["id"] = 10;
    doc["status"] = true;
    doc["value"] = 3.14;

    JsonObject obj = doc.createNestedObject("nestedObject");
    obj["key"] = 40;
    obj["description"] = "myDescription";
    obj["active"] = true;
    obj["qty"] = 1.414;

    JsonArray arr = doc.createNestedArray("nestedArray");
    arr.add("B");
    arr.add(45);
    arr.add(2.1728);
    arr.add(false);

    serializeJson(doc, json);
    Serial.println(json);
}

void DeserializeComplex()
{
    String json = "{\"text\":\"myText\",\"id\":10,\"status\":true,\"value\":3.14,\"nestedObject\":{\"key\":40,\"description\":\"myDescription\",\"active\":true,\"qty\":1.414},\"nestedArray\":[\"B\",45,2.1728,true]}";

    StaticJsonDocument<300> doc;
    DeserializationError error = deserializeJson(doc, json);
    if (error) { return; }

    String text = doc["text"];
    int id = doc["id"];
    bool stat = doc["status"];
    float value = doc["value"];

    Serial.println(text);
    Serial.println(id);
    Serial.println(stat);
    Serial.println(value);

    int key = doc["nestedObject"]["key"];
    String description = doc["nestedObject"]["description"];
    bool active = doc["nestedObject"]["active"];
    float qty = doc["nestedObject"]["qty"];

    Serial.println(key);
    Serial.println(description);
    Serial.println(active);
    Serial.println(qty);

    String item0 = doc["nestedArray"][0];
    int item1 = doc["nestedArray"][1];
    float item2 = doc["nestedArray"][2];
    bool item3 = doc["nestedArray"][3];

    Serial.println(item0);
    Serial.println(item1);
    Serial.println(item2);
    Serial.println(item3);
}

void setup()
{
    Serial.begin(115200);

    Serial.println("===== Complex Example =====");
    Serial.println("-- Serialize --");
    SerializeComplex();
    Serial.println();
    Serial.println("-- Deserialize --");
    DeserializeComplex();
    Serial.println();
}

void loop()
{
}
```

```json
{
    "text": "myText",
    "id": 10,
    "status": true,
    "value": 3.14,
    "nestedObject": {
        "key": 40,
        "description": "myDescription",
        "active": true,
        "qty": 1.414
    },
    "nestedArray": ["B", 45, 2.1728, true]
}
```