---
title: "Ice Fishing Robot Project"
date: 2022-01-08 10:00:00 +0200
categories: [robotics, projects]
tags: [arduino, robotics, outdoor, automation]
image: /assets/img/icerobot.jpg  # Add your cover image here
---

# Autonomous Ice Fishing Robot

## Project Overview
Built an ice fishing robot capable of:

- Lower the bait to predetermined depth.
- Perform jigging sequence.
- Detect fish bites using load cell.
- Reel fish in.
- Measure fish weight.
- Turn fish away from hole.



## Components
- Arduino Nano
- 2 Nema 17 stepper motors
- HX711 load cell
- A4988 Stepper drivers
- Piezo buzzer
- Button
- Makita battery
- Power bank
- Custom 3D printed parts
- Fishing gear

![Electronics](/assets/img/icerobot_closeup_electronics.jpg)
*Field testing in -10°C conditions*

##Learning
This was my biggest project at the time and i leanred a lot in the process.
I learned about topics like stepper motor usage, stepper driver issues, load cell calibraton, CAD modeling. 
One of the big challenges was to have something strong enough to mount the motors shaft so the system could rotate to rotate as i did not have couplers available. I solved this by designing a custom part that attached the motor to the legs of the tripod.


## Publicity
I was awarded the Science diploma "Sammon Siru 2022" of my upper secondary school for this project.


![CAD](/assets/img/icerobot_cad.jpg)
*The robot modeled in Fusion 360*




## Video Demonstration of lowering the bait.
{% include embed/youtube.html id='b-0ppkjH3dU' %}


### Code
This has not been made pretty to look at it just is what it is.

```cpp
#include "HX711.h"

#define DOUT  3
#define CLK  2
HX711 scale;

float calibration_factor = 970;

#define NAPPI 11
float aloita = 0 ;

#define BAUD (9600)
#define DIR_PIN1 4
#define STEP_PIN1 5         //nosto
#define ENA_PIN1 6
#define DIR_PIN2 8
#define STEP_PIN2 9        // kääntö
#define ENA_PIN2 10
#define BUZZER 12
const int steps_rev=200;
float howdeep=0;


void Nosto(float revs,int dir,int nopeus){
  float steps=revs*200;

  if (dir==1){
    digitalWrite(DIR_PIN1, HIGH);
    Serial.println("nostetaan...");
  }
  else if (dir == 0){
    digitalWrite(DIR_PIN1, LOW);                                                                                   // 1 = ylöspäin, 0 = alaspäin
    Serial.println("lasketaan...");
  }
  for (int i=0; i < steps; i++){
    digitalWrite(STEP_PIN1,HIGH);
    delayMicroseconds(nopeus);
    digitalWrite(STEP_PIN1,LOW);
    delayMicroseconds(nopeus);
  }
}
  void Kaanto(float aste,int dir,int nopeus){
    float kerta = aste/1.8;
    if (dir==1){  
      digitalWrite(DIR_PIN2, HIGH);
    }
    else if (dir == 0){
      digitalWrite(DIR_PIN2, LOW);
    }
    for (int i=0; i < kerta; i++){
      digitalWrite(STEP_PIN2,HIGH);
      delayMicroseconds(nopeus);
      digitalWrite(STEP_PIN2,LOW);
      delayMicroseconds(nopeus);
    }
  }

     
	//delay(4000);
  
void setup() {
  Serial.begin(BAUD);
  pinMode(NAPPI,INPUT); // Nappi
  pinMode(ENA_PIN1,OUTPUT); // Enable
  pinMode(STEP_PIN1,OUTPUT); // Step
  pinMode(DIR_PIN1,OUTPUT); // Dir
  digitalWrite(ENA_PIN1,LOW); // Set Enable low
  pinMode(ENA_PIN2,OUTPUT); // Enable
  pinMode(STEP_PIN2,OUTPUT); // Step
  pinMode(DIR_PIN2,OUTPUT); // Dir
  digitalWrite(ENA_PIN2,LOW); // Set Enable low
  pinMode(BUZZER, OUTPUT); // Buzzer
  scale.begin(DOUT, CLK);
  scale.set_scale(calibration_factor);
  scale.tare();
  Serial.print("setup done");
}

void loop() {
  while (aloita == 0){
  Serial.println("waiting for button");                                       //button
	if (digitalRead(NAPPI)==HIGH){
		aloita = 1;
	}
  }
  
  while(aloita == 1){ 
    Serial.println("prööt");
    tone(BUZZER, 300, 100);
    delay(150);
    tone(BUZZER, 300, 100);
    delay(150);                                                                  //startup sound
    tone(BUZZER, 300, 100);
    delay(150);
    tone(BUZZER, 500, 500);
    
    Serial.println("initiating");
    delay(2000);


    
	if(howdeep == 2){                                                          //sen jälkee ku irrottanu kalan ni siirtää takas aukon päälle
		Nosto(2,1,2000);
	}
    Kaanto(90,1,9000); // Käännetään siima aukon päälle
    delay(1000);
    Serial.print(scale.get_units(), 2);
    Serial.print(" rammaa\n");
    
    while (scale.get_units()>=-1){
      Serial.print(scale.get_units(), 2);                                     //laskee alas kunnes sensori osuu pohjaa
      Serial.print(" rammaa\n");
      Nosto(0.5,0,3000);// 1 = high, muut = low
      howdeep=howdeep+0.5;
    }
    Nosto(1,1,2000);
    howdeep=howdeep-1;
    
    //digitalWrite(ENA_PIN2,LOW); // Set Enable low  aijd;oiafjoiawjfoi[awjfdio[
    //Serial.println("/////LOOOOW////");

    
    for (int i=0;i<6;i++){                                                     //back and forth jigging

      Nosto(0.25,1,2000);
      howdeep=howdeep-0.25;
      
      if (scale.get_units()>10){
        
        //digitalWrite(ENA_PIN2,HIGH); // Set Enable HGIH
        //Serial.println("/////HIIIIGH////");
        
        Nosto(howdeep,1,2000);
		howdeep = 0;                                                                 //pulling fish up 
        Kaanto(90,0,9000);
        aloita = 0;
        i = 6;
        Serial.println("//////////////////////////////////"); 
        Serial.println("KALAKALAKALAKALA!!!!!!");
        Serial.print("Paino: ");  
        Serial.print(scale.get_units());
        Serial.println(" g");
        Serial.println("//////////////////////////////////");
        
		Nosto(2,0,2000);
		howdeep = 2;

    Serial.println("prööt");
    tone(BUZZER, 600, 100); 
    delay(300);
    tone(BUZZER, 600, 100);
    delay(120);
    tone(BUZZER, 600, 100);
    delay(120);                                                                   //celebration sound
    tone(BUZZER, 600, 100);
    delay(250);
    tone(BUZZER, 600, 100);
    delay(250);
    tone(BUZZER, 1000, 850);
    delay(2000);
      
      }
      delay(1000);
      
      
      if (i==5){
        Nosto(5*0.25,0,2000);
        i=0;
        howdeep=howdeep+1.25;
		if (scale.get_units()>10){
			Nosto(howdeep*1,1,2000);
			howdeep = 0;
			Kaanto(90,0,9000);
			aloita = 0;
			i = 6;
			Serial.println("Kala kaannetty onnistuneesti!! \n");
			Nosto(2,0,2000);
			howdeep = 2;
        }
        }
      
		 }
        //delay(1000); 
      }
    }
```
