---
layout: labitem
title: Part 2
permalink: /lab2/IQ-USRP
firstHeading: Part 2 - IQ and using a USRP
---

[**Back to Part 1**](part1.md) | [**Continue to Lab 3**](../_lab3/introduction.md)

## Objectives

This part of the lab will guide you through...
<!-- #TODO finish -->

---

## Part 2 Deliverables

<!-- #TODO update -->
- GRC file of BLAH
- There is 1 question in this part of the lab. It is clearly indicated as in the last part.
  - The question requires approximately 1 line of writing and address concepts, not details. Answer the questions and submit a single page containing the answers to your TA at the end of the lab. This should build upon your answers to the two questions posed in the last part of the lab.

---

## Setup

The small grey box is the USRP software‐defined radio. The USRP digitally downconverts the received (Rx) input signal into I/Q format and sends it via Ethernet to the computer. The USRP also digitally upconverts an I/Q signal from the computer to an RF signal at the transmitter (Tx) output. The USRP's Rx input is connected to a VHF (Very High Frequency) antenna on the ELW roof. The Tx output can be connected to the oscilloscope and spectrum analyzer.

- Verify that the USRP at your station is connected as shown below.

  ![part2_usrp-connect.png](./figures/part2_usrp-connect.png)<br>
  __*USRP front panel*__

## I/Q Receiver

### I/Q Receiver output

- Review [IQ theory](../_docs/pdriessen_textbook.pdf) (sections 1.2 and 1.3).

- Open the GRC file [general-IQ-from-USRP.grc](data/general-IQ-from-USRP.grc). This flowgraph implements the mathematics on the last page of the IQ theory document.

  - The USRP source does IQ downconversion on the WBX daughtercard and outputs the complex signals I(t) + jQ(t).
  - This output is connected to 4 blocks that extract the magnitude, phase, real and imaginary parts of the complex signal, as well as an XY scope.
  - The USRP source is tuned to a fixed frequency of 200 MHz, i.e. the LO frequency synthesizer in the WBX daughtercard is set to 200 MHz.

- Double‐click the USRP source block to bring up a window with all of the USRP parameters. This general I/Q receiver is set up to receive a signal in a range around 200 MHz at level -40 dBm from the signal generator at the back of the lab.

- Execute the flowgraph. Observe the Output Display window with 5 tabs labelled **Scope Plot, Magnitude, Phase, Real** and **Imaginary**.
  - The Scope Plot tab should show a circle
  - Magnitude will show a (noisy) DC level
  - Phase will show a phase ramp wrapping between -π and π
  - Real and Imaginary will show (noisy) sine waves.

- The scope autorange may need to be switched off and the time base adjusted to get good displays.

- Determine the frequency *f~b~* of the sine waves using the Phase display as well as the Real and Imaginary displays by placing your mouse cursor over the scope plot to show the time offset at different points on the waveform. This frequency *f~b~* represents the offset between the received RF signal *f~c~* and the USRP local oscillator *f~LO~*, so that
 *f~b~* = *f~c~  - f~LO~*

- Confirm that *f~b~* is as expected.

- The USRP source block has the *clock source* set to use an *external* 10 MHz clock reference frequency, and the same external reference is used for the signal generator. Thus the frequency difference between the USRP source block (local oscillator) and signal generator RF frequency will be observed to be exactly as expected from their respective frequency settings.
  - If we change the USRP source block to use an *internal* clock reference, then expect to observe some frequency error between the signal generator and the USRP frequency settings as they are running from independent oscillators.
  - Try changing the USRP clock source to *internal* and repeat the frequency measurement of the I and Q outputs.

### Dynamic range with IQ signals

- Ask the TA to vary the 200 MHz signal generator level from ‐100 dBm to 0 dBm in 20 dB steps (or do it yourself is there are no other groups working).

- Observe and describe how the signals look at each signal level, and explain why.
  - The waveform appearance results from clipping in the 2 ADCs (one ADC for I, one ADC for Q).

### Noise figure

- Review [link budget notes](data/350link4.pdf) with particular attention to **equation (14)**.

- When you tune the receiver to a frequency where there is no station broadcasting, there is still a residual noise floor visible on the spectrum display. The noise level can be estimated by looking at the display and observing the level in dBm (dB relative to one milliwatt). This is thermal noise that is calculated from the formula
 *P~noise~ = kT(S/N)WF*
 where
  - k is Boltzmann's constant
  - T is the noise temperature, typically 290 degrees Kelvin
  - W is the bandwidth in Hz and
  - F is a dimensionless noise figure representing imperfect amplifiers.

- By substituting values for *k* and *T* and taking logs of both sides of this formula, we can write in dB:
 P~noise~ = -228.6 + T + (S/N) + W + F.

- Estimate the noise figure of the receiver based on the noise level in dBm on the computer spectrum display and the receiver bandwidth of 250 KHz.
  - All the variables in the noise figure equation except F are known or can be measured, so the equation can be solved for F.
  - The specified value for the URSP with WBX board is 5.5 dB.

## Carrier Wave Transmitter

In this section, we test the transmit functions of the USRP that we can use later when building a communications system. We will observe the transmitted spectrum, minimum and maximum power level in dBm (dB relative to one milliwatt). You will use both the osciloscope and the spectrum analyzer at your bench to view and measure the output from the USRP transmitter.

- Review the theory of [spectrum analyzers](../_docs/pdriessen_textbook.pdf) (section 1.4). For more detailed information, you may also wish to review [Spectrum Analyzer Basics](./data/5965-7920E.pdf) and [The Basics of Spectrum Analyzers](./data/spec_analyzer.pdf). The concepts presented here will be applicable to any spectrum analyzer you may use in your career.

- Open [this GRC file](data/tx_carrier.grc).

- Observe that the USRP sink center frequency is set to 50 MHz. This block represents the USRP transmitter hardware.

- Observe that the sine and cosine signal sources are configured for 10 kHz.

- Connect the USRP Tx output to the spectrum analyzer and execute the flowgraph. A scope display will come up along with three buttons that allow you to select different values for Q(t).

- Set the spectrum analyzer's center frequency to 50 MHz and the span to 50 kHz by using the FREQUENCY and SPAN buttons. Adjust the LEVEL as necessary.

- What do you observe on the spectrum analyzer display with Q(t) = 0? Try the other two options for Q(t). What do you observe on the spectrum analyzer?

- What is the minimum and maximum signal power output from the USRP? The USRP output power level can be set via the dialog box obtained by double‐clicking on the USRP sink in the flowgraph. Measure the power using both the oscilloscope and spectrum analyzer and verify they are the same.

  >Note: Recall from your circuit theory that the output power is relative to the load impedance. On the spectrum analyzer, the input is 50Ω, which matches the expected impedance of a typical antenna. The input to the scope on the other hand is high impedance, effectively open circuit, to prevent damage to the internal circuits. To compare measurement on the scope to that of the spectrum analyzer, the line from the USRP transmitter should be terminated with a 50Ω terminator. Alternatively, some circuit theory can be applied to determine the equivalent power output measured across an open circuit and a 50Ω load.

--

[**Continue to Lab 3**](../_lab3/lab3.md)
