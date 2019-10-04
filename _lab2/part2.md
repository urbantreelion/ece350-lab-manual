---
layout: labitem
title: Part 2
permalink: /lab2/IQ-USRP
firstHeading: Part 2 - IQ and using a USRP
---

[**Back to Part 1**](part1.md) | [**Continue to Lab 3**](../_lab3/introduction.md)

## Objectives

This part of the lab will guide you through receiving and transmitting signals using a USRP software-defined radio. While using the USRP as a receiver you will learn about:

- frequency offsets
- USRP clock sources and reference signal synchronization
- dynamic range andclipping.

While transmitting with the USRP, you will:

- see the effect of transmitting real signals, as well as complex signals
- explore the minimum and maximum power output from the USRP
- measure the power of the USRPs transmission using both an oscilloscope and a spectrum analyzer

---

## Part 2 Deliverables
<!-- #TODO update -->
- There is 1 question in this part of the lab. It is clearly indicated as in the last part.
  - The question requires approximately 1 line of writing and address concepts, not details. Answer the questions and submit a single page containing the answers to your TA at the end of the lab. This should build upon your answers to the two questions posed in the last part of the lab.

---

## Setup

The small grey box is the USRP software‐defined radio. The USRP digitally downconverts the received (Rx) input signal into I/Q format and sends it via Ethernet to the computer. The USRP also digitally upconverts an I/Q signal from the computer to an RF signal at the transmitter (Tx) output. The USRP's Rx input is connected to a VHF (Very High Frequency) antenna on the ELW roof. The Tx output can be connected to the oscilloscope and spectrum analyzer.

- Verify that the USRP at your station is connected as shown below. If it does not, there are BNC connector cables available at the front of the lab.

  ![part2_usrp-connect.png](./figures/part2_usrp-connect.png)<br>
  __*USRP front panel*__

## I/Q Receiver

### I/Q Receiver output

- Review [IQ theory](../_docs/pdriessen_textbook.pdf) (sections 1.2 and 1.3).

- Download and open the GRC file [general-IQ-from-USRP.grc](data/general-IQ-from-USRP.grc). This flowgraph implements the mathematics on the last page of the IQ theory document.

  - The USRP source does I/Q downconversion on the WBX daughtercard and outputs the complex signals I(t) + jQ(t).
  - This output is connected to 4 blocks that extract the magnitude, phase, real and imaginary parts of the complex signal, as well as a constellation scope.
  - The USRP source is tuned to a fixed frequency of 200 MHz, i.e. the LO frequency synthesizer in the WBX daughtercard is set to 200 MHz.

- Double‐click the USRP source block to bring up a window with all of the USRP parameters. This general I/Q receiver is set up to receive a signal in a range around 200 MHz at level -10 dBm from the signal generator at the back of the lab.

- Execute the flowgraph. Observe the Output Display window with 4 tabs labelled **IQ Plane, Magnitude, Phase** and **IQ Scope Plot**.

  - The Scope Plot tab should show a circle
  - Magnitude will show a (noisy) DC level
  - Phase will show a phase ramp wrapping between -π and π (saw-tooth wave) with a period that is the reciprocal of the frequency offset (f<sub>b</sub>)
  - Real and Imaginary will show (noisy) sine waves.

  > Change the *X Max* parameter and use the *Autoscale* button on some of the plots to get a cleaner display.

- Determine the frequency *f<sub>b</sub>* of the sine waves using the Phase display as well as the Real and Imaginary displays by placing your mouse cursor over the scope plot to show the time offset at different points on the waveform. This frequency *f<sub>b</sub>* represents the offset between the received RF signal *f<sub>c</sub>* and the USRP local oscillator *f<sub>LO</sub>*, so that

  *f<sub>b</sub>* = *f<sub>c</sub> - f<sub>LO</sub>*

- Confirm that *f<sub>b</sub>* is as expected (ask your TA for *f<sub>c</sub>*)

