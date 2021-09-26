# Binary Timer (Homework 3)

In this homework you will create an interactive binary timer based on the schematics of a circuit. You will have to create a circuit on a breadboard and write the code on the micro:bit to control the timer.

Click on the image below to see a [demo video](https://youtu.be/W0_Qsd6zZZQ) of how the timer works:

<a href="https://youtu.be/W0_Qsd6zZZQ">
<p align="center">
<img src="data/video_thumbnail.png" width="80%" />
</p>
</a>

The **objectives** of this homework are to learn:

- digital input and output, analog input and output (PWM) using micro:bit
- controlling many LEDs using shift registers
- binary numbers and binary operators

## Content of this folder

- [_SubmissionForm.md_](./SubmissionForm.md): the form containing the information about your submission.
- _data_: a folder with the schematics and images use in this document.
- _README_: this file, containing the instructions.

## How does the timer work?

<p align="center">
<img src="data/overview.png" width="80%" />
</p>

TODO
TODO
TODO
TODO
TODO

## Getting started

The homework consists of two parts. Create the **hardware** of the circuit based on the schematics, and write the **firmware** (e.g., the MicroPython code) to make it work.

### Hardware

This is an overview of the circuit (click on the image to see a larger view in PDF format).

<a href="data/schematics.pdf">
<p align="center">
<img src="data/schematics.png" width="80%" />
</p>
</a>

- The **Voltage regulator** takes as input 9V from the battery and output voltage at 5V. The status LED shows the LED on when the circuit is operating.
- We use two 8-bit shift registers (74HC595) to source current to 16 LEDs (4 for hours, 6 for minutes and 6 for seconds). Use any of the methods shown in class to send data to the shift registers.
- The 16 LEDs are not connected directly to GND. Instead, there is an NPN transistor (TIP120) between the cathodes and ground. The base of the transistor is connected to pin 3 of the micro:bit, which uses PWM to modulate how all LEDs are on. The PWM value is determined by how the potentiometer (connected to P0) is rotated (from 0% to 100%).

#### Bill of Material (BOM)

| Designator | Name           | Quantity |
| ---------- | -------------- | -------- |
| J1         | MICROBIT_EDGE  | 1        |
| B1         | +9V            | 1        |
| C1,C2      | 0.1u           | 2        |
| LEDs       | Various colors | 17       |
| Q1         | TIP120         | 1        |
| R1,R3      | 2.2k           | 2        |
| R2         | 680            | 1        |
| R4...R19   | 470            | 16       |
| R20        | 330            | 1        |
| RPOT       | 100k           | 1        |
| SW1        | SPDT           | 1        |
| SW2        | Push Button    | 1        |
| U1         | LM7805         | 1        |
| U2, U3     | SN74HC595      | 2        |

Few notes and tips:

- You can purchase any micro:bit connector. The one I used is [this one](https://www.devicemart.co.kr/goods/view?no=1360855).
- I used three colors of LEDs (_red_ for hours, _green_ for minutes, and _yellow_ for seconds and the status LED). You can use any color you want, but make sure that hours, minutes and seconds are shown with different colors. Depending on the resistors you choose, you might want to adjust the value of your resistors.
- SW1 is a SPDT. You can choose any one you want. I used [this one](https://www.devicemart.co.kr/goods/view?no=2647).
- I used two 2.2KΩ resistors (R1, R3) between the SPI pins of the micro:bit. They are optional, but I found they reduce current leakage, so I recommend you place them as well on your circuit.

### Firmware

The firmware should be written in MicroPython and should run on the micro:bit.

Here some notes and tips:

- To control the shift registers you can use any of the methods learned in class. Here a short example of how you could setup SPI on the micro:bit.

```python
# Setup SPI
spi.init(10000, 8, 0,
         sclk=pin13, mosi=pin15, miso=pin14)

# and write some value (here, for example, 0x07)
spi.write(b'\x07')
```

- You can organize the LEDs in any order you want to, but the user should see 4 LEDs for hours, 6 for minutes and 6 for seconds. This configuration allows to cover any time from 00:00:00 to 11:59:59 (the maxim time).

Here an example of how the LEDs are mapped to time.

<p align="center">
<img src="data/time_mapping.png" width="80%" />
</p>

- Use an analog pin (P0) to read the rotation of the potentiometer, and maps the value to a PWM duty cycle (on P3).
