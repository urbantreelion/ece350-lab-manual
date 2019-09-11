---
layout: labitem
title: Tutorial 3
permalink: /lab1/tutorial3
firstHeading: Tutorial 3 - Understanding and transmitting AM waveforms
---

## Objectives

This tutorial is a guide to AM signal waveforms. In this tutorial you will learn:

- Theory and equations of AM signals and the complex mixer
- How to construct an AM transmitter flowgraph to generate an AM waveform with a sinusoidal message and observe the waveform and spectrum
- Construction of AM transmitter flowgraphs with square wave and pseudo-random data messages

---

## Deliverables

- GRC file of AM transmitter and receiver.

---

## AM flowgraphs

- Review AM transmitter theory in the [textbook](../_docs/pdriessen_textbook.pdf) (section 2.1).

- Using the following GRC files as a starting point:
  
  - [AM_TX.grc](data/AM_TX.grc)
  - [different_waveforms.grc](data/different_waveforms.grc)
  
 Carry out the steps in the [AM Transmitter procedures](data/AM_procedures_TX.pdf)

- Start GRC as was done in the previous tutorials. If GRC is already open, open the `.grc` files by selecting *File->Open*, or clicking on the Open logo, ![tutorial3_grc-open-icon.png](figures/tutorial3_grc-open-icon.png).

- If you are unsure of the functionality of any of the blocks in the linked tutorial, please consult the [Documentation](https://wiki.gnuradio.org/index.php/Category:Block_Docs), or ask your TA.

- Questions to be answered during the lab
  - For an AM signal as written in the text section 2.1 page 35, what is the acceptable range of the parameter k_a so that the message can be recovered perfectly in the receiver?
  - Sketch the spectrum of an AM signal with a message signal that is the sum of two cosine waves. Repeat for a message signal that is the product of two cosine waves.
  - What is the cutoff frequency of the low pass filter used in a synchronous AM receiver (where the received signal is multiplied with the carrier waveform and lowpass filtered).
  - Consider a signal with sampling rate of 256 kHz. We use rational resampler block with decimation factor of 3 and interpolation factor of 4. What would be the new sampling rate of the signal?

---

[**Begin Tutorial 4**](tutorial4.md)
