# Obstacle-Avoiding-Bot
#include<Servo.h>

int o;

int tmax=0;
int point;
int greenLed=4;

Servo myservo;
int pos;

unsigned long travelTime;
int trigPin=12;
int echoPin=11;
int ledPin=3;

unsigned long od;

int j;

int en1=5;
int en2=6;

void setup() {
  myservo.attach(9);
  delay(15);

  pinMode(trigPin,OUTPUT);
  pinMode(echoPin,INPUT);
  pinMode(ledPin,OUTPUT);
  Serial.begin(9600);
  pinMode(greenLed,OUTPUT);

 

   pinMode(en1,OUTPUT);
  pinMode(en2,OUTPUT);  
 
}

void pointWheel() {
  for (pos = 45; pos <= 130; pos +=15   ) {           // goes from 0 degrees to 180 degrees
                                                   // in steps of 1 degree
    myservo.write(pos); 
      digitalWrite(trigPin,LOW);
      delayMicroseconds(20);
      digitalWrite(trigPin,HIGH);
      delayMicroseconds(20);
      digitalWrite(trigPin,LOW);  
       travelTime=pulseIn(echoPin,HIGH);
       if(pos==0)
       {
        tmax=travelTime;
        point=0;
       }
       
       if(tmax<=travelTime && pos!=0)
       {
        tmax=travelTime;
        point=pos;
        digitalWrite(ledPin,HIGH);
        delay(1000);
         digitalWrite(ledPin,LOW);
       }
       
      Serial.println(travelTime); 
      Serial.println("MAX TRAVEL TITME IS");
      Serial.println(tmax); 
       Serial.println("AND THR POINTIMG NO IS"); 
        Serial.println(point);                       // tell servo to go to position in variable 'pos'
       delay(15);                                   // waits 15ms for the servo to reach the position
  }
   Serial.println("MAX TRAVEL TITME IS");
      Serial.println(tmax); 
       Serial.println("AND THe POINTIMG NO IS"); 
        Serial.println(point); 
        delay(1000);
        myservo.write(point);
        digitalWrite(greenLed,HIGH);
         delay(5000);
          digitalWrite(greenLed,LOW);
}

int checkObstacle() {

  

 digitalWrite(ledPin,LOW);
 digitalWrite(trigPin,LOW);
 delayMicroseconds(20);
 digitalWrite(trigPin,HIGH);
 delayMicroseconds(20);
 digitalWrite(trigPin,LOW);
 od=pulseIn(echoPin,HIGH);
 Serial.println(od);
 delay(25);
 if(od<=1000)
 {
  
  digitalWrite(ledPin,HIGH);
  
  }
  return od;
  
 
  }
 void moveforward() {

  digitalWrite(en1,HIGH);
  digitalWrite(en2,LOW);

}
void movereverse() {

  digitalWrite(en1,LOW);
  digitalWrite(en2,HIGH);

}
void idealPoint()
{
 myservo.write(90); 
}

void stopBot() {

  digitalWrite(en1,LOW);
  digitalWrite(en2,LOW);

}

void loop(){
    moveforward();
    o=checkObstacle();
    if(o<=1000)
      {
       
        stopBot();
        pointWheel();
        moveforward();
        delay(100);
        stopBot();
        idealPoint();
        
     }
 

}
