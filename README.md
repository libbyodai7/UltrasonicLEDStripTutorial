# UltrasonicLEDStripTutorial
A tutorial for Product Design Yr 2 students showing how to connect an LED Strip to an ultrasonic sensor via Arduino.
This project creates a sensor that lights up like a parking light, more of the strip will light up the closer an object is to the sensor. 

# Things you will need:

* An Arduino (I use an Uno R3)
* Jumper wires
* A HC-SR04 ultrasonic distance sensor 

(Link to buy: https://thepihut.com/products/ultrasonic-distance-sensor-hcsr04?variant=1054704288&currency=GBP&utm_medium=product_sync&utm_source=google&utm_content=sag_organic&utm_campaign=sag_organic&gclid=Cj0KCQiAhs79BRD0ARIsAC6XpaVypLZlap8l62f8JR5IFJDFWGiK4Qdg0jtQXO1iTJ-mvDenLQNAx_caAvtQEALw_wcB)

* A 5V LED strip -> it is important to make sure this is an addressable strip. This means it contains microchips so you can program it directly. You can also program non addressable strips but it is far more annoying. Also, non addressable strips are 12V which means you need to add any capacitors and resistors.

(Link to buy: https://www.amazon.co.uk/CHINLY-WS2812B-Individually-addressable-Waterproof/dp/B07TPSB35N/ref=sxts_sxwds-bia-wc-nc-drs1_0?adgrpid=68953279172&cv_ct_cx=chinly+led&dchild=1&gclid=Cj0KCQiAhs79BRD0ARIsAC6XpaVbpuyZa2CyvJ81_stluFxfMmWluwkadKK5VUFugelQ5RDJll7iOVcaAk5cEALw_wcB&hvadid=318284761871&hvdev=c&hvlocphy=9046952&hvnetw=g&hvqmt=e&hvrand=10621946272758932147&hvtargid=kwd-570279153789&hydadcr=26217_1767513&keywords=chinly+led&pd_rd_i=B07TPSB35N&pd_rd_r=451506e5-5b8a-44dc-b254-db4ff006aabc&pd_rd_w=riEhO&pd_rd_wg=m7w1e&pf_rd_p=344521d5-1bc6-4c7a-9776-9c81f49df30a&pf_rd_r=BSHFFT5TSQ6DNVK4V5QF&psc=1&qid=1605652357&sr=1-1-0f636240-5ebc-47e0-a5f7-bb2a7fa20976&tag=googhydr-21 )

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

Once installed


