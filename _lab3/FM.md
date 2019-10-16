---
layout: labitem
title: Part 1
permalink: /lab3/FM-signals
firstHeading: Part 1 - FM Signal waveforms
---

[**Back to Lab 3 Intro**](introduction.md) | [**Begin Part 2**](FLEX-sync.md)

## Objectives

This part of the lab is a guide to FM signal waveforms. In this tutorial you
will:

- learn the theory and equations of FM signals, power spectra, bandwidths, and FM demodulation

- construct an FM transmitter flowgraph to generate an FM waveform with both a sinusoidal message and a square wave message

- construct an FM receiver flowgraph to recover the message from the FM waveform

---

## Part 1 Deliverables
<!-- #TODO review -->

- Two GRC files of SSB demodulators. You will be stepped through building them.
- There is 1 question in this part. It is clearly indicated.
  - The question requires approximately 1 line of writing, and addresses a concept, not details. Answer the question and submit a single page containing the answers to your TA at the end of the lab.

---

## FM flowgraphs

You are going to build flowgraphs to transmit and receive FM signals that are simulation-only and do not (yet) use the USRP (that will come later in this lab!).

### FM transmitter theory

- Review the theory of [FM Signals](../_docs/pdriessen_textbook.pdf) (section 5).
- Recall that for a sinusoidal modulating wave,

    ***m(t) = A<sub>m</sub>cos(2πf<sub>m<\sub>t)***

  the FM wave can be written as any of:
    ***s(t) = A<sub>c</sub>cos(2πf<sub>c<\sub>t + βsin(2πf<sub>m<\sub>t))***
    ***s(t) = I(t)cos(2πf<sub>c<\sub>t) - Q(t)sin(2πf<sub>c<\sub>t)***
    ***s(t) = a(t)cos(2πf<sub>c<\sub>t +ϕ(t))***

  where:
    ***a(t) = A<sub>c</sub>***
    ***ϕ(t) = βsin(2πf<sub>m<\sub>t)***
    ***i(t) = A<sub>c</sub>cos(2πf<sub>m<\sub>t)***
    ***q(t) = A<sub>c</sub>sin(2πf<sub>m<\sub>t)***

<!-- #TODO what is beta -->

<!-- 
- Using the following GRC files as a starting point:
  - [FM_Transmitter.grc](data/FM_Transmitter.grc)
  - [fm_receiver.grc](data/fm_receiver.grc)

- Carry out the steps in the [FM Tutorial](data/FM_procedures.pdf) -->

### Building an FM transmitter for a sine message

You'll start by transmitting a sinusoidal message. The equations for this are shown in the [theory section](#fm-transmitter-theory) above.

- Start GRC and change the default sampling rate to be 200 kHz.

- This flowgraph will be your first deliverable. Save it as `FM_transmitter.grc`, and in the *Options* block, set the following:

  - **Title:** FM transmitter
  - **Author:** V00xxxxxx, V00yyyyyy (where all of your student numbers are included)

- From the equations above, create some variables controlled with sliders.
  - Add a *QT GUI Range* for each of ***f<sub>m</sub>***, ***f<sub>c</sub>*** and ***β***.
  - For the message frequency, range from 0-10 kHz with a default value of 2 kHz.
  - For the carrier frequency, range from 0-100 kHz with a default value of 25 kHz.
  - For the beta value, fange from 0-4 with a default value of 4, and a step size of 0.025.

- Construct a flowgraph representation of ***ϕ(t)*** using a *Signal Source* block and a *Multiply Const* block.
  - Change the *Output Type* to *Float*.
  - Use the variables from the *QT GUI Range* widgets for ***β*** and ***f<sub>m</sub>***.
  - The flowgraph should now look like the following figure.

    ![fmtx_phi-of-t-grc.png](./figures/fmtx_phi-of-t-grc.png)<br>
    __*Graphical representation of ϕ(t)*__

  - **Can you read this flowgraph as equivalent to the equation for ***ϕ(t)***?**

