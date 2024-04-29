> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/como-hacer-un-control-pid-de-iluminacion-constante-con-arduino/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa


## como-hacer-un-control-pid-de-iluminacion-constante-con-arduino
```csharp
#include <PIDController.hpp>

const long A = 1000; //Resistencia en oscuridad en KΩ
const int B = 15; //Resistencia a la luz (10 Lux) en KΩ
const int Rc = 10; //Resistencia calibracion en KΩ

const int PIN_LDR = A0;
const int PIN_LED = 9;

PID::PIDParameters < double > parameters(0.4, 6.0, 0.0001);
PID::PIDController < double > pidController(parameters);

void setup()
{
    Serial.begin(115200);
    pinMode(PIN_LED, OUTPUT);

    pidController.Input = analogRead(PIN_LDR);
    pidController.Setpoint = 350;

    pidController.TurnOn();
}

float readLDR()
{
    float mean = 0;
    for (auto i = 0; i < 100; i++) {
        auto V = (float) analogRead(PIN_LDR);
        mean += (V * A * 10) / (B * Rc * (1024 - V));
    }
    mean /= 100;
    return mean;
}

void loop() 
{
    auto ilum = readLDR();
    Serial.println(ilum);

    pidController.Input = ilum;
    pidController.Update();

    analogWrite(PIN_LED, (int) pidController.Output);
}
```


