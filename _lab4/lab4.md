---
layout: labitem
title: Lab 4
permalink: /lab4/
---

# Lab 4 - PSK systems

## Deliverables

- modified complex BPSK flowgraph

- block diagram version of modified complex BPSK flowgraph showing a mathematical representation of the signal at each point

- a comment on the difference between two methods of pulse shaping: using square waves versus using an impulse train as input to a pulse shaping filter.

- a few sentences explaing your observations with non-zero frequency offset

## Testing a PSK system

In this section, we modify a complete PSK transmitter-receiver system assuming a perfect channel and perfect synchronization (timing).

Start by reviewing the theory in the text chapters 6 (pulse shaping) and 7 (PSK) and the GRC patches therein. The following GRC patches (flowgraphs) will help you understand how BPSK and QPSK are generated:

- Real BPSK: [bpsk_txrx_real.grc](./data/bpsk_txrx_real.grc)

- Complex BPSK: [bpsk_txrx_complex.grc](./data/bpsk_txrx_complex.grc)

- Real QPSK: [qpsk_txrx_real.grc](./data/qpsk_txrx_real.grc)

- Complex QPSK: [qpsk_txrx_complex.grc](./data/qpsk_txrx_complex.grc)

Observe that for each of these 4 flowgraphs, the signal source is a square wave (not an impulse train) representing a `1010...` data pattern represented at the given sample rate. What is the bit rate and the symbol rate in each case? How many samples per symbol? Observe how the signal constellation rotates when the frequency offset is not equal to zero.

We now modify the Complex BPSK flowgraph to use a random data pattern and a non-square pulse shape.

- Replace the signal source with a random data pattern generated using a generalized linear feedback shift register (*GLFSR source*).

- The GLFSR source outputs one sample per bit. Use a *Repeat* block so that each data bit generates a square pulse. What interpolation factor should be chosen and why?

- Add a scope sink at the output of the *Repeat* block to observe the square pulses.

- Add a low pass filter to shape the square pulses into more rounded pulses. What is the advantage of using a rounded pulse rather than a square pulse?

- Add a scope sink at the output of the low pass filter to observe the transmitted eye diagram with rounded pulses.

- Add a scope sink at the output of the receiver to observe the received eye diagram. Observe how the received eye diagram changes as the frequency offset is changed from zero. Explain your observations.

- Add a scope sink in XY mode at the output of the receiver to observe the received signal constellation. This constellation is not sampled, so it includes the entire waveform, not just the points where the data is sampled. Observe how the constellation changes as the frequency offset is changed from zero. Explain your observations.

In the following steps, we create the pulse shape using impulse inputs
instead of square wave inputs. This method enables the use of an FIR
pulse shaping filter such as the *Root Raised Cosine Filter* whose
coefficients (taps) exactly represent the desired pulse shape.

- The GLFSR source outputs one sample per bit. We want to create an impulse train waveform with many samples per bit that will be the input to a pulse shaping filter.

- Use an *Interpolating FIR filter* so that each data bit generates an impulse. The interpolation factor should be chosen to be the same as before. Why? The filter has only one tap set to be the same as the interpolation factor.

- Add a scope sink at the output of the *Interpolating FIR filter* to observe the impulse train waveform.

- Add an FIR low pass filter to shape the impulses into pulses with reasonable pulse shape.

- Replace the Interpolating FIR filter and the FIR low pass filter with a *Root Raised Cosine Filter*. The input to the RRC filter is at the symbol rate (one sample per bit). The filter gain should be set to be the same number as the interpolation factor. Set the RRC filter length to an appropriate value, try different values and observe the effect.

- Add a scope sink at the output of the *Root Raised Cosine Filter* to
observe the transmitted eye diagram.

- Add a scope sink at the output of the receiver to observe the
received eye diagram. Observe how the received eye diagram changes
as the frequency offset is changed from zero.

- Add a scope sink in XY mode at the output of the receiver to observe the received signal constellation. Observe how the constellation changes as the frequency offset is changed from zero. Explain your observations.

Optional: repeat the above changes using the Complex QPSK flowgraph.

Optional modifications: add a noise source, receiver filter and bit error rate counter

**Congratulations, you've completed the lab portion of ECE 350!**