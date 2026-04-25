![t2](https://github.com/sadeem058/Stepper-motor-with-LCD/blob/main/task.png)

# Solve:

![1](https://github.com/sadeem058/Stepper-motor-with-LCD/blob/main/q.jpeg)
![2](https://github.com/sadeem058/Stepper-motor-with-LCD/blob/main/e.jpeg)
![3](https://github.com/sadeem058/Stepper-motor-with-LCD/blob/main/w.jpeg)

## Components
Arduino Uno
2x Stepper Motors (28BYJ-48)
2x Driver ULN2003
Jumper Wires
Breadboard
Power Supply

## Wiring & Pin Mapping
🔹 Stepper Motor 1
IN1 → Pin 8
IN2 → Pin 9
IN3 → Pin 10
IN4 → Pin 11
🔹 Stepper Motor 2
IN1 → Pin 4
IN2 → Pin 5
IN3 → Pin 6
IN4 → Pin 7

 ## How It Works

This project controls two stepper motors using the AccelStepper library.

Each motor moves to a specific number of steps
Once it reaches the target position, it reverses direction
The motion continues back and forth continuously
Both motors run independently but at the same time

# Solve:

This project is built using an Arduino Uno to control two motors and display the status on an LCD.
![c3](https://github.com/sadeem058/Stepper-motor-with-LCD/blob/main/components3.png)

## Components
Components used:
Arduino Uno
L293D Motor Driver
2 stepper Motors
LCD I2C (16x2)
9V Battery
Jumper wires

## Wiring & Pin Mapping
Arduino Pin
L293D IN1    8
L293D IN2    9
L293D IN3    10
L293D IN4    11
LCD SDA      A4
LCD SCL      A5
VCC          5V
GND          GND

## How It Works
This project controls the movement of two motors in a specific sequence while displaying the direction on the LCD.
Depending on the code, the motors perform these movements:
First, both motors move forward for 30 seconds.
![f](https://github.com/sadeem058/Stepper-motor-with-LCD/blob/main/forword.png)
Then, both motors move backward for 60 seconds.
![b](https://github.com/sadeem058/Stepper-motor-with-LCD/blob/main/back.png)
After that, the robot moves right and left repeatedly for 1 minute.
![R](https://github.com/sadeem058/Stepper-motor-with-LCD/blob/main/%60.png)
![L](https://github.com/sadeem058/Stepper-motor-with-LCD/blob/main/1.png)

Finally, all motors stop.
The LCD shows the current command (Forward, Backward, Right, Left, or Stop) during each step.

## Code

```
#include <Wire.h>
#include <LiquidCrystal_I2C.h>

LiquidCrystal_I2C lcd(0x27, 16, 2);

// أرجل L293D
int IN1 = 8;
int IN2 = 9;
int IN3 = 10;
int IN4 = 11;

void setup() {
  pinMode(IN1, OUTPUT);
  pinMode(IN2, OUTPUT);
  pinMode(IN3, OUTPUT);
  pinMode(IN4, OUTPUT);

  lcd.init();
  lcd.backlight();
}

// أمام
void forward() {
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
}

// خلف
void backward() {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
}

// يمين
void right() {
  digitalWrite(IN1, HIGH);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, HIGH);
}

// يسار
void left() {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, HIGH);
  digitalWrite(IN3, HIGH);
  digitalWrite(IN4, LOW);
}

// توقف
void stopMotors() {
  digitalWrite(IN1, LOW);
  digitalWrite(IN2, LOW);
  digitalWrite(IN3, LOW);
  digitalWrite(IN4, LOW);
}

void loop() {

  // أمام 30 ثانية
  lcd.clear();
  lcd.print("Forward");
  forward();
  delay(30000);

  // خلف 60 ثانية
  lcd.clear();
  lcd.print("Backward");
  backward();
  delay(60000);

  // يمين ويسار دقيقة
  unsigned long startTime = millis();

  while (millis() - startTime < 60000) {

    lcd.clear();
    lcd.print("Right");
    right();
    delay(3000);

    lcd.clear();
    lcd.print("Left");
    left();
    delay(3000);
  }

  // توقف
  lcd.clear();
  lcd.print("Stop");
  stopMotors();

  while (1);
}
```

## Explanation

#include <Wire.h>
Library for I2C communication.

#include <LiquidCrystal_I2C.h>
Library to control the LCD display.

LiquidCrystal_I2C lcd(0x27, 16, 2);
Defines the LCD address and size.

int IN1 = 8;
Defines the pin for motor control.

lcd.init();
Initializes the LCD screen.

lcd.backlight();
Turns on the screen light.

forward();
Function to move both motors forward.

backward();
Function to move both motors backward.

stopMotors();
Function to turn off all motor signals.

lcd.clear();
Clears the screen to write a new message.

lcd.print("Forward");
Displays the word "Forward" on the screen.

delay(30000);
Wait for 30 seconds while moving.

unsigned long startTime = millis();
Starts a timer to track the 1-minute movement.

while (millis() - startTime < 60000)
