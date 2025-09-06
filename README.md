# Technology-Design-Foundations
A repository of my explorations as part of the 'Technology Design Foundations' course

## 'Hello World' + Blinking LED 
Tuesday, 09/09/2025

Based on our experimentation with the Arduino 'Hello World' program on tuesday, we were tasked with merging the program with the 'LED Blink' code. The objective was for the serial monitor to output 'Hello World' while simultaneously having the LED on the Arduino blink.

I came into this thinking it would be an easy task to complete (considering we had both of the source code files available), but to my surprise, I ran into a couple of issues along the way.

At first. this is how I merged the code:
```
/*
  Hello World
*/

int led = 13;
void setup() {
//initialize serial communications at 9600 baud rate
Serial.begin(9600)
pinMode(led, OUTPUT);
}
void loop(){
//send 'Hello, world!' over the serial port
Serial.println("Hello, world!");
//wait 100 milliseconds so we don't drive ourselves crazy
delay(1000);
}
// the loop function runs over and over again forever
void loop() {
  digitalWrite(led, HIGH);   // turn the LED on (HIGH is the voltage level)
  delay(1000);                       // wait for a second
  digitalWrite(led, LOW);    // turn the LED off by making the voltage LOW
  delay(1000);                       // wait for a second
}
```

