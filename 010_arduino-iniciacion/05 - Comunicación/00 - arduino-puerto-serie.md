> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/arduino-puerto-serie/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

## Recibir información desde el Arduino
```csharp
int cont=0;

void setup(){
  //iniciamos el puerto serie
  Serial.begin(9600);
}

void loop(){
  //Imprimimos el valor del contador
  Serial.print("Contador: ");
  Serial.println(cont);
  
  //incrementamos el contador y esperamos un segundo
  cont++;
  delay(1000);
}
```


## Enviar información al Arduino
```csharp
int option;
int led = 13;

void setup(){
  Serial.begin(9600);
  pinMode(led, OUTPUT); 
}

void loop(){
  //si existe datos disponibles los leemos
  if (Serial.available()>0){
    //leemos la opcion enviada
    option=Serial.read();
    if(option=='a') {
      digitalWrite(led, LOW);
      Serial.println("OFF");
    }
    if(option=='b') {
      digitalWrite(led, HIGH);
      Serial.println("ON");
    }
  }
}
```


## Enviar valores numéricos
```csharp
int option;
int led = 13;

void setup(){
  Serial.begin(9600);
  pinMode(led, OUTPUT); 
}

void loop(){
  //si existe información pendiente
  if (Serial.available()>0){
    //leeemos la opcion
    char option = Serial.read();
    //si la opcion esta entre '1' y '9'
    if (option >= '1' && option <= '9')
    {
      //restamos el valor '0' para obtener el numeroenviado
      option -= '0';
      for(int i=0;i<option;i++){
         digitalWrite(led, HIGH);
         delay(100);
         digitalWrite(led, LOW);
         delay(200);
      }
    }
  }
}
```


