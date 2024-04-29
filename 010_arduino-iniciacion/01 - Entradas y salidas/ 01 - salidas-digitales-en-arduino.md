> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/salidas-digitales-en-arduino/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa


## Código
```cpp
const int pin = 2;
 
void setup() {
  Serial.begin(9600);    //iniciar puerto serie
  pinMode(pin, OUTPUT);  //definir pin como salida
}
 
void loop(){
  digitalWrite(pin, HIGH);   // poner el Pin en HIGH
  delay(1000);               // esperar un segundo
  digitalWrite(pin, LOW);    // poner el Pin en LOW
  delay(1000);               // esperar un segundo
}
```

```cpp
const int pin = 2;
int option;

void setup(){
  Serial.begin(9600);
  pinMode(pin, OUTPUT); 
}
 
void loop(){
  //si existe información pendiente
  if (Serial.available()>0){
    //leeemos la opcion
    char option = Serial.read();
    if (option == '0' )  //si el valor es 0
    {
         digitalWrite(pin, LOW);  //apagamos el pin
    }
    else if (option == '1' )
    {
         digitalWrite(pin, HIGH);  //encendemos el pin
    }
    delay(200);
  }
}
```


