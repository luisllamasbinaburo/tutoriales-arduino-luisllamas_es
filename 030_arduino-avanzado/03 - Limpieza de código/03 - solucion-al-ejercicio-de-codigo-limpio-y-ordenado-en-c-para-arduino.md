> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/solucion-al-ejercicio-de-codigo-limpio-y-ordenado-en-c-para-arduino/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

## Constantes
```cpp
#pragma once

const char* SSID = "YOUR_SSID";
const char* PASSWORD = "YOUR_PASSWORD";
const char* HOST = "192.168.0.XX";

const float DEFAULT_TEMPERATURE = 19;
const float DEFAULT_THRESHOLD = 1;
const float TURN_ON_THRESHOLD = 2;

const String TURN_OFF_URL = "http://192.168.0.XX:XXX/3?02621FF5870F11FF=I=3";
const String TURN_ON_URL = "http://192.168.0.XX:XXX/3?02621FF5870F11FF=I=3";
```

```cpp
#pragma once

const uint8_t SWITCH_PIN = 14 ;// interrupt 0 Pin 2 in Arduino is a must due to interrupts
const uint8_t CLOCK_PIN = 13; // interrupt 1 Pin 3 in Arduino is a must due to interrupts
const uint8_t DATA_PIN = 11;

const uint8_t LED_PIN = 16;

const uint8_t DHT_PIN = 12;
```


## Enums
```cpp
enum COMMAND_OPERATION
{
    INCREASE,
    DECREASE,
    TURN_ON,
    TURN_OFF,
    SET_DEFAULT,
    SET_SETPOINT
};
```

```cpp
enum COMMAND_RESULT
{
    EMPTY,
    VALID,
    INVALID,
    NOT_CONNECTED
};
```

```cpp
enum THERMOSTAT_STATUS
{
    OFF,
    ON,
};
```


## Componentes
```cpp
components/Button.hpp
components/Encoder.hpp
components/Led.hpp
components/Display_I2C_Lcd.hpp
components/TemperatureSensor_DHT11.hpp
components/ThresholdController.hpp
```


## ButtonComponent
```cpp
#pragma once

class ButtonComponent
{
public:
    void Init()
    {
        pinMode(SWITCH_PIN, INPUT_PULLUP);
        attachInterruptArg(digitalPinToInterrupt(SWITCH_PIN), ButtonComponent::GetISR, this, FALLING);
    }

    bool HasBeenPressed()
    {
        return pressedStatus;
    }

    void Restart()
    {
        pressedStatus = false;
        lastMillis = millis();
    }

    void ISR()
    {
        unsigned long lastMillis;
        if((unsigned long)(millis() - lastMillis) >= DEBOUNCE_MILLIS)
        {
            lastMillis = millis();

            pressedStatus = true;
        }
    }

private:
    volatile bool pressedStatus;

    const unsigned long DEBOUNCE_MILLIS = 100;
    unsigned long lastMillis;

    void static GetISR(void* switchComponent)
    {
        ((ButtonComponent*)switchComponent)->ISR();
    }
};
```


## EncoderComponent
```cpp
#pragma once

class EncoderComponent
{
public:
    void Init()
    {
        pinMode(CLOCK_PIN, INPUT);
        pinMode(DATA_PIN, INPUT);

        attachInterruptArg(digitalPinToInterrupt(CLOCK_PIN), EncoderComponent::GetISR, this, FALLING);
    }

    void SetCounter(int value)
    {
        counter = value;
    }

    bool HasChanged()
    {
        return change != 0;
    }

    int GetCounter()
    {
        return counter;
    }

    void IncreaseCounter()
    {
        counter++;
    }

    void DecreaseCounter()
    {
        counter--;
    }
    
    void Restart()
    {
        change = 0;
    }

    void ISR()
    {
        if((unsigned long)(millis() - lastMillis) >= DEBOUNCE_MILLIS)
        {
            lastMillis = millis();

            if(digitalRead(DATA_PIN) == LOW)
            {
                counter++;
            }
            else
            {
                counter--;
            }

            change++;
        }
    }

private:
    volatile int counter;
    volatile int change;

    const unsigned long DEBOUNCE_MILLIS = 100;
    unsigned long lastMillis;
    
    void static GetISR(void* encoderComponent)
    {
        ((EncoderComponent*)encoderComponent)->ISR();
    }
};
```


