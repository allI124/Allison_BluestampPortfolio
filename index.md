Self Driving Car
Replace this text with a brief description (2-3 sentences) of your project. This description should draw the reader in and make them interested in what you've built. You can include what the biggest challenges, takeaways, and triumphs from completing the project were. As you complete your portfolio, remember your audience is less familiar than you are with all that your project entails!

| **Engineer** | **School** | **Area of Interest** | **Grade** |
|:--:|:--:|:--:|:--:|
| Allison L | St. Ignatius College Preparatory | Mechanical Engineering | Incoming Sophmore


![Headstone Image](logo.svg)
  
# Final Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/F7M7imOVGug" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>

For your final milestone, explain the outcome of your project. Key details to include are:
- What you've accomplished since your previous milestone
- What your biggest challenges and triumphs were at BSE
- A summary of key topics you learned about
- What you hope to learn in the future after everything you've learned at BSE



# Second Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/ooFm7ke5JDY?si=H7RZNkHNrPo6AQaj" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

For my second Milestone, I added an LCD screen and LEDs to my self driving car. The LCD screen will say whether the robot is moving forwards, backwards, or backing left or right. I started off by trying to get the LCD to display some text and then I changed that code so that the screen would say what direction the robot is going (forward, backwards) For the LEDs, they blink when the robot is going backwards When the car is backing left the blue LED would blink, when the robot is backing right the red LED will blink, and while going backwards both LEDs blink 

# First Milestone

<iframe width="560" height="315" src="https://www.youtube.com/embed/hiCx63Lz9fU?si=V7Ab60oo49iPlXfh" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe>

My first milestone was assembling and adding code for my self driving car. While building the car I came up to a few challenges consiting with wireing. After checking and rewireing I fixed the issue where a wire was put into the wrong place. Now my car can move and avoid obstacles


