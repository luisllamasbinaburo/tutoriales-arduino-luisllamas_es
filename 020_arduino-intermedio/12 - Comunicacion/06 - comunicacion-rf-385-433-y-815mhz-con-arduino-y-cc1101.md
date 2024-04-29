> Códigos de ejemplo de los tutoriales de www.luisllamas.es
>
> Enlace entrada: https://www.luisllamas.es/comunicacion-rf-385-433-y-815mhz-con-arduino-y-cc1101/
>
> Todo el contenido distribuido bajo licencia CCC, salvo indicación expresa

## Emisor
```cpp
#include <ELECHOUSE_CC1101_SRC_DRV.h>

void setup(){

    Serial.begin(9600);

    if (ELECHOUSE_cc1101.getCC1101()){         // Check the CC1101 Spi connection.
    Serial.println("Connection OK");
    }else{
    Serial.println("Connection Error");
    }

    ELECHOUSE_cc1101.Init();              // must be set to initialize the cc1101!
    ELECHOUSE_cc1101.setCCMode(1);       // set config for internal transmission mode.
    ELECHOUSE_cc1101.setModulation(0);  // set modulation mode. 0 = 2-FSK, 1 = GFSK, 2 = ASK/OOK, 3 = 4-FSK, 4 = MSK.
    ELECHOUSE_cc1101.setMHZ(433.92);   // Here you can set your basic frequency. The lib calculates the frequency automatically (default = 433.92).The cc1101 can: 300-348 MHZ, 387-464MHZ and 779-928MHZ. Read More info from datasheet.
    ELECHOUSE_cc1101.setSyncMode(2);  // Combined sync-word qualifier mode. 0 = No preamble/sync. 1 = 16 sync word bits detected. 2 = 16/16 sync word bits detected. 3 = 30/32 sync word bits detected. 4 = No preamble/sync, carrier-sense above threshold. 5 = 15/16 + carrier-sense above threshold. 6 = 16/16 + carrier-sense above threshold. 7 = 30/32 + carrier-sense above threshold.
    ELECHOUSE_cc1101.setCrc(1);      // 1 = CRC calculation in TX and CRC check in RX enabled. 0 = CRC disabled for TX and RX.
    
    Serial.println("Rx Mode");
}
byte buffer[61] = {0};

void loop(){

    //Checks whether something has been received.
    //When something is received we give some time to receive the message in full.(time in millis)
    if (ELECHOUSE_cc1101.CheckRxFifo(100)){
    
    if (ELECHOUSE_cc1101.CheckCRC()){    //CRC Check. If "setCrc(false)" crc returns always OK!
    Serial.print("Rssi: ");
    Serial.println(ELECHOUSE_cc1101.getRssi());
    Serial.print("LQI: ");
    Serial.println(ELECHOUSE_cc1101.getLqi());
    
    int len = ELECHOUSE_cc1101.ReceiveData(buffer);
    buffer[len] = '\0';
    Serial.println((char *) buffer);
    for (int i = 0; i < len; i++){
    Serial.print(buffer[i]);
    Serial.print(",");
    }
    Serial.println();
    }
    }
}
```


