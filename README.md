# CircuitPython
This repository will actually serve as a aid to help you get started with your own template.  You should copy the raw form of this readme into your own, and use this template to write your own.  If you want to draw inspiration from other classmates, feel free to check [this directory of all students!](https://github.com/chssigma/Class_Accounts).
## Table of Contents
* [Table of Contents](#TableOfContents)
* [Hello_CircuitPython](#Hello_CircuitPython)
* [CircuitPython_Servo](#CircuitPython_Servo)
* [CircuitPython_LCD](#CircuitPython_LCD)
* [NextAssignmentGoesHere](#NextAssignment)
---

## Hello_CircuitPython

### Description & Code
For this assignment we were given code that prints hello world in the serial moniter

Here's how you make code look like code:

```from time import sleep

while True:
    print("Hello world!!!!!")
    sleep(5)
```


### Evidence


![spinningMetro_Optimized](https://user-images.githubusercontent.com/54641488/192549584-18285130-2e3b-4631-8005-0792c2942f73.gif)



### Wiring
none

### Reflection
This assignment was very easy because there was no wiring and all it does is print hello world.



## CircuitPython_Servo

### Description & Code
this assignment was to make a servo work using curcuit python
```# SPDX-FileCopyrightText: 2018 Kattni Rembor for Adafruit Industries
#
# SPDX-License-Identifier: MIT

"""CircuitPython Essentials Servo standard servo example"""
import time
import neopixel
import board
import pwmio
from adafruit_motor import servo
# create a PWMOut object on Pin A2.
pwm = pwmio.PWMOut(board.D3, duty_cycle=2 ** 15, frequency=50)
dot = neopixel.NeoPixel(board.NEOPIXEL, 1)
BUTTON = 1
dot.brightness = 1
# Create a servo object, my_servo.
my_servo = servo.Servo(pwm)

while True:
  for angle in range(0, 180, 180):  # 0 - 180 degrees, 100 degrees at a time.
        my_servo.angle = angle
  for angle in range(180, 0, -180): # 180 - 0 degrees, 100 degrees at a time.
        my_servo.angle = angle
```

### Evidence
![image](https://github.com/matthewbowling123/CPython/blob/master/Images/IMG-0413%20(2).MOV)

### Wiring
![image](https://user-images.githubusercontent.com/112979288/193122380-858af000-209d-4212-bf57-06e1568c96f2.png)

### Reflection
This assignment was a litlle more difficult then the previous one but not too hard and I learned a lot more about CPython.



## CircuitPython_LCD

### Description & Code
This assignment we need to print numbers on an LCD that can count up and down using buttons.
```# get and i2c object
i2c = board.I2C()
btn = DigitalInOut(board.D3)
btn2 = DigitalInOut(board.D4)
btn.direction = Direction.INPUT
btn.pull = Pull.UP
# some LCDs are 0x3f... some are 0x27.
lcd = LCD(I2CPCF8574Interface(i2c, 0x27), num_rows=2, num_cols=16)
cur_state = True
prev_state = True
prev_state2 = True

buttonPress = 0
delta = 1

lcd.set_cursor_pos(1, 15)
lcd.print("+")
lcd.set_cursor_pos(0, 0)
lcd.print("BTN is up  ")

while True:
    cur_state2 = btn2.value
    if cur_state2 != prev_state2:
        if not cur_state2:
            delta = -delta #changes delta to negative
            if delta > 0:
                lcd.set_cursor_pos(1, 15)
                lcd.print("+")
            else:
                lcd.set_cursor_pos(1, 15)
                lcd.print("-")
    
    prev_state2 = cur_state2

    cur_state = btn.value
    if cur_state != prev_state:
        if not cur_state:
            buttonPress = buttonPress + delta #depending on delta value button press will have a posotive or negative value added to it
            lcd.set_cursor_pos(0, 0)
            lcd.print("BTN is down")
        else:
            lcd.set_cursor_pos(0, 0)
            lcd.print("BTN is up  ")
        
        lcd.set_cursor_pos(1, 0)
        lcd.print("BTN Press:")
        lcd.print(str(buttonPress))     
        lcd.print(" ")

    prev_state = cur_state
    time.sleep(.05) #delay is .05 of a second

```

### Evidence

![image](https://github.com/matthewbowling123/CPython/blob/master/Images/IMG-0437%20(1).MOV)

### Wiring


### Reflection
This assignment was very difficult but I managed to do it. It deffinatly taught me a lot about CPython.




## Motor Control

### Description & Code

```import board
import time
from analogio import AnalogOut, AnalogIn
import simpleio

motor = AnalogOut(board.A1)
pot = AnalogIn(board.A0)

while True:
    print(simpleio.map_range(pot.value, 96, 65520, 0, 65535))
    motor.value = int(simpleio.map_range(pot.value, 96, 65520, 0, 65535))
    time.sleep(.1)

```

### Evidence


https://user-images.githubusercontent.com/112979288/199817203-d69ee332-9d6f-46f2-a885-088d9efbf0a8.MOV


### Wiring
![image](https://user-images.githubusercontent.com/112979288/199819107-4601f0b6-d339-412a-a06d-b9a65ec0ded4.png)

### Reflection
I had done this assignmemnt last year so I did not find it too difficult. I still did think however that it was good practice for CPython.

## Tempreture Sensor

### Description and code

```import board
import analogio
import time
from lcd.lcd import LCD
from lcd.i2c_pcf8574_interface import I2CPCF8574Interface

# get and i2c object
i2c = board.I2C()

# some LCDs are 0x3f... some are 0x27.
lcd = LCD(I2CPCF8574Interface(i2c, 0x27), num_rows=2, num_cols=16)


TMP36_PIN = board.A0  # Analog input connected to TMP36 output.


# Function to simplify the math of reading the temperature.
def tmp36_temperature_C(analogin):
    millivolts = analogin.value * (analogin.reference_voltage * 1000 / 65535)
    return (millivolts - 500) / 10


# Create TMP36 analog input.
tmp36 = analogio.AnalogIn(TMP36_PIN)

# Loop forever.
while True:
    # Read the temperature in Celsius.
    temp_C = tmp36_temperature_C(tmp36)
    # Convert to Fahrenheit.
    temp_F = (temp_C * 9/5) + 32
    # Print out the value and delay a second before looping again.
    print("Temperature: {}C {}F".format(temp_C, temp_F))
    lcd.clear()
    lcd.print("Temperature:\n{:.2f}C {:.2f}F".format(temp_C, temp_F)) # print to LCD with exactly 2 decimal points of precision
    time.sleep(1.0)

```
    
### Evidence
![image](https://user-images.githubusercontent.com/112979288/227624366-f5e36d32-f678-48c8-927f-1c9246251b47.png)

### Wiring
    
![image](https://user-images.githubusercontent.com/112979288/227623516-04ae6eea-58d4-4add-be6c-38580e40be92.png)

### Credit
credit to river for most of thiscode and video. He figured out how it works and also how to run CPython code while its github connnection was down.

## Rotary Encoder

### code
again, all credit goes to River with this code. I couldent figure out the Encoder.
```import rotaryio
import board
import digitalio
import neopixel
from lcd.lcd import LCD
from lcd.i2c_pcf8574_interface import I2CPCF8574Interface

# get and i2c object
i2c = board.I2C()

# some LCDs are 0x3f... some are 0x27.
lcd = LCD(I2CPCF8574Interface(i2c, 0x27), num_rows=2, num_cols=16)

led: neopixel.Neopixel = neopixel.NeoPixel(board.NEOPIXEL, 1) # initialization of hardware
print("neopixel")

led.brightness = 0.1

button = digitalio.DigitalInOut(board.D12)
button.direction = digitalio.Direction.INPUT
button.pull = digitalio.Pull.UP

colors = [("stop", (255, 0, 0)), ("caution", (128, 128, 0)), ("go", (0, 255, 0))]

encoder = rotaryio.IncrementalEncoder(board.D10, board.D9, 2)
last_position = None
while True:
    position = encoder.position
    if last_position is None or position != last_position:
        lcd.clear()
        lcd.print(colors[position % len(colors)][0])
    if(not button.value):
        led[0] = colors[position % len(colors)][1]
    last_position = position
```

### Wiring
![image](https://user-images.githubusercontent.com/112979288/236309062-a446f74f-1e2b-4d4c-a475-9ccb8ec87165.png)


### reflection
This project was difficult to figure out, Using other peoples Githubs was easily the most helpfull part and i doubt i could have completed it without those.

## Photointerruptor

### code
```import time
import digitalio
import board

photoI = digitalio.DigitalInOut(board.D7)
photoI.direction = digitalio.Direction.INPUT
photoI.pull = digitalio.Pull.UP

last_photoI = True
last_update = -4

photoICrosses = 0

while True:
    if time.monotonic()-last_update > 4:
        print(f"The number of crosses is {photoICrosses}")
        last_update = time.monotonic()
    
    if last_photoI != photoI.value and not photoI.value:
        photoICrosses += 1
    last_photoI = photoI.value
```
### wiring
![68747470733a2f2f726976717565732e6769746875622e696f2f646f63732f70686f746f696e74636972637569742e706e67](https://user-images.githubusercontent.com/112979288/228960276-fcf0cf46-c8bb-44de-8479-41e7e4d952da.png)

### reflection
We did this in engineering 2 so i just did the exact same thing.