# Schematics 
Here's where you'll put images of your schematics. [Tinkercad](https://www.tinkercad.com/blog/official-guide-to-tinkercad-circuits) and [Fritzing](https://fritzing.org/learning/) are both great resoruces to create professional schematic diagrams, though BSE recommends Tinkercad becuase it can be done easily and for free in the browser. 

# Code

```c++
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x27, 16, 2); // I2C address 0x27, 16 column and 2 rows

#define led_pin 12
#define led_pin 11

int buzzer = 13;

const int A_1B = 5;
const int A_1A = 6;
const int B_1B = 10;
const int B_1A = 9;

const int echoPin = 4;
const int trigPin = 3;

const int rightIR = 7;
const int leftIR = 8;

float readSensorData() {
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  float distance = pulseIn(echoPin, HIGH) / 58.00; //Equivalent to (340m/s*1us)/2
  return distance;
}

void moveForward(int speed) {
  analogWrite(A_1B, 0);
  analogWrite(A_1A, speed);
  analogWrite(B_1B, speed);
  analogWrite(B_1A, 0);

   lcd.clear();
  lcd.setCursor(3, 0); 
  lcd.print("Going");
  
  lcd.setCursor(2, 1); // set the cursor to column 2, line 1
  lcd.print("forwards");
    
  digitalWrite(11,HIGH);
  digitalWrite(12,HIGH);
}

void moveBackward(int speed) {
  analogWrite(A_1B, speed);
  analogWrite(A_1A, 0);
  analogWrite(B_1B, 0);
  analogWrite(B_1A, speed);

   lcd.clear();
  lcd.setCursor(3, 0); 
  lcd.print("Backing");
  lcd.setCursor(2, 1); // set the cursor to column 2, line 1
  lcd.print("Up");

  digitalWrite(11,HIGH);
  digitalWrite(12,HIGH);
  delay(1000);
  digitalWrite(11,LOW);
  digitalWrite(12,LOW);
  delay(1000);
  
}


void backLeft(int speed) {
  analogWrite(A_1B, speed);
  analogWrite(A_1A, 0);
  analogWrite(B_1B, 0);
  analogWrite(B_1A, 0);

   lcd.clear();
  lcd.setCursor(3, 0); 
  lcd.print("Backing");
  lcd.setCursor(2, 1); // set the cursor to column 2, line 1
  lcd.print("Right");

  digitalWrite(11,LOW);
  digitalWrite(12,HIGH);
  delay(1000);
  digitalWrite(11,HIGH);
  digitalWrite(12,HIGH);
  delay(1000);
  
  digitalWrite(11,HIGH);
  digitalWrite(12,LOW);
  delay(1000);
  digitalWrite(11,HIGH);
  digitalWrite(12,HIGH);
  delay(1000);
}

void backRight(int speed) {
  analogWrite(A_1B, 0);
  analogWrite(A_1A, 0);
  analogWrite(B_1B, 0);
  analogWrite(B_1A, speed);

   lcd.clear();
  lcd.setCursor(3, 0); 
  lcd.print("Backing");
  lcd.setCursor(2, 1); // set the cursor to column 2, line 1
  lcd.print("Left");

  digitalWrite(11,LOW);
  digitalWrite(12,HIGH);
  delay(1000);
  digitalWrite(11,HIGH);
  digitalWrite(12,HIGH);
  delay(1000);
}

void stopMove() {
  analogWrite(A_1B, 0);
  analogWrite(A_1A, 0);
  analogWrite(B_1B, 0);
  analogWrite(B_1A, 0);

   lcd.clear();
  lcd.setCursor(3, 0); 
  lcd.print("Obstacle");
  lcd.setCursor(2, 1); // set the cursor to column 2, line 1
  lcd.print("Detected");
}

void setup() 
{
  Serial.begin(9600);

  //motor
  pinMode(A_1B, OUTPUT);
  pinMode(A_1A, OUTPUT);
  pinMode(B_1B, OUTPUT);
  pinMode(B_1A, OUTPUT);

  //ultrasonic
  pinMode(echoPin, INPUT);
  pinMode(trigPin, OUTPUT);

  //IR obstacle
  pinMode(leftIR, INPUT);
  pinMode(rightIR, INPUT);

lcd.init(); // initialize the lcd
  lcd.backlight();

pinMode(11, OUTPUT);
pinMode(12,OUTPUT);

pinMode(buzzer,OUTPUT);//initialize the buzzer pin as an output

}

void loop() 
{

  int left = digitalRead(leftIR);  // 0: Obstructed   1: Empty
  int right = digitalRead(rightIR);

  if (!left && right) 
  {
    backRight(150);  
  } else if (left && !right) {
    backLeft(150);  
  } else if (!left && !right) {
    moveBackward(150);
  } else {
    float distance = readSensorData();
    Serial.println(distance);
    if (distance > 50) { // Safe
      moveBackward(200);
    } else if (distance < 10 && distance > 2) { // Attention
      moveBackward(200);
      delay(1000);
      backLeft(150);
      delay(500);
      backRight(150);
    } else {
      moveForward(150); 
    }
  }
}
```

# Bill of Materials

| **Part** | **Note** | **Price** | **Link** |
|:--:|:--:|:--:|:--:|
| Sunfounder Kit | Parts to build the robot | $69.99 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6](https://www.amazon.com/SunFounder-Compatible-Tutorials-Including-Controller/dp/B0B778L1DZ/ref=sr_1_1?crid=3CWLUR2WOCT3T&dib=eyJ2IjoiMSJ9.AUt0K_vpp6TA093ZGDL86jVpAz_sy1xEIXv5txiUwz4qXvrnulxGWVN9Cj_a_lTwXDb9AAaLhGIu0MQb9cTQYPDOR-9PZXRM0X5-kyYSMZixhlGrdboHS7QBoMJ7b7-4eHUFpI3MAPrW7kSiUweI6nw3ulo6tK-9PMiDV_inQyFRD0TT_TO6heUklP4nIgjw_bjDyQY-5cwW1HtOFQQ87A.KZY4UI3Svjoil9SXh0mumelybhWMKcgOXKV5OHTn74E&dib_tag=se&keywords=sunfounder+ultimate+starter+kit&qid=1721836007&sprefix=sunfounder%2Caps%2C146&sr=8-1)/"> Link </a> |
| Motors | Makes the robot move | $10.99 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6](https://www.amazon.com/AEDIKO-Motor-Gearbox-200RPM-Ratio/dp/B09N6NXP4H/ref=sr_1_4?crid=1JP29NIWBLH2M&dib=eyJ2IjoiMSJ9.Wq3jKgOLbqtEP772vMD4pV5f-w3PLBdEpKqguykXOb0JFO14f4Dq0m_VDVUMUFtR8WFINUEticI3GXcoGqwXPqK9yIh04PhCktgccMz9zAUiKXMJPwmOTUp_6av3XuFD0lXo9WngN9iKI6YgZrhEEs9qnqbcB1GnvgntCdKz8Q1dFuNu61NgSE6Z8vBk3FRpaNcr1lCI7FApTiNi0Qce8gbfmMn6oUggZQHpIOKKZ6s.M7WsZ_ZZtm3rm93kKgw0NOxt1McVBYX6m55oGxu1xxI&dib_tag=se&keywords=dc+motor+with+gearbox&qid=1715911706&sprefix=dc+motor+with+gearbox%2Caps%2C126&sr=8-4)/"> Link </a> |
| 9V Batteries | Powers the robot | $12.36 | <a href="[https://www.amazon.com/Arduino-A000066-ARDUINO-UNO-R3/dp/B008GRTSV6](https://www.amazon.com/Amazon-Basics-Performance-All-Purpose-Batteries/dp/B00MH4QM1S/ref=sr_1_5_pp?crid=3TQ7ANPH958JM&dib=eyJ2IjoiMSJ9.bmcV2Upj_vpB6G9CFlPPxYAryat512da7ekZjc52HecXSTmtx7PbJ50EgQFPCMqlAxjOUq-tL4vQTpozlHvH89bMwx-HJoyGcdz6EY8HrMxahTiqOXkoP7ewkDcgHoMhmHamdlQfW6FBHO0Gm-DYZZnnMuvEU3qOpemA8PGEvRhEx4-lGaBZhrvls039G1-9SizAW-YRGXZ2fFrdVDlREyyOhAuxXZaE5QqUxWesRQgP9UfGOYaInRWTTPwhDbXFa-RPzGbU1C_u4wq-NMqKBtWEQqR9-cA8O3FYOx3icEY.dtKJmI2T-iCmMM_bYnbiHUWzhKpJDRxS-bBmZIwYFKM&dib_tag=se&keywords=9v+batteries&qid=1720651326&rdc=1&s=electronics&sprefix=9v+batteries%2Celectronics%2C105&sr=1-5)/"> Link </a> |

# Other Resources/Examples
One of the best parts about Github is that you can view how other people set up their own work. Here are some past BSE portfolios that are awesome examples. You can view how they set up their portfolio, and you can view their index.md files to understand how they implemented different portfolio components.
- [Example 1](https://trashytuber.github.io/YimingJiaBlueStamp/)
- [Example 2](https://sviatil0.github.io/Sviatoslav_BSE/)
- [Example 3](https://arneshkumar.github.io/arneshbluestamp/)

To watch the BSE tutorial on how to create a portfolio, click here.
