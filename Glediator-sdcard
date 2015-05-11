// Glediator Arduino UNO sketch by Jens Andrée
// 500k bauds with 80 pixels no problem
// sdcard stream for stand-alone operation.

#include <FastLED.h>
#include <SPI.h>
#include <SD.h>

#define NUM_LEDS 80
#define DATA_PIN 2
#define CLOCK_PIN 3
#define CMD_NEW_DATA 1
#define BAUD_RATE 500000  //if using Glediator via serial

File fxdata;
CRGB leds[NUM_LEDS];

void setup()
{
  FastLED.addLeds<WS2801, DATA_PIN, CLOCK_PIN, RGB>(leds, NUM_LEDS); //se doc for different LED strips
//  Serial.begin(BAUD_RATE); // when using Glediator via usb
  Serial.begin(115200);
  
  for(int y = 0 ; y < NUM_LEDS ; y++) 
  {
    leds[y] = CRGB::Black; // set all leds to black during setup
  }
  FastLED.show();

  pinMode(10, OUTPUT); // CS/SS pin as output for SD library to work.
  digitalWrite(10, HIGH); // workaround for sdcard failed error...

  if (!SD.begin(4))
  {
    Serial.println("sdcard initialization failed!");
    return;
  }
  Serial.println("sdcard initialization done.");
  
  // test file open
  fxdata = SD.open("myanim.dat");  // read only
  if (fxdata)
  {
    Serial.println("file open ok");      
    fxdata.close();
  }
}

int serialGlediator ()
{
  while (!Serial.available()) {}
  return Serial.read();
}

void loop()
{

// uncomment for Glediator  
//  while (fileGlediator () != CMD_NEW_DATA) {}
//  Serial.readBytes((char*)leds, NUM_LEDS*3);

  fxdata = SD.open("myanim.dat");  // read only
  if (fxdata)
    {
      Serial.println("file open ok");      
    }

  while (fxdata.available()) 
  {
    fxdata.readBytes((char*)leds, NUM_LEDS*3);
    FastLED.show();
    delay(50); // set the speed of the animation. 20 is appx ~ 500k bauds
  }
  
  // close the file in order to prevent hanging IO or similar throughout time
  fxdata.close();
}
