# pysdr-research-lab
# 1.Introduction
## Software-Defined Radio (SDR):
Software-Defined Radio (SDR) is a radio communication system where components that have traditionally been implemented in hardware (like mixers, filters, modulators, and demodulators) are instead implemented using software on a personal computer or an embedded system.

Instead of a fixed, single-purpose radio circuit, an SDR uses a general-purpose hardware device (like an RTL-SDR or HackRF) that simply converts radio waves into digital data. This digital data is then sent to a computer, where software is used to do all the heavy lifting of signal processing.
### Traditional Radio:
- A traditional radio is like a physical book. It's built for one purpose, and you can't easily change how it works. You buy an FM radio, and it can only receive FM signals. Its functions are "hard-wired."
### Software-Defined Radio:
- An SDR is like a computer. The hardware is just a flexible tool that processes raw data. The software you install on it determines its function. You can write software to turn it into an FM radio, an aircraft tracker, a weather satellite receiver, or a police scanner—all using the same piece of hardware
# 2.Frequency Domain
The frequency domain describes how much of the signal lies within each given frequency band over a range of frequencies.
## Time Domain vs Frequency Domain:
Time Domain: Representation of a signal as a function of time.
![](https://pysdr.org/_images/time_and_freq_domain_example_signals.png)
Frequency Domain: Representation of a signal as a function of frequency.
## Fourier Series:
The Fourier Series is a mathematical method used to represent any periodic signal as a sum of simple sine and cosine functions (or equivalently, complex exponentials) with different frequencies and amplitudes.

- It decomposes a periodic signal into its fundamental frequency and harmonics.

- This is useful for analyzing signals in both time and frequency domains.

The basics of the frequency domain start with understanding that any signal can be represented by sine waves summed together. When we break a signal down into its composite sine waves, we call it a Fourier series. Here is an example of a signal that is made up of only two sine waves:

![](https://pysdr.org/_images/summing_sinusoids.svg)

Here is another example; the red curve in the below approximates a sawtooth wave by summing up to 10 sine waves. We can see that it’s not a perfect reconstruction–it would take an infinite number of sine waves to reproduce this sawtooth wave due to the sharp transitions:

![](https://pysdr.org/_images/fourier_series_triangle.gif)

To understand how we can break down a signal into sine waves, or sinusoids, we need to first review the three attributes of a sine wave:

### 1.Amplitude:
Amplitude is the maximum value or strength of a signal from its mean position.
### 2.Frequency:
Frequency is the number of cycles of a repeating signal that occur in one second.
### 3.Phase:
Phase is used to represent how the sine wave is shifted in time, anywhere from 0 to 360 degrees (or 0 to 2pie
), but it must be relative to something to have any meaning, such as two signals with the same frequency being 30 degrees out of phase with each other.

![](https://pysdr.org/_images/amplitude_phase_period.svg)

## Time-Frequency Pairs:
Time-frequency pairs refer to the relationship between a signal's properties in the time domain and its properties in the frequency domain. This is a fundamental concept in digital signal processing (DSP) and software-defined radio (SDR).
### Key Time-Frequency Pairs
1.A single sine wave in the time domain corresponds to a single spike in the frequency domain. The sine wave's frequency will determine where the spike appears on the frequency plot. This means a simple, repetitive signal has only one tone, or frequency.

![](https://pysdr.org/_images/sine-wave.png)

2.A very sharp pulse (an "impulse") in the time domain corresponds to a flat line in the frequency domain. This means a very short, sharp burst of energy contains a huge range of frequencies.

![](https://pysdr.org/_images/impulse.png)

3.The frequency domain has a strong spike, which happens to be at the frequency of the square wave, but there are more spikes as we go higher in frequency. It is due to the quick change in time domain, just like in the previous example. But it’s not flat in frequency. It has spikes at intervals, and the level slowly decays (although it will continue forever). A square wave in time domain has a sin(x)/x pattern in the frequency domain (a.k.a. the sinc function).

![](https://pysdr.org/_images/square-wave.svg)

4.A constant, unchanging signal (a DC signal) in the time domain corresponds to a single spike at 0 Hz in the frequency domain. Since the signal is not changing at all, it has no frequency other than zero.

![](https://pysdr.org/_images/dc-signal.png)

## Fourier Transform:
The Fourier Transform is a mathematical technique that converts a signal from the time domain into the frequency domain, representing the signal in terms of its frequency components.
- It decomposes any signal into a sum of sinusoidal functions with different frequencies, amplitudes, and phases.
- Widely used in signal processing, communications, and physics to analyze the frequency content of signals.

## Time-Frequency Properties:
Time-frequency properties are fundamental relationships between how a signal behaves in the time domain and its corresponding content in the frequency domain. These properties are critical for understanding and manipulating signals in digital signal processing (DSP) and software-defined radio (SDR).

#### 1.Linearity Property:
The Fourier Transform is a linear operation, meaning the transform of a sum of signals is equal to the sum of their transforms.

<img src="https://github.com/user-attachments/assets/754869b1-dc0d-4dc8-9af9-d8680816dcfe" alt="Fourier Transform" width="572" height="96">

### 2.Frequency Shift Property:
Multiplying a signal by a complex exponential in the time domain shifts its spectrum in the frequency domain.

<img width="384" height="72" alt="Screenshot (258)" src="https://github.com/user-attachments/assets/2b3560fd-813c-48ee-b5c8-1b9403d1b5f5" />

![](https://pysdr.org/_images/freq-shift.svg)

Frequency shift is integral to DSP because we will want to shift signals up and down in frequency for many reasons. This property tells us how to do that (multiply by a sine wave). Here’s another way to visualize this property:

![](https://pysdr.org/_images/freq-shift-diagram.svg)

### 3.Scaling in Time Property:

<img width="419" height="189" alt="Screenshot (259)" src="https://github.com/user-attachments/assets/ba20be4d-1c0f-4a52-9f6e-f797c76b0c99" />


Scaling in time essentially shrinks or expands the signal in the x-axis. What this property tells us is that scaling in the time domain causes inverse scaling in the frequency domain. For example, when we transmit bits faster we have to use more bandwidth. The property helps to explain why higher data rate signals take up more bandwidth/spectrum. If time-frequency scaling was proportional instead of inversely proportional, cellular carriers would be able to transmit all the bits per second they wanted without paying billions for spectrum!

![](https://pysdr.org/_images/time-scaling.svg)

### Convolution in Time Property:
<img width="502" height="79" alt="Screenshot (260)" src="https://github.com/user-attachments/assets/8593481e-92d9-4cab-94d9-e6f73b3de9aa" />

 When we convolve time domain signals, it’s equivalent to multiplying the frequency domain versions of those two signals. It is very different from adding together two signals.

![](https://pysdr.org/_images/two-signals.svg)

### Convolution Property:
Convolution in the time domain corresponds to multiplication in the frequency domain.

## Fast Fourier Transform (FFT)
Definition: The Fast Fourier Transform (FFT) is an efficient algorithm used to compute the Discrete Fourier Transform (DFT) of a sequence.
- It reduces the computational complexity from 
𝑂
(
𝑁
2
)
O(N
2
) (normal DFT) to 
𝑂
(
𝑁
log
⁡
𝑁
)
O(NlogN), making it much faster for large datasets.
- FFT is widely used in signal processing, communications, image processing, and spectrum analysis.

![](https://pysdr.org/_images/fft-block-diagram.svg)

#### Key Points:
FFT is not a new transform → It is just a fast way to calculate the DFT.

Applications: Real-time spectrum analyzers, audio/image compression, wireless communication, SDR (Software Defined Radio).

Efficiency: Especially powerful when the number of data points 
N is a power of 2.

We will only be dealing with 1 dimension FFTs in this textbook (2D is used for image processing and other applications). For our purposes, think of the FFT function as having one input: a vector of samples, and one output: the frequency domain version of that vector of samples. The size of the output is always the same as the size of the input. If I feed 1,024 samples into the FFT, I will get 1,024 out. The confusing part is that the output will always be in the frequency domain, and thus the “span” of the x-axis if we were to plot it doesn’t change based on the number of samples in the time domain input. Let’s visualize that by looking at the input and output arrays, along with the units of their indices:

![](https://pysdr.org/_images/fft-io.svg)

Because the output is in the frequency domain, the span of the x-axis is based on the sample rate. When we use more samples for the input vector, we get a better resolution in the frequency domain (in addition to processing more samples at once).

## Negative Frequencies
Definition: Negative frequencies are the mathematical representation of sinusoidal signals that rotate in the opposite direction compared to positive frequencies in the complex plane.

In real signals, negative frequencies do not exist as separate physical signals — they are a mathematical construct that appears naturally when using Fourier Transform with complex exponentials.

For real-valued signals, the spectrum is symmetric: the positive and negative frequencies contain the same information.

![](https://pysdr.org/_images/negative-frequencies2.svg)

Now, when the SDR gives us the samples, it will appear like this:

![](https://pysdr.org/_images/negative-frequencies3.svg)

we tuned the SDR to 100 MHz. So the signal that was at about 97.5 MHz shows up at -2.5 MHz when we represent it digitally, which is technically a negative frequency. In reality it’s just a frequency lower than the center frequency.

#### Key Point:
###### Complex Exponential Representation

A sinusoid can be written as:

<img width="711" height="165" alt="Screenshot (261)" src="https://github.com/user-attachments/assets/1cb0558a-4b91-499c-b9f8-d21cd5f29e36" />


###### Why They Matter

- Negative frequencies help simplify analysis of signals in Fourier Transform and modulation.

- They make signal processing math elegant and symmetric.

###### Physical Meaning

In real life, energy is not transmitted at a “negative” frequency; instead, negative frequencies describe the mirror image of the positive spectrum.

![](https://pysdr.org/_images/negative_freq_animation.gif)

## Order in Time Doesn’t Matter
Changing the order things happen in the time domain doesn’t change the frequency components in the signal. I.e., doing a single FFT of the following two signals will both have the same two spikes because the signal is just two sine waves at different frequencies. Changing the order the sine waves occur doesn’t change the fact that they are two sine waves at different frequencies. This assumes both sine waves occur within the same time span fed into the FFT.

![](https://pysdr.org/_images/fft_signal_order.png)

## FFT in Python
OBJECTIVE:let’s actually look at some Python code and use Numpy’s FFT function, np.fft.fft().

    import numpy as np
    t = np.arange(100)
    s = np.sin(0.15*2*np.pi*t)


<img width="1854" height="988" alt="Screenshot (264)" src="https://github.com/user-attachments/assets/3f2b9d1f-fbcb-421f-ab5e-f7d7d15a1cc2" />

<img width="1829" height="894" alt="Screenshot (265)" src="https://github.com/user-attachments/assets/4e4bb912-479c-4da8-9ff7-f66b0fac4ec7" />

### FFT shift using python

#### Final Code Example

    import numpy as np
    import matplotlib.pyplot as plt
    
    Fs = 1 # Hz
    N = 100 # number of points to simulate, and our FFT size
    
    t = np.arange(N) # because our sample rate is 1 Hz
    s = np.sin(0.15*2*np.pi*t)
    S = np.fft.fftshift(np.fft.fft(s))
    S_mag = np.abs(S)
    S_phase = np.angle(S)
    f = np.arange(Fs/-2, Fs/2, Fs/N)
    plt.figure(0)
    plt.plot(f, S_mag,'.-')
    plt.figure(1)
    plt.plot(f, S_phase,'.-')
    plt.show()
##### Got the Code running and the required plots
<img width="1902" height="820" alt="Screenshot (266)" src="https://github.com/user-attachments/assets/2ef24514-4481-4372-a832-56f10659dbb1" />

<img width="1863" height="962" alt="Screenshot (267)" src="https://github.com/user-attachments/assets/065b1cfd-2348-458b-a43e-eb450a7c4754" />

## Windowing
When we use an FFT to measure the frequency components of our signal, the FFT assumes that it’s being given a piece of a periodic signal. It behaves as if the piece of signal we provided continues to repeat indefinitely. It’s as if the last sample of the slice connects back to the first sample. It stems from the theory behind the Fourier Transform. It means that we want to avoid sudden transitions between the first and last sample because sudden transitions in the time domain look like many frequencies, and in reality our last sample doesn’t actually connect back to our first sample. To put it simply: if we are doing an FFT of 100 samples, using np.fft.fft(x), we want x[0] and x[99] to be equal or close in value.

![](https://pysdr.org/_images/windows.svg)

A simple approach for beginners is to just stick with a Hamming window, which can be created in Python with np.hamming(N) where N is the number of elements in the array, which is your FFT size. In the above exercise, we would apply the window right before the FFT. After the 2nd line of code we would insert:

 s = s * np.hamming(100)

 ## FFT Sizing
 FFT sizing refers to choosing the number of sample points (length) for the Fast Fourier Transform (FFT). The size directly determines the frequency resolution, accuracy, and computational efficiency of the frequency-domain representation. Selecting the right FFT size is crucial in signal processing and software-defined radio (SDR) applications because it affects how clearly and accurately the spectral components of a signal can be observed.

#### Key Points:
###### 1.Frequency Resolution:
- Larger FFT size gives finer frequency bins.

- Frequency resolution is calculated as:

where f_s is the sampling frequency and N is the FFT size.

###### 2.Trade-offs:
- Large FFT size: Higher frequency resolution but more computation and memory usage.

- Small FFT size: Faster computation but coarser frequency bins and lower spectral detail.

###### 3.Relation to Windowing:
- FFT size selection is critical for windowed signals too. Windowing smooths the signal edges, and the proper FFT size helps minimize spectral leakage.

###### 4.Practical Considerations:
- Power-of-2 FFT sizes (128, 256, 512, 1024…) are commonly used for faster radix-2 FFT computation.

- In SDR and spectrum analysis, FFT size is adjusted according to signal duration and the desired frequency resolution.

## Spectrogram/Waterfall
A spectrogram (also called a waterfall plot) is a time-varying visualization of a signal’s frequency content. It shows how the spectrum of a signal evolves over time. On the plot:

- X-axis = Time

- Y-axis = Frequency

- Color intensity = Signal power (amplitude) at a given frequency and time

In SDR (Software Defined Radio), this is often displayed as a waterfall, where new frequency slices are continuously added (like water flowing down), allowing the user to see both the current spectrum and its historical changes.

![](https://pysdr.org/_images/spectrogram_diagram.svg)

###### Key Points:
1.A spectrogram is essentially a sequence of FFTs taken on short overlapping time windows.

2.The waterfall view helps track signals that vary in time (e.g., bursts, modulated carriers, or interference).

3.Strong signals appear as bright lines, while weak or absent signals appear dark.

4.It is widely used in SDR receivers, spectrum analyzers, and communication systems for monitoring signal activity.

![](https://pysdr.org/_images/waterfall.png)

Got the required spectrogram after running the example code

    import numpy as np
    import matplotlib.pyplot as plt
    
    sample_rate = 1e6
    
    # Generate tone plus noise
    t = np.arange(1024*1000)/sample_rate # time vector
    f = 50e3 # freq of tone
    x = np.sin(2*np.pi*f*t) + 0.2*np.random.randn(len(t))
    # simulate the signal above, or use your own signal
    
    fft_size = 1024
    num_rows = len(x) // fft_size # // is an integer division which rounds down
    spectrogram = np.zeros((num_rows, fft_size))
    for i in range(num_rows):
        spectrogram[i,:] = 10*np.log10(np.abs(np.fft.fftshift(np.fft.fft(x[i*fft_size:(i+1)*fft_size])))**2)
    
    plt.imshow(spectrogram, aspect='auto', extent = [sample_rate/-2/1e6, sample_rate/2/1e6, len(x)/sample_rate, 0])
    plt.xlabel("Frequency [MHz]")
    plt.ylabel("Time [s]")
    plt.show()
<img width="1661" height="986" alt="Screenshot (268)" src="https://github.com/user-attachments/assets/6d98b9d5-49e7-4f11-bf3a-e81998762ff0" />

## FFT Implementation

For those who prefer to think in code rather than equations, the following shows a simple Python implementation of the FFT, along with an example signal consisting of a tone plus noise, to try the FFT out with

    import numpy as np
    import matplotlib.pyplot as plt
    
    def fft(x):
        N = len(x)
        if N == 1:
            return x
        twiddle_factors = np.exp(-2j * np.pi * np.arange(N//2) / N)
        x_even = fft(x[::2]) # yay recursion!
        x_odd = fft(x[1::2])
        return np.concatenate([x_even + twiddle_factors * x_odd,
                               x_even - twiddle_factors * x_odd])
    
    # Simulate a tone + noise
    sample_rate = 1e6
    f_offset = 0.2e6 # 200 kHz offset from carrier
    N = 1024
    t = np.arange(N)/sample_rate
    s = np.exp(2j*np.pi*f_offset*t)
    n = (np.random.randn(N) + 1j*np.random.randn(N))/np.sqrt(2) # unity complex noise
    r = s + n # 0 dB SNR
    
    # Perform fft, fftshift, convert to dB
    X = fft(r)
    X_shifted = np.roll(X, N//2) # equivalent to np.fft.fftshift
    X_mag = 10*np.log10(np.abs(X_shifted)**2)
    
    # Plot results
    f = np.linspace(sample_rate/-2, sample_rate/2, N)/1e6 # plt in MHz
    plt.plot(f, X_mag)
    plt.plot(f[np.argmax(X_mag)], np.max(X_mag), 'rx') # show max
    plt.grid()
    plt.xlabel('Frequency [MHz]')
    plt.ylabel('Magnitude [dB]')
    plt.show()

FFT implementation example code with a successful plot output.

<img width="1661" height="986" alt="Screenshot (268)" src="https://github.com/user-attachments/assets/d708d951-da8b-478c-b032-2552374ce9ba" />

# IQ Sampling
We also cover Nyquist sampling, complex numbers, RF carriers, downconversion, and power spectral density. IQ sampling is the form of sampling that an SDR performs, as well as many digital receivers (and transmitters).

## Sampling Basics

The microphone is a transducer that converts sound waves into an electric signal (a voltage level). That electric signal is transformed by an analog-to-digital converter (ADC), producing a digital representation of the sound wave. To simplify, the microphone captures sound waves that are converted into electricity, and that electricity in turn is converted into numbers. 

Whether we are dealing with audio or radio frequencies, we must sample if we want to capture, process, or save a signal digitally.Let’s say we have some random function,s(t) 
, which could represent anything, and it’s a continuous function that we want to sample:

![](https://pysdr.org/_images/sampling.svg)

We record the value of at regular intervals of seconds, known as the sample period. The frequency at which we sample, i.e., the number of samples taken per second, is simply . We call this the sample rate, and it’s the inverse of the sample period.

## Nyquist Sampling
The Nyquist Sampling Theorem (also called the Shannon–Nyquist theorem) states that:

A continuous-time signal can be perfectly reconstructed from its samples if it is sampled at a rate that is at least twice the maximum frequency present in the signal.

###### Key Points:

1.Sampling Rate ≥ 2 × f<sub>max</sub>

- If a signal’s highest frequency component is f<sub>max</sub>, then the minimum sampling rate must be 2fmax.

- This minimum rate is called the Nyquist Rate

2.Nyquist Frequency

- It is equal to half of the sampling rate (f<sub>s</sub>/2).

- This is the highest frequency that can be correctly represented.

3.Aliasing

- If the signal is sampled below the Nyquist rate, different frequency components overlap and distort each other.

- This effect is called aliasing

4.Practical Use

In digital communication, SDR, and DSP, Nyquist sampling ensures that no information is lost when converting an analog signal to digital form.

## Quadrature Sampling

Quadrature Sampling (also called Complex Sampling or IQ Sampling) is a method of representing a bandpass signal by mixing it with two sinusoids that are 90° out of phase:

- In-phase (I): Signal × cos(2πf<sub>c</sub>t)

- Quadrature (Q): Signal × sin(2πf<sub>c</sub>t)

![](https://pysdr.org/_images/IQ_wave.png)

The two outputs (I and Q) together form a complex baseband signal:

x(t)=I(t)+jQ(t)
This allows a high-frequency bandpass signal to be shifted down to baseband for easy digital processing.

# Complex Numbers

the IQ convention is an alternative way to represent magnitude and phase, which leads us to complex numbers and the ability to represent them on a complex plane. 

![](https://pysdr.org/_images/complex_plane_1.png)

phase is the angle between the vector and 0 degrees, which we define as the positive real axis:

![](https://pysdr.org/_images/complex_plane_2.png)

<img width="1543" height="896" alt="Screenshot (271)" src="https://github.com/user-attachments/assets/9a9e7f4d-45d1-44ea-985d-05041675b7d9" />

# Complex Numbers in FFTs

 When you take the FFT of a series of samples, it finds the frequency domain representation. We talked about how the FFT figures out which frequencies exist in that set of samples (the magnitude of the FFT indicates the strength of each frequency). But what the FFT also does is figure out the delay (time shift) needed to apply to each of those frequencies, so that the set of sinusoids can be added up to reconstruct the time-domain signal. That delay is simply the phase of the FFT. The output of an FFT is an array of complex numbers, and each complex number gives you the magnitude and phase, and the index of that number gives you the frequency. 

 # Receiver Side

 the perspective of a radio receiver that is trying to receive a signal (e.g., an FM radio signal). Using IQ sampling, the diagram now looks like:

 ![](https://pysdr.org/_images/IQ_diagram_rx.png)

 What comes in is a real signal received by our antenna, and those are transformed into IQ values.using two ADCs, and then we combine the pairs and store them as complex numbers. In other words, at each time step, you will sample one I value and one Q value and combine them in the form I+JQ (i.e., one complex number per IQ sample). There will always be a “sample rate”, the rate at which sampling is performed. Someone might say, “I have an SDR running at 2 MHz sample rate.Throughout this textbook you will become very familiar with how IQ samples work, how to receive and transmit them with an SDR, how to process them in Python, and how to save them to a file for later analysis

# Carrier and Downconversion
#### Carrier:
- A carrier is a high-frequency sinusoidal signal (cosine or sine wave) used to carry information in communication systems.

- Mathematically:
C(t)= A cos(2pieft+fie)

where is the carrier frequency. 

- In modulation (AM, FM, QAM etc.), the baseband information (voice, data, video) is superimposed on the carrier.

- Example: FM radio at 100 MHz → the 100 MHz sine wave is the carrier.

#### Downconversion:
- Downconversion is the process of shifting a high-frequency signal (RF, bandpass) down to a lower frequency (baseband or IF) so it can be easily processed.

- It is done by mixing the signal with a carrier (local oscillator, LO) inside a receiver.

- the RF signal is shifted to baseband (centered at 0 Hz).

- This is the essence of quadrature downconversion (I/Q sampling) in SDRs.


# Receiver Architectures
Receiver Architecture is the systematic design and organization of different functional blocks in a communication receiver that process incoming radio frequency (RF) signals and recover the original transmitted information. It defines how signals move through the receiver—from the antenna, through amplification, filtering, frequency downconversion, and analog-to-digital conversion—before being processed in the digital domain (DSP).

It specifies the signal flow, the role of each stage (RF front-end, mixer, local oscillator, baseband processing), and the trade-offs in sensitivity, selectivity, noise, complexity, and cost.

![](https://pysdr.org/_images/receiver_arch_diagram.svg)

# Baseband and Bandpass Signals
The difference between baseband and bandpass signals, which are fundamental concepts in radio communication.

### Baseband Signals

A baseband signal is a signal centered around 0 Hz. It represents the raw data, like an audio signal or the output of a downconverted radio signal. Because it's at a low frequency, it requires a lower sample rate to capture, making it efficient to work with on a computer.

### Bandpass Signals

A bandpass signal is a signal that exists at some higher radio frequency (RF), away from 0 Hz. This is the type of signal that is actually transmitted through the air (e.g., Wi-Fi, FM radio). Bandpass signals are always real and do not contain imaginary components, as you cannot transmit imaginary data.

![](https://pysdr.org/_images/baseband_bandpass.png)

# DC Spike and Offset Tuning

### 1.DC Spike

- In SDR receivers (especially direct conversion receivers), the Local Oscillator (LO) leaks into the mixer, producing a constant DC component at 0 Hz in the spectrum.

- When you look at the FFT of the signal, it shows up as a tall spike exactly at DC (0 Hz).

- This spike is not a real signal—it’s an artifact of the hardware.

👉 Example: If you tune your SDR to 100.0 MHz, you may see a big spike at the center of the spectrum (0 Hz relative). That’s the DC spike.

### 2.Offset Tuning

- To avoid the DC spike interfering with your signal of interest, SDRs often use offset tuning.

- In offset tuning, instead of tuning the LO exactly at the desired frequency, the LO is shifted (offset) by a small amount.

- The signal of interest is received away from DC, and later in digital processing it is shifted back to the correct frequency.

👉 This moves the DC spike out of the way, so the actual signal is not corrupted.

![](https://pysdr.org/_images/dc_spike.png)

![](https://pysdr.org/_images/offtuning.png)

  
 
 
 













