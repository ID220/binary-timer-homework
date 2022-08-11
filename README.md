# Homework 3: Binary Timer

In this homework, you will create an interactive binary timer based on the schematics of a circuit. You will have to create a circuit on a breadboard and write the MicroPython code to control the timer.

Click on the image below to see a [demo video](https://youtu.be/km6rSBD77C8) of how the timer works:

<a href="https://youtu.be/km6rSBD77C8">
<p align="center">
<img src="data/video_thumbnail.png" width="80%" />
</p>
</a>

The **objectives** of this homework are to learn:

- digital input and output, analog input, and PWM using MicroPython
- controlling many LEDs using shift registers
- binary numbers and binary operators

## Content of this folder

- [_SubmissionForm.md_](./SubmissionForm.md): the form containing the information about your submission.
- _data_: a folder with the schematics and images used in this document.
- _README_: this file, containing the instructions.

## How does the timer work?

<p align="center">
<img src="data/overview.png" width="100%" />
</p>

The timer has two modes of operation: an **input mode** to allow users to insert a specific time (hours, minutes, and seconds), and a **countdown mode** that shows a countdown from the time set by the user down to 0.

The time is displayed in binary format using an LED display (4 red LEDs for hours, 6 green LEDs for minutes, and 6 yellow LEDs for seconds), controlled by a pair of shift registers placed in series.

<p align="center">
<img src="data/time_mapping.png" width="80%" />
</p>

---

When the RPi is turned on, the timer is in input mode, and the BlinkPico LED matrix shows `H` for hours. By clicking the **C button**, the user can cycle through minutes (`M`), seconds (`S`), and then back to hours (`H`), and so on. By clicking (or holding down) on the **A and B buttons**, the user can decrement or increment 1 unit the counter of hours, minutes, or seconds, depending on the current selection. If reaching the maximum or minimum values, the counter wraps back (e.g., 59 minutes + 1 goes back to 0 minutes, or 0 - 1 goes to 59).

Once the user is done inputting a time, the user can click on a pushbutton to initiate the counter. The counter shows the time decreasing 1 by 1 until it reaches 0 and it stops. For the demo, the timer counts down every 0.2 seconds.

Finally, a potentiometer allows controlling the brightness of all the LEDs simultaneously, from 0 brightness (LEDs are off) to maximum brightness (LEDs fully on).

## Detailed instructions

The homework consists of two parts. Create the **hardware** of the circuit based on the schematics, and write the **firmware** (e.g., the MicroPython code) to make it work.

### Hardware

This is an overview of the circuit (click on the image to see a larger view in PDF format).

<a href="data/schematics.pdf">
<p align="center">
<img src="data/schematics.png" width="80%" />
</p>
</a>

- The **Voltage regulator** takes as input 9V from the battery and the output voltage at 5V. The status LED shows the LED when the circuit is operating. Remember to use the bypass capacitors.
- Be **extremely CAREFUL** of not to mix 3V and 5V. `Anything connected back to the Raspberry PI Pico should be 3V, or the board will be damaged!!` Check out the schematics _very carefully_.
- We use two 8-bit shift registers (74HC595) to source current to 16 LEDs (4 for hours, 6 for minutes, and 6 for seconds). Use any of the methods shown in class to send data to the shift registers.
- The 16 LEDs are not connected directly to GND. Instead, there is an NPN transistor (TIP120) between the cathodes and the ground. The base of the transistor is connected to pin 10 of RPI, which uses PWM to modulate how all LEDs are on. The PWM value is determined by how the potentiometer (connected to Analog 0) is rotated (from 0% to 100%).

#### Bill of Material (BOM)

| Designator     | Name           | Quantity |
| -------------- | -------------- | -------- |
| B1             | +9V            | 1        |
| C1,C2          | 0.1u           | 2        |
| LEDs           | Various colors | 17       |
| Q1             | TIP120         | 1        |
| R1...R18       | To compute     | 18       |
| RPOT1          | 100k           | 1        |
| SW1            | SPDT           | 1        |
| SW2            | Push Button    | 1        |
| U1             | LM7805         | 1        |
| U2, U3         | SN74HC595      | 2        |
| U4 + BlinkPico | RPi Pico       | 2        |

Few notes and tips:

- I used three colors of LEDs (_red_ for hours, _green_ for minutes, and _yellow_ for seconds and the status LED). You can use any color you want, but make sure that hours, minutes, and seconds are shown with different colors. Depending on the resistors you choose, you might want to adjust the value of your resistors. Also, note that the _yellow_ LEDs are represented in _blue_ in the schematics.
- SW1 is an SPDT. You can choose anyone you want. I used [this one](https://www.devicemart.co.kr/goods/view?no=2647).

### Firmware

The firmware should be written in MicroPython and should run on the Raspberry PI Pico

Here are some notes and tips:

- To control the shift registers you can use any of the methods learned in class. Here is a short example of how you could set up SPI on the RPi:

```python
# Setup SPI
spi = SPI(1, 10_000, sck=Pin(14), mosi=Pin(15))

# and write some value (here, for example, 0x07)
spi.write(b'\x07')
```

- You can organize the LEDs in any order you want to (e.g., they do not have to be in sequential order if you do not want to), but the user should see 4 LEDs for hours, 6 for minutes, and 6 for seconds. This configuration allows covering any time from 00:00:00 to 11:59:59 (the maxim time).
- During the countdown the BlinkPico display should be all filled up. When the time of the countdown reaches 0, you can display again `H`.
- Use an analog pin (e.g., `ADC0`) to read the rotation of the potentiometer, and maps the value to a PWM duty cycle (on Pin 10).

## Submission and grading

You have to submit the following items:

- The form with your information: [_SubmissionForm.md_](./SubmissionForm.md)(`10%`)
- A video demo of your demo with the prototype. The video should have decent quality, so consider editing it. (`35%`)
- Clear photos (max 3) of your breadboard circuit showing the circuit overview and details (`20%`)
- Your MicroPython sketch with the code (`35%`)

Take clear photos of your breadboard, the [_SubmissionForm.md_](./SubmissionForm.md), and the PDF containing the schematics of the circuit, zip them together (_zip_, not _ALZIP_, _rar_ or others) and submit this resulting file using the [homework submission system](https://homework.prototyping.id). Make sure the zip file is smaller than 20MB.

**DO NOT SUBMIT THE VIDEO USING THE HOMEWORK SYSTEM**. Instead, upload the video online (YouTube, Google Drive...) and place a public link to the video inside the [_SubmissionForm.md_](./SubmissionForm.md) file.

<p align="center">
<img src="data/hwsystem.png" width="500" />
</p>

Please note that:

1. Only submissions made with the system will be considered (no direct emails to TA or Prof).
2. You can re-submit as many times as you want before the deadline &ndash; we will consider your last submission.
3. Submissions after the deadline are **NOT** accepted.
4. If the file is too large the system won’t allow the submission. Remove from the zip file unnecessary files, and make sure to compress your images.
5. Do not copy from the Internet or colleagues without attribution. Remember the _honor code policy_.
6. The instructor reserves the right of assigning, at his discretion, a bonus or penalty of up to 10% (e.g., qualitative assessment of the code or of the prototype).
