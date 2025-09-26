# Technology-Design-Foundations :robot: :hammer_and_wrench:
A repository of my explorations as part of the 'Technology Design Foundations' course

## Table of Contents 
[Week 1 - Electronics](#week-1---electronics)

[Week 1 - Fabrication](#week-1---fabrication)

[Week 2 - Electronics](#week-2---electronics)

[Week 2 - Fabrication](#week-2---fabrication)

[Week 3 - Electronics](#week-3---electronics)

[Week 3 - Fabrication](#week-3---fabrication)


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













































