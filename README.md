# karibuKenya
/* Interactive installation by Patricia Mwenda 
   Arduino codes are based on Kate Hartman / Nick Puckett's examples:
   Fade multiple LEDs at different rates and servo sweep
   DIGF-6037-001 (Fall 2020) Creation & Computation 
 */
 
#include <Servo.h>

int servoPin = 8;           //Pin that the servo is attached to.
int moveRate = 50;        //the time between updates in milliseconds
int minAngle = 0;     //sets the low point of the movement range
int maxAngle = 360;   //sets the high point of the movement range
int moveIncrement = 4;    // how much to move the motor each cycle 
int servoAngle1;

long lastTimeYouMoved1;

Servo servo; 
 
int sensorPin = A0;
int sensorVal;

int ledPins[] = {10,11,12};    
long lastTimeYouFaded[] ={1,1,3,}; 
boolean ledStates[] = {false,false,false,false,false};
int fadeAmount ;    // how many points to fade the LED by  
int ledTotal = sizeof(ledPins) / sizeof(int);  
                                     

void setup() {
  servo.attach(servoPin);

}

void loop() {  
 
  sensorVal=analogRead(sensorPin);
  if (sensorVal>600){
    fadeAmount = 200;
  } else if (sensorVal<300){
    fadeAmount = 1000; 
  } else {
    fadeAmount = 400;
  }

 
 if(millis()-lastTimeYouMoved1>=moveRate){                                
    servo.write(servoAngle1);
    servoAngle1 += moveIncrement;
    if (servoAngle1 <= minAngle || servoAngle1 >= maxAngle) {
      moveIncrement =- moveIncrement;
    }
    lastTimeYouMoved1 = millis();    
  }
  
  for(int i=0;i<ledTotal;i++){                            
    if(millis()-lastTimeYouFaded[i]>=fadeAmount){
      ledStates[i] = !ledStates[i];
      lastTimeYouFaded[i] = millis();
      pinMode(ledPins[i],OUTPUT);
      digitalWrite(ledPins[i],ledStates[i]);
    }        
  }
}
