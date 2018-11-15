/*
 * Copyright 2018 Damien Bobrek, Daniel Bertak, Nicolas Bontems
 * Permission is hereby granted, free of charge, to any person obtaining a copy
 * of this software and associated documentation files (the "Software"), to 
 * deal in the Software without restriction, including without limitation the 
 * rights to use, copy, modify, merge, publish, distribute, sublicense, and/or 
 * sell copies of the Software, and to permit persons to whom the Software is 
 * furnished to do so, subject to the following conditions:
 * 
 * The above copyright notice and this permission notice shall be included in 
 * all copies or substantial portions of the Software.
 * 
 * THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR 
 * IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, 
 * FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE 
 * AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER 
 * LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING 
 * FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS
 * IN THE SOFTWARE.
*/


// Code for LCD configuration modified from code found at
// http://learning.grobotronics.com/2013/07/controlling-lcd-displays-with-the-hitachi-hd44780-driver/ 
// Can also be found on Github at
// https://gist.github.com/grobotronics/6062488
/*
  The circuit:
 * LCD RS pin to digital pin 12
 * LCD Enable pin to digital pin 11
 * LCD D4 pin to digital pin 9
 * LCD D5 pin to digital pin 8
 * LCD D6 pin to digital pin 7
 * LCD D7 pin to digital pin 6
 * LCD R/W pin to ground
 * 10K potentiometer:
 * ends to +5V and ground
 * wiper to LCD VO pin (pin 3)
 */
// include the library code:
#include <LiquidCrystal.h>
// initialize the library with the numbers of the interface pins
LiquidCrystal lcd(12, 11, 9, 8, 7, 6);


// Code for Joystick configuration modified from code found at
// https://www.brainy-bits.com/arduino-joystick-tutorial/
// Arduino pin numbers
const int SW_pin = 2; // digital pin connected to switch output
const int X_pin = 0; // analog pin connected to X output
const int Y_pin = 1; // analog pin connected to Y output


// Code for 7-segment display modified from code found at
// https://docs.labs.mediatek.com/resource/linkit7697-arduino/en/tutorial/driving-7-segment-displays-with-74hc595
//
// Use one 74HC595 to control a common-anode seven-segment display
//

// pin 11 of 74HC595 (SHCP)
const int bit_clock_pin = 5;
// pin 12 of 74HC595 (STCP)
const int digit_clock_pin = 4;
// pin 14 of 74HC595 (DS)
const int data_pin = 3;

// digit pattern for a 7-segment display
const byte digit_pattern[16] =
{
  B00111111,  // 0
  B00000110,  // 1
  B01011011,  // 2
  B01001111,  // 3
  B01100110,  // 4
  B01101101,  // 5
  B01111101,  // 6
  B00000111,  // 7
  B01111111,  // 8
  B01101111,  // 9
  B01110111,  // A
  B01111100,  // b
  B00111001,  // C
  B01011110,  // d
  B01111001,  // E
  B00000000,   // off
};

 const byte digit_pattern2[16] =
{
  B00000001,  // 0
  B00000010,  // 1
  B00000100,  // 2
  B00001000,  // 3
  B00010000,  // 4
  B00100000,  // 5
  B01000000,  // 6
  B10000000,  // 7
  B01111111,  // 8
  B01101111,  // 9
  B01110111,  // A
  B01111100,  // b
  B00111001,  // C
  B01011110,  // d
  B01111001,  // E
  B00000000,   // off
};


void update_one_digit(int data)
{
  int i;
  byte pattern;
  
  // get the digit pattern to be updated
  pattern = digit_pattern[data];

  // turn off the output of 74HC595
  digitalWrite(digit_clock_pin, LOW);
  
  // update data pattern to be outputed from 74HC595
  // because it's a common anode LED, the pattern needs to be inverted
  shiftOut(data_pin, bit_clock_pin, MSBFIRST, pattern);
  
  // turn on the output of 74HC595
  digitalWrite(digit_clock_pin, HIGH);
}

void update_one_digit2(int data)
{
  int i;
  byte pattern;
  
  // get the digit pattern to be updated
  pattern = digit_pattern2[data];

  // turn off the output of 74HC595
  digitalWrite(digit_clock_pin, LOW);
  
  // update data pattern to be outputed from 74HC595
  // because it's a common anode LED, the pattern needs to be inverted
  shiftOut(data_pin, bit_clock_pin, MSBFIRST, pattern);
  
  // turn on the output of 74HC595
  digitalWrite(digit_clock_pin, HIGH);
}


// Joystick poll code
typedef enum {no_action = 0, button_pressed, up, upright, right, downright, 
              down, downleft, left, upleft} command_t;

