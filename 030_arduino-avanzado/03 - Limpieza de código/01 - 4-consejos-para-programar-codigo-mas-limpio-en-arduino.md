> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/4-consejos-para-programar-codigo-mas-limpio-en-arduino/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

## Crea un fichero de constantes
```cpp
// ejemplo
const int NUM_CHANNELS = 3;
const int NUM_MEASURES = 10;
const float SENSOR_GAIN = 3.5;
   // .... más constantes
```


## Minimiza los comentarios
```cpp
// leo la temperatura del sensor
int temperatura = analogRead(pin_sensor)

// multiplico la temperatura por dos
temperatura = temperatura * 2;
```


## Usa nombres de variables representativos
```cpp
// nope
int v = analogRead(ps);
float r = v * f;
float t = r * fs;

// sipe
int voltaje = analogRead(PIN_SENSOR);
float raw_temperature = voltaje * CONVERSION_C_MV;
float temperature = raw_temperature * FACTOR_SENSOR;
```


## Usa nombres de funciones representativos
```cpp
// nope
int DoMagic(int ps) { … }

// sipe
int GetTemperature(int pin_sensor) { … }
```


## Limpieza en parámetros de funciones
```cpp
// nope
int GetTemperatura(int ps, int i, int d) { … }

// pseeee
int GetTemperature(int pin_sensor, int interval, int delay) { … }

// sipe
int GetTemperature(Config configuration) { … }
```


## Evita los números "mágicos"
```cpp
//nope
int temperatura = tension * 3 * 1.674;

//sipe
int temperatura = tension * NUM_CHANNELS * CONVERSION_C_MV
```


## No te evites variables intermedias
```cpp
// ejemplo 1
//nope
bool result = Validate(GetTemperature(GetVoltaje(PIN_SENSOR)));

//sipe
int voltage = GetVoltaje(PIN_SENSOR);
float temperature = GetTemperature(voltage);
bool isValid = Validate(temperature);

// ejemplo 2
//nope
if(GetMeasure(pin_sensor)  { …} 

//sipe
auto isValid = GetMeasure(PIN_SENSOR);
if(isValid)  { … }
```


## Usa enumeraciones
```cpp
// nope
if( Key == 0) MotorUp();

// sipe
enum Control_Key { UP, DOWN, LEFT, RIGHT }

if( Key == Control_Key.UP) MotorUp();
```


## Una única salida por función
```cpp
// nope
int miFuncion()
{ 
  if(esto) return 5;
  //... más codigo

  for(loquesea)
  {
    if(otraCosa) return 10;
  }
  
   if(aún otro condicional) return 12;
   //... mucho más codigo
}

//sipe
int miFuncion()
{ 
   int result;
   if(esto) result = 5;
   //... más codigo

  for(loquesea)
  {
    if(otraCosa) result = 10;
  }
  
   if(aún otro condicional) result = 12;
   //... mucho más codigo
   
   return result;
}
```

```cpp
// pseee aceptable
void MoverMotor(int parametro)
{
  if(parametro < 10) return;
  if(parametro > 210) return;
  
  //... mover motor
}
```


## Ajusta el Scope de las variables
```cpp
// nope
float raw;
float a;
float b;
float temperature;
void CalculateTemperatura()
{
  temperature= a * raw + b
}

// sipe
float CalculateTemperature(float raw, float a, float b)
{
  return a * raw + b
}
```


## Evita las operaciones de bit y las máscaras
```cpp
int T = (REG >> 2 & b00001001 ) || ( TMCR_6 >> 3 & b00100101 );
```


## Precremento y postcremento
```cpp
//nope
var miValor = miVector[i++] * 5.7;

//asi si
i++;
auto miValor = miVector[i] * 5.7;
```


## Usa los bucles que tocan
```cpp
// nope, don't do that
for(;; condition)
{
}
```


## Optimiza con cabeza
```cpp
// un aplauso para esta falta de sentido común
int T = (REG >> 2 & b00001001 ) || ( TMCR_6 >> 3 & b00100101 );
delay(5000);
```


## Evita usar #define
```cpp
// nope

#define pin_sensor 5;

// sipe
const int PIN_SENSOR = 5;
```


