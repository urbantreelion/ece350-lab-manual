---
layout: labitem
title: Part 3
permalink: /lab3/FM-receiver-USRP
firstHeading: Part 3 - FM receiver with USRP
---

[**Back to Part 2**](FM-receiver-simulation.md) | [**Begin Part 4**](FLEX-sync.md)

## Objectives

This part of the lab is a guide to receiving real FM signal waveforms. You
will:
<!-- #TODO update -->
- use a premade receiver to 

---

## Part 3 Deliverables
<!-- #TODO review -->

<!-- - Two GRC files of SSB demodulators. You will be stepped through building them. -->
- There is 1 question in this part. It is clearly indicated.
  - The question requires approximately 1 line of writing, and addresses a concept, not details. Answer the question and submit a single page containing the answers to your TA at the end of the lab.

---

## FM broadcast receiver

In this section, we consider a practical FM receiver that can receive real off-air FM signals using the USRP.

- Open [this GRC flowgraph](data/FM_USRP_receiver.grc).

- This flowgraph implements an FM receiver. Compare this receiver to the one you made.

  - What new blocks are added and what is their function?
  - Which blocks are identical or perform similar functions?

- Execute the flowgraph. The screen shows a spectrum and several sliders. The radio is tuned to 98.1 MHz (an FM station in Seattle), the RF gain is set to 7 and the AF gain is set to 0.3. The radio can be tuned using the sliders.

- Notice the noise level is around ‐100 dBm and the signal peak level is 20‐40 dB higher than that. Notice the spectrum analyzer covers a bandwidth of 250 kHz corresponding to the half the sampling rate set for the USRP source block in the flowgraph.

## FM data receiver

- Modify the FM receiver flowgraph by replacing the FM demodulation block with the "homemade" FM demodulator using the *Delay* and *Complex Conjugate* blocks.

- Use the USRP to tune to the 2-level FSK signal at 142.17 MHz. This signal is the control channel for the [CREST public safety radio system](http://www.crest.ca/).

- Observe the demodulator output using a *QT GUI Time Sink*. The filter bandwidth and decimation rate may require adjustment, since the bandwidth of this CREST signal is about 25 kHz compared to 200 KHz for FM broadcast signals.

- Check the persistence box on the scope and reduce the alpha to minimum. You will observe a so-called [eye diagram](http://en.wikipedia.org/wiki/Eye_pattern) of the data, as shown below. The eye diagram shows the number of milliseconds per bit. Find the bit rate (the number of bits per second).

  ![eye_diagram.png](./figures/eye_diagram.png)<br>
  __*Eye diagram of the data shown in a GNU Radio Companion scope sink.*__

## Dynamic Range with FM

- Review the theory of [dynamic range](data/DynamicRange.pdf). These notes will also be useful for subsequent sections on dynamic range with IQ signals and on noise figure.

- Retune the USRP to the FM broadcast station at 98.1 MHz.

- Increase the RF gain from 7 dB to 20 dB or more.
  - Notice that the signal level increases and then suddenly both noise and signal jump up and the audio changes to a different program. What is happening is that a strong signal somewhere within the 40 MHz bandwidth of the USRP's receiver is clipping the 14 bit A/D converter in the USRP. The 14 bit A/D converter has a dynamic range of about 84 dB (14 bits times 6 dB per bit), so a signal above about ‐100 dBm + 84 dB = ‐16 dBm will clip the converter. With the RF gain set to around 20 dB, the receiver becomes non‐linear and the audio from the strong signal is cross‐modulating on top of the signal at 98.1 MHz.
  - Cross‐modulation can be shown to occur by modelling the non‐linear receiver as having the output:

    $$ y(t) = a_1 s(t) + a_2 s^2 (t) + a_3 s^3 (t) $$

     (ignoring higher order terms), where $$ s(t) $$ is the sum of the strong and the weak signal.
  - Reduce the RF gain and notice that the original signal is restored. Next, we will look for this strong signal.

- Tune the FM receiver to 101.9 MHz (the local UVic radio station CFUV). Notice that the signal level is much higher (close to ‐30 dBm). Now increase the RF gain to at least 20 dB and observe the signal level can be increased to above ‐16 dBm, however, the audio is not changed. This signal at 101.9 MHz was causing the clipping.

- Experiment more with the FM receiver. Notice that many signals can be received, FM signals are spaced every 0.2 MHz with an odd last digit, from 88.1 MHz up to 107.9 MHz.

---

## Deliverables

From this lab part, there are the following deliverables:
<!-- #TODO add -->
<!-- - `FM_receiver.grc` -->
<!-- - The answer to [Question 1](#deliverable-question-1) -->

<!-- **Do not attach the `top_block.py` or `.dat` files.** -->

---

[**Back to Part 2**](FM-receiver-simulation.md) | [**Begin Part 4**](FLEX-sync.md)