## ThesholdController
```cpp
class ThresholdController
{

public:
    float SetPoint = DEFAULT_TEMPERATURE;
    float Threshold = DEFAULT_THRESHOLD;

    void TurnOn()
    {
        isTurnedOn = true;
    }
    void TurnOff()
    {
        isTurnedOn = false;
    }

    bool IsTurnedOn()
    {
        return isTurnedOn;
    }

    bool IsTurnedOff()
    {
        return !isTurnedOn;
    }

    bool Update(float value)
    {
        if(IsTurnedOn() && value >= SetPoint + Threshold)
        {
            TurnOff();
        }

        if(IsTurnedOff() && value <= SetPoint - Threshold)
        {
            TurnOn();
        }
    }

private:
    bool isTurnedOn;
};
```


## LedComponent
```cpp
class LedComponent
{
public:
    uint8_t Led_Pin;

    LedComponent(uint8_t led_Pin)
    {
        Led_Pin = led_Pin;
    }

    void Init()
    {
        pinMode(Led_Pin, OUTPUT);
        TurnOff();
    }

    void TurnOn()
    {
        digitalWrite(Led_Pin, HIGH);
    }

    void TurnOff()
    {
        digitalWrite(Led_Pin, LOW);
    }
};
```


## Display
```cpp
#pragma once

class IDisplay
{
public:
    void virtual Init() = 0;

    void virtual Show(float temp, float target) = 0;
};
```

```cpp
#pragma once

#include "../bases/IDisplay.hpp"

class DisplayComponent_I2C_Lcd : public IDisplay
{
public:
    DisplayComponent_I2C_Lcd()
    {
        Lcd = nullptr;
    }

    DisplayComponent_I2C_Lcd(LiquidCrystal_I2C& lcd)
    {
        Lcd = &lcd;
    }

    void Init()
    {
        Lcd->backlight();
        Lcd->clear();

        Lcd->print("Room Temp:");
        Lcd->setCursor(0, 1);
        Lcd->print("Setpoint:");
    }

    void Show(float temp, float target)
    {
        Lcd->setCursor(11, 0);
        Lcd->print(temp, 0);
        Lcd->print(" C ");
        Lcd->setCursor(11, 1);
        Lcd->print(target);
        Lcd->print(" C ");
    }
private:
    LiquidCrystal_I2C* Lcd;
};
```


## Sensor de temperatura
```cpp
#pragma once

class ISensor
{
public:
    void virtual Init() = 0;

    float virtual Read() = 0;
};
```

```cpp
#pragma once

#include "../bases/ITemperatureSensor.hpp"
#include <MeanFilterLib.h>

class TemperatureSensorComponent_DHT11: public ITemperatureSensor
{
public:
    TemperatureSensorComponent_DHT11() : meanFilter(10)
    {
    }
    
    MeanFilter<float> meanFilter;

    DHTesp dht;

    void Init()
    {
        dht.setup(DHT_PIN, DHTesp::DHT11);
    }

    float Read()
    {
        auto rawTemperature = dht.getTemperature();
        auto humidity = dht.getHumidity();  //TODO: Esto no lo usa para nada
        auto temperature = meanFilter.AddValue(rawTemperature);

        Serial.print("\tAVG:\t");
        Serial.println(temperature, 0);

        return temperature;
    }
};
```


## Servicios
```cpp
#pragma once

#include "../constants/enums.hpp"

struct CommandBase
{
    COMMAND_RESULT Result;
    COMMAND_OPERATION Operation;
};
```

```cpp
services/UserInputService.hpp
services/ComunicationService.hpp
```


## UserInputService
```cpp
#pragma once

#include <WiFi.h>

#include "../constants/enums.hpp"

struct UserInputCommand : public CommandBase {};

class UserInputService
{
public:
    UserInputCommand HandleUserInput(EncoderComponent encoder, ButtonComponent button)
    {
        UserInputCommand result;
        result.Result = COMMAND_RESULT::EMPTY;

        CompoundUserInputCommand(result,  HandleUserInput(encoder));
        CompoundUserInputCommand(result,  HandleUserInput(button));
        
        return result;
    }

    UserInputCommand HandleUserInput(EncoderComponent encoder)
    {
        UserInputCommand result;
        result.Result = COMMAND_RESULT::EMPTY;

        if(encoder.HasChanged())
        {
            result.Result = COMMAND_RESULT::VALID;
            result.Operation = COMMAND_OPERATION::SET_SETPOINT;
        }

        return result;
    }

    UserInputCommand HandleUserInput(ButtonComponent button)
    {
        UserInputCommand result;
        result.Result = COMMAND_RESULT::EMPTY;

        if(button.HasBeenPressed())
        {
            result.Result = COMMAND_RESULT::VALID;
            result.Operation = COMMAND_OPERATION::SET_DEFAULT;
        }

        return result;
    }

private:
    UserInputCommand CompoundUserInputCommand(UserInputCommand base, UserInputCommand candidate)
    {
        UserInputCommand compounded = base;

        if(candidate.Result == COMMAND_RESULT::VALID)
        {
            compounded = candidate;
        }
        
        return compounded;
    }
};
```


