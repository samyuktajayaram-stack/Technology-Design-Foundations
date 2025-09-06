# Technology-Design-Foundations
A repository of my explorations as part of the 'Technology Design Foundations' course

## 'Hello World' + Blinking LED 
<ins>Tuesday, 09/09/2025<ins>

Based on our experimentation with the Arduino 'Hello World' program on Tuesday, we were tasked with merging the program with the 'LED Blink' code. The objective was for the serial monitor to output 'Hello World' while simultaneously having the LED on the Arduino blink.

I came into this thinking it would be an easy task to complete (considering we had both of the source code files available), but to my surprise, I ran into a couple of issues along the way.

At first. this is how I merged the code:
```C++
int led = 13;
void setup() {
Serial.begin(9600)
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



