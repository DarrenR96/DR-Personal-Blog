---
# Feel free to add content and custom Front Matter to this file.
# To modify the layout, see https://jekyllrb.com/docs/themes/#overriding-theme-defaults

layout: default
permalink: /projects/micromonitor/
categories: projects
---

## MicroController Based Pulse & Glucose Monitor

This system was the final project for the Microprocessor Applications & Systems (ECNG3006) course in the BSc. Electrical & Computer engineering programme.

_This is a general breakdown of only the unique/technical aspects of the project. For the full technical analysis & circuit diagrams, click [here](https://sites.google.com/site/ecng30061718/wiki/group-f-mazerunners)._

![alt text](/img/microp1.png "Project in action")
_Prototype of system_

---

### System Requirements:

| Requirement |                                                                                                                  Description                                                                                                                   |
| :---------: | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------: |
|     R1      |                                                          The system has to measure a persons heart/pulse rate (in beats per minute), and estimate a person's glucose level (in mg/dL)                                                          |
|     R2      |                                                         The system has to estimate EITHER heart rate variability using pNN50 (as a %) OR a person's oxygen saturation level (as a %).                                                          |
|     R3      |                                                      The system has to measure the ambient temperature (in degrees Centigrade to +/- 0.1 degree Centigrade) at which a reading is taken.                                                       |
|     R4      |                                                                            The system should be capable of reporting all 4 (live) measurements on the LCD display.                                                                             |
|     R5      |                                                                               All sensors/system measurements should be calibrated and/or the error quantified.                                                                                |
|     R6      | The system should provide audible warnings if the (live) measurements fall outside of acceptable range for pulse rate, glucose level, and heart rate variability/oxygen saturation (allowing for the sensor/system resolution/accuracy error). |
|     R7      |                                    The system should allow a user to start storing a pre-defined set of measurements to external flash RAM, and to interrupt the ongoing storage of a set of measurements.                                     |
|     R8      |                                                                       The system should allow the user to scroll through ALL measurements stored on external flash RAM.                                                                        |
|     R9      |                                           he system should allow a user to specify intervals at which readings should be stored to external flash RAM, and the number of measurements to be stored.                                            |
|     R0      |                                                                           The system should tolerate a power/brown-out reset without corrupting information/display.                                                                           |

---

### System Operation

The [µC/OS-II](https://www.micrium.com/rtos/kernels/) real-time operating system was utilized in the development. This project was developed to be operated in a foreground/background manner. A super-loop was used to call modules that performs certain operations (background) while Interrupt Service Routines (ISRs) are used to handle asynchronous events (foreground).

---

### LCD Module

#### LCD Hardware

The LCD chosen was the HITACHI HD44780U. The LCD module utilizes seven pins total on the PIC. Four lines are used for data transfer between the LCD and the PIC. The remaining three lines are used for register select (RS), the read or write select (R/W) and the enable (E) connections. The power supply used for the LCD display is 5V and is off the same rail powering the PIC. A 10kΩ potential resistor is connected between the Vee and ground to allow for the user to change the contrast of the screen. The entire circuit diagram of the LCD module and the chosen PIC ports chosen can be seen below.

#### Software

The XLCD.h library was used to interface the LCD display with the PIC. This library was chosen as it was supported by the C18 compiler and allowed for integration between hardware and software. The functions within the XLCD library are already fully created and allowed for the transmission of data between the LCD and the PIC.

Psuedocode for LCD Module can be found [here](https://sites.google.com/site/ecng30061718/wiki/group-f-mazerunners/hardware-modules/lcd-display).

---

### Temperature Sensor

#### Hardware

The temperature sensor used was the DS1822 one-wire digital thermometer. This thermometer was used because of its 9-12 bit centigrade measurements for a higher degree of accuracy. This thermometer also has an alarm function which can be further implemented into the system. The DS1822 is special in that it only needs one bus to transmit data to the PIC, which is advantageous in this situation since there will be various systems connecting to the PIC. The DS1822 is capable of handling temperatures from -55 to 125 degrees celsius. As seen in the circuit diagram below, a transistor is used to trigger the DS1822. The circuit functions so that when a reading is to be taken, a high signal is sent from port RE2 to the transistor, which in turn enables the DS1822 to turn on and begin its transfer of data to RB5.

#### Software

The one wire digital thermometer has an associated C18 library which allows the programmer to easily access the thermometer's data. This C18 library is called the one wire library and is a header file labelled as "ow.h". To initialize this library, the specific port which is used for the one wire transfer has to be defined at the top of the ow.h header file. This library has reset functions which initially clears the DS1822 and write and read functions which allows the manipulation of the data on the DS1822. When reading data from the DS1822, there are two bytes that represent the temperature. These are known as the LS and MS bytes. After extraction from the DS1822, these bytes are processed and then the resulting temperature value is stored.

Psuedocode for the Temperature Sensor module can be found [here](https://sites.google.com/site/ecng30061718/wiki/group-f-mazerunners/hardware-modules/temperature-sensor).

---

### IR/Receiver

#### Hardware

The heart rate and heart rate variability and glucose level hardware will be discussed separately as they are different module to each other.

The signal for the heart rate monitor is generated from a QSD123 photo transistor that switches on when an infrared light from an QED223 infrared LED. The QED223 was chosen over the QED123 as it has a higher dispersion angle for the Infrared light which in turns allows more light to travel through the subject and hence have a higher degree of accuracy. The conditioning circuit is primarily built using two operational amplifier followed by a low pass filter. The operational amplifier used was a LM324N as it allowed for 4 amplifiers within one chip and saves physical space on the breadboard. The first amplifier has a gain of 160 and the second has a gain of 560. The low pass filter is used to eliminate the noise from other equipment. The cut-off of the low pass filter was determined to be 5 Hz so that it will not interfere with the pulse rate signal. The signal is then sent to RB0 on the PIC for the heart rate monitor module. A comparator was also built and the signal was sent to RC1 (CCP1) for the heart rate variability module. This can be seen in the circuit diagram below.

The signal for the glucose monitor is also generated from a phototransistor and the QED223 infrared LED. This was chosen because of its wavelength of 880nm which was the closest available to 935nm, which is the wavelength in which research was done and available for. The signal from the photodiode is then sent to a high pass filter. This high pass filter was designed to remove the DC component of the PPG signal. The high pass filter had a cutoff frequency of 0.8Hz. The output of the highpass filter is then fed into a trans-impedance amplifier which converts the photodiode current into a voltage. The LM324N was used in designing the trans-impedance amplifier through evaluating the circuit parameters. This is done so that the dynamic range is not lost. Finally the signal from the trans-impedance filter fed into a low pass filter. Again the LM324N was used to build the low pass filters. This low pass filter is used to remove the noise associated with the power signal so that the PPG would be accurate. This can be seen in the circuit diagram below.

#### IR/Receiver Software

The heart rate, heart rate variability and glucose module will all be discussed separately.

Using C code and the C18 compiler, the PIC was configured to set RB0 as an input to the system. A timer was also used (Timer3) to overflow and interrupt every 0.2 seconds. Also an interrupt was set on the input of RB0 to interrupt every time a pulse is detected. When a pulse is detected and an interrupt occurs, then a counter increments. This counter is used in representing the heart rate. After 10 seconds of counting or after the overflow counter reaches 50, the Timer interrupts and the counter that increments within the pulse interrupt is multiplied by 6 and then displayed on the LCD screen. 10 seconds was used as the interval time to update the heart rate register as it allowed for the subjects pulse to have a higher accuracy due to an increased sampling time.

For the heart rate variability section, the pin RC1 was used since it allowed for capture mode to be enabled on that pin. The capture was set as on and to capture every rising edge coming into the pin. An interrupt was set up on RC1 that will interrupt every time a pulse is detected. Within the interrupt service routine, a capture of the pin is taken and the post-pulse capture is also taken and the both are compared in terms of timing. With every pulse, a counter NN is incremented. The time of the rising edge of the second pulse is then compared to the first pulse and if the value is over 50ms, a counter NN50 is incremented. After 10 samples, the heart rate variability value is then calculated using the formula (NN50/NN)\*(100). This value of 10 samples was chosen so that an accurate value would be obtained.

To properly represent the glucose level of the subject, the optical density of the wavelength must be determined. The signal is sent to the PIC on pin RA0/AN0. This pin was configured as an ADC to accept the signal at intervals. After a certain time t, the timestamp is recorded along with the amplitude of the signal. Afterwards the time t+1 is also recorded with the amplitude of the signal. The optical density of the wavelength must then be calculated via processing. The formula for the optical density was
OD = log ( 1 + (ΔI(t(i))/(I(i+1)) ).
The estimation of the glucose level was done using Beer Lambert's law.

Psuedocode for the IR/Reciever module can be found [here](https://sites.google.com/site/ecng30061718/wiki/group-f-mazerunners/hardware-modules/3-ir-visible-light-led-receiver).

---

### Flash RAM

#### Hardware

The flash RAM module chosen was the SST39SF040. This module was chosen due to its large size (4Mbit) which allows for multiple readings to be recorded over a period of time. To connect the RAM chip to the PIC18, 5 shift registers were needed. Four serial-input, parallel-output shift registers were used to write data to the flash memory and one parallel-input, serial output was used to retrieve data from the flash memory. For the serial-input, parallel-output shift registers, the SN54165 chip was used. This was chosen because the data shifting also allowed for the data to be output in a serial manner, which meant that multiple shift registers would have been able to be connected together in series to eliminate the need of pin usage on the PIC. The first 8 data out pins from the first SN54165 was connected to the address pins A0-A7 pins on the flash module. The next 8 pins from the second shift register was connected to A8-A15. The third shift register connected to A16-A18 and the data pins D0-D4. The final input shift register connected to the data pins D5-D7. To completely operate the input of data to the flash RAM, the Data bits followed by the address bits would need to be written in that order from the first shift register go down. Then once all the bits were written, all would be "bit-banged" to the flash module. A single parallel input, serial output shift register was used as means to a data input to the PIC. The component chosen was the SN74ALS166. This was chosen as it allowed data to be serially input to the PIC as well as allow control ports to fully allow the user to recognize the status of the bits being shifted. The full data connections between all chips can be seen below.

#### Software

The flash RAM module mainly depends on the status of the CE#, WE# and OE# ports to manipulate the data. The setting and clearing of the ports can all be done with C code. These three ports were mapped to pins RA3, RA4 and RA5 respectively. The commands written to initiate the operation of the device is done by setting WE# to low and keeping CE# low. The address bus is then latched to the falling edge of WE#. To read data from the flash RAM, the CE# and CO# ports has to be manipulated. This is done within C code by setting CE# and CO# initially to be low to obtain data from the outputs. This module is programmed at one byte at a time. To program data, it must be ensured that the sector that is to be programmed must be cleared. Afterwards, the byte address and byte data must be loaded.
In this setup, to write data to the flash RAM, the G pin on the shift registers must be enabled by setting RB5 high. This G pin is known as the enable pin which enables the functionality of the shift register. After that data bits must be sent on the SER pin which corresponds to the RB2 pin on the PIC. For each data bit that is sent on the SER pin, the RCK pin must be set high whenever the bit is ready to be transferred. This can be done by setting pin RB3 on the PIC. Once this is done the data bits to be stored followed by the address bits on where the data should be stored should be sent on each time the RCK pin is high. Then the data can be programmed to the device by setting the CE# and WE# pins (pins RA3 and RA4 respectively on the PIC).
To retrieve data from the Flash RAM module, a similar approach must be taken from above in which the address of the data must be sent to the shift registers and to the chip using the CE#, WE#, serial and data output pins. Once this is done, the output functionality is enabled by setting OE# and CE#. The data is then sent to the SN74ALS166 chip. The data can then be retrieved by monitoring the SER output ( pin RB6 ) while oscillating pin RA1.

Psuedocode for the Flash RAM module can be found [here](https://sites.google.com/site/ecng30061718/wiki/group-f-mazerunners/hardware-modules/flash-ram).
