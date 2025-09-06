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

But in true programming fashion, there was another problem to fix. Every so often, the serial monitor would display the incorrect word. So, instead of saying 'Hello World', it would sometimes just say 'Hell!'.

<img width="260" height="171" alt="Screenshot 2025-09-05 at 9 12 46 PM" src="https://github.com/user-attachments/assets/1de0880e-a174-42a2-bff8-bb0ddc5401c5" />

At first I thought that maybe the baud rate in the code wasn't matching the one on the serial monitor. But the two were the set to the same rate. After a quick search, I found that the issue could be that it was printing the word too frequently which was overflowing the serial monitor. I changed the delay to 500, and this seemed to do the trick. 

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


Seeing the program run (and actaully do it correctly), was so cool. I felt like I had actually made something work. I decided to experiment with an external LED light next. 

## Blinking external LED

I began by first figuring out which leg of the LED was the anode and which was the cathode. I didn't want to shortcirucit my LED on the first try. I learnt that the longer led was the anode (+ve) and the cathode was the shorter led (-ve). 

After this, I needed to find the value of the resistors in my kit. I found a great tool online that helped me with this. I plugged in the band colours, and it gave me the ohm value of the resistor. I didn't end up needing to calculate the value myself. 

<img width="443" height="642" alt="Screenshot 2025-09-06 at 12 54 57 PM" src="https://github.com/user-attachments/assets/d5ca37da-9f9d-4390-b74e-467540020893" />
Link to the site: https://www.calculator.net/resistor-calculator.html?bandnum=4&band1=brown&band2=black&band3=blue&multiplier=orange&tolerance=gold&temperatureCoefficient=brown&type=c&x=Calculate

I then created the actual circuit using the Arduino Uno, 2 jumper cables (male to male), the resistor (330 ohm) and a green LED. I followed the circuit diagram provided in Sudhu's repository, and plugged in the source code. I then hit upload and watched the magic happen!

Here's a video of the program running: 




