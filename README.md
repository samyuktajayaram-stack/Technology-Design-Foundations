# Technology-Design-Foundations :robot: :hammer_and_wrench:
A repository of my explorations as part of the 'Technology Design Foundations' course

## Table of Contents 
[Week 1 - Electronics](#week-1---electronics)

[Week 1 - Fabrication](#week-1---fabrication)

# Week 1 - Electronics 
_Tuesday, 02/09/2025 - Tuesday, 09/09/2025_

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

Finally, my rings were complete! I walked outside and picked up some leaves and flowers that had fallen on the ground and placed them in my ring. For my pegboard ring, I wove the words MDes into it. For further iterations of these rings, I pan on adding a support shaft on the side of the nature ring to better hold the plants I place in it. For the pegboard, I would like to place the holes slightly wider apart and have an odd number of holes in each column. 

Here are a few images of my rings:

![PHOTO-2025-09-11-00-02-35](https://github.com/user-attachments/assets/243daa2b-7530-429b-a62f-35d827dbcc6c)

![PHOTO-2025-09-11-00-02-52](https://github.com/user-attachments/assets/16ea0054-0931-4fad-a5a8-93319ae14934)

![PHOTO-2025-09-11-00-03-05](https://github.com/user-attachments/assets/128b0378-b9da-4555-a99c-dea0601e1a88)

![PHOTO-2025-09-11-00-03-21](https://github.com/user-attachments/assets/ea890336-6cb1-46fb-bb35-093b448c011f)

![PHOTO-2025-09-11-00-04-17](https://github.com/user-attachments/assets/92b8ecaa-1ef8-45db-912c-50278a841425)

![PHOTO-2025-09-11-00-04-28](https://github.com/user-attachments/assets/4de54e2d-03e1-4c78-84d2-399809539abb)

**Reflection**: I had so much fun making these rings. Right from getting to know how to use the laser cutting, to revising errors with each iteration and finally seeing the ring take shape was really rewarding. I want to be able to further experiment with this machine by etching and engracing as well as making movable mechanical objects using just parts i cut from the laser cutter. 
