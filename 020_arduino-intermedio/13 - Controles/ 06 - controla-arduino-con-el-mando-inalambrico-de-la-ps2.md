> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/controla-arduino-con-el-mando-inalambrico-de-la-ps2/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa


## Librería y código
```csharp
#include <PS2X_lib.h>

PS2X ps2x;

int error = 0; 
byte type = 0;
byte vibrate = 0;

void setup(){
  Serial.begin(57600);

  error = ps2x.config_gamepad(13,11,10,12, true, true);   //setup pins and settings:  GamePad(clock, command, attention, data, Pressures?, Rumble?) check for error

  if(error == 0){
    Serial.println("Found Controller, configured successful");
    Serial.println("Try out all the buttons, X will vibrate the controller, faster as you press harder;");
    Serial.println("holding L1 or R1 will print out the analog stick values.");
    Serial.println("Go to www.billporter.info for updates and to report bugs.");
  }

  else if(error == 1)
  Serial.println("No controller found, check wiring, see readme.txt to enable debug. visit www.billporter.info for troubleshooting tips");

  else if(error == 2)
  Serial.println("Controller found but not accepting commands. see readme.txt to enable debug. Visit www.billporter.info for troubleshooting tips");

  else if(error == 3)
  Serial.println("Controller refusing to enter Pressures mode, may not support it. ");

  type = ps2x.readType();   
}

void loop(){ 

  if(type == 1){ 
    ps2x.read_gamepad(false, vibrate);  //Lectura del estado

    //Lectura de los botones de dirección  
    if(ps2x.Button(PSB_PAD_UP)) {
      Serial.print("Up held this hard: ");
      Serial.println(ps2x.Analog(PSAB_PAD_UP), DEC);
    }
    if(ps2x.Button(PSB_PAD_RIGHT)){
      Serial.print("Right held this hard: ");
      Serial.println(ps2x.Analog(PSAB_PAD_RIGHT), DEC);
    }
    if(ps2x.Button(PSB_PAD_LEFT)){
      Serial.print("LEFT held this hard: ");
      Serial.println(ps2x.Analog(PSAB_PAD_LEFT), DEC);
    }
    if(ps2x.Button(PSB_PAD_DOWN)){
      Serial.print("DOWN held this hard: ");
      Serial.println(ps2x.Analog(PSAB_PAD_DOWN), DEC);
    }   

    //Lectura del estado de botones
    if (ps2x.NewButtonState())
    {
      if(ps2x.Button(PSB_RED))
      Serial.println("Circle pressed");
      if(ps2x.Button(PSB_GREEN))
      Serial.println("Triangle pressed");
      if(ps2x.Button(PSB_PINK))
      Serial.println("Square pressed");
      if(ps2x.Button(PSB_BLUE))
      Serial.println("X pressed");
      
      if(ps2x.Button(PSB_START))
      Serial.println("Start is being held");
      if(ps2x.Button(PSB_SELECT))
      Serial.println("Select is being held");
      
      if(ps2x.Button(PSB_L3))
      Serial.println("L3 pressed");
      if(ps2x.Button(PSB_R3))
      Serial.println("R3 pressed");
      if(ps2x.Button(PSB_L2))
      Serial.println("L2 pressed");
      if(ps2x.Button(PSB_R2))
      Serial.println("R2 pressed");
    }      

    //Lectura de las palancas analógicas
    Serial.print("StickValues:");
    Serial.print(ps2x.Analog(PSS_LY), DEC); //Mando izquierdo
    Serial.print(",");
    Serial.print(ps2x.Analog(PSS_LX), DEC); 
    Serial.print(",");
    Serial.print(ps2x.Analog(PSS_RY), DEC); //Mando derecho
    Serial.print(",");
    Serial.println(ps2x.Analog(PSS_RX), DEC); 
  }
  else
  {
    return;
  }

  delay(500);  
}
```