## ComunicationService
```cpp
#pragma once

#include <WiFi.h>

#include "../constants/enums.hpp"

struct CommunicationCommand : public CommandBase {};

class ComunicationService
{
public:
    ComunicationService(int port) : server(port)
    {
    }

    void Start()
    {
        Serial.println();
        Serial.print("Connecting to ");
        Serial.println(SSID);

        WiFi.begin(SSID, PASSWORD);
        while(WiFi.status() != WL_CONNECTED)
        {
            delay(500);
            Serial.print(".");
        }

        Serial.println("");
        Serial.println("WiFi connected");
        Serial.println(WiFi.localIP());

        StartServer();
    }

    void StartServer()
    {
        server.begin();
        Serial.println("Server started");
    }

    CommunicationCommand ProcessRequest(String request)
    {
        CommunicationCommand result;
        result.Result = COMMAND_RESULT::EMPTY;

        if(request.indexOf("") != -10)
        {
            if(request.indexOf("/+") != -1)
            {
                result.Operation = COMMAND_OPERATION::DECREASE;
            }
            if(request.indexOf("/-") != -1)
            {
                result.Operation = COMMAND_OPERATION::DECREASE;
            }
            if(request.indexOf("/ON") != -1)
            {
                result.Operation = COMMAND_OPERATION::TURN_ON;
            }
            if(request.indexOf("/OFF") != -1)
            {
                result.Operation = COMMAND_OPERATION::TURN_OFF;
            }
        }
        else
        {
            result.Result = COMMAND_RESULT::INVALID;
        }

        return result;
    }

    String response = "HTTP/1.1 200 OK\r\n"
        "Content-Type: text/html\r\n\r\n"
        "<!DOCTYPE HTML>\r\n<html>\r\n"
        "<p>Setpoint Temperature <a href='/+'\"><button>+</button></a>&nbsp;<a href='/-'\"><button>-</button></a></p>"
        "<p>Boiler Status <a href='/ON'\"><button>ON</button></a>&nbsp;<a href='/OFF'\"><button>OFF</button></a></p>";

    void SendResponse(WiFiClient& client, const float temperature, const float setpoint)
    {
        client.flush();
        client.print(response);

        client.println();
        client.print("Room Temperature = ");
        client.println(temperature);
        client.println();
        client.print("Setpoint = ");
        client.println(setpoint);
        delay(1);
    }

    CommunicationCommand HandleCommunications(const float temperature, const float setpoint)
    {
        CommunicationCommand result;
        WiFiClient client = server.available();

        if(!client)
        {
            result.Result = COMMAND_RESULT::NOT_CONNECTED;
        }

        // TODO: esta espera bloqueante no tiene sentido
        while(!client.available())
        {
            delay(1);
        }

        auto req = client.readStringUntil('\r');
        client.flush();

        auto command = ProcessRequest(req);
        if(command.Result == COMMAND_RESULT::INVALID)
        {
            // TODO: este stop tampoco tiene sentido
            client.stop();
        }
        else
        {
            result = command;
            SendResponse(client, temperature, setpoint);
        }

        return result;
    }

private:
    WiFiServer server;
};
```


## InsteonAPI
```cpp
#pragma once

#include "constants/config.h"

class InsteonApi
{
public:
    static void SetOn()
    {
        PerformAction(TURN_ON_URL);
    }

    static void SetOff()
    {
        PerformAction(TURN_OFF_URL);
    }

private:
    static void PerformAction(const String& URL)
    {
        HTTPClient http;

        http.begin(URL);
        http.addHeader("Content-Type", "text/plain");

        int httpCode = http.POST("Message from ESP8266");
        String payload = http.getString();

        Serial.println(httpCode);
        Serial.println(payload);

        http.end();
    }
};
```


