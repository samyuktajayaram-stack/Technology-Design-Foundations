# Technology-Design-Foundations :robot: :hammer_and_wrench:
A repository of my explorations as part of the 'Technology Design Foundations' course

## Table of Contents 
[Week 1 - Electronics](#week-1---electronics)

[Week 1 - Fabrication](#week-1---fabrication)

[Week 2 - Electronics](#week-2---electronics)

[Week 2 - Fabrication](#week-2---fabrication)

[Week 3 - Electronics](#week-3---electronics)

[Week 3 - Fabrication](#week-3---fabrication)

[Week 4 - Electronics](#week-4---electronics)

[Week 4 - Fabrication](#week-4---fabrication)

[Week 5 - Electronics](#week-5---electronics)

[Week 5 - Fabrication](#week-5---fabrication)

[Week 6 - Electronics](#week-6---electronics)

[Week 6 - Fabrication](#week-6---fabrication)

[Week 7 - Electronics](#week-7---electronics)

[Week 7 - Fabrication](#week-7---fabrication)

[Week 8 - Electronics](#week-8---electronics)

[Week 8 - Fabrication](#week-8---fabrication)

[Week 9 - Electronics](#week-9---electronics)

[Week 9 - Fabrication](#week-9---fabrication)

[Week 10 - Electronics](#week-10---electronics)

[Week 10 - Fabrication](#week-10---fabrication)

[Week 11 - Electronics](#week-11---electronics)

[Week 11 - Fabrication](#week-11---fabrication)

[Week 12 - Electronics](#week-12---electronics)

[Week 12 - Fabrication](#week-12---fabrication)

[Week 13 - Electronics](#week-13---electronics)

[Week 13 - Fabrication](#week-13---fabrication)

[Week 14 - Electronics](#week-14---electronics)

[Week 14 - Fabrication](#week-14---fabrication)


# Week 1 - Electronics 
_Tuesday, 09/02/2025 - Tuesday, 09/09/2025_

I spent week one brushing up on my basics again. I have worked with an Arduino before, but I felt like I lacked some of the foundational understanding of **HOW** it worked and what to do if it didn't.

### <ins>'Hello World' + Blinking LED<ins>

Based on our experimentation with the Arduino 'Hello World' program on Tuesday, we were tasked with merging the program with the 'LED Blink' code. The objective was for the serial monitor to output 'Hello World' while simultaneously having the LED on the Arduino blink.

I came into this thinking it would be an easy task to complete (considering we had both of the source code files available), but to my surprise, I ran into a couple of issues along the way.

At first, this is how I merged the code:

```C++
int led = 13;
void setup() {
Serial.begin(9600);
pinMode(led, OUTPUT);
}

void loop(){
Serial.println("Hello, world!");
delay(1000);
}

void loop() {
  digitalWrite(led, HIGH);   
  delay(1000);                      
  digitalWrite(led, LOW);    
  delay(1000);                       
}
```

And this was the error message that popped up on my IDE:


<img width="838" height="242" alt="Screenshot 2025-09-06 at 11 12 02 AM" src="https://github.com/user-attachments/assets/92db25a7-68b5-4054-8071-76c41442366c" />


Now looking back at this code, the error is pretty obvious. There were two loop functions, when there should have only been one. When I was merging the code, I figured that since they were separate functions (displaying 'Hello World' and blinking on the LED) that they needed separate loops to run.


I troubleshooted on Chatgpt and got this resposne. I then changed the code and merged the two loop functions. 


<img width="764" height="125" alt="Screenshot 2025-09-06 at 11 14 34 AM" src="https://github.com/user-attachments/assets/6fc3197f-21f9-49dc-a29d-997269ffa38d" />


After digging through Reddit, Geeks for Geeks and ChatGPT, I learnt that when there are 2 loop() functions, the compiler doesn't know which one to run which results in an error being thrown.

**After the code had been updated, the program displayed 'Hello World' and blinked at the same time!**

But in true programming fashion, there was another problem to fix. Every so often, the serial monitor would display the incorrect word. So, instead of saying 'Hello World', it would sometimes just say 'Hell!'. Albeit pretty hilarious, it still needed to be rectified. 

<img width="147" height="167" alt="Screenshot 2025-09-05 at 9 17 14 PM" src="https://github.com/user-attachments/assets/90cfae2a-3ed8-4ee7-813e-b17aba865e3f" />

At first I thought that maybe the baud rate in the code wasn't matching the one on the serial monitor. But the two were set to the same rate. After a quick search, I found that the issue could be that it was printing the word too frequently which was overflowing the serial monitor. I changed the delay to 500, and this seemed to do the trick. 

<img width="260" height="171" alt="Screenshot 2025-09-05 at 9 12 46 PM" src="https://github.com/user-attachments/assets/7c75ca0c-538b-4f90-86a0-d4f7b2abf79c" />

This was my final code:
```C++
/*
  Hello World
*/
int led = 13;
void setup() {
//initialize serial communications at 9600 baud rate
Serial.begin(9600);
pinMode(led, OUTPUT);
}

void loop(){
//send 'Hello, world!' over the serial port
Serial.println("Hello, world!");
//wait 100 milliseconds so we don't drive ourselves crazy
delay(500);
 digitalWrite(led, HIGH);   // turn the LED on (HIGH is the voltage level)
  delay(500);                       // wait for a second
  digitalWrite(led, LOW);    // turn the LED off by making the voltage LOW
  delay(500);                       // wait for a second
}
```

Click on the image to watch the final video of the model:

<a href="https://youtube.com/shorts/bj5K1jneAmc">
  <img src="https://github.com/user-attachments/assets/a3b70ca2-1951-4b25-971c-1c9146c1f529" alt="Blinking LED + Hello World Demo" width="500"/>
</a>


Seeing the program run (and actaully do it correctly after all that troubleshooting), was awesome. I decided to experiment with an external LED light next. 

### <ins>Blinking external LED<ins>

I began by first figuring out which leg of the LED was the anode and which was the cathode. I didn't want to shortcirucit my LED on the first try. I found that the longer leg was the anode (+ve) and the cathode was the shorter leg (-ve). 

After this, I needed to find the value of the resistors in my kit. I found a great tool online that helped me with this. I plugged in the band colours, and it gave me the ohm value of the resistor. I didn't end up needing to calculate the value myself. 

<img width="443" height="642" alt="Screenshot 2025-09-06 at 12 54 57 PM" src="https://github.com/user-attachments/assets/d5ca37da-9f9d-4390-b74e-467540020893" />

[Resistor Calculator](https://www.calculator.net/resistor-calculator.html?bandnum=4&band1=brown&band2=black&band3=blue&multiplier=orange&tolerance=gold&temperatureCoefficient=brown&type=c&x=Calculate)


I then created the actual circuit using the Arduino Uno, 2 jumper cables (male to male), the resistor (330 ohm) and a green LED. I followed the circuit diagram provided in Sudhu's repository, and plugged in the source code. I then hit upload and watched the magic happen!

Here's a video of the program running and an image of the circuit: 

https://github.com/user-attachments/assets/3ef50555-c3d8-49e9-ba3a-f3de25550d79

![IMG_3663](https://github.com/user-attachments/assets/43089fd2-3dfb-4339-9e5c-422d47a0eb96)

I then noticed that I had an **HC - SR04 Ultrasonic Sensor** in my kit. I decided to try to see if I could create a circuit where the LED would only flash when something came close to the Ultrasonic Sensor. The next section details my exploration with building that circuit. 

### <ins>Flashing LED using the HC - SR04 Ultrasonic Sensor<ins> 

I used these project files as reference:
1. [DIY Motion-Activated LED with Arduino & Ultrasonic Sensor](https://www.hackster.io/computeservicestechnology/diy-motion-activated-led-with-arduino-ultrasonic-sensor-cd0fee)
2. [Arduino Ultrasonic Sensor & LED](https://www.youtube.com/watch?v=9oTX20deOJs)

I began by first trying to understand what the various pins on the HC - SR04 sensor meant. There were 4 pins - GND, Echo, Trig, VCC. GND was ground, Echo was the output pin, Trig was the input pin and VCC was for power supply. 

I then began connecting the sensor, the LED and the resistor on a breadboard. This was then connected to the Arduino using jumper cables.
1. GND -> Ground
2. VCC -> 5V
3. Trig -> Digital Pin 9
4. Echo -> Digital Pin 10
5. Anode of the LED -> Digital Pin 8
6. Cathode of the LED -> GND

Below is a circuit diagram I made on TinkerCAD to simulate the model before:

<img width="700" height="618" alt="Screenshot 2025-09-06 at 2 29 39 PM" src="https://github.com/user-attachments/assets/b288a7e2-8da3-4a62-b6db-d3c4f6ed588b" />

[HC-SR04 + LED Tinkercad Project](https://www.tinkercad.com/things/1dZ03uLVOUf-hc-sr04-led?sharecode=3l7n070TypRqj6ZrWTR8lr-tbEiIGrffgCdBVhNSTYA)

I then plugged in the code ([DIY Motion-Activated LED with Arduino & Ultrasonic Sensor](https://www.hackster.io/computeservicestechnology/diy-motion-activated-led-with-arduino-ultrasonic-sensor-cd0fee)), and uploaded the sketch. I was a litte worried that I may casue the LED to short (or even worse, fry the Arduino itself), but the outcome worked just as it was supposed to!

Here is the code:

```C++
// Define pin connections
const int trigPin = 9;
const int echoPin = 10;
const int ledPin = 8;

// Set a distance threshold for detecting movement (in cm)
const int movementThreshold = 20; // Adjust this to your desired distance

void setup() {
  // Initialize pin modes
  pinMode(trigPin, OUTPUT);
  pinMode(echoPin, INPUT);
  pinMode(ledPin, OUTPUT);

  // Start Serial Monitor for debugging
  Serial.begin(9600);
  Serial.println("Starting distance sensor...");
}

void loop() {
  // Trigger the sensor to send out a pulse
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);

  // Measure the duration of the pulse
  long duration = pulseIn(echoPin, HIGH);

  // Calculate distance (in cm)
  int distance = duration * 0.034 / 2;

  // Print distance to Serial Monitor for debugging
  Serial.print("Distance: ");
  Serial.print(distance);
  Serial.println(" cm");

  // Check if an object is within the threshold distance
  if (distance > 0 && distance <= movementThreshold) {
    Serial.println("Object detected within range. Turning LED on.");
    digitalWrite(ledPin, HIGH); // Turn on the LED
  } else {
    Serial.println("No object detected or out of range. Turning LED off.");
    digitalWrite(ledPin, LOW); // Turn off the LED
  }

  // Delay to avoid overwhelming the sensor
  delay(200);
}
```

Click on the image to view the video:

<a href="https://youtu.be/7V5QqX-i0go">
  <img src="https://github.com/user-attachments/assets/99a428ec-ee5d-444b-9c4c-42f77326d82f" 
       alt="Blinking LED + Hello World Demo" 
       width="500"/>
</a>

It was SO cool to see this come to life. I feel like I could keep iterating on this forever (maybe adding a microphone and programming it such that the led only lights up when it picks up on a sound. or even having a row of LED's that light up based on proximity to the Ultrasonic Sensor. The possibilities are endless!)

### <ins>LDR + 2 LED'S<ins>

The next task for week 1 was to experiment with an LDR and multiple LED's. While they didn’t have to be combined, I decided to merge the two prompts and see what I could come up with. I built a circuit where one LED turns on when there’s light, and the other turns on when it’s dark.

I found a great refernece tutortial online - [Arduino Tutorial: Controlling LEDs with LDR Sensor – Hackster.io](https://www.hackster.io/Rabadaba/arduino-tutorial-controlling-leds-with-ldr-sensor-8813c2). I modified this code, for 2 LED's rather than 4. The goal was to have the green LED turn on where there was light and the Blue turn on in the absence of it.

I began by first simulating my code on TinkerCAD. I'm still slightly nervous to test out my program directly on the Arduino, so I always test if things work here first. I connected the circuit on TinkerCAD, and plugged in the code. 

<img width="1223" height="835" alt="Screenshot 2025-09-09 at 8 17 24 AM" src="https://github.com/user-attachments/assets/4cfa0e3d-adfd-462c-8eef-ec8801dae20f" />

[LDR + LED'S Tinkercad Project](https://www.tinkercad.com/things/apzIUwVpRB8/editel?returnTo=%2Fdashboard%2Fdesigns%2Fcircuits)

The program displayed an error and wasn't able to compile. The error message stated **'In function 'void loop()':25:23: error: 'ldrLevel2' was not declared in this scope 25:23: note: suggested alternative: 'ldrlevel2'**. I spent the next 10 minutes trying to figure out why it said ldrlevel2 was not declared when I had explicitly declared it in the beginning. Then after rereading the error message, I realized my mistake. Instead of 'ldrlevel2', I had written 'ldrLevel2' with a capital L. It was the tiniest yet most frustrating error I've had to troubleshoot. 

<img width="602" height="826" alt="Screenshot 2025-09-09 at 8 27 35 AM" src="https://github.com/user-attachments/assets/fbd54bba-5eac-46a6-8bbe-f9788d004167" />

Once the code was running smoothly, I transferred it to the Arduino IDE, and built the actual circuit. I uploaded the sketch and it worked without any errors.

Here is my modified code:

```C++
// variables ( You can change the pins as you wish)
const int ldrPin = A0;    
const int ledPin1 = 12; 
const int ledPin2 = 11; 

int ldrValue = 0;
int ldrlevel1=600; 
int ldrlevel2=750; 


void setup() {
  Serial.begin(9600);    
  pinMode(ledPin1, OUTPUT);
  pinMode(ledPin2, OUTPUT);
}

void loop() {
  ldrValue = analogRead(ldrPin); 
  Serial.println(ldrValue);

  if (ldrValue < ldrlevel1) {
    digitalWrite(ledPin2, HIGH);    //LED on pin 11 is on
     digitalWrite(ledPin1, LOW);   // LED on pin 12 OFF
  }
  else if (ldrValue < ldrlevel2) {
    digitalWrite(ledPin2, LOW);   // LED on pin 11 OFF
    digitalWrite(ledPin1, HIGH);  // LED on pin 12 ON
  }
  else {
    digitalWrite(ledPin1, LOW);   // both led's OFF
    digitalWrite(ledPin2, LOW);
  }
}

```
Click on the image below to watch the video:

[![LDR + LED's circuit using Arduino](https://github.com/user-attachments/assets/fc7f93d6-402d-4b90-b6dd-1e3748cf020b)](https://youtu.be/nao-wBUOpL8)

I really enjoyed this process (even the troubleshooting). It's always quite fufilling to see your end product come to life, even if it took a bit to get there. 

**Reflection**: Now that I have gotten the chance to refamiliarize myself with the basics of the Arduino and have learned how to connect it to an LED or a sensor to generate specific outputs, I want to experiment with mishmashing code from different sources to see how it behaves. I also want to practice troubleshooting based on the error messages produced by the Arduino IDE, learning the types of errors that can occur and how to pinpoint the exact issues in my code.

Towards my last exploration, I felt like I was starting to get a hang of what certain parts of the code meant and how it changed the overall output. There were also some great online resources that I used to help figure things out along the way. I still have a way to go, but I'm starting to feel much more confident than I was before with physical computing.

# Week 1 - Fabrication 
_Thursday, 09/04/2025 - Thursday, 09/11/2025_

This week was all about learning to use the laser cutter. During my undergraduate program, I had the chance to use the laser cutter a few times but the the machine was always operated by a design specialist. We were only allowed to watch how the file got set up for a cut, as opposed to cutting it ourselves. This was the first time I was going to be operating the machine myself. 

Our task for week 1 was to design a ring that represented a part of our personality or a cause we believed in. I love hiking and being in nature, and I have a habit of picking up flowers and leaves I find on the ground along my trail. I wanted to design a ring that could come with me on my hikes and give me a small place to keep those flowers and leaves.

For my second ring experiment, I wanted to represent my love for all things stitching, sewing, and embroidery. I was curious whether it would even be possible to embroider onto laser-cut wood, and that question became the starting point for this design.

I began by first sketching out all my desigs on a rough sheet of paper. Once I had finalized my general form, I took these sketches to Illustrator to create the final print file. 

<img width="662" height="795" alt="Screenshot 2025-09-10 at 10 09 38 PM" src="https://github.com/user-attachments/assets/221e40c5-17c8-4750-b6d9-1cf788376567" />

This was my inspiration for the second ring:

<img width="499" height="604" alt="Screenshot 2025-09-09 at 9 13 29 PM" src="https://github.com/user-attachments/assets/108c0b1b-93b5-4948-99ee-3974a3c5b89f" />

Illustrator was pretty intuitive for the most part, but I really struggled with drawing arcs for the top and botton of my ring. I couldn't get the arc to be perfectly circular, and it wouldn't curve out all the way to meet the other end of the ring. I then tried to just draw a circle and subtract half of the path to make an arc, and that too was not working. I troubleshooted on chatGPT and also searched up references and tutorials online to try to pinpoint the issue, but nothing seemed to work. I finally resigned to just making the botton of the ring square shaped. 

<img width="473" height="669" alt="Screenshot 2025-09-08 at 7 50 19 PM" src="https://github.com/user-attachments/assets/d82bcacf-5914-440a-9a16-3d05706526cf" />

I finished sizing and drawing out both ring files (and made sure to measure my ring size so it would fit), and went in to the studio to cut my rings. I came into this process thinking that my rings would be perfect after my first cut, but that was far from what happened (Ironically, this seems to be a recurring theme throghout my journal entries so far. I'm beginning to think I need to downplay some of my abilities).

Setting up the first print took a bit of work. I followed all the steps I remembered from Chris's hands-on training last week, but for some reason the laser wouldn't move. After trying for a few minutes, I was told I would have to turn on the machine first. I seemed to have skipped the most crucial first step. After the laser cutter was running, I set the nozzle to the start position, added the material type and thickness and checked if the print would fit on my plywood piece. I then pressed start and watched the cut begin! 

As I started cutting my rings, I began noticing several dimension errors. The square slots I had designed for the nature-inspired ring were too small and slipped through the corrugated laser cutter base, and the main frame of the ring turned out thinner than I intended. In the pegboard ring, the holes were far too tiny to thread a needle through. For both rings, the ring size was also slightly too large. On Illustrator, the rings appeared to have the correct measurements, but once I actually cut them, I realized the dimensions were different in reality. I went back and revised my files to correct these issues.

Here are some pictures of the first cut:

<img width="673" height="627" alt="Screenshot 2025-09-10 at 11 05 32 PM" src="https://github.com/user-attachments/assets/1a35fc26-bb31-4175-801b-54b8a971e7df" />

<img width="680" height="896" alt="Screenshot 2025-09-10 at 11 06 14 PM" src="https://github.com/user-attachments/assets/d0b72711-fb54-4970-8d82-f5e52d9a62b6" />

<img width="692" height="676" alt="Screenshot 2025-09-10 at 11 06 35 PM" src="https://github.com/user-attachments/assets/f9494784-b900-4daf-a721-b73e77521986" />

![PHOTO-2025-09-10-23-54-44](https://github.com/user-attachments/assets/42bc0a63-ae41-45ff-a0e0-18e41dcc7839)

For my second cut, I reduced the ring size on both rings. For the nature ring, I increased the thickness of the frame, and upon Chris's suggestion, made the supports like a finger joint. This way, I would be able to print it as one piece and it would also give me ample space to put plants into the slots. For the pegboard ring, I increased the size of the holes. 

I then set up my second file on the laser cutter, and began cutting. This times, there were a few more dimension errors. The supports for the nature ring and the holes on the pegboard were both too large. I went back and reworked on the file. 

Here's a picture and video from the second cut:

https://github.com/user-attachments/assets/7f39e6b4-2f52-4764-9272-fe50a07fcba4

![PHOTO-2025-09-10-23-40-44](https://github.com/user-attachments/assets/c4eedd2e-dcf9-4a31-b066-6c1c2aaefd8c)

On my third attempt, everything was in order. The dimensions, spacing and thickness were all redone and they matched my original vision for the rings. I was hoping third time was the charm, but unfortunately I left the power on high which increased the kerf and changed the dimensions of my cut. I went back to laser cut a fourth time, and changed the power setting before I started. 

Here's a picture and video from the third cut:

https://github.com/user-attachments/assets/a15a16e9-8c37-4231-9136-6c40f4e633de

![PHOTO-2025-09-10-23-50-14](https://github.com/user-attachments/assets/154ff70c-dbc8-4a33-8fee-9daca327a74e)

The fourth time I laser cut my rings, the settings on the machine were correct and all my dimensions came out as intented! It was so cool to see the rings finally come to form. 

Here's video from the fourth cut:

https://github.com/user-attachments/assets/052bc3af-d68d-4b77-8838-f0d1aa82bf07

And a picture of all my interations from 1 through 4:

![PHOTO-2025-09-10-23-55-13](https://github.com/user-attachments/assets/98727cd6-ca79-46fc-ae27-444c1e3ecc21)

Next, I sanded all my pieces down. After that, Chris showed me how to properly glue pieces of wood together using wood glue. I attached the pieces and left it to dry. 

![PHOTO-2025-09-10-23-58-10](https://github.com/user-attachments/assets/f10f7e45-5178-4e97-8be8-a5ec457236e4)

Finally, my rings were complete! I walked outside and picked up some leaves and flowers that had fallen on the ground and placed them in my ring. For my pegboard ring, I wove the words MDes into it. For further iterations of these rings, I plan on adding a support shaft on the side of the nature ring to better hold the plants I place in it. For the pegboard, I would like to place the holes slightly wider apart and have an odd number of holes in each column. 

Here are a few images of my rings:

![PHOTO-2025-09-11-00-02-35](https://github.com/user-attachments/assets/243daa2b-7530-429b-a62f-35d827dbcc6c)

![PHOTO-2025-09-11-00-02-52](https://github.com/user-attachments/assets/16ea0054-0931-4fad-a5a8-93319ae14934)

![PHOTO-2025-09-11-00-03-05](https://github.com/user-attachments/assets/128b0378-b9da-4555-a99c-dea0601e1a88)

![PHOTO-2025-09-11-00-03-21](https://github.com/user-attachments/assets/ea890336-6cb1-46fb-bb35-093b448c011f)

![PHOTO-2025-09-11-00-04-17](https://github.com/user-attachments/assets/92b8ecaa-1ef8-45db-912c-50278a841425)

![PHOTO-2025-09-11-00-04-28](https://github.com/user-attachments/assets/4de54e2d-03e1-4c78-84d2-399809539abb)

**Reflection**: I had so much fun making these rings. Right from getting to know how to use the laser cutting, to revising errors with each iteration and finally seeing the ring take shape was really rewarding. I want to be able to further experiment with this machine by etching and engracing as well as making movable mechanical objects using just parts i cut from the laser cutter. 

# Week 2 - Electronics 
_Tuesday, 09/09/2025 - Tuesday, 09/16/2025_

### <ins>2 LED'S + RGB LED <ins>
For week 2, we began by trying to get 2 LED bulbs to glow at the same time or alternatively on after the other. I connected the circuit using two 330 ohm resistors, two LED lights (red and green) and the Arduino Uno. I then modified the 'Blink' code from Sudhu's Git repository to be used for 2 LED's and then also changed the delay on each bulb so that they would glow at a short interval from each other. 

This was how I modified the code:

```C++
/*
  Blink

  Turns an LED on for one second, then off for one second, repeatedly.

  Most Arduinos have an on-board LED you can control. 
  On the UNO it is attached to digital pin 13

  This example code is modified from.
  https://www.arduino.cc/en/Tutorial/BuiltInExamples/Blink
*/

int led1 = 13;  // define a variable to hold the pin number of the internal LED
int led2 = 9;

// the setup function runs once when you press reset or power the board
void setup() {
  // initialize digital pin LED_BUILTIN as an output.
  pinMode(led1, OUTPUT);
  pinMode(led2, OUTPUT);
}

// the loop function runs over and over again forever
void loop() {
  digitalWrite(led1, HIGH);   // turn the LED on (HIGH is the voltage level)
  delay(1000);                       // wait for a second
  digitalWrite(led1, LOW);    // turn the LED off by making the voltage LOW
  delay(1000);                       // wait for a second
 digitalWrite(led2, HIGH);   // turn the LED on (HIGH is the voltage level)
  delay(500);                       // wait for a second
  digitalWrite(led2, LOW);    // turn the LED off by making the voltage LOW
  delay(500);                       // wait for a second
}
```
I defined the two LED's as separate variables ('led1' and 'led2'). I then added two functions within the loop for both led's separately. This was a great exercise for me to understand at the most basic level, how I can modify code based on my circuit components.

Here is a video of the circuit in action:

https://github.com/user-attachments/assets/e171ac6e-f109-475d-bbfe-c8e6edc2b227

Next, we used the same 'Blink' code on an RGB LED. As I learnt, an RGB LED is different from a regular LED in 2 distinct ways. (1) A regular LED emits light of a single, fixed color, while an RGB LED contains three separate Red, Green, and Blue LEDs within a single unit that can be mixed to produce a wide range of colors (2) a regular LED has 2 legs, while an RGB LED has 4. The 4 legs of the RGB LED are for Red, -ve, Green and Blue. 

We attached the circuit together. Here, the RGB LED needed 3 resistors to limit the current flowing through each internal R, G, and B LED chip independently.  

<img width="1280" height="720" alt="image" src="https://github.com/user-attachments/assets/cd6db362-b06f-4f8a-adfb-f4a2b2362ec8" />

We plugged it into the IDE and this was the outcome:

https://github.com/user-attachments/assets/7d8e3886-b785-4fb1-bfd5-8b238127784f

### <ins> 2 Buttons to Control a Servo Motor <ins>

Out next task was to create a circuit with a button. I used a servo motor which could be controlled with two buttons. The idea was to have the servo move a certain degree based on which button I press. 

I referenced this tutorial to create this circuit: [Controling servo motors with buttons and arduino](https://projecthub.arduino.cc/bruno_opaiva/controling-servo-motors-with-buttons-and-arduino-bcb3b6)

I first started by creating a model on TinkerCAD to simulate my circuit. 

<img width="1086" height="812" alt="Screenshot 2025-09-15 at 9 02 19 PM" src="https://github.com/user-attachments/assets/45ff8de6-cab8-456b-9903-b608206ffac4" />

The code and the circuit worked as intended, so I began replicating it on the breadboard and connected the arduino to the main circuit. Here is the link to the simulation: [Servo motor + buttons ]([https://projecthub.arduino.cc/bruno_opaiva/controling-servo-motors-with-buttons-and-arduino-bcb3b6](https://www.tinkercad.com/things/1CzQAv0Txur/editel?returnTo=%2Fdashboard))

I then plugged in the code into the IDE and voila! Below is a video and pictures of my experimentation. 

https://github.com/user-attachments/assets/c5badf06-2ce0-4e66-96bf-1efe39159c6e

![PHOTO-2025-09-15-21-06-29](https://github.com/user-attachments/assets/83f0ce92-eecc-4359-9c52-d1b65a568465)

![PHOTO-2025-09-15-21-06-29](https://github.com/user-attachments/assets/df60b582-79a3-4de9-ae82-73be3909da46)

Below is the code I used:

```C++

int button = 2;   //pin of the first button
int button1 = 3;  //pin  of the second button
#include<Servo.h> //include the servo library
Servo servo;  //create a servo object
int pos = 0;  //initial position of the servo
void  setup() {
  // put your setup code here, to run once:
  servo.attach(9);  //pin  used by the servo
  pinMode(button, INPUT_PULLUP);  //define first button as  input pullup
  pinMode(button1, INPUT_PULLUP); //define second button as input  pullup
  /*
  INPUT_PULLUP send to arduino LOW signal, so, when you press  the button, you send a LOW signal to arduino
  */
}

void loop() {
  // put your main code here, to run repeatedly:
  if (digitalRead(button) ==  LOW) { //if Value read of the button ==LOW:
    pos++;  //increases the value  of the "pos" variable each time the push button of the left is pressed
    delay(5);  //5 milliseconds of delay
    servo.write(pos); //servo goes to variable pos
  }
  if (digitalRead(button1) == LOW) { //if Value read of the button ==LOW:
    pos--;  //decreases the value of the "pos" variable each time the push button  of the right is pressed
    delay(5); //5 milliseconds of delay
    servo.write(pos);  //servo goes to variable pos
  }
}
```

This exploration was fun! I do have one unanswered question. I did observe that when the circuit was connected to the power source, i.e my laptop, even before I had put the code into the IDE, the servo would spin from side to side for about 10 seconds and then stop. Why does that happen? There was a button in place which I assumed would prevent that from happening unless otherwise pressed?

### <ins> Potentiometer + RGB LED <ins>

Our final task for this week was to create a circuit where an RGB LED light is controlled by a Potentiometer. I had never seen, heard or used a Potentiometer before, so the first step was to understand what it was before I could add it to my circuit. I learnt that a potentiometer is a three-terminal variable resistor that allows for the adjustment of resistance or voltage by moving a wiper along a resistive element. Essentially it could control/adjust the amount of volatage that flows through a circuit. I decided to challenge myself and see if I could use the Potentiometer to essentially adjust the RGB light across a variety of colours. 

I referenced this tutorial for this experiment: [Arduino – Control RGB LED with Potentiometer]([https://projecthub.arduino.cc/bruno_opaiva/controling-servo-motors-with-buttons-and-arduino-bcb3b6](https://roboticsbackend.com/arduino-control-rgb-led-with-potentiometer/))

I began by first creating my simulation on TinkerCAD. I haven't fully built the confidence to build my circuit directly without testing first, but I assume once I get a hang of the basics I will get more comfortable in trying directly with the arduino itself. 

Here is the model on TinkerCAD:

<img width="1042" height="834" alt="Screenshot 2025-09-15 at 9 17 44 PM" src="https://github.com/user-attachments/assets/1c230b77-e807-40d8-af2e-bd6e431748a5" />\

link to the simulation: [Potentiometer](https://www.tinkercad.com/things/2NDs9T8sERg/editel?returnTo=%2Fdashboard)

When I ran the simulation here, at first nothing happened. The RGB light did not glow even though there were no errors popping up from the code. At first, I thought it may have been because I used a higher resistance value. But after taking a quick look at my circuit again, I realized I had forgotted to connect a wire to ground. Even though it was a small error, it was nice that I was able to recognize where I went wrong myself. I added the wire to GND and the circuit worked as I'd wanted it to. 

I then trasferred this to the actual board, and ran the code on the IDE. The resulting experiment on the Arduino was SO COOL to see!!!! It reminded of the remote controlled strip LED lights that are controlled by a remote and can glow in a varity of different shades. 

Here is the code I referenced:

```C++
#define RGB_RED_PIN 11
#define RGB_BLUE_PIN 10
#define RGB_GREEN_PIN 9
#define POTENTIOMETER_PIN A0

void setup()
{
  pinMode(RGB_RED_PIN, OUTPUT);
  pinMode(RGB_BLUE_PIN, OUTPUT);
  pinMode(RGB_GREEN_PIN, OUTPUT);
}

void loop()
{ 
  int potentiometerValue = analogRead(POTENTIOMETER_PIN);
  int rgbValue = map(potentiometerValue, 0, 1023, 0, 1535);

  int red;
  int blue;
  int green;
  
  if (rgbValue < 256) {
    red = 255;
    blue = rgbValue;
    green = 0;
  }
  else if (rgbValue < 512) {
    red = 511 - rgbValue;
    blue = 255;
    green = 0;
  }
  else if (rgbValue < 768) {
    red = 0;
```

Here are some images and a video of the cicuit working:

https://github.com/user-attachments/assets/273f10a4-cdcd-4c9e-812d-2bd11113ff49

![PHOTO-2025-09-15-21-26-08](https://github.com/user-attachments/assets/dc1e01eb-3746-4931-9c49-94eed0fc3667)

![PHOTO-2025-09-15-21-26-08](https://github.com/user-attachments/assets/5aae944f-bcf1-4d10-9f47-fb4df0a1d5bc)

![PHOTO-2025-09-15-21-26-06](https://github.com/user-attachments/assets/dfa3d1d2-5f52-4c28-a007-f31c1614f61a)

![PHOTO-2025-09-15-21-26-07](https://github.com/user-attachments/assets/1f75be81-181e-4e12-ae65-e4b6a9aca520)

![PHOTO-2025-09-15-21-26-07](https://github.com/user-attachments/assets/3ba8a026-7b1a-4c53-a51e-36140758c6ee)

![PHOTO-2025-09-15-21-26-07](https://github.com/user-attachments/assets/8a780698-a09f-4833-99b2-01aedbf8a88a)

![PHOTO-2025-09-15-21-26-07](https://github.com/user-attachments/assets/023d234f-3a53-461e-8d5b-8192c96f94e6)

![PHOTO-2025-09-15-21-26-07](https://github.com/user-attachments/assets/9aaafaec-ab6a-47d3-826b-733e6d169c46)

![PHOTO-2025-09-15-21-26-08](https://github.com/user-attachments/assets/613ae1e8-0898-4945-91ad-6d20f645c574)

![PHOTO-2025-09-15-21-26-08](https://github.com/user-attachments/assets/5ad85ba0-856b-42d3-95af-4ec6503ea239)

![PHOTO-2025-09-15-21-26-08](https://github.com/user-attachments/assets/f35a0c75-7a00-4c38-84ed-b7991006cb4a)

![PHOTO-2025-09-15-21-26-08](https://github.com/user-attachments/assets/5fa9d3e7-4105-4fae-9fa9-41c8ab1fd7c6)

![PHOTO-2025-09-15-21-26-08](https://github.com/user-attachments/assets/f6a4a71e-089f-4e8f-b265-3876b6208b79)

**Reflection**: Overall, these exercises have been really fun to experiment with. I have started to understand how to not only create a circuit where an LED can be controlled by a certain sensor or even a simple on/off conductor like a button, but even how other actuators can be manipulated as well. This has been a week of constant experimentation and learning, and I've thoroughly enjoyed it!

# Week 2 - Fabrication 
_Thursday, 09/11/2025 - Thursday, 09/18/2025_

After week 1 of laser cutting, week two was all about 3D printing. This was the week I was really looking forward to.

Going in, I had the image that this process was going to be really difficult and something that I would'nt be able to pick up easily. I definitely hit some bumps along the way, but I feel much more confident with CAD modelling and 3D printing as a whole. I'm excited to keep testing with different prints. 

We began this process by getting familliarized with Fusion. I had never used fusion before, so this was defintely the biggest learning curve for me (I'm still learning a lot of it). We got a brief overview, and then got right back into making and playing around with rings!

Task 1 for the next set of our rings was a to replicate the same ring we laser cut, but in 3D print form. I imported the SVG file of my ring from illustrator to Fusion, and extruded each individual part. I then combined them all together as one unified body. This was relatively simpler as I did not need to make a whole new ring with new dimensions again. 

<img width="563" height="789" alt="Screenshot 2025-09-26 at 1 05 52 PM" src="https://github.com/user-attachments/assets/fb98bf28-3882-4a5c-aefe-ecc3ea637160" />

I then began on task 2, which was to 3D model and print a whole new ring (without any constraints, so I coul go as big as I wanted!). I decided I wanted to make a knuckle ring that have a small pan flute on the top. I spoke with Cody, the design specialist, and he suggested that I take a look at a pan flute that he had printed using the Prusa. The flute was AWESOME and played the coolest sounds. I wanted to replicate that on ring form. My first model did not work as the holes were too small and the ring was also too tiny to house such large tubes without the print collapsing in on itself. So I tried another approach, and made the base wider. I added larger tubes for the flue, and added holes at the bottom for the sound to be generated out of. I did have some troubles with this model on Fusion like joining without cutting, combining bodies and separating the tube that was inserted as a support through the flutes, but I was able to work it all out with the help of my peers. I then exported that model to the Prusa slicer and sliced it with my previous ring. 

<img width="593" height="809" alt="Screenshot 2025-09-26 at 1 04 10 PM" src="https://github.com/user-attachments/assets/3517bb74-7fee-463d-8787-d2b8389b9852" />
My first ring CAD model

<img width="684" height="750" alt="Screenshot 2025-09-26 at 1 05 06 PM" src="https://github.com/user-attachments/assets/9f6b7366-3312-4db0-89dc-d7db7c71461d" />
the updated ring model. 

After both models had been sliced (which was super cool because I got to see how it would print each layer and also how the whole model was structured from ther inside out), I copied the G-code on to the SD card of the 3D printer and uploaded that on to the machine. I selected my file on the machine and hit start and that was it! I watched the first 2 layers print, and then left it to print by itself. 

<img width="420" height="445" alt="Screenshot 2025-09-26 at 1 08 48 PM" src="https://github.com/user-attachments/assets/e74fc93c-0019-403b-b0cb-b4e5a900efc3" />

https://github.com/user-attachments/assets/4b5c8b1d-6810-4157-8b0e-c1ef7aed4739

https://github.com/user-attachments/assets/5dcd44b7-8921-444e-a76a-a4c7fc16c97c

When I came back (about 3.5 hours later :0), my model had fully printed! I had added organic supports, which were pretty simple to remove with a pair of pliers. My flute ting actually played a tune (which I was half expecting I would have to print again just because of the internal cavities of the object, but it workerd great). My other ring, though it came out looking fine, the sizing was incorrect. The ring was fine on my laser cut file, but slightly too small on my 3D printed outcome. I'm still yet to understand what could have triggered that happening since all the measurements were exact on the SVG. 

![PHOTO-2025-09-26-13-13-11](https://github.com/user-attachments/assets/64c7ee18-cbdb-48d6-ba95-adaf3cd7eee3)

![PHOTO-2025-09-26-13-13-23](https://github.com/user-attachments/assets/45da1fa2-e62c-4a40-ad11-b32ce003bf8e)

Here are some pictures, videos and also an audio of me playing the flute!



![PHOTO-2025-09-26-13-13-50](https://github.com/user-attachments/assets/fc8af247-74f8-44f2-abe2-0e61a6c00474)

![PHOTO-2025-09-26-13-13-52](https://github.com/user-attachments/assets/49aa4eb2-be19-41f5-92ea-41a99ed0e8b7)

![PHOTO-2025-09-26-13-13-55](https://github.com/user-attachments/assets/c96b332a-c595-4769-8c00-96a0b1a5999a)

[Project Media (Google Drive)](https://drive.google.com/file/d/1J1Q-N7qW-AwadvjluC-lGez13lBQBu99/view?usp=sharing)


**Reflection**: I had so much fun working with the 3D printer. It was definitely quite intimidating going into it, but the process was much simpler once I started. I do need a LOT of practice on Fusion before i can start making larger, more complex models. 

# Week 3 - Electronics 
_Tuesday, 09/16/2025 - Tuesday, 09/23/2025_

In week 3, we began class by building a circuit that used an ultrasonic sensor to control a servo motor. This was a relatively simple setup, adapted from the Arduino “Sweep” example code for servo motors and modified to integrate the ultrasonic sensor.

https://github.com/user-attachments/assets/145038d5-875a-4a7d-9a81-4cf9fcb0a791

![PHOTO-2025-09-26-13-27-07](https://github.com/user-attachments/assets/63d8ded4-3eb1-4300-afa1-f8c498ccb7f7)

The rest of the week, we began working on our first big project submission for a kinetic origami sculpture that was made using a sensor of choice and a servo motor. I tested out an ultrasonic sensor, a PIR and a an LDR to see what worked best when intergrated with the servo. I found that the PIR and the LDR were pretty glitchy when used in parallel with the servo. They were not as realiable with sensing either movement or light and subsequrntly outputting the servo movement. I decided that the ultrasonic sensor was the most sensitive in this particular setup, and decided to use that for my project. 

Apart from this project exploration, for my studio foundations class I worked on a project that interegrated an Arduino circuit. The project was to create a digital heirloom that could be passed on to the next generation and have meaning to a persons culture/family. My product was a audi-scnet memorybox that played a audo memory while emitting an associated scnet. After consulting wtih Chris and Sudhu, I built a circuit using an Arduino UNO, a mini SD card reader, a 5v fan and a speaker. The setup took a while as I had to trasnfer the audio to an SD card in a particular way and have it connected to the SD card reader. Once that was done, I modified some existing code and had some help from Sudhu to troubleshoot. I connected the Fan to the 5V power bank from our kit (which was placed near a small sponge that was dipped in essential oils) and then laser cut and 3D printed to enclose all the parts. I plugged it in and it played my recorded audio and emit the smell of jasmine flower! This experimentation would not have been possible without the learnings from TDF, both in electronics and Fabrication. Here are some pictures:

<img width="3024" height="4032" alt="Heirloom picture hero" src="https://github.com/user-attachments/assets/01ed0170-efdd-45aa-8450-937dd9525ce7" />

![PHOTO-2025-09-26-13-46-56](https://github.com/user-attachments/assets/06ef52d7-30e6-4e24-8e47-6762f9da89bc)

**Reflection**: It was an exciting week full of arduino circuits. I learnt so much about how to build parallel abd serial circuits, how to edit code to include differnt sensors and how to measure when a sensor is working and what values it outputs. 

# Week 3 - Fabrication
_Thursday, 09/18/2025 - Thursday, 09/25/2025_

I continued working on my Kinetic origami project for week 3 of fabrication. I began by experimenting with a variety of origami folds and patter ns, testing how each could move when actuated. While some designs looked visually appealing, they didn’t translate well into motion, and others were too delicate to reliably integrate with a circuit. After iterating through several options, I identified a for m that both appealed to me and seemed structurally suitable. From there, I explored which types of movement would complement this fold and whether they could be achieved using a servo motor.

![PHOTO-2025-09-26-13-53-02](https://github.com/user-attachments/assets/2cfdce79-f2f0-40bf-a1fd-f33aa45fdb6b)

![PHOTO-2025-09-26-13-52-20](https://github.com/user-attachments/assets/60c2d80b-3727-4906-8841-e91998f16a57)

I decided to go ahead with the hyperbolic structure since I felt it had the most range of motion and there were many ways in which it could be actuated. 

When working on the form, there were a couple of iterations that I made. I started with just simlply sticking wooden skewers into a carboard box, taping the servos on either side and then attaching the origami in the middle. This form, though in unpolished form, gave the origami the most range of motion. The skewers moved with the the servo, which gave the origami a full twisting motion (which was the intented effect). 

<img width="645" height="755" alt="Screenshot 2025-09-26 at 2 09 45 PM" src="https://github.com/user-attachments/assets/70cc9c3f-0232-435d-b1c2-45e73301b0b1" />

Once I had finalized the circuit and movement mechanism for the protoype, I began building the final form. I chose to use two rigid walls on either side of the box to hold the servos with the origami centered between them. However, after assembling the form, connecting the circuit, and attaching the origami, I discovered that the walls severely limited the range of motion. The structure kept catching between the walls, which in turn caused the servos to malfunction. This led me to revert to using skewers, which better supported the back-and-forth movement of the origami piece. 

![PHOTO-2025-09-26-14-10-23](https://github.com/user-attachments/assets/5f4db3eb-28ca-40d7-851c-ca0ffe1584bc)

In my next attempt, I mounted the servos on bamboo skewers to give the origami piece the movement it needed. This setup worked much better. However, the build quality of this form was poor. It was finicky and lacked a polished outer appearance.

<img width="784" height="704" alt="Screenshot 2025-09-26 at 2 10 51 PM" src="https://github.com/user-attachments/assets/d796078c-3510-43ae-b444-3339cf37222e" />

At this stage, I decided to approach the form from a different angle and experimented with moving the origami piece while it was placed on its side. In this iteration, the motion was driven by two pieces of thread, a central servo, and a makeshift spindle structure that curled the thread to pull the origami inward. Although the setup worked in principle, it required constant attention when the circuit was running. If I were to redo this, I would 3D print the spindle and the box so everything fit perfectly and hence functioned well.

![IMG_6014](https://github.com/user-attachments/assets/963793e0-0f34-4d2f-9397-5b77987f0034)

<img width="464" height="476" alt="Screenshot 2025-09-26 at 2 15 37 PM" src="https://github.com/user-attachments/assets/eeceef78-7852-4704-8923-43d9a3387de2" />

In my final iteration, I returned to exploring the twist motion of the origami fold. This time, I mounted one side of the origami to a stationary base and let the other side move with the help of a servo. I designed and laser-cut a mounting base to hold one side of the piece in place. This was my first time cutting and gluing acrylic together. The first time I printed it, the size of the joints and the height of the actual mounting base was too big. I returned back to my Illustrator file and made the edits I needed. I then recut it and this time the dimensons were correct. I then began gluing the parts together. The glue took a really long time to dry was also hard to apply. I could not get it to stick the first time I tried. I consulted with Claire at the makerspace and she suggested I use an blade to scrape off the first layer of glue and try again. This time it worked, and the parts had bonded together pretty well. I then attached this to the main cardboard box and stuck the origami to the top part of the stand.

https://github.com/user-attachments/assets/95b1b4d9-7925-40c3-af10-837155f640f5

<img width="413" height="573" alt="Screenshot 2025-09-26 at 2 24 40 PM" src="https://github.com/user-attachments/assets/87996616-f082-4f69-b944-f0d04b49f75c" />

![PHOTO-2025-09-26-14-26-44](https://github.com/user-attachments/assets/f263a980-281a-4688-9f84-4c058cb8bd79)

![PHOTO-2025-09-26-14-26-28](https://github.com/user-attachments/assets/735b888d-e18b-43ad-8383-18dd4d68f9e8)

**Refelction**: If there is one thing that I will take away from this project, it is how much form can affect function. Even though I had figured out how the circuit and movemnt would work, the form determined whether it would be doable or not. 

# Week 4 - Electronics 
_Tuesday, 09/23/2025 - Tuesday, 09/30/2025_

Week 4 was focused on getting the origami structure up and running. In my first prototype, I wanted to control the hyperbolic origami structure that I had chosen with two servos, rotating it around a central axis in opposite directions. Although I successfully integrated the ultrasonic sensor with the circuit, the fold itself moved only in one direction rather than alternating. This was an iteration that needed further work and refinement of code so the servos could operate in true opposite directions.

<img width="477" height="356" alt="Screenshot 2025-09-26 at 2 32 42 PM" src="https://github.com/user-attachments/assets/56e1def3-1ad6-408f-85d2-0e8ddf0c130b" />

For the circuit, I experimented with an ultrasonic sensor, an LDR, and a PIR sensor. The PIR and LDR proved unreliable and didn’t deliver the results I needed, so I ultimately chose to proceed with the ultrasonic sensor.

<img width="411" height="548" alt="Screenshot 2025-09-26 at 2 33 13 PM" src="https://github.com/user-attachments/assets/93c32601-caf3-4866-b618-f0b27654fa76" />

[Link](https://youtube.com/shorts/Bu6_5G8F2Oc?feature=share)

<img width="418" height="424" alt="Screenshot 2025-09-26 at 2 33 57 PM" src="https://github.com/user-attachments/assets/9cf0c5d4-2f93-4aaa-94c2-b2bbb8ea0fc7" />

Trying to understand why my code was not moving the servos in opposite direction and how exactly I can go about troubleshooting. This was Sudhus explanation on how I could get it to work. 

In my second attempt, I got the servos to run in opposite directions. I tested this by attaching pieces of tape to each servo horn to observe their movement and then tweaking the code accordingly. Finally, after a few different versions, I was able to mounted the servos onto two bamboo skewers and connected the servo horns to the center of the hyperbolic origami structure. In this iteration, the sides of the origami did not move as I intended. I then went back and changed the code to allow the servos to rotate in the same direction, and this helped me achieve the intended effect. I also experimented with using just one servo while keeping the other side of the structure fixed; this setup worked well too, allowing the origami to move side to side from one point.

<img width="694" height="678" alt="Screenshot 2025-09-26 at 2 35 59 PM" src="https://github.com/user-attachments/assets/401b0d54-0bd1-4246-b5f5-c584825ace52" />

Here, I tested the movement of the servo motors in opposite directions. Since this mechanism did not yield the results I wanted, I pivoted to another solution. This troubleshooting of the problem really helped me understand how the internal mechanics needed to function.

<img width="459" height="643" alt="Screenshot 2025-09-26 at 2 36 31 PM" src="https://github.com/user-attachments/assets/a89bd509-edc6-40ec-a8cc-c41e8b774c19" />

In this iteration, I made one side of the hyperbolic origami stationary, and used a servo to move the other side. This setup worked well, and origami twisted from side to side. [Link](https://youtube.com/shorts/Q4I5t0AZt0Y?feature=share)

<img width="569" height="668" alt="Screenshot 2025-09-26 at 2 39 03 PM" src="https://github.com/user-attachments/assets/28c7fcff-df5a-4c0c-8861-4df2b48cff39" />

In this final prototype, I attached two servos to the origami structure using 2 bamboo skewers. This was a great setup, because the bamboo skewers allowed the origami a full range of motion when it moved. [Link](https://youtube.com/shorts/BwvIaUCZsvU?feature=share)

When creating the final prototype, I faced several issues with the form. Hence, I decided this project from a different angle and experimented with moving the origami piece in another orinettaion. In this iteration, the motion was driven by two pieces of thread, a central servo, and a makeshift spindle structure that curled the thread to pull the origami inward. Although the setup worked in principle, it required constant attention when the circuit was running. The curling motion of the thread was unpredictable; sometimes it wouldn’t curl enough, the servo horn would detach, or the servo would pull the thread incorrectly making the system unreliable. Despite these challenges, the movement itself, when it worked, resembled a gentle heartbeat, which I found very satisfying to watch.

<img width="377" height="441" alt="Screenshot 2025-09-26 at 2 49 05 PM" src="https://github.com/user-attachments/assets/d477f6e2-1d21-4312-a3cd-dfa622cfd038" />

![PHOTO-2025-09-26-14-49-37](https://github.com/user-attachments/assets/906ae3cb-451c-4e09-8d69-a26c1840fe1a)

<img width="552" height="909" alt="Screenshot 2025-09-26 at 2 50 40 PM" src="https://github.com/user-attachments/assets/cf290510-7a69-43b8-bce0-b1b6b89ed685" />

This was the final working prototype. [Link](https://youtube.com/shorts/h_3fSLcp2IU?feature=share)

In my final iteration, I returned to exploring the twist motion of the origami fold. This time, I mounted one side of the origami to a stationary base and let the other side move with the help of a servo. I designed and laser-cut a mounting base to hold one side of the piece in place and cut a notch in the bottom cardboard base for the servo. After placing the circuit inside the box and fixing the origami, I modified the Arduino “sweep” servo code to increase its speed so the motion resembled a “running” effect. Once assembled, the piece achieved a full back-and-forth range of motion. I did have some issues with the sensor detection, but with some help from online resources, ChatGPT and my peers, I was able to get it back up and running again.

For the ciruit in the final prototype, I connected the servo (Power → Power, GND → GND, Signal → ~5) and the ultrasonic sensor (Power → Power, GND → GND, Trig → 12, Echo → ~11) to the Arduino and then adjusted the code to set the desired speed, position, and range of motion for the servo. This was the final prototype. I preferred the faster speed on the servo as the motion felt lively and resembled a running object or person. 

Here was the code I modified and used:

```C++
//(This code was a combination of the 'sweep' code, ChatGPT for troubleshooting and my peers/makerspace specialists guidance on issues)
#include <Servo.h>
#include <Ultrasonic.h>

Servo myservo;
Ultrasonic ultrasonic(12, 11);  // Trig=12, Echo=11

float pos = 0; // variable to store the servo position (Cody Glen from the makerspace suggested I change position from 'int' to 'float' so the servo would move more smooth)
int stepDir = 1; // +1 for forward, -1 for backward

void setup() {
  Serial.begin(9600);
  myservo.attach(5); // attaches the servo on pin ~5 to the Servo object
}

void loop() {
  int distance = ultrasonic.read();   // distance in cm
  Serial.println(distance); 

  // Only sweep if no hand (distance >=10 cm);
  if (distance >= 10) {
    // move faster: jump 3° per update 
    pos += stepDir * 3;
    if (pos >= 180) {
      pos = 180;
      stepDir = -1;
    }
    if (pos <= 0) {
      pos = 0;
      stepDir = 1;
    }
    myservo.write(pos);  // tell servo to go to position in variable 'pos'
    delay(20); // waits 20 ms for the servo to reach the position

  } else {
    // Hand detected: stop servo at current position
    myservo.write(pos);
    delay(4);
  }
}
```

<img width="971" height="715" alt="Circuit Diagram_Kinetic Origami" src="https://github.com/user-attachments/assets/1f341112-592e-40bc-8fe3-0afcb481c442" />

<img width="577" height="783" alt="Screenshot 2025-09-26 at 2 55 47 PM" src="https://github.com/user-attachments/assets/5682775c-483c-4c03-8e98-45050fa72deb" />


![PHOTO-2025-09-26-14-59-11](https://github.com/user-attachments/assets/40d3005d-9c31-40e3-af24-367c59614b29)


![PHOTO-2025-09-26-14-59-11](https://github.com/user-attachments/assets/50708b8c-f198-409e-b675-68a588e7bade)


![PHOTO-2025-09-26-14-59-12](https://github.com/user-attachments/assets/077ff07b-885b-4dfe-90e9-51e508d2bee8)


![PHOTO-2025-09-26-14-59-12](https://github.com/user-attachments/assets/2015ea15-3e8d-423d-90af-fef6f455232d)

[Link to working Ultrasonic Sensor](https://youtube.com/shorts/HxV3xgsXDB4?feature=share)

[Final Prototype Link](https://drive.google.com/file/d/1IEtP90--ExHfsvl0UQZxa2HVzvmjkZft/view?usp=sharing)

[Link to Project Folder](https://drive.google.com/drive/folders/1yFFv-a1pZ86ZencWEstLLtQNcUA3YIR-?usp=sharing)

On thursday, I presented my model and received feedback from my peers! I loved getting to see how everyone interpreted this project and learning from other peoples ideas.

**Reflection**: This project was so much fun! I feel like with each iteration, I learnt something new. I tested differnt codes, sensors, movements, materials etc., and with each one, I was able to make improvements to the next one.

# Week 4 - Fabrication
_Thursday, 09/25/2025_

The first critique of the semester was a success! My project finally came together at the end, and I learnt a alot along the way. I was really happy that after so many iterations (and thinking that it wasn't going to work), it worked really smoothly. It was also SO awesome to see the work done by my peers. I got so many new ideas, and felt like I could have so many more iterations based on what I had seen. I will take these learnings into my next prject. 

A picture from my critique below!

![PHOTO-2025-09-26-15-21-47](https://github.com/user-attachments/assets/9fc0cfcf-8091-451b-b3fc-d428c811e144)

# Week 5 - Electronics 
_Tuesday, 09/30/2025 - Tuesday, 10/7/2025_

On tuesday, we got introduced to the new project for this week - 'Expressive Mechanics'. For this project, we needed to design and construct a kinetic mechanism that responds to live camera input using computer vision and machine learning. The goal was to to explore how "mechanical systems can interact with and react to their environment in surprising, engaging ways". 

We started by playing around on open processing and using a simple code writte by Sudhu to modify and play around with. We learnt how to make a sky, a sun and a few clouds. We then saw how to make those clouds move. I played around with colours as well to see how that can be done. 

Here is a video of the exploration. 

https://github.com/user-attachments/assets/67b5d22c-ad27-4929-beb4-a1c904057e06

We then moved to connecting the arduino to p5js code by connecting to the serial port. For this, is was important that the serial monitor on the arduino was closed, else this connection would not work. For this experimentation, we used a potentiometer, and the arduino to receive data when the potentiometer was moved. We plugged in the code that Sudhu gave us, both into the arduino IDE and open processing and paired the serial port. This was the result:

https://github.com/user-attachments/assets/efca5a11-936e-45c1-adcc-3dd6a2046b0d

After experimenting for a bit, we moved to deciding what we would do for our actual project. We were given an H-Bridge module, an aduino Uno and a DC motor to work with. I began by first trying to figure out what kinetic movement I wanted to do. At first, I wanted to have a small stick figure that imitated a dance move that I did. But after sketching it out, I realized it may be a bit more complicated to accomplish in a week. I then pivoted to looking at something similar, where if I were to do a wave with my arms, that the sculpture would also replicate that movement by waving. I began by looking through how to replicate this movement on P5js. It took a couple of iterations to get right. 

First, I sketched out the movement on a piece of paper so I could understand the logic better and what would be required to make it work. 

![PHOTO-2025-10-09-14-40-08](https://github.com/user-attachments/assets/34d89dd9-1db6-4968-a1ac-6da1f5128cd6)

I referred to tutorials by Jeff Thompson [Link](https://www.youtube.com/@jeffkthompson), The coding train [Link](https://www.youtube.com/channel/UCvjgXvBlbQiydffZU7m1_aw) and Sudhu's tutorials that he provided in class. For any troubleshooting and additional help, I used ChatGPT. 

I then started a couple of iterations to get this movement working. I started by referring to code snippets from Jeff Thompson's channel for skeleton tracking. This was largely sucessful, but lacked the realiabilty I needed for a final prototype. It was not very accurate, and the logic for the wave in this scenario had many different components/points on the body to consider before it could complete the function and run the motor. In these iterations, there were a few problems I observed: 

1. The wave movement was interepreted too early by the program. When picking up just one hand, the console would print 'Wave Detected'
2. The movements were jittery and not reliable
3. For a few, the wave just would not get picked up as a movement and no action would be outputted

Here are some videos of my iterations and tests with skeleton tracking/tracking the whole arm:

<img width="1672" height="958" alt="1" src="https://github.com/user-attachments/assets/7a21b173-7b11-4162-ae32-6890dde2d014" />

<img width="1728" height="988" alt="2" src="https://github.com/user-attachments/assets/bca787d5-01a1-47ca-8f7f-6fb9b8a72370" />

https://github.com/user-attachments/assets/62581362-59ed-4a05-a00e-68d786a60d2e

https://github.com/user-attachments/assets/36c234a2-14f3-4ec3-8b58-bda70be42731

This version was the best one of the lot. But it was unreliable when in use. If would pick up a 'Wave' even when the whol function was not complete. 

https://github.com/user-attachments/assets/b706ec9e-d1a3-4974-b4a7-0332775bd460

![PHOTO-2025-10-09-15-07-08](https://github.com/user-attachments/assets/611a19d1-1622-442a-b47f-fe8766c8c515)

https://github.com/user-attachments/assets/133ccf1e-ccf7-49af-9614-624c73b446f4

Next, I applied the same logic Sudhu used for detecting ear tilts. I mapped the two shoulder points and set up the logic: when the right shoulder is higher than the left, followed by the left being higher than the right, a wave is detected, and the console prints “Sam’s wave detected”

This took a few tries. Intially there was a lot of lag, the shoulder points weren't getting mapped correctly and the wave was being detected with even a slight movement in the shoulders or after picking up just one shoulder higher than the other. Finally, after some troubleshooting I was able to get it to work. It was really smooth and printed 'Sam's wave detected' reliably after each wave was completed. 

Here are a few videos of these iterations:

https://github.com/user-attachments/assets/f9690594-747e-4d09-b04d-4f803f2fbb5f

https://github.com/user-attachments/assets/92b0f5bd-c5ef-4117-a4ad-61dfa306d22b

https://github.com/user-attachments/assets/1a94af94-3163-4402-9a43-9763bde629ef

Final Version:

https://github.com/user-attachments/assets/51d6f67d-807a-471e-92e8-b191c228d893

https://github.com/user-attachments/assets/6004e8a1-65af-41e7-9d5a-056e5df6e66d

**Reflection**: I was really happy with this outcome! It took a few tries, but it was fun to figure out what the problem was with each one and trying to find a fix. Also, online resources and ChatGPT really helped pinpoint issues. 

# Week 5 - Fabrication
_Thursday, 9/25/2025 - Thursday, 10/2/2025_

For the hardware of this project, I had to make a cam shaft with planks of wood on top to make it move in a wave like motion. I consulted with Chris on my design, and he gave me a lot of helpful tips on how to make it work. He then suggested I try to do a mini prototype of it, and test the functionality. 

The first thing Chris had me check was the tolerance limit for the wooden dowel. The dowel itself was 0.25", but when the hole for it is cut on a laser cutter, we need to take into consideration the kerf. So I cut holes of sizes ranging from 0.25" - 0.2". I then checked with the dowel which hole was the best fit. The goal was for it to be tight and not move the cam pieces around. For this purpose, 0.23" was the best fit.

![PHOTO-2025-10-09-16-46-36](https://github.com/user-attachments/assets/1ecaf606-3794-4f31-9dfa-e04d47bed26a)

In my first iteration, the wood planks were too thin to sit on a cam. Though they had the range of motion needed to move in a wave, they would have to be aligned perfectly at all times to work. The balance of it would also be difficult to achieve and pretty finicky. 

Here is an image of the first iteration of the wood planks that would sit on top of the cam:

![PHOTO-2025-10-09-16-46-04](https://github.com/user-attachments/assets/e0a598fb-2364-403c-8083-e7abb0ae540e)

Chris then showed me a hinge mechanism made by another student and this inspired me to start thinking of hinges rather than freely suspending the wooden planks on a dowel. 

![PHOTO-2025-10-09-16-48-57](https://github.com/user-attachments/assets/cc1f9512-2cee-4208-b714-fc0a57437a93)
**Student project**

I then remembered the living hinges that Chris had shown us during week 1 of TDF. This seemed like the best option considering that it provided the flexibility of a hinge without needed multiple parts (It was also a compliant mechanism, which is something Chris always talks about in class!).

I then tested out the hinge with different interations. The first iteration was really flexible, but it also meant it moved from side to side and too far up and down. The motion would be unstable on a cam. It also broke almost immediately from the fragility of the hinges as they were spaced so close together. The next iteration was not as tightly packed. I spaced out the living hinge lines and made the width shorter to give the wood plank some rigidity. For the width and height of the plank, I intially thought to make it short and thin, but Chris suggested that if I made it longer and wider it would exagerate the look of the wave. So I went ahead with the suggestion. 

![PHOTO-2025-10-10-09-06-46](https://github.com/user-attachments/assets/01f286fd-9d8d-40f0-a5fe-79d7a6f23e96)

After I had figured out the wood planks that sat on top of the cam, it was time to make the cam itself and space them out. I had cut out the cam circles to test how balanced the wood planks on top would be, but the assembly of the whole shaft remained. I put all the cam circles on a wooden down, and spaced them out using the width of the wooden planks as tolerence. I then moved them around the same axis to create a wave like formation, and glued them in place for extra support. I then used some extra plywood pieces, and drilled a a hole on either side to hold the cam shalft. I glued a long piece of plywood at the back of the stand to hold the wood planks to rest on the cam shaft. Finally, I added the motor and connected it to the dowel using the connector piece that Chris printed for us. 

![PHOTO-2025-10-10-09-06-57](https://github.com/user-attachments/assets/94ab205e-c7ef-4be3-bd93-0d3afc2699f7)

Finally I asked Chris to help me connect the motor, and we tested to see if the prototype worked as expected. Here is the video of the prototype in action!

https://github.com/user-attachments/assets/96126152-5934-47f9-a174-33a71541c0c4

**Reflection**: It was really great to be able to build a rapid prototype and learning from my mistakes along the way. I felt like I had a better idea of what my final outcome needed to have, and what directions I should to avoid (or wouldn't work), just from this one prototype.

# Week 6 - Electronics 
_Tuesday, 10/7/2025 - Tuesday, 10/14/2025_

On Wednesday, I began by connecting the various components together. In this project, we were working with an Arduino Uno, an H-bridge and DC motor.

![PHOTO-2025-10-10-09-33-48](https://github.com/user-attachments/assets/01100bd1-12c4-41ee-9467-3f99c4114718)

I spoke to some of my peers who had connected their circuits together, and followed their process. I began by first soldering my wires to the DC motor. I have soldered before, but this time around the process took a little longer than I expected. Even with the soldeing stand and the metal clamps, the wires kept shifting in place and made it had to solder. After a couple of tries, (and also accidentally melting some of the plastic casing on the motor) I got the wires connected. After this, I connected the three components together and screwed the wires conneted to the H-bridge in place so it wouldn't move/shift. 

<img width="390" height="545" alt="Screenshot 2025-10-10 at 11 34 54 AM" src="https://github.com/user-attachments/assets/a9c07045-2ac3-4c97-8cde-7a0ec0b6e473" />

I tested out the connection using the arduino simple code through the L298N library. It worked as I'd hoped. 

https://github.com/user-attachments/assets/21980bb0-9f5d-4371-af34-c8c6b4390870

I then moved to intergrating my P5js code from earlier with the arduino and serial connection code that Sudhu had provided for us. This process was pretty straightforward, since we were given the entire thing beforehand. After I connected it all together, I couldn't get the motor to run. Even though the command to run was sent to the arduino, (and printed in the P5js console), the motor would not move. I troubleshooted on ChatGPT, but still couldn't get it to work. Finally I realized, that it was because I had not put in the correct pin numbers in the code 💔. After I added that to the code based on my circuit, the motor started to turn!

https://github.com/user-attachments/assets/78c256f7-61dd-4e6d-805e-520812aa90a7

Here is a small diagram of the flow from my understanding:

<img width="818" height="578" alt="Screenshot 2025-10-10 at 5 35 16 PM" src="https://github.com/user-attachments/assets/4289da6e-c370-4e35-88eb-45e3de1b34f2" />

Here is te P5js code:

```javascript
let serialOptions = { baudRate: 115200  };
let serial;
let portName, receivedData;

let video;
let poseNet;
let poses = []; //import libraries

let waveState = 0; // 0 = start, 1 = right shoudler up, 2 = left shoulder up
let threshold = 20; // minimum difference in pixels to count as "higher" than other shoulder

function setup() {
  createCanvas(640, 480); //size of the video frame. 
  video = createCapture(VIDEO);
  video.size(width, height);
  video.hide();
  
   // Setup Web Serial using serial.js
  serial = new Serial();
  serial.on(SerialEvents.CONNECTION_OPENED, onSerialConnectionOpened);
  serial.on(SerialEvents.CONNECTION_CLOSED, onSerialConnectionClosed);
  serial.on(SerialEvents.DATA_RECEIVED, onSerialDataReceived);
  serial.on(SerialEvents.ERROR_OCCURRED, onSerialErrorOccurred);
  
  	// this creates the button. you could optionally use something else
	// to call the connect() function and start the connection! 
  button = createButton('Click me to connect to your Arduino!');
  button.position(0, 0);
  button.mousePressed(connect);

  poseNet = ml5.poseNet(video, modelReady); //posNet learning model to detect where body parts are
  poseNet.on('pose', gotPoses);
}

function modelReady() {
  console.log("PoseNet ready");
}

function gotPoses(results) {
  poses = results;
}

function draw() {
  image(video, 0, 0, width, height);

  if (poses.length > 0) {
    let pose = poses[0].pose;

    let rightShoulder = pose.rightShoulder; //position for right shoulder
    let leftShoulder = pose.leftShoulder; //position for left shoulder

    // Draw shoulders dots, one in red and one in blue
    fill(255, 0, 0);
    ellipse(rightShoulder.x, rightShoulder.y, 20);
    fill(0, 0, 255);
    ellipse(leftShoulder.x, leftShoulder.y, 20);

    let diff = rightShoulder.y - leftShoulder.y; //to check the differnece between shoulder heights. If value is -ve, then right is higher, if positive then left is higher. 

    // Wave detection logic
    if (waveState === 0 && diff < -threshold) { 
      // Right higher than left
      waveState = 1;
      console.log("Right shoulder higher than left");
    } else if (waveState === 1 && diff > threshold) {
      // Left higher than right
      waveState = 2;
      console.log("Left shoulder higher than right");
    }

    // Loop completed, left higher at the end. Which means that the wave is complete
if (waveState === 2 && diff > threshold) {
  console.log("Wave has been detected");
  
  // Send data to the Arduino
  if (serial.isOpen()) {
    serial.write("RUN\n"); // send command
    console.log("Sent RUN command to Arduino");
  } else {
    console.log("Serial port not open — cannot send RUN command");
  }

  // Reset state to detect again
  waveState = 0;
}

    }
}

/**
 * Callback function by serial.js when there is an error on web serial
 * 
 * @param {} eventSender 
 */
 function onSerialErrorOccurred(eventSender, error) {
  console.log("onSerialErrorOccurred", error);
}

/**
 * Callback function by serial.js when web serial connection is opened
 * 
 * @param {} eventSender 
 */
function onSerialConnectionOpened(eventSender) {
  console.log("onSerialConnectionOpened");
}

/**
 * Callback function by serial.js when web serial connection is closed
 * 
 * @param {} eventSender 
 */
function onSerialConnectionClosed(eventSender) {
  console.log("onSerialConnectionClosed");
}

/**
 * Callback function serial.js when new web serial data is received
 * 
 * @param {*} eventSender 
 * @param {String} newData new data received over serial
 */
function onSerialDataReceived(eventSender, newData) {
  console.log("onSerialDataReceived", newData);
	//console.log(serial.getPort());
	receivedData = newData; //also save the data in our received data variable

}



function onSerialConnectionClosed(eventSender) {
  console.log("onSerialConnectionClosed");
  portName = "Not connected";
}


/**
 * Called by the browser when our special button is clicked
 */
function connect() {
  if (!serial.isOpen()) {
    serial.connectAndOpen(null, serialOptions);
  }
}

```

Arduino code:

```C++
#include <L298N.h>

const int IN1 = 9;
const int IN2 = 8;
const int EN = 10;

L298N motor(EN, IN1, IN2);

bool motorRunning = false;
unsigned long motorStartTime = 0;

void setup() {
  Serial.begin(115200);
  Serial.println("Motor Ready");

  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(EN, OUTPUT);

  motor.stop(); // ensure stopped
}

void loop() {
  // Check for serial data
  if (Serial.available() > 0) {
    String rcvdSerialData = Serial.readStringUntil('\n');
    rcvdSerialData.trim();

    if (rcvdSerialData == "RUN") {
      Serial.println("Wave detected — running motor for 10s!");

      // Run motor forward at speed 200
      digitalWrite(IN1, HIGH);
      digitalWrite(IN2, LOW);
      analogWrite(EN, 200);

      motorRunning = true;
      motorStartTime = millis();
    }
  }

  // Stop motor after 10 seconds
  if (motorRunning && millis() - motorStartTime >= 10000) {
    digitalWrite(IN1, LOW);
    digitalWrite(IN2, LOW);
    analogWrite(EN, 0);

    Serial.println("Motor stopped after 10s");
    motorRunning = false;
  }
}
```

**Reflection**: This process was loads of fun. I had never used P5js before, let alone connected it to an arduino to perform a certain action. Having an smaller session with Sudhu through the week really helped me in understanding how the P5js code and the ardunio run spearateley and also together as a unit. As he explained it, the P5js code was the super brain and decided when the motor needed to run, while the arduino used that imput and ran the motor. Also, my peers were SOO helpful with any and all my questions, and I learnt so much from them too. 

# Week 6 - Fabrication
_Thursday, 10/2/2025 - Thursday, 10/9/2025_

This week involved A LOT of fabrication. After I had gotten my rapid prototype to work last week, I was confident on my next steps. 

I began by drawing out everything on illustrator and getting my file ready for cutting. For the cam shaft, I wanted the spacing and tolerance to be exact with each turn, so the wave would look more natural. I wasn't very confident in my skills to do this manually, so instead I put 2 holes on each cam circle that were at a 30 degreen angle apart. This way, when I was adding the circles to the cam shaft, I could use a small drillbit or cut a jig that would help me angle them at exactly the same width apart. I also used the wood planks to space the cam circles at the exact distance from each other. This worked even better than I could had expected, and the circles were all very evenly spaced out. 

![PHOTO-2025-10-10-13-36-02](https://github.com/user-attachments/assets/56fa76de-d50f-41b1-872a-eb7cbd9d55b3)

This is how I spaced it out with the drill bit as a guide:
![PHOTO-2025-10-10-13-36-17](https://github.com/user-attachments/assets/91c4e576-f978-489f-94b5-cf93266d9e62)

![PHOTO-2025-10-10-13-36-40](https://github.com/user-attachments/assets/cdd2625f-3548-4eb8-bd11-b024837868f1)

The next thing I did was cut out the 25 wooden planks that woud sit on top of the cam shaft. I added the living hinges at the same configuration as my prototype, just removed about 2 mm from the top to make it as rigid as possible. I then cut them all out. 

![PHOTO-2025-10-10-13-37-40](https://github.com/user-attachments/assets/f3d80fed-dd7d-4d2d-9845-0ad5232facbe)

https://github.com/user-attachments/assets/c0d92e61-61fc-4b9f-812b-91fe9ac70ffd

I then cut the base and the stand with a longer finger joint. I wanted to stack three pieces together so the cam would have enough support. The first prototype did not work out because I did not account for the thickness of the wood when stacked together (beginner's mistake ahhh). I them made changes, cut it out and this time is worked perfect!

![PHOTO-2025-10-10-13-42-14](https://github.com/user-attachments/assets/4b7e2609-281f-44c0-9c0f-26cc04f16fc9)

After sanding everything, it was ready to assemble. I placed the dowel into th stand. Here I realized that the dowel had been cut too short. I cut out some washers and fixed that on the side to give it some length. This fixed the problem. Lastly, I cut pieces to mount the motor and the circuit, drilled the motor in place, and stuck the wood planks on top exactly at the center of each cam so it would be balanced when it rotated. Chris also suggested that I add a support beam in the ceneter to hold the weight of the shaft. I added that in and just like that my prototype was ready.....or so I thought. When I went to power the motor, the cam shaft wouldn't move. I checked teh code which was fine, and also checked if the motor was actually running and it was. At this point, I was pretty stresses. It was the night before the submission and suddenly my prototype had stopped working. After consulting with a student helper, he suggested that the prototype could need some grease around the cam circles and the dowel to get rid of some of the friction between the wood pieces. This worked wonders, and the prototype worked immediately after. I realized it was also the friction of the center support beam that was causing it to be hard to move. So I added extra grease in that space for it to move freely. and finally it ACTUALLY worked

a video of it working right after adding some grease:

https://github.com/user-attachments/assets/71d41c92-f24c-41b2-8c8b-6d40540fa410

Here are some pictures and videos of it working: 

![PHOTO-2025-10-10-13-46-37](https://github.com/user-attachments/assets/c1da6857-7331-4d12-8a79-c35a49bbeaba)

![PHOTO-2025-10-10-13-46-53](https://github.com/user-attachments/assets/c1971bb2-f3e4-4bd4-a3cb-9d8ad5f6b662)

![PHOTO-2025-10-10-13-47-06](https://github.com/user-attachments/assets/2e121af3-8b6c-42c4-8d2c-b689dacf8c40)

![PHOTO-2025-10-10-13-47-19](https://github.com/user-attachments/assets/9bd4cd9c-e176-4cb1-9100-9fdd3e3871e1)

![PHOTO-2025-10-10-13-47-37](https://github.com/user-attachments/assets/c48108cf-554c-479f-ab84-9510c4c60e3d)

https://github.com/user-attachments/assets/85c4945f-d76d-4383-85c0-323f7bad15ab

During my critique, I got loads of feedback and also appreciation for my project!! The main feedback was to make the cam shaft an even number of circle (mine was 25), since there was a slight misalignment when it moved. Apart from this, I also got to see my peers work. Special shout out to Elisa, who used audio in her project (which I had no idea was even possible) and Vivian, who's project rotated around an axis and hit a bunch of glasses filled with water to make sounds!! Everyone's projects were fanastic and there was such a great range. 

**Reflection**: This project was a whirlwind of leanring. I began this week not knowing what a camshaft was, and ended with a whole project with that as the focus. I also had never used P5js before and so that was also a new experience for me. All in all, it was a great experinece. There are so many insights I will take into my final project, and so many potential ideas I got from just getting to freely experiment. 

# Week 7 - Electronics 
_Tuesday, 10/14/2025 - Tuesday, 10/21/2025_

We began this week by getting familizarized with the ESP feather V2. I started by first soldeirng the header pins to the board. After I had my pins soldered in, I began to follow Roopa's tutorial to connect the feather to a weather API.

The first step in the process was connecting my Feather to the Berkeley network. I registered on the Berkeley IoT portal and received a unique password, which I later used to connect the device to Wi-Fi by adding it to the code. After following the remaining steps in the tutorial, I successfully programmed the Feather’s onboard light to flash in different colors representing the weather conditions in Berkeley! 

Here is a video of that exploration:

https://github.com/user-attachments/assets/bec8ff36-b2ad-48c3-9dc5-f8b52bf9e3f8

We were then introduced to our next project; ambient display. The brief required creating an ambient device that conveys real-world information through subtle, aesthetic variations in form, movement, sound, color, or light using API data. We then got into teams of two. Ishani and I decided to team up together for this project. The first course of action was to brainstorm ideas and understand what we wanted to collectively achieve in this project. We both decided that we wanted to create an ambient display that would motivate a user to improve a certain aspect of their daily routine. We intially wanted to use screen time data, to track when a user is nearing their screen limit for the day, but we soon realized that it was really hard to access this data due to certain privacy standards that were built in place. 

So we decided to explore step count data using the fitbit API. This was a much more complex process than we had intially anticipated as you have to go throigh an Oauth process to get access to a users health data. From this, an access token needed to be generated every 8 horus to allow someont to have continue access to this data. We decided to take the rest of the week to learn more abot how we could extract this information in real time. The Fitbt API documention for this was really comprehensive, and helped us understad not only how we would be able to access this data, but what data we needed to call to get step count information. 

I also looked into how to connect the feather to a dc motor as we had decided that we wanted the medium for our ambient display to be motion. I found a tutorial from Last Minute Engineering that was partciularly helpful. Below is the link:

[Last Minute engineering](https://lastminuteengineers.com/esp32-l298n-dc-motor-control-tutorial/)

**Reflection**: This week was all about experimenting and learning about the Oauth process (That I knew nothing about before this project started). I also learnt a lot about how API's work (and also what they are :x) and how to reliably intergrate real-time data/infomration into any project.

# Week 7 - Fabrication
_Thursday, 10/16/2025 - Thursday, 10/23/2025_

For the fabrication, Ishani and I began by thinking of forms that could best represent our step count. In one of those brainstorming sessions, we came up with the idea of creating a drawing machine that translates daily movement into visual form, generating increasingly complex drawings as the user takes more steps. We began by making a few prototypes using plywood that had been laser cut. These iterations worked great and we got a feel for how this drawing machine would work. Here are some images of these explorations and the drawings it created:

<img width="642" height="771" alt="Screenshot 2025-11-01 at 12 33 01 AM" src="https://github.com/user-attachments/assets/2f3c56d4-c435-4ee2-923d-c9db413039f5" />

<img width="621" height="757" alt="Screenshot 2025-11-01 at 12 32 35 AM" src="https://github.com/user-attachments/assets/6b475cf5-67bc-4f18-8013-3f034de4abf9" />

<img width="640" height="715" alt="Screenshot 2025-11-01 at 12 33 38 AM" src="https://github.com/user-attachments/assets/e6170097-40bf-44f4-8bf0-0ab71f620ac5" />

Though we really liked this idea, after speaking with Chris, we realized it was not necessarily great ambient display due to it's fast and frequent pace. This device needed to embody calm and slow computing. So we decided to pivot to a differnt idea. We began by first sketching potential displays out. Through that process, we decided to use a magnet to carry a ball beaaring upwards within a track to symbolize a person moving 

We then began fleshing the idea out and drawiung more detailed sketches. Here are a few below:

<img width="608" height="643" alt="Screenshot 2025-11-01 at 8 39 28 AM" src="https://github.com/user-attachments/assets/af3f443a-dfc1-4360-b805-5b8c34bcf547" />

<img width="520" height="416" alt="Screenshot 2025-11-01 at 8 39 43 AM" src="https://github.com/user-attachments/assets/89b123c3-d45c-457a-a2b6-84f9e75e501b" />

<img width="628" height="787" alt="Screenshot 2025-11-01 at 8 40 03 AM" src="https://github.com/user-attachments/assets/0fa2ceee-70c2-4e4f-b5f8-bfb69fb8ab3f" />

<img width="604" height="754" alt="Screenshot 2025-11-01 at 8 40 22 AM" src="https://github.com/user-attachments/assets/76a30f07-4acf-41e5-a981-e590f5b6ccc5" />

<img width="636" height="851" alt="Screenshot 2025-11-01 at 8 40 43 AM" src="https://github.com/user-attachments/assets/4afce35a-1bd3-4793-9561-2a6ef36421c7" />

<img width="647" height="857" alt="Screenshot 2025-11-01 at 10 07 32 AM" src="https://github.com/user-attachments/assets/dc68c9dc-b48a-4823-8910-0e4ad549d16c" />

After speaking with Chris and a few of the student helpers, We figured out how we would make the internal mechanism work. We then got to rapid prototyping our first iteration. We cut out 4 pieces of plywood and made a conveyor belt and spindle using a couple of pieces of kerf cut wood. We then inserted dowels into the sides and attahced a row of magnets on the coveryor belt. The ball bearing was attached on the outside, and actually moved pretty smoothly up and down the track. Here are a few photos of that:

<img width="442" height="626" alt="Screenshot 2025-11-01 at 8 44 14 AM" src="https://github.com/user-attachments/assets/006bfd6f-9714-41b3-856a-c96801a96029" />

<img width="448" height="628" alt="Screenshot 2025-11-01 at 9 40 31 AM" src="https://github.com/user-attachments/assets/d5b249b6-d8eb-4e0c-834f-86070f3acff5" />

Having done this prototype, we were able to get an initial look into what it would take to make this display on a larger scale. We also realized through this prototype, that the magnet had to be extremely close to the edge of the case (but not too close that it was unable to move) for it to continue holding the ball bearing while moving. Since we wanted to do our final display in a curved form, we had a lot to figure our in terms of tolerances to make sure that the magnet held on within the path and was released after the step goal was completed. We then starting sketching out the final forms and creating our illustrator files to continue testing this form. We wanted the final display to be made with acrylic, but before we did that we had to make sure every measurement was correct and working in wood. 

Here is a sketch of the final form:

<img width="628" height="845" alt="Screenshot 2025-11-01 at 10 07 07 AM" src="https://github.com/user-attachments/assets/7505b3a7-759b-453e-a645-634b032642dc" />

<img width="636" height="845" alt="Screenshot 2025-11-01 at 10 06 53 AM" src="https://github.com/user-attachments/assets/4c025411-8b48-4b52-a8f9-3e3ea08382bf" />

<img width="640" height="856" alt="Screenshot 2025-11-01 at 10 10 19 AM" src="https://github.com/user-attachments/assets/f2572dd6-df41-44c7-9e2c-77c1a1c0c95a" />

The final form that we decided on was a curved struture, with a pinball style mechansim in the back such that when the ball bearing falls (or, in other words the user completes their step goals for the day), they will receive an audio cue that their goal has been reached. 

**Reflection**: Week one of this project was BRUTAL. I feel like we had so many ideas, yet so many of them were not really ambient display. I was so used to creating projects that were fast paced and gave an output to a user really quickly as opposed to being slow. Since calm computing is all about slowing things down, this was a bit of a shift and I can to change my mindset to work with that brief. I really enjoyed it!

# Week 8 - Electronics 
_Tuesday, 10/21/2025 - Tuesday, 10/28/2025_

After having gone through the documentation for the Fitbit API, I started following the steps they had to extract step count data. The first thing I did was to create a developers account with Fitbit. I then created a request for my project with the Fitbit API, and through that got a Client ID and a Client Secret that was unique to my project.

<img width="456" height="863" alt="Screenshot 2025-10-23 at 6 35 36 PM" src="https://github.com/user-attachments/assets/5c3e521a-6c01-4964-bc30-b36186ef32a1" />

<img width="523" height="339" alt="Screenshot 2025-10-23 at 6 35 26 PM" src="https://github.com/user-attachments/assets/de6d78bc-de64-44a4-981c-ae0d01d625f2" />

With the help of Roopa, I competed the Oauth process and generated the URL ID from localhost. I then generated a curl request with the client ID, secret and the URL ID and put that into my terminal. This was the curl request:

```shell
curl -X POST https://api.fitbit.com/oauth2/token -u 23TGS9:59c75fe8a09ad80d8d20d9a9bd796687 -d client_id=23TGS9 -d grant_type=authorization_code -d redirect_uri=https://localhost/ -d code=4f795f2bd6b24484d50e9c3aec440a21f98dac33
```

<img width="1279" height="674" alt="Screenshot 2025-10-23 at 6 35 50 PM" src="https://github.com/user-attachments/assets/12a3d9f7-adea-4e5f-a0be-3b9933f89541" />

This generated the access token and refresh token (that expires every 8 hours). This is how it looked on my terminal:

<img width="1728" height="179" alt="Screenshot 2025-10-23 at 6 43 15 PM" src="https://github.com/user-attachments/assets/af98109e-17ec-4594-8d69-556776773f31" />

Roopa then suggested that I see if I can actually extract the data I want from this. So we generated another curl request with the link to the exact data endpoint we wanted to extract from. Here was the curl request I used:

```shell
curl -X GET "https://api.fitbit.com/1/user/-/activities/list.json?beforeDate=2025-10-23&sort=desc&limit=10&offset=0" -H "Authorization: Bearer eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiIyM1RHUzkiLCJzdWIiOiI1UFhEUFIiLCJpc3MiOiJGaXRiaXQiLCJ0eXAiOiJhY2Nlc3NfdG9rZW4iLCJzY29wZXMiOiJyaHIgcmFjdCBycHJvIiwiZXhwIjoxNzYxMzY3MDgyLCJpYXQiOjE3NjEzMzgyODJ9.39dpOetqhT7mj09_LEH5BgOxygmzblYfv73NfPDWjPk"
```

This gave me a whole bunch of data in the terminal, and all of it was real-time data from the fitbit account directly. From this, I knew that I would be able to get the data we needed for the project. This is what it looked like:

<img width="1728" height="1081" alt="Screenshot 2025-10-24 at 9 23 00 AM" src="https://github.com/user-attachments/assets/4fe97a7c-f0e4-4812-ade0-dc339c6a0c5e" />

I then inputted the code into the IDE, attached the client sceret, ID, access and refresh tokens and ran the program. It gave me the step count accumulated for that day! Here is the image of that output:

<img width="1728" height="1115" alt="Screenshot 2025-10-23 at 7 01 58 PM" src="https://github.com/user-attachments/assets/1369fdd8-59ff-434c-954f-09a384c01719" />

**Reflection**: This week was SO much fun. Getting this to work, despite the pretty complicated process was really rewarding. It was also really cool seeing the data update anytime I walked around. I was so used to using static data, that when I saw it change in real-time, it was super exciting. 

# Week 8 - Fabrication
_Thursday, 10/23/2025 - Thursday, 10/30/2025_

This week involved LOTS of prototyping and several rounds of trial, error and iteration. We made 7 prototypes this week just to get the tolerances for the magnet and the ball precise. With each one we learnt soemthing new. In the second iteration, we laser cut a form with a kerf cut at the top so we could curve it in place. The conveyor belt was made using a rubber tire, that was taped together in place. Unforntuanteluy this prototype was too close to the edge of the outer casing, which meant the magnets couldn't go over the curve of the form. Additionally, the rubber belt was curved and hence too taut and there was not enough slack for the spindles to move freely.

<img width="632" height="844" alt="Screenshot 2025-11-01 at 12 12 44 PM" src="https://github.com/user-attachments/assets/b39b4f07-6bce-461c-a4fc-078a42bdd274" />

Using the same form, we replaced the rubber belt with a kerf cut belt since we felt that worked really well the first time we prototyped. It was taut enough but also flexible to move freely. This prototype worked really well. The only thing we had to change was the press fit of the dowels into the spindle. Since it was really loose, the conveyor belt was not moving as intended. For the rapid prototype, we also made it using toilet paper rolls which also didn't have the friction we needed to make it move smoothly. But apart from this, the ball bearing was moving over the curve and to the other side which we were really happy to see! 

Here is a video of it in action:

https://github.com/user-attachments/assets/9aa4b28e-6881-46cf-9132-f5db5ef88b40

<img width="627" height="843" alt="Screenshot 2025-11-01 at 12 20 00 PM" src="https://github.com/user-attachments/assets/11fcceef-72c1-404b-8264-72259d981789" />

<img width="1134" height="846" alt="Screenshot 2025-11-01 at 12 20 32 PM" src="https://github.com/user-attachments/assets/f6b1a64f-ab5d-42d2-b769-45e6632a6ea9" />

The next 2 iteration were to get the tolerances even more precise and making sure the ball moved over the curve every time. We kept playing around with different sizes, spindle placements, outer casing sizes and also the size of the spindles inside. This helped inform the dimensions of our final cut in acrylic. Since acrylic is expensive, we wanted to be sure of what to cut before we went in with it. Here are some images of the next iterations

We also realized through this process that the spindles had to be printed with the outer circle being of the same outer height as the width of the magnet so it would be exact and wouldn't protrude out of the casing. In the iterations we had made so far, the magnet was larger than that tolerance, but still moved since the kerf cut at the top expanded as it moved. Here are some images of it:

<img width="632" height="846" alt="Screenshot 2025-11-01 at 12 32 32 PM" src="https://github.com/user-attachments/assets/7583f043-4657-4fa4-a9c5-9deac5a4198d" />

<img width="637" height="847" alt="Screenshot 2025-11-01 at 12 32 47 PM" src="https://github.com/user-attachments/assets/994d0286-eb91-479c-b8b8-99161c882af4" />

We 3D modeled new spindles, but unfortiunately got the sizing slightly off the first time. We measured and reprinted, and it was perfect!

Iteration 1 spindle:

<img width="634" height="849" alt="Screenshot 2025-11-01 at 12 35 47 PM" src="https://github.com/user-attachments/assets/6904593e-a1d2-4024-a30d-f7ab1e315602" />

Iteration 2 spindle:

<img width="639" height="852" alt="Screenshot 2025-11-01 at 12 36 13 PM" src="https://github.com/user-attachments/assets/355e3b9c-7350-45cf-a6b0-cbacab52f72c" />

<img width="894" height="857" alt="Screenshot 2025-11-01 at 1 14 01 PM" src="https://github.com/user-attachments/assets/b64700d6-1a27-4cb4-9f66-d1db6c52fb53" />

Next, also tried a belt with paper. We were inspired after seeing Nengi's expressive mechanics project and decided to try it out. We used a thing piece of paper, and taped it together in belt form. We also used grease on the spindles to make it moved more smoothly. This unfortunately did not work either, and so we went back to kerf cut wood.

<img width="629" height="848" alt="Screenshot 2025-11-01 at 12 39 21 PM" src="https://github.com/user-attachments/assets/a9f1e314-81fb-4701-b2c7-21acea99386a" />

<img width="636" height="853" alt="Screenshot 2025-11-01 at 12 39 40 PM" src="https://github.com/user-attachments/assets/0f65b4dd-e081-4781-9362-5f08a38de150" />

Finally, we felt ready (ish) to cut our acrylic piece. Chris suggested we cut only the front panel first (and thank god he did), and then work our way to the next. We cut out the first piece, and then he helped us use a blow torch and a metal cylider to curve it around that radius. This was my first time curving arcylic and it was so much fun!!! Here are some images of that process:

<img width="639" height="855" alt="Screenshot 2025-11-01 at 1 01 35 PM" src="https://github.com/user-attachments/assets/9bdcac05-4b39-40cf-a8c2-e78eb4808577" />

<img width="641" height="852" alt="Screenshot 2025-11-01 at 1 01 50 PM" src="https://github.com/user-attachments/assets/362313cc-0cfa-4d03-8b5e-ceb007f3970d" />

<img width="640" height="859" alt="Screenshot 2025-11-01 at 1 02 28 PM" src="https://github.com/user-attachments/assets/cc9af2cc-b1aa-4dc1-a942-f135f08a1d4c" />

After that was bent in place, we decided to test the side panels once again in wood to make sure it fit and worked properly. In hindsight, it was good we did this, because we soon realized that since the arcylic was bent manually, there was bound to be slight imperfections. So all the calcualtions we had done before for the tolerances of the magnet and the ball bearing were not going to work. We decided to keep prototyping in wood to get the frame tolerance right. We also realized that the 3d printed spindles were too smooth and did not have enough friction to hold the belt in place. Again, inspired by Nengi's previous project, we used tiny elastic rubberbands to add friction to the surface. This worked great (Shoutout Nengi!!!). 

<img width="632" height="855" alt="Screenshot 2025-11-01 at 1 12 41 PM" src="https://github.com/user-attachments/assets/ed71cb66-8c81-431b-ad34-27eadbb1bace" />

<img width="630" height="846" alt="Screenshot 2025-11-01 at 1 12 57 PM" src="https://github.com/user-attachments/assets/329603ad-468b-4030-9071-ed124cd23c09" />

<img width="636" height="853" alt="Screenshot 2025-11-01 at 1 13 09 PM" src="https://github.com/user-attachments/assets/54bde355-2cdd-4590-9ca3-8063ce961e5f" />


At this point, Chris gave us two flat rubber pieces to try to use for the belt. Since we were slightly worried about the fragility of the kerf cut wood, we decided to try this out. We sewed the two ends of the belt together and left enough slack for it to turn. This iteration was good, but due to the friction of the belt ut kept coming off of the spindle and gettimg stuck. We decided that kerf cut was best for the belt, but started to look into ways to make it more sturdy. We tried gaffers tape, but this was sort of pointless cause it removed the ability of the kerf cut to bend, and cuased it to immediately snap. Finally we just decided that we would go with the kerf cut, no changes needed. 

<img width="637" height="855" alt="Screenshot 2025-11-01 at 1 17 30 PM" src="https://github.com/user-attachments/assets/20cb9ed9-9a0f-49f1-aec3-620d292dca88" />

<img width="637" height="855" alt="Screenshot 2025-11-01 at 1 18 20 PM" src="https://github.com/user-attachments/assets/9fcc958a-4078-4bf8-88c7-6ec2593863f5" />

<img width="637" height="852" alt="Screenshot 2025-11-01 at 1 23 41 PM" src="https://github.com/user-attachments/assets/7e49fcb0-8cd7-4521-858a-e21e525e8042" />

After sticking the magnets to the belt and getting it to a stage where it was able to move the ball bearing, we decided to cut the final acrylic pieces. We glued all the pieces together and went to test the mechanism. Just as we thought everything was working well, the ball bearing was too heavy to be moved over the curve. At this point, we decided to try another magnet. This worked fine, just that it was pulling on the belt too much and also was getting attached after dropping off the curve (which we didn't want). Next we tried covering a magnet with foil, this also worked finem though it just didn't look very polished. We decided to try with a screw and this worked great.

https://github.com/user-attachments/assets/b00af8e6-0315-413f-a8be-4a7c6c6d37f2

![PHOTO-2025-11-01-13-50-56](https://github.com/user-attachments/assets/95355d28-bd24-4f29-937c-0890b6efd4bc)

https://github.com/user-attachments/assets/de39aff5-8e4b-431d-aef4-bdcabdb76876

Next was to make the back of the outer casing, which would house the pin ball mechanism and would move the magnet back to the front of the structure after the day. Ishani cut all the pieces and glued them in the pattern. We then decided to add side panels to bring it to the front. This worked great! The last part was just to get make a stand and a box to house the electronics. And just like that, our prototype was ready!

<img width="640" height="856" alt="Screenshot 2025-11-01 at 1 52 40 PM" src="https://github.com/user-attachments/assets/fad67f22-b178-4669-9d0c-e89544fff220" />

<img width="639" height="857" alt="Screenshot 2025-11-01 at 1 51 41 PM" src="https://github.com/user-attachments/assets/785664ca-51dc-498f-811b-a0d036608538" />

<img width="638" height="856" alt="Screenshot 2025-11-01 at 1 53 05 PM" src="https://github.com/user-attachments/assets/3d4a5a22-b3de-4479-8cc6-81c4df78f0a0" />

Here are all the prototypes we made to reach the final:

<img width="629" height="743" alt="Screenshot 2025-11-01 at 1 54 34 PM" src="https://github.com/user-attachments/assets/ae1e733f-6bf7-4347-a86c-f42e0914293e" />

**Reflection**: This process truly showed me what it meant to prototype and learn from past iterations. We learnt so much with each iteration, and it ony made our final that much better. Every little thing in this display had to be carfully caliberated. It was a massive amount of work to get to our final, but I'm really proud of how we persisted through it. There were many times through this week where i was unsure this disaply woud even work (or if we would even have anything to show), but we made it work!! Shoutout to Ishani who was the best teammate to have on this project. She handled fabrication like a pro, and helped us get all our tolerances down to a T. She was always optimistic and positive, and I really needed that when I was doubting our work. Overall, I'm so happy with how our work turned out :)

# Week 9 - Electronics 
_Tuesday, 10/28/2025 - Tuesday, 11/4/2025_

The next step in the project was to get the motor running so it could be conected to our final ambient display. I used a similar setup to what I had used last time for my expresive mechanics project, but just swapped the the arduino ourt for the feather. This process was slightly hard, since I couldn't get the motor to run. I asked Manny at the makerspace for help, who helped me with all my troubleshooting. We used Last Minute engineering to figure out how to make the circuit. We tested out each inddividual part separately and also checked to see if any connection was loose. Through this, we realized that both the H-bridges had been fried. Once this was changed, it worked as intended!

We then merged the motor code we had used with the previous API call code. This made the motor move everytime someone took 10 steps with the Fitbit watch on. It was slightly jerky at first, but after changing the speed and delay it worked as we wanted!

Here is an image of the circuit:

<img width="636" height="849" alt="Screenshot 2025-11-01 at 2 20 31 PM" src="https://github.com/user-attachments/assets/7e9bf062-64de-4c4e-a359-d0d9e25deb67" />

<img width="1121" height="830" alt="Screenshot 2025-11-01 at 2 21 34 PM" src="https://github.com/user-attachments/assets/176dd1da-84af-432d-92f2-fa131dcb734e" />

<img width="634" height="848" alt="Screenshot 2025-11-01 at 2 23 35 PM" src="https://github.com/user-attachments/assets/4dc4cdf1-d576-4b3e-b1a1-2ebac46dabce" />

Here is the final Code

```C++
//Code and circuit connections were sourced from a combination of the fitbit api guide (https://dev.fitbit.com/build/reference/web-api/developer-guide/), last minute engineers (https://lastminuteengineers.com/esp32-l298n-dc-motor-control-tutorial/) and chatgpt

#include <WiFi.h>
#include <HTTPClient.h>
#include <ArduinoJson.h>
#include <secrets.h>

// Wi-Fi Credentials Berkeley
const char* ssid = SECRET_SSID;
const char* password = SECRET_PASSWORD;

// Fitbit API Info
String accessToken = "eyJhbGciOiJIUzI1NiJ9.eyJhdWQiOiIyM1RHUzkiLCJzdWIiOiI1UFhEUFIiLCJpc3MiOiJGaXRiaXQiLCJ0eXAiOiJhY2Nlc3NfdG9rZW4iLCJzY29wZXMiOiJyaHIgcmFjdCBycHJvIiwiZXhwIjoxNzYxODgzOTQzLCJpYXQiOjE3NjE4NTUxNDN9.NVzQlylWZje3Cy5aa6LEDsJon--OWPqYxNarFmiklRo";
const char* fitbitEndpoint = "https://api.fitbit.com/1/user/-/activities/steps/date/today/1d.json";

// Motor pin Connections
int enA = 14;  // ENA pin
int in1 = 27;  // IN1 pin
int in2 = 12;  // IN2 pin

// PWM Setup
const int freq = 1000;
const int pwmChannelA = 0;
const int resolution = 8;
const int dutyCycle = 200;  // Max speed PWM value (0–255)

// Timing
const int interval = 10000; // Fetch Fitbit every 10 seconds
const int motorRunTime = 5000; // Motor runs for 5 seconds when triggered

// Step tracking
int lastThreshold = 0;

void setup() {
  Serial.begin(115200);

  // Motor setup
  pinMode(in1, OUTPUT);
  pinMode(in2, OUTPUT);
  ledcAttachChannel(enA, freq, resolution, pwmChannelA);
  stopMotor();

  // Wi-Fi connection
  Serial.print("Connecting to Wi-Fi");
  WiFi.begin(ssid, password);
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("\nConnected to Wi-Fi");

  // Initial Fitbit fetch
  getFitbitSteps();
}

void loop() {
  getFitbitSteps();
  delay(interval);
}

// Fetch Fitbit steps and trigger motor rotation
void getFitbitSteps() {
  if (WiFi.status() == WL_CONNECTED) {
    HTTPClient http;
    http.begin(fitbitEndpoint);
    http.addHeader("Authorization", "Bearer " + accessToken);

    int httpResponseCode = http.GET();

    if (httpResponseCode == 200) {
      String payload = http.getString();
      StaticJsonDocument<512> doc;
      DeserializationError error = deserializeJson(doc, payload);

      if (!error) {
        int currentSteps = doc["activities-steps"][0]["value"].as<int>();
        Serial.print("Current steps: ");
        Serial.println(currentSteps);

        int currentThreshold = currentSteps / 10;  // Every 10 steps, the motpr is triggered once

        if (currentThreshold > lastThreshold) {
          Serial.println("🚶 New 10-step threshold reached!");
          runMotorForDuration(motorRunTime); // Motor is tuned to run for 5 seconds
          lastThreshold = currentThreshold;
        }
      } else {
        Serial.println("Failed to parse JSON");
      }
    } else {
      Serial.print("HTTP request failed, code: ");
      Serial.println(httpResponseCode);
    }

    http.end();
  } else {
    Serial.println("Wi-Fi not connected!");
  }
}

// Run motor for fixed time with smooth acceleration
void runMotorForDuration(int durationMs) {
  int rampSteps = 50;
  int rampDelay = 10;
  int rampTime = rampSteps * rampDelay;

  // Ramp up
  for (int speed = 0; speed <= dutyCycle; speed += dutyCycle / rampSteps) {
    ledcWrite(enA, speed);
    digitalWrite(in1, HIGH);
    digitalWrite(in2, LOW);
    delay(rampDelay);
  }

  // Run at full speed (minus ramp in/out time)
  int runTime = durationMs - (2 * rampTime);
  if (runTime > 0) delay(runTime);

  // Ramp down
  for (int speed = dutyCycle; speed >= 0; speed -= dutyCycle / rampSteps) {
    ledcWrite(enA, speed);
    delay(rampDelay);
  }

  stopMotor();
}

// Stop motor helper function
void stopMotor() {
  digitalWrite(in1, LOW);
  digitalWrite(in2, LOW);
  ledcWrite(enA, 0);
}
```

***Reflection***: This process was made a lot easier since we had already worked with a DC motor on the last project. Troubleshooting was also fun, though I wonder why all my H-bridges fried?

# Week 9 - Fabrication
_Thursday, 10/30/2025 - Thursday, 11/6/2025_

On Thursday, we had our critique. We displayed our project to the class, and received some great feedback and comments! Our display worked so well, and we were able to demonstrate how a person would use this device on a daily basis. It felt so rewarding, especically after all that prototyping, troubleshooting and iterations to see it come to life. I'm really proud of the work we have put out.

Here is our Demo Video of the working model:

https://github.com/user-attachments/assets/69eb9847-ac53-41a3-af83-669bbd30987f

Here is a systems map of how this system is to be used by a user and how it works:

<img width="1678" height="617" alt="Screenshot 2025-11-01 at 2 41 06 PM" src="https://github.com/user-attachments/assets/46e27b83-c99e-4583-a169-93cbd1465e3d" />


# Week 10 - Electronics
_Tuesday, 11/04/2025 - Tuesday, 11/11/2025_

After the success that was everyon's ambient display projects, it was time for us all to begin thinking about what we wanted to do for our final TDF projects of the semester. The teaching team had asked us all to bring poster proposals of the projects we wanted to work on in the last 5 weeks, pin them up to share with the class. We were asked to read up about other projects, and find people to collaborate with. 

I had two proposal ideas. The first one was a computer vision trained tennis instructor. As someone who enjoys playing tennis, I always find it challenging to figure out my mistakes/where I could improve without a coach on standby. And more often than not, minute incorrect changes such as how a player grips a racket or how they set up for an incoming ball can be the cause for future sports injuries. I wanted to create a product that was able to predict and provide real-time information and feedback to a player based on their game. The second project proposal I had, was to design and build a robot prototype Pipebot with the goal of creating a novel solution to clear blocked pipes and sewers lines. The project was a last minute addition to my proposal, after I had faced a clogged bathtub situation earlier athat morning. So, in true design spirit, I decided to put two and two together. 

After displaying my proposals, Skye, Alistair and I decided to form a team and tackle the PipeBot project. After forming project teams, we had a meeting where we built out a project planning board on theliminal.design, a public version of which can be found [here](https://theliminal.design/board/public/d224c769-b92d-49c0-9ea0-173eaa4d703c). In this first meeting we discussed a variety of early ideas for locomotion within a pipe, blockage detection and clearnace. As can be seen in the liminal design board, we also had preliminary discussions of biomicicry and soft robotics, and the different technologies we may need to acquire and integerate based on the chosen design (For example, if we were to investigate some kind of pneumatic bladder system, we would need to acquire and test with solenoids and pneumatics and integrate them into our microcontroller). 

<img width="1348" height="934" alt="Screenshot 2025-12-11 at 5 58 55 PM" src="https://github.com/user-attachments/assets/9814cd49-1a06-4874-827a-c4ebcfe17eae" />
_Screenshot of the Liminal Board we made_

After this first meeting, we made our project proposal and fleshed out the more granular details of our project including our timeline, the types of experiemnets we were going to do and what we expeceted to achieve by the end of the project. 

Here is the link to the proposal:

[project Proposal](https://docs.google.com/document/d/1nQPd7dcVCOMeYm6GPB019YwkTV47S4BaPJA_fgNv3n4/edit?usp=sharing)


<img width="1327" height="623" alt="Screenshot 2025-12-11 at 6 14 52 PM" src="https://github.com/user-attachments/assets/da587958-f5a4-4174-bc5a-a29b5587809c" />

_Early system architecture diagram from the project proposal_


<img width="866" height="597" alt="Screenshot 2025-12-11 at 6 15 35 PM" src="https://github.com/user-attachments/assets/905150f3-2ff3-4e76-becb-751e35a76cb9" />

_The teams proposed timeline for project experimentation and delivery_

After the proposal was created, and we had a point of reference to start from, we began conducting secondary research and brainstorming ideas of how we wanted to execute each experiment. We found a few great research papers and soft robotic solutions online that we took as inspiration. Below are a few images of those explorations:

<img width="906" height="903" alt="Screenshot 2025-12-11 at 6 49 57 PM" src="https://github.com/user-attachments/assets/1be14907-480c-4105-9125-e3a6b545d33c" />

_One Linear Actuator Peristaltic Motion_


<img width="485" height="457" alt="Screenshot 2025-12-11 at 6 53 40 PM" src="https://github.com/user-attachments/assets/2994e0e8-e840-44b8-b470-d05da48ae03a" />

_Rotating movement with Wheels_


<img width="607" height="609" alt="Screenshot 2025-12-11 at 6 58 46 PM" src="https://github.com/user-attachments/assets/c61df22b-ab07-48a5-b09e-f1f975e3ec86" />

_Peristaltic Movement with pneumatics_

