int poll_joystick()
{
  if (digitalRead(SW_pin) == 0)
    return button_pressed;

  unsigned int x_val = analogRead(X_pin);
  unsigned int y_val = analogRead(Y_pin);

  if(y_val < 23)
  {
    if(x_val < 23)
      return upleft;
    else if (x_val > 1000)
      return upright;

    return up; // if there's no serious x-axis tilt, just return the y-axis one
  }
  else if (y_val > 1000)
  {
    if(x_val < 23)
      return downleft;
    else if (x_val > 1000)
      return downright;

    return down; // if there's no serious x-axis tilt, just return the y-axis one
  }
  
  if(x_val < 23)
    return left;
  else if (x_val > 1000)
    return right;

  return no_action;
}

void setup()
{
  // LCD 
  // set up the LCD's number of columns and rows: 
  lcd.begin(16, 2);
  // Print a message to the LCD.
  lcd.print("hello, world!");

  // Joystick setup
  pinMode(SW_pin, INPUT);
  digitalWrite(SW_pin, HIGH);

  // 7-segment display setup
  pinMode(data_pin, OUTPUT);
  pinMode(bit_clock_pin, OUTPUT);
  pinMode(digit_clock_pin, OUTPUT);  
}


void loop()
{
  int score=5;
  update_one_digit(score);
  // LCD 
  // set up the LCD's number of columns and rows: 
  lcd.begin(16, 2);
  // Print a message to the LCD.
     lcd.print("HELLO PLAYER!");
     delay(1000);
     lcd.clear();
     lcd.print("With joystick,");
     delay(1000);
     lcd.clear();
     lcd.print("go UP, DOWN,"); 
     delay(1000);
     lcd.clear();
      lcd.print("LEFT, or RIGHT,");
      delay(1000);
     lcd.clear();
     
     lcd.clear();
     

  while (1)
  {
  command_t jc = (command_t)poll_joystick();

if ( jc == button_pressed )
   {lcd.print("FREE MODE");
    delay(800);
     lcd.clear();
  freemode();
   }
  
  if ( jc==left ) 
     {lcd.print("LEFT");
  score = score+rand() % 2;
  score = score-rand() % 3;
  update_one_digit(score);
     delay(800);
     lcd.clear();
   }
 
   
  if ( jc==right )
    {lcd.print("right");
  score = score-rand()%2;
  score = score+rand()%4;
  update_one_digit(score);
     delay(800);
     lcd.clear();
   }

   
  if ( jc==up) 
    {lcd.print("up");
  score = score+rand() % 5;
  score = score-rand() % 4;
  update_one_digit(score);
     delay(800);
     lcd.clear();
   }
  
  
  if ( jc==down)
    {lcd.print("down!");
  score = score-rand() % 2;
  score = score+rand() % 4;
  update_one_digit(score);
     delay(800);
     lcd.clear();
   }
  
  if (score<0)
  {while(1){
    command_t jc = (command_t)poll_joystick();
    lcd.print("YOU LOST");
  
  update_one_digit(0);
  delay(200);
  lcd.clear();
  if ( jc == button_pressed )
   {lcd.print("FREE MODE");
    delay(800);
     lcd.clear();
  freemode();
   }
 
  }
  }
   
  
  
  if (score>9)
  {
    while(1){
      command_t jc = (command_t)poll_joystick();
    lcd.print("YOU WON!!");
  update_one_digit(10);
  delay(200);
  lcd.clear();
  
  update_one_digit(0);
    delay(200);
    
    if ( jc == button_pressed )
   {lcd.print("FREE MODE");
    delay(800);
     lcd.clear();
  freemode();
   }
   
   
  }
  }

}
  

}

void freemode(void)
{
  delay(500);
 

  command_t jc = (command_t)poll_joystick();

while(1){
  command_t jc = (command_t)poll_joystick();
if ( jc==no_action ) 
   { lcd.print("GO AHEAD");
  
  update_one_digit2(6);
     delay(20);
     lcd.clear();}
     
   if ( jc==upleft ) 
     {lcd.print("UPLEFT");
  
  update_one_digit2(5);
      delay(20);
     lcd.clear();
   }
   
  if (jc==downleft ) 
     {lcd.print("DOWNLEFT");
  update_one_digit2(4);
      delay(20);
     lcd.clear();
   }
   
 
  if ( jc==upright)
    {lcd.print("UPRIGHT");
  update_one_digit2(1);
      delay(20);
     lcd.clear();
   }
   if (jc==downright )
    {lcd.print("DOWNRIGHT");
  update_one_digit2(2);
     delay(20);
     lcd.clear();
   }
   
  if ( jc==up) 
    {lcd.print("UP");
  update_one_digit2(0);
      delay(20);
     lcd.clear();
   }
  
  
  if ( jc==down)
    {lcd.print("DOWN");
  update_one_digit2(3);
      delay(20);
     lcd.clear();
   }
   
   if ( jc == button_pressed )
   {lcd.print("PLAYMODE");
  loop();
     delay(800);
     lcd.clear();
   }
     }
}