## Modelo (parte II)
```cpp
#pragma once

#include "constants/config.h"
#include "constants/pinout.h"
#include "constants/enums.hpp"

#include "bases/CommandBase.hpp"
#include "bases/ITemperatureSensor.hpp"
#include "bases/IDisplay.hpp"

#include "components/Led.hpp"
#include "components/Encoder.hpp"
#include "components/Button.hpp"
#include "components/Display_I2C_Lcd.hpp"
#include "components/TemperatureSensor_DHT11.hpp"
#include "components/ThresholdController.hpp"

#include "services/UserInputService.hpp"
#include "services/ComunicationService.hpp"

#include "InsteonApi.h"

class IotThermostatModel
{
public:
    IotThermostatModel() : communicationService(303), led(LED_PIN)
    {
    }

    void Init(IDisplay& idisplay, ITemperatureSensor& itempSensor)
    {
        display = &idisplay;
        temperatureSensor = &itempSensor;

        display->Init();
        temperatureSensor->Init();
        encoder.Init();
        button.Init();

        communicationService.Start();
    }

    float GetTemperature()
    {
        ReadTemperature();
        return currentTemperature;
    }

    void SetSetpoint(float value)
    {
        controller.SetPoint = value;
        encoder.SetCounter(value);
    }

    float GetSetPoint()
    {
        return controller.SetPoint;
    }

    void Run()
    {
        auto userInputResult = inputService.HandleUserInput(encoder, button);
        ProcessCommand(userInputResult);

        ReadTemperature();

        UpdateControllerStatus();

        auto communicationResult = communicationService.HandleCommunications(currentTemperature, GetSetPoint());
        ProcessCommand(communicationResult);

        display->Show(currentTemperature, GetSetPoint());
    }

private:
    void ReadTemperature()
    {
        currentTemperature = temperatureSensor->Read();
    }

    void UpdateControllerStatus()
    {
        auto isControllerTurnedOn = controller.Update(currentTemperature);
        auto newStatus = isControllerTurnedOn ? THERMOSTAT_STATUS::ON : THERMOSTAT_STATUS::OFF;

        if(status != newStatus)
        {
            PerformStatusChange(newStatus);
            status = newStatus;
        }
    }

    void PerformStatusChange(THERMOSTAT_STATUS status)
    {
        switch(status)
        {
        case OFF:
            InsteonApi::SetOff();
            led.TurnOff();
            break;

        case ON:
            InsteonApi::SetOn();
            led.TurnOn();
            break;

        default:
            break;
        }
    }

    void ProcessCommand(CommandBase command)
    {
        if(command.Result == COMMAND_RESULT::VALID);
        {
            ProcessOperation(command.Operation);
            Serial.println(controller.SetPoint);
        }
    }

    void ProcessOperation(COMMAND_OPERATION operation)
    {
        switch(operation)
        {
        case SET_DEFAULT:
            SetSetpoint(DEFAULT_TEMPERATURE);
            break;

        case SET_SETPOINT:
            SetSetpoint(encoder.GetCounter());
            break;

        case INCREASE:
            encoder.DecreaseCounter();
            Serial.println("You clicked -");
            break;

        case DECREASE:
            encoder.IncreaseCounter();
            Serial.println("You clicked -");

        case TURN_ON:
            SetSetpoint(currentTemperature + TURN_ON_THRESHOLD);
            Serial.println("You clicked Boiler On");
            break;

        case TURN_OFF:
            SetSetpoint(DEFAULT_TEMPERATURE);
            Serial.println("You clicked Boiler Off");
            break;

        default:
            break;
        }
    }

    THERMOSTAT_STATUS status = THERMOSTAT_STATUS::OFF;

    EncoderComponent encoder;
    ButtonComponent button;
    LedComponent led;

    IDisplay* display;
    ISensor* temperatureSensor;

    ThresholdController controller;

    UserInputService inputService;
    ComunicationService communicationService;

    float currentTemperature;
};
```

```cpp
#include <WiFi.h>
#include <HTTPClient.h>

#include <Wire.h>
#include <LiquidCrystal_I2C.h>
#include <DHTesp.h>

#include "IotThermostatModel.hpp"

LiquidCrystal_I2C lcd(0, 0, 0);
DisplayComponent_I2C_Lcd display(lcd);

TemperatureSensorComponent_DHT11 dht;

IotThermostatModel iotThermostat;

void setup()
{
    Serial.begin(115200);
    delay(10);

    iotThermostat.Init(display, dht);
}

void loop()
{
    iotThermostat.Run();
    delay(1000);
}
```


## Refactorizar
```cpp
void Run()
{
  auto userInputResult = inputService.HandleUserInput(encoder, button);
  ProcessCommand(userInputResult);

  ReadTemperature();

  UpdateControllerStatus();

  auto communicationResult = communicationService.HandleCommunications(currentTemperature, GetSetPoint());
  ProcessCommand(communicationResult);

  display->Show(currentTemperature, GetSetPoint());
}
```


## Eficiencia
```csharp
//Program size: 888.322 bytes (used 68% of a 1.310.720 byte maximum) (26,15 secs)
//Minimum Memory Usage: 40176 bytes (12% of a 327680 byte maximum)
```

```csharp
//Program size: 887.634 bytes (used 68% of a 1.310.720 byte maximum) (15,41 secs)
//Minimum Memory Usage: 40216 bytes (12% of a 327680 byte maximum)
```


