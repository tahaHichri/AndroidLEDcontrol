# AndroidLEDcontrol
Control LED from android using Bluetooth module and Arduino board

This app allows you to:
Detect nearby bluetooth devices
Pair to a bluetooth device (PIN code authentication might be required)
Light up / Turn off all LEDs at once
control the brightness of each LED separately ( 25 levels of brighteness )
Disconnect from a bluetooth device

<i>You may need for this project</i>
android Studio, Arduino IDE, Bluetooth HC-05 module, arduino board.
(connections are specified in comments of arduino code) so it is pretty easy :)



<img src="http://i.imgur.com/kQzC331.png" alt="screenshot" height="600"/>


<i>Actual equipment used for this project</i><br />
<img src="http://i.imgur.com/bTCelfW.jpg" alt="entire circuit" height="200"/>


Requirements:
works on android ICE_CREAM_SANDWICH_MR1 (SDK LEVEL 15) or newer versions.

<h3>Arduino code</h3>

``` arduino
#include <SoftwareSerial.h>
#include <stdio.h>


int ledONE = 9;
int ledTWO = 2;
int ledTHREE = 3;


int bluetoothTX = 11 ;
int bluetoothRX = 10 ;
char receivedValue ;

SoftwareSerial bluetooth ( bluetoothTX, bluetoothRX );

void setup()
{
  Serial.begin(9600);  
  Serial.println("console> ");
  
  pinMode(ledONE, OUTPUT);
  pinMode(ledTWO, OUTPUT);
  pinMode(ledTHREE, OUTPUT);
 
  bluetooth.begin(115200);
  bluetooth.print("$$$");
  delay(100);
  bluetooth.println("U,9600,N");
  bluetooth.begin(9600);

}


void loop()
{
int brightness = 0;
int led = 0;
signed int data = 0;

 if( bluetooth.available() )
  {
    data = (int) bluetooth.read();
    Serial.println( data );                 // for debugging, show received data

      if(data == 26) 
      {
        allDown() ;
      }else if(data == 89)
      {
        allUp() ;
      }else{
        
      led = data / 100;                     // which led to select ?
      brightness = data % 100 ;             // 0 - 25 , LED brightness ( * 10 ) for actual value
           
    switch(led)                             // Now, let's select the right led.
    {
      
      case 0  : setLEDthree ( brightness * 10 );  break;
      case 1  : setLEDone ( brightness * 10 );    break;
      case 2  : setLEDtwo ( brightness * 10 );    break;
      default : setLEDthree ( brightness * 10 );  break;       // DO NOT know what to do ? must be led 3
      
    }
      }

    bluetooth.flush();                       // IMPORTANT clean bluetooth stream, flush stuck data

  }

}


// SHUT DOWN all leds
void allDown()
{
    digitalWrite(ledONE,  LOW ) ; 
     digitalWrite(ledTWO,  LOW ) ; 
      digitalWrite(ledTHREE,  LOW ) ; 
}

// ALL up now :)

void allUp()
{
    digitalWrite(ledONE,  HIGH) ; 
     digitalWrite(ledTWO,  HIGH ) ; 
     digitalWrite(ledTHREE,  HIGH ) ; 
}


void setLEDone(int brighteness)
{
 analogWrite(ledONE,  brighteness ) ; 
}

void setLEDtwo(int brighteness)
{
 analogWrite(ledTWO,  brighteness ) ; 
}

void setLEDthree(int brighteness)
{
 analogWrite(ledTHREE,  brighteness ) ; 
}



```





# CONTRIBUTE TO THIS PROJECT
<i>All contribution to this project are welcome! Get involved by forking the source now!</i>









