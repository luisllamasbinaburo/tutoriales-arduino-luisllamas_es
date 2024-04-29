> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/como-implementar-un-controlador-pid-en-arduino/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

## Controlador PID a mano
```cpp
// Asignaciones pins
const int PIN_INPUT = A0;
const int PIN_OUTPUT = 3;

// Constantes del controlador
double Kp=2, Ki=5, Kd=1;

// variables externas del controlador
double Input, Output, Setpoint;
 
// variables internas del controlador
unsigned long currentTime, previousTime;
double elapsedTime;
double error, lastError, cumError, rateError;
 
void setup()
{
  Input = analogRead(PIN_INPUT);
  Setpoint = 100;
}    
 
void loop(){
  
  pidController.Compute();
  
  Input = analogRead(PIN_INPUT);      // leer una entrada del controlador
  Output = computePID(Input);    // calcular el controlador
  delay(100);
  analogWrite(PIN_OUTPUT, Output);      // escribir la salida del controlador
}
 
double computePID(double inp){     
        currentTime = millis();                          // obtener el tiempo actual
        elapsedTime = (double)(currentTime - previousTime);     // calcular el tiempo transcurrido
        
        error = Setpoint - Input;                               // determinar el error entre la consigna y la medición
        cumError += error * elapsedTime;                    // calcular la integral del error
        rateError = (error - lastError) / elapsedTime;       // calcular la derivada del error
 
        double output = kp*error + ki*cumError + kd*rateError;     // calcular la salida del PID
 
        lastError = error;                                    // almacenar error anterior
        previousTime = currentTime;                           // almacenar el tiempo anterior
 
        return output;
}
```


## Controlador PID con librería
```cpp
#include <PIDController.hpp>

const int PIN_INPUT = 0;
const int PIN_OUTPUT = 3;

PID::PIDParameters<double> parameters(4.0, 0.2, 1);
PID::PIDController<double> pidController(parameters);

void setup()
{
  pidController.Input = analogRead(PIN_INPUT);
  pidController.Setpoint = 100;

  pidController.TurnOn();
}

void loop()
{
  pidController.Input = analogRead(PIN_INPUT);
  pidController.Update();

  analogWrite(PIN_OUTPUT, pidController.Output);
}
```


