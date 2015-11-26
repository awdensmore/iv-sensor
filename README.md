# iv-sensor
A sensor for voltage and current measurements used in electrochemical impedance spectroscopy. Output is suitable for 3V logic level (DSPs)

###**Operation**
The purpose of this sensor is to accurately measure the current and voltage response of a battery excited by an EIS waveform. The current is measured using a transducer (U1) and passed directly to an output connected to the ADC of a DSP.

The voltage measurement is more complicated. Because the EIS signal is a ~20mV sine wave on top of a ~12V DC signal, the DC component must be removed and the AC component amplified before it is input to the ADC for digitization. This is accomplished in several steps:

1. **Determine DC component of voltage signal**: The battery voltage is measured on the low side of the transducer (I- @ U1). That signal is passed through a voltage divider (R1 / R2), after which it is passed to an external ADC via pin 2 of P1. The external DSP uses a software low-pass filter to remove the AC component, then passes the signal back to this PCB via pin 1 of P1. However, because the DSP only has PWM output (not a pure analog output), this signal needs a DAC. The DAC is built using F1 / F2, which act as a 4-pole programmable active filter. At this point the signal (FO @ pin 1 of F2) is DC and proportional to the original battery DC component, but not the same voltage. It is therefore amplified using OP1, and is now within a few mV of the original battery DC voltage (OP1-OUT @ pin 6 of OP1).
2. **Remove DC component**: The signal created by step 1 is then removed from the original signal (I-) via OP2 (OP2_OUT @ pin 6). We now have only the ~20mV AC signal.
3. **Amplification & offset**: The AC signal is amplified ~35x at OP-3, and then offset by 1.5V at OP-4 so that the signal is always positive. The final EIS voltage signal (Vo) is output to the DSP at pin 3 of P1.

![Sensor Operation](/Plot/IVsensor_operation.png)

**Component Values**

| Component | Value |
|----------|-------|
R1 | 21.92k
R2 | 5.57k
R3 | 82.2k
R4 | 4.64M
R5 | 4.7M
R6 | 50.9k
R7 | 15.9k
R8 | 4.57M
R9 | 4.66M
R10 | 51.0k
R11 | 984
R12 | 100.1k
R13 | 19.98k
R14 | 19.98k
R15 | 100.4k
R16 | 62.3k
R17 | 62.2k
R18 | 62.1k
R19 | 62.3k
R20 | 100.0k
R21 | 2.99k
R22 | 1k
R23 | 100.3k
R24 | 22k
R25 | 21.87k
R26 | TBD
R27 | TBD
R28 | TBD
C1 - C2 | 470uF
C3 - C16 | 0.1uF