## Receptor
```cpp
#include <ELECHOUSE_CC1101_SRC_DRV.h>

void setup(){

    Serial.begin(9600);

    if (ELECHOUSE_cc1101.getCC1101()){         // Check the CC1101 Spi connection.
    Serial.println("Connection OK");
    }else{
    Serial.println("Connection Error");
    }

    ELECHOUSE_cc1101.Init();              // must be set to initialize the cc1101!
    ELECHOUSE_cc1101.setCCMode(1);       // set config for internal transmission mode.
    ELECHOUSE_cc1101.setModulation(0);  // set modulation mode. 0 = 2-FSK, 1 = GFSK, 2 = ASK/OOK, 3 = 4-FSK, 4 = MSK.
    ELECHOUSE_cc1101.setMHZ(433.92);   // Here you can set your basic frequency. The lib calculates the frequency automatically (default = 433.92).The cc1101 can: 300-348 MHZ, 387-464MHZ and 779-928MHZ. Read More info from datasheet.
    ELECHOUSE_cc1101.setSyncMode(2);  // Combined sync-word qualifier mode. 0 = No preamble/sync. 1 = 16 sync word bits detected. 2 = 16/16 sync word bits detected. 3 = 30/32 sync word bits detected. 4 = No preamble/sync, carrier-sense above threshold. 5 = 15/16 + carrier-sense above threshold. 6 = 16/16 + carrier-sense above threshold. 7 = 30/32 + carrier-sense above threshold.
    ELECHOUSE_cc1101.setCrc(1);      // 1 = CRC calculation in TX and CRC check in RX enabled. 0 = CRC disabled for TX and RX.
    
    Serial.println("Rx Mode");
}
byte buffer[61] = {0};

void loop(){

    //Checks whether something has been received.
    //When something is received we give some time to receive the message in full.(time in millis)
    if (ELECHOUSE_cc1101.CheckRxFifo(100)){
    
    if (ELECHOUSE_cc1101.CheckCRC()){    //CRC Check. If "setCrc(false)" crc returns always OK!
    Serial.print("Rssi: ");
    Serial.println(ELECHOUSE_cc1101.getRssi());
    Serial.print("LQI: ");
    Serial.println(ELECHOUSE_cc1101.getLqi());
    
    int len = ELECHOUSE_cc1101.ReceiveData(buffer);
    buffer[len] = '\0';
    Serial.println((char *) buffer);
    for (int i = 0; i < len; i++){
    Serial.print(buffer[i]);
    Serial.print(",");
    }
    Serial.println();
    }
    }
}
```


## RC-Switch
```cpp
#include <ELECHOUSE_CC1101_SRC_DRV.h>
#include <RCSwitch.h>

int pin; // int for Receive pin.

RCSwitch mySwitch = RCSwitch();

void setup() {
  Serial.begin(9600);


#ifdef ESP32
pin = 4;  // for esp32! Receiver on GPIO pin 4. 

#elif ESP8266
pin = 4;  // for esp8266! Receiver on pin 4 = D2.

#else
pin = 0;  // for Arduino! Receiver on interrupt 0 => that is pin #2

#endif 

  ELECHOUSE_cc1101.Init();
  //ELECHOUSE_cc1101.setRxBW(812.50);  // Set the Receive Bandwidth in kHz. Value from 58.03 to 812.50. Default is 812.50 kHz.
  //ELECHOUSE_cc1101.setPA(10);
  ELECHOUSE_cc1101.setMHZ(433.92); 
  
  mySwitch.enableReceive(pin);  // Receiver on interrupt 0 => that is pin #2
  ELECHOUSE_cc1101.SetRx();  // set Receive on
}

void loop() {
  if (mySwitch.available()) 
  {
    output(mySwitch.getReceivedValue(), 
    mySwitch.getReceivedBitlength(), 
    mySwitch.getReceivedDelay(), 
    mySwitch.getReceivedRawdata(),
    mySwitch.getReceivedProtocol());
    mySwitch.resetAvailable();
  }
}
```


## NewRemoteReceiver
```cpp
#include <ELECHOUSE_CC1101_SRC_DRV.h>
#include <NewRemoteReceiver.h>

int pin;

void setup() {
  Serial.begin(115200);


#ifdef ESP32
pin = 4;  // for esp32! Receiver on GPIO pin 4. 

#elif ESP8266
pin = 4;  // for esp8266! Receiver on pin 4 = D2.

#else
pin = 0;  // for Arduino! Receiver on interrupt 0 => that is pin #2

#endif  

  ELECHOUSE_cc1101.Init();            // must be set to initialize the cc1101!
  //ELECHOUSE_cc1101.setRxBW(812.50);  // Set the Receive Bandwidth in kHz. Value from 58.03 to 812.50. Default is 812.50 kHz.
  //ELECHOUSE_cc1101.setPA(10);
  ELECHOUSE_cc1101.setMHZ(433.92);
  ELECHOUSE_cc1101.SetRx();     // set Receive on
  
  NewRemoteReceiver::init(pin, 2, onReceived);
  Serial.println("Receiver initialized");    
}

void loop() {

}

void onReceived(unsigned int period, unsigned long address, unsigned long groupBit, unsigned long unit, unsigned long switchType) 
{
  Serial.print("Code: ");
  Serial.print(address);
  Serial.print(" Period: ");
  Serial.println(period);
  Serial.print(" unit: ");
  Serial.println(unit);
  Serial.print(" groupBit: ");
  Serial.println(groupBit);
  Serial.print(" switchType: ");
  Serial.println(switchType);

}
```