- Add a *Throttle* block after the ***β*** multiplication.
- In order to take the sine and cosine of ***ϕ(t)***, split the output of the *Throttle* block into two *Transcendental* blocks.
  - One of the *Transcendental* blocks should have `cos` set as the function, and the other should have *sin* set as the function.
  - These two streams represent ***I(t)*** and ***Q(t)*** from the equations in the [theory section](#fm-transmitter-theory) above. Notice that ***A<sub>c</sub>*** is not used and so is equal to 1.

- Following the equations in the [theory section](#fm-transmitter-theory), ***I(t)*** and ***Q(t)*** are multiplied by ***cos(2πf<sub>c<\sub>t)*** and ***-sin(2πf<sub>c<\sub>t)*** of the carrier frequency respectively.
  - Add two *Signal Source* blocks and two *Multiply* blocks.
  - Ensure that ***cos(2πf<sub>c<\sub>t)*** is multiplied with ***I(t)*** while ***-sin(2πf<sub>c<\sub>t)*** is multiplied with ***Q(t)***.
    - To get the negative value in front of the sine, choose an amplitude of -1 in the appropriate *Signal Source* block.

- Use an *Add* block to combine the two streams and obtain ***s(t)***. The flowgraph should look like the figure below.

  ![fmtx_s-of-t-grc.png](./figures/fmtx_s-of-t-grc.png)<br>
  __*Graphical representation of s(t)*__

- Visualize the output of the flowgraph using a:
  - *QT GUI Time Sink* at ***m(t)***
  - *QT GUI Time Sink* at ***s(t)***
  - *QT GUI Frequency Sink* at ***s(t)***
  > As always, you are free to use the *QT GUI Tab Widget* to organize the GUI however you like. If you need a refresher on how to use this tool, it is covererd in the [intro tutorials](../_intro/introduction.md).

- Leaving default values unchanged, the output should look like the following figures.
  
  ![fmtx_m-of-t-sine-scope.png](./figures/fmtx_m-of-t-sine-scope.png)<br>
  __*m(t) with default values in time domain*__

  ![fmtx_s-of-t-sine-scope.png](./figures/fmtx_s-of-t-sine-scope.png)<br>
  __*s(t) with default values in time domain*__

  ![fmtx_s-of-t-sine-fft.png](./figures/fmtx_s-of-t-sine-fft.png)<br>
  __*s(t) with default values in frequency domain*__

  - Experiment with adjusting the three sliders.

---
#### Deliverable Question 1

What does beta control?

---

- Add a *File Sink* block as well to capture ***s(t)*** but leave it disabled for now.

- Your flowgraph should look similar to the following.

  ![fmtx_sine-modulator-grc.png](./figures/fmtx_sine-modulator-grc.png)<br>
  __*Graphical representation of s(t)*__

- Enable the *File Sink* block and save a file called `FM_TX_2kHz_sine.dat`. You will need to execute the flowgraph for a few seconds to build the file.

### Building an FM transmitter for an FSK message

Up until now the transmitted message has been a sine wave of frequency ***f<sub>m</sub>***. You will now simulate transmitting a Frequency Shift Keying (FSK) signal by transmitting a square waveform (which is an FSK pattern of 101010...).

The integral of a square waveform is a triangular waveform with the same frequency as the square waveform. So for frequency modulation, it is enough to replace the sunisoidal source in the flowgraph with a triangular source.

- Change the *Signal Source* block to output a triangular waveform of frequency ***f<sub>m</sub>***.

- With the *File Sink* block disabled, execute the flowgraph and observe the various plots. Adjust the sliders to see how it impacts the transmitted signal.

- The following figures show ***m(t)*** and ***s(t)*** with default parameters.

  ![fmtx_m-of-t-square-scope.png](./figures/fmtx_m-of-t-square-scope.png)<br>
  __*m(t) with default values in time domain*__

  ![fmtx_s-of-t-square-scope.png](./figures/fmtx_s-of-t-square-scope.png)<br>
  __*s(t) with default values in time domain*__

  ![fmtx_s-of-t-square-fft.png](./figures/fmtx_s-of-t-square-fft.png)<br>
  __*s(t) with default values in frequency domain*__

- Enable the *File Sink* block and save a file called `FM_TX_2kHz_square.dat`. You will need to execute the flowgraph for a few seconds to build the file.

- Save this GRC file now, it is a deliverable.

### FM broadcast receiver

In this section, we consider a practical FM receiver that can receive
real off-air FM signals using the USRP.

- Open [this GRC flowgraph](data/wfm_rx.grc).

- Observe the flowgraph comprising various blocks and interconnections, as well as predefined variable blocks not connected to anything.
  - Blocks with blue connection points signify complex signals (I and Q), and orange connection points signify real floating point signals.
  - The USRP source block represents the USRP receiver hardware. This block outputs a complex signal that travels through various blocks (including sample rate conversion), eventually arriving at an audio sink.
  - The audio sink represents the computer's audio output hardware.

- This flowgraph implements an FM receiver.Compare this receiver to the FM receiver from the FM waveforms tutorial in the previous section.
  - What new blocks are added and what is their function?
  - Which blocks are identical or perform similar functions?

- Execute the flowgraph by first clicking the Generate button followed by the Execute button. The screen shows an FM receiver with a spectrum analyzer display and several sliders. The radio is tuned to 98.1 MHz (an FM station in Seattle), the RF gain is set to 7 and the AF gain is set to 300 m. The radio can be tuned using the sliders or the keyboard arrow keys.

- Notice the noise level is around ‐100 dBm and the signal peak level is 20‐40 dB higher than that. Notice the spectrum analyzer covers a bandwidth of 250 kHz corresponding to the half the sampling rate set for the USRP source block in the flowgraph.

### FM data receiver

- Modify the FM receiver flowgraph by replacing the FM demodulation block with the "homemade" FM demodulator using the Delay and Complex Conjugate blocks.

- Use the USRP to tune to the 2-level FSK signal at 142.17 MHz. This signal is the control channel for the [CREST public safety radio system](http://www.crest.ca/).

- Observe the demodulator output on the scope. The filter bandwidth and decimation rate may require adjustment, since the bandwidth of this CREST signal is about 25 kHz compared to 200 KHz for FM broadcast signals.

- Check the persistence box on the scope and reduce the alpha to minimum. You will observe a so-called [eye diagram](http://en.wikipedia.org/wiki/Eye_pattern) of the data, as shown below. The eye diagram shows the number of milliseconds per bit. Find the bit rate (the number of bits per second).

  ![eye_diagram.png](./figures/eye_diagram.png)<br>
  __*Eye diagram of the data shown in a Gnu Radio Companion scope sink.*__

### Dynamic Range with FM

- Review the theory of [dynamic range](data/DynamicRange.pdf). These notes will also be useful for subsequent sections on dynamic range with IQ signals and on noise figure.

- Retune the USRP to the FM broadcast station at 98.1 MHz.

- Increase the RF gain from 7 dB to 20 dB or more.
  - Notice that the signal level increases and then suddenly both noise and signal jump up and the audio changes to a different program. What is happening is that a strong signal somewhere within the 40 MHz bandwidth of the USRP's receiver is clipping the 14 bit A/D converter in the USRP. The 14 bit A/D converter has a dynamic range of about 84 dB (14 bits times 6 dB per bit), so a signal above about ‐100 dBm + 84 dB = ‐16 dBm will clip the converter. With the RF gain set to around 20 dB, the receiver becomes non‐linear and the audio from the strong signal is cross‐modulating on top of the signal at 98.1 MHz.
  - Cross‐modulation can be shown to occur by modelling the non‐linear receiver as having the output: ![cross-mod-eqn](figures/eqn_cross_modulation.png) (ignore higher order terms), where s(t) is the sum of the strong and the weak signal.
  - Reduce the RF gain and notice that the original signal is restored. Next, we will look for this strong signal.

- Tune the FM receiver to 101.9 MHz (the local UVic radio station CFUV). Notice that the signal level is much higher (close to ‐30 dBm). Now increase the RF gain to at least 20 dB and observe the signal level can be increased to above ‐16 dBm, however, the audio is not changed. This signal at 101.9 MHz was causing the clipping.

- Experiment more with the FM receiver. Notice that many signals can be received, FM signals are spaced every 0.2 MHz with an odd last digit, from 88.1 MHz up to 107.9 MHz.