- The USRP source block has the *Clock Source* set to use an *External* 10 MHz clock reference frequency, and the same external reference is used for the signal generator. Thus the frequency difference between the USRP source block (local oscillator) and signal generator RF frequency will be observed to be exactly as expected from their respective frequency settings.

  - If we change the USRP source block to use an *Internal* clock reference, then expect to observe some frequency error between the signal generator and the USRP frequency settings as they are running from independent oscillators.
  - Try changing the USRP clock source to *Internal* and repeat the frequency measurement of the I and Q outputs.

### Dynamic range with IQ signals

- Ask the TA to vary the 200 MHz signal generator level from ‐10 dBm (dB relative to one milliwatt) to 10 dBm in 1 dB steps.

- Observe and describe how the signals look at each signal level, and explain why.
  - The waveform appearance results from clipping in the 2 ADCs (one ADC for I, one ADC for Q).

---

#### Deliverable Question 2

Why do you see the constellation plot of the I/Q plane get squashed from a circle into a square as you increase the power of the received signal?

---

## IQ Transmitter

### Carrier wave transmission

In this section, we test the transmit functions of the USRP that we can use later when building a communications system. We will observe the transmitted spectrum, minimum and maximum power level in dBm. You will use both the osciloscope and the spectrum analyzer at your bench to view and measure the output from the USRP transmitter.

- Review the theory of [spectrum analyzers](../_docs/pdriessen_textbook.pdf) (section 1.4)
  > For more detailed information, you may also wish to review [Spectrum Analyzer Basics](../_docs/5965-7920E.pdf) and [The Basics of Spectrum Analyzers](../_docs/spec_analyzer.pdf). **The concepts presented here will be applicable to any spectrum analyzer you may use in your career.**

- Download and open [this GRC file](data/tx_carrier.grc).

- Observe that the USRP sink center frequency is set to 50 MHz. This block represents the USRP transmitter hardware.

- Observe that the sine and cosine signal sources are configured for 10 kHz.

- Connect the USRP Tx output to the spectrum analyzer and execute the flowgraph. A scope display will come up along with three buttons that allow you to select different values for Q(t).

- Set the spectrum analyzer's center frequency to 50 MHz and the span to 50 kHz by using the FREQUENCY and SPAN buttons. Adjust the LEVEL as necessary.

- What do you observe on the spectrum analyzer display with Q(t) = 0? Try the other two options for Q(t). What do you observe on the spectrum analyzer?

---

#### Deliverable Question 3

The GRC flowgraph shows a complex stream getting fed into the USRP. How come when **Q(t)=0** a real spectrum is shown on the spectrum analyzer?

---

### USRP power levels

- What is the minimum and maximum signal power output from the USRP? The USRP output power level can be set via the *QT GUI Range Widget* seen when running the flowgraph. Measure the power using both the oscilloscope and spectrum analyzer and verify they are the same.

  >Recall from your circuit theory that the output power is relative to the load impedance. On the spectrum analyzer, the input is 50Ω, which matches the expected impedance of a typical antenna. The input to the scope on the other hand is high impedance, effectively open circuit, to prevent damage to the internal circuits. To compare measurement on the scope to that of the spectrum analyzer, the line from the USRP transmitter should be terminated with a 50Ω terminator. Alternatively, some circuit theory can be applied to determine the equivalent power output measured across an open circuit and a 50Ω load.
  > Equation 1.5 in [the textbook](../_docs/pdriessen_textbook.pdf) shows that with a 50Ω input, **P<sub>dBn</sub> = 20logV + 13**.

---

#### Deliverable Question 4

Why is the minimum output power of the USRP greater than 0?

---

## Deliverables

From this part of the lab keep the following files to submit to your TA:

- The answers to [Question 2](#deliverable-question-1), [Question 3](#deliverable-question-3) and [Question 4](#deliverable-question-4)

Combine these deliverables with those from the last part of the lab and submit them as a package. As your TA where to submit them, and remember to put your name and student number on everything (If you filled out the *Options* block in the `.grc` files correctly, your student numbers will be recorded in them.)

**Do not attach the `top_block.py` files**

---

[**Back to Part 1**](Part1.md) | [**Begin Lab 3**](../_lab3/introduction.md)
