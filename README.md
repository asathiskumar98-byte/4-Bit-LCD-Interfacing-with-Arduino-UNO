🧠 4-Bit LCD Interfacing with Arduino UNO
📖 Overview

This project demonstrates how to interface a 16x2 LCD display with an Arduino UNO using 4-bit communication mode.
The 4-bit mode reduces pin usage by sending data in two nibbles (4 bits each), allowing you to save Arduino I/O pins for other peripherals.

⚙️ Hardware Requirements

Arduino UNO

16x2 LCD Display

Jumper Wires

Breadboard

Potentiometer (for contrast adjustment)

🔌 Pin Connections
LCD Pin	Description	Arduino Pin
RS	Register Select	D3
RW	Read/Write	D4
EN	Enable	D5
D4	Data Bit 4	D10
D5	Data Bit 5	D11
D6	Data Bit 6	D12
D7	Data Bit 7	D13
💻 Arduino Code
const int RS = 3;
const int RW = 4;
const int EN = 5;
const int D4 = 10;
const int D5 = 11;
const int D6 = 12;
const int D7 = 13;

void printdata(unsigned char data)
{
  if(data&0x10 == 0x10) { digitalWrite(D4,HIGH); }
  else { digitalWrite(D4,LOW); }

  if(data&0x20 == 0x20) { digitalWrite(D5,HIGH); }
  else { digitalWrite(D5,LOW); }

  if(data&0x40 == 0x40) { digitalWrite(D6,HIGH); }
  else { digitalWrite(D6,LOW); }

  if(data&0x80 == 0x80) { digitalWrite(D7,HIGH); }
  else { digitalWrite(D7,LOW); }
}

void lcd_data(unsigned char data)
{
  printdata(data&0x0F);
  digitalWrite(RS,HIGH);
  digitalWrite(RW,LOW);
  digitalWrite(EN,HIGH);
  delay(2);
  digitalWrite(EN,LOW);

  printdata(data<<4);
  digitalWrite(RS,HIGH);
  digitalWrite(RW,LOW);
  digitalWrite(EN,HIGH);
  delay(2);
  digitalWrite(EN,LOW);
}

void lcd_command(unsigned char command)
{
  printdata(command&0x0F);
  digitalWrite(RS,LOW);
  digitalWrite(RW,LOW);
  digitalWrite(EN,HIGH);
  delay(2);
  digitalWrite(EN,LOW);

  printdata(command<<4);
  digitalWrite(RS,LOW);
  digitalWrite(RW,LOW);
  digitalWrite(EN,HIGH);
  delay(2);
  digitalWrite(EN,LOW);
}

void lcd_string(unsigned char str[], unsigned char num)
{
  for(unsigned char i=0; i<num; i++)
  {
    lcd_data(str[i]);
  }
}

void lcd_initialise()
{
  lcd_command(0x28);
  lcd_command(0x0C);
  lcd_command(0x06);
  lcd_command(0x01);
}

void setup()
{
  pinMode(RS,OUTPUT);
  pinMode(RW,OUTPUT);
  pinMode(EN,OUTPUT);

  pinMode(D4,OUTPUT);
  pinMode(D5,OUTPUT);
  pinMode(D6,OUTPUT);
  pinMode(D7,OUTPUT);

  lcd_initialise();
}

void loop()
{
  lcd_command(0x80);
  lcd_string("Hello",5);
  lcd_command(0xC0);
  lcd_string("World",5);
}

📺 Output
Hello
World

🧩 Key Concepts

Bitwise operations for pin control

4-bit LCD communication protocol

Manual LCD command and data transfer

💡 Future Enhancements

Display sensor data (Temperature, Humidity, etc.)

Implement scrolling or blinking text

Integrate with IoT or serial input
