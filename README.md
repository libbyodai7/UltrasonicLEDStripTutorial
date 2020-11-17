# UltrasonicLEDStripTutorial
A tutorial for Product Design Yr 2 students showing how to connect an LED Strip to an ultrasonic sensor via Arduino.
This project creates a sensor that lights up like a parking light, more of the strip will light up the closer an object is to the sensor. 

![video of the project](https://github.com/libbyodai7/UltrasonicLEDStripTutorial/blob/main/ledUltra.gif)

# Things you will need:

* An Arduino (I use an Uno R3)
* Jumper wires
* A HC-SR04 ultrasonic distance sensor 

[Link to buy](https://thepihut.com/products/ultrasonic-distance-sensor-hcsr04?variant=1054704288&currency=GBP&utm_medium=product_sync&utm_source=google&utm_content=sag_organic&utm_campaign=sag_organic&gclid=Cj0KCQiAhs79BRD0ARIsAC6XpaVypLZlap8l62f8JR5IFJDFWGiK4Qdg0jtQXO1iTJ-mvDenLQNAx_caAvtQEALw_wcB)

* A 5V LED strip -> it is important to make sure this is an addressable strip. This means it contains microchips so you can program it directly. You can also program non addressable strips but it is far more annoying. Also, non addressable strips are 12V which means you need to add any capacitors and resistors.

[Link to buy](https://www.amazon.co.uk/CHINLY-WS2812B-Individually-addressable-Waterproof/dp/B07TPSB35N/ref=sxts_sxwds-bia-wc-nc-drs1_0?adgrpid=68953279172&cv_ct_cx=chinly+led&dchild=1&gclid=Cj0KCQiAhs79BRD0ARIsAC6XpaVbpuyZa2CyvJ81_stluFxfMmWluwkadKK5VUFugelQ5RDJll7iOVcaAk5cEALw_wcB&hvadid=318284761871&hvdev=c&hvlocphy=9046952&hvnetw=g&hvqmt=e&hvrand=10621946272758932147&hvtargid=kwd-570279153789&hydadcr=26217_1767513&keywords=chinly+led&pd_rd_i=B07TPSB35N&pd_rd_r=451506e5-5b8a-44dc-b254-db4ff006aabc&pd_rd_w=riEhO&pd_rd_wg=m7w1e&pf_rd_p=344521d5-1bc6-4c7a-9776-9c81f49df30a&pf_rd_r=BSHFFT5TSQ6DNVK4V5QF&psc=1&qid=1605652357&sr=1-1-0f636240-5ebc-47e0-a5f7-bb2a7fa20976&tag=googhydr-21 )

# Steps

Connect the components to the Arduino Uno. I use a mini breadboard so I can connect both the sensor and LED strip to the Uno's 5V pin. The connections go:

# LED Lights

* 5V - 5V 
* DI - PIN 6
* GND - GND

# Ultrasonic Sensor
* VCC-5V
* Trig - PIN 3
* Echo - PIN2
* GND - GND

# Code

To program the Arduino we need to download the NeoPixel library from Adafruit. This makes it really easy to program.
In the Arduino IDE go to 
Tools -> Manage Libraries
Then search for Adafruit NeoPixel and it should come up.

Once installed download the code files in this project and open them in Arduino IDE.

```
#define echoPin 2 // attach pin D2 Arduino to pin Echo of HC-SR04
#define trigPin 3 //attach pin D3 Arduino to pin Trig of HC-SR04

// defines variables
long duration; // variable for the duration of sound wave travel
int distance; // variable for the distance measurement
int pixelLength; //variable to hold the number of pixels that need to light up


#include <Adafruit_NeoPixel.h>
#ifdef __AVR__
 #include <avr/power.h> // Required for 16 MHz Adafruit Trinket
#endif

// Which pin on the Arduino is connected to the NeoPixels?
// On a Trinket or Gemma we suggest changing this to 1:
#define LED_PIN    6

// How many NeoPixels are attached to the Arduino?
#define LED_COUNT 60

// Declare our NeoPixel strip object:
Adafruit_NeoPixel strip(LED_COUNT, LED_PIN, NEO_GRB + NEO_KHZ800);
```
Defines some of the starting variables. We set the pins for the sensor and LED strip. We also define distance variables and vaiables to hold the amount of LEDS we want to light up.

```
void setup() {
  pinMode(trigPin, OUTPUT); // Sets the trigPin as an OUTPUT
  pinMode(echoPin, INPUT); // Sets the echoPin as an INPUT
  Serial.begin(9600); // // Serial Communication is starting with 9600 of baudrate speed
  Serial.println("Ultrasonic Sensor HC-SR04 Test"); // print some text in Serial Monitor
  Serial.println("with Arduino UNO R3");

  strip.begin();           // INITIALIZE NeoPixel strip object (REQUIRED)
  strip.show();            // Turn OFF all pixels ASAP
  strip.setBrightness(50); 
}
```
Initialises the sensor and serial communication and the strip.

```
void loop() {
 // strip.clear();  
  // Clears the trigPin condition
  digitalWrite(trigPin, LOW);
  delayMicroseconds(2);
  // Sets the trigPin HIGH (ACTIVE) for 10 microseconds
  digitalWrite(trigPin, HIGH);
  delayMicroseconds(10);
  digitalWrite(trigPin, LOW);
  // Reads the echoPin, returns the sound wave travel time in microseconds
  duration = pulseIn(echoPin, HIGH);
  // Calculating the distance
  distance = duration * 0.034 / 2; // Speed of sound wave divided by 2 (go and back)

```
Gets the distance from the sensor and converts it into a readbale format (cm).

So now we have the distance from the sensor, we just need to control the LED strip with the values it has given us.

```
   if(distance > 40){
    pixelLength = 1;
    colorWipe(strip.Color(255,   0,   0), 50, pixelLength); // Red
    Serial.println("no lights"); 
  }
```
Here we are turning all the LED strip off if the distance is less than 40cm.

```
 if(distance < 40){
    pixelLength =  map(distance, 40, 0, 0, 60);
    colorWipe(strip.Color(255,   0,   0), 50, pixelLength); // Red
  };
  ```
  
  If the distance is less than 40cm we are using the [map function](https://www.arduino.cc/reference/en/language/functions/math/map/) to figure out how many lights to light up. The function takes the distance from 40 going down to 0 and then maps that to a value from 0 to 60 (the maximum number of LED pixels.) So if something is 40cm away, 0 leds will light up. If something is 0 cm away, 60 leds will light up. If something is in the middle, 20cm away, 30 leds will light up.)
  
  The last thing to do is write the colorWipe function to light up the strip. I used the strandTest from the example in the Adafruit Neopixel library as a base.
  
  ```
  void colorWipe(uint32_t color, int wait, int pixel) {
   strip.clear();  //turns all the lights off so they can get a new value
  for(int i=0; i<pixel; i++) { // For each pixel in strip until we get to the number we calculted before
    strip.setPixelColor(i, color);         //  Set pixel's color (in RAM)
    strip.show();                          //  Update strip to match
    delay(wait);                           //  Pause for a moment
  }

  delay(300); 
    
}
```

Here we use a for loop to turn each light to be red until we get to the pixelLength value we calculated before.

And that's it! If you're got this working see if you can change some of the values in the code to try and change the led color, make the leds last longer or make the led "follow" the object.
