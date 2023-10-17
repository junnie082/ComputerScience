# Data Communication 03

### Introduction to Physical Layer 1

#### : Data and Signals

#### 2023.10.17.(화)

## Data and signals

#### : Periodic analog signals, Digital signals

### Communication at PHY layer

- Communication at physical (PHY) layer

  - A scenario in which a scientist working in a research company, Sky Research, needs to order a book related to her research from an online bookseller, Scientific Books.

- Alice and Bob need to exchange data

- Communication at the PHY layer means exchanging signals

- Both data and signals: analog or digital

![datacommunication1](/assets/images/2023-10-17-2/datacommunication1.png)

### Analog and Digital Data

- Data can be analog or digital.
  - The term analog data refers to information that is continuous;
  - Digital data refers to informaion that has discrete states.
  - For example,
    - An analog clock that has hour, minute and second hands give information in a continuous form; the movements of the hands are continuous.
    - On the other hand, a digital clock that reports the hours and the minutes will change suddenly from 8:05 to 8:06.

### Analog and Digital Signals

- Like the data they represent, signals can be either analog or digital.

  - An analog signal has infinitely many levels of intensity over a period of time.
    - As the wave moves from value A to value B, it passes through and includes an infinite number of values along its path.
  - A digital signal, on the other hand, can have only a limited number of defined values.
    - Although each value can be any number, it is often as simple as 1 and 0.

- Comparison of analog and digital signals

![datacommunication2](/assets/images/2023-10-17-2/datacommunication2.png)

### Periodic and Nonperiodic

- A periodic signal completes a pattern within a measurable time frame, called a period, and repeats that pattern over subsequent identical periods.

  - The completion of one full pattern is called a cycle.

- A nonperiodic signal changes without exhibiting a pattern or cycle that repeats over time.

- Both analog and digital signals.

### Periodic Analog Signal

- Periodic analog signals can be classified as simple or composite.
  - A simple periodic analog signal, a sine wave, cannot be decomposed into simpler signals.
  - A composite periodic analog signal is composed of multiple sine waves.

### Sine Wave

- The sine wave is the most fundamental form of a periodic analog signal.
  - When we visualize it as a simple oscillating curve, its change over the course of a cycle is smooth and consistent, a continuous, rolling flow.
  - A sine wave.
    - Each cycle consists of a single arc above the time axis followed by a single arc below it.

![datacommunication3](/assets/images/2023-10-17-2/datacommunication3.png)

- Example
  - The power in your house can be represented by a sine wave with a peak amplitude of 155 to 170 V.
    However, it is common knowledge that the voltage of the power in U.S. homes is 110 to 120 V. This discrepancy is due to the fact that these are root mean square (rms) values. The signal is squared and then the average amplitude is calculated. The peak value is equal to 2^(1/2) x rms value.

![datacommunication4](/assets/images/2023-10-17-2/datacommunication4.png)

- Example

  - The voltage of a battery is a constant; this constant value can be considered a sine wave, as we will see later. For example, the peak value of an AA battery is normally 1.5V.

- Period: T

  - The amount of time, in seconds, a signal needs to complete 1 cycle

- Frequency: f

  - The number of periods in 1 sec

- Frequency and period and the inverse of each other

  - f = 1/T
  - T = 1/f

- Hertz (Hz)

  - Cycle per second

- Two signals with the same phase and amplitude, but different frequencies

![datacommunication5](/assets/images/2023-10-17-2/datacommunication5.png)

- Units of period and frequency

![datacommunication6](/assets/images/2023-10-17-2/datacommunication6.png)

- Example
  - The power we use at home has a frequency of 60 Hz (50 Hz in Europe). The period of this sine wave can be determined as follows:

> T = 1/f = 1/60 = 0.0166 s = 0.0166 x 10^3 ms = 16.6 ms

- This means that the period of the power for our lights at home is 0.0116 s, or 16.6 ms. Our eye are not sensitive enough to distinguish these rapid changes in amplitude.

* Example

  - Express a period of 100 ms in microseconds.
  - Solution
    - From the previous Table, we find the equivalents of 1 ms (1 ms is 10^3 s) and 1 s (1 s is 10^6 μs). We make the following substitutions:

> 100 ms = 100 x 10^3 s = 100 x 10^(-3) x 10^6 μs = 10^2 x 10^(-3) x 10^6 μs = 10^5 μs

- Example
  - The period of a signal is 100 ms. What is its frequency in kiloherts?
  - Solution
    - First we change 100 ms to seconds, and then we calculate the frequency from the period (1 Hz = 10^(-3) kHz).

> 100 ms = 100 x 10^(-3) s = 10^(-1) s
> f = 1/T = 1/(10^-1) Hz = 10 Hz = 10 x 10^(-3) kHz = 10^(-2) kHz

### Phase

- The term phase, or phase shift, describes the position of the waveform relative to time 0.

  - If we think of the wave as something that can be shifted backward or forward along the time axis, phase describes the amount of that shift.

- Phase is measured in degrees or radians

  - 360o is 2π rad
  - 1o is 2π/360 rad
  - 1 rad is 360/(2π)o

- Three sine waves with different phases

![datacommunication7](/assets/images/2023-10-17-2/datacommunication7.png)

- Example
  - A sine wave is offset 1/6 cycle with respect to time 0. What is its phase in degrees and radians?
  - Solution
    - We know that 1 complete cycle is 360o. Therefore, 1/6 cycle is

> 1/6 x 360 = 60o = 60 x 2π/360 rad = π/3 rad = 1.046 rad

### Wavelength

- Wavelength is another characteristic of a signal traveling through a transmission medium.

- Wavelength binds the period or the frequency of a simple sine wave to the propagation speed of the medium.

![datacommunication8](/assets/images/2023-10-17-2/datacommunication8.png)

- The frequency of a signal is independent of the medium

- The wavelength depends on both the frequency and medium

- For wavelength λ, propagation speed c (3×10^8), and frequency f,

> Wavelength = (propagation speed) x period = propagation speed/frequency

> λ = c/f

- For example

> λ = c/f = 3x10^8/4x10^14 = 0.75 x 10^(-6) m = 0.75 μs

### Time and Frequency Domains

- A sine wave is comprehensively defined by its `amplitude`, `frequency`, and `phase`.

- We have been showing a sine wave by using what is called a time domain plot.

- The time-domain plot shows changes in signal amplitude with respect to time (it is an amplitude-versus-time plot).

- Phase is not explicitly shown on a time-domain plot.

- The time-domain and frequency-domain plots of a sine wave

![datacommunication9](/assets/images/2023-10-17-2/datacommunication9.png)

- Example
  - The frequency domain is more compact and useful when we are dealing with more than one sine wave. For example, Figure 3.9 shows three sine waves, each with different amplitude and frequency. All can be represented by three spikes in the frequency domain.
  - The time domain and frequency domain of three sine waves

![datacommunication10](/assets/images/2023-10-17-2/datacommunication10.png)

### Fourier Transform: Time domain vs. Frequency Domain

- Orthogonal coordinates for expressing a point
  - A point P in the plane (a) has no numerical representation until we define a reference coordinate frame (b), which has origin point O and orthogonal coordinate axes X and Y
  - Its coordinates p=(px, py) are respectively the extents of P along X and Y from the origin (c).

![datacommunication11](/assets/images/2023-10-17-2/datacommunication11.png)

- A set of signals {𝜙0(𝑡),⋯,𝜙𝑛(𝑡)} are called orthogonal on an interval (a, b) if any two signals 𝜙m(𝑡) and and 𝜙k(𝑡) in the set satisfy

![datacommunication12](/assets/images/2023-10-17-2/datacommunication12.png)

- If the magnitude of each signal 𝜙0(𝑡) is set to one, it is called normalized.

- A set of normalized orthogonal functions can form an orthonormal basis set

- The complex exponential signals
  ![datacommunication13](/assets/images/2023-10-17-2/datacommunication13.png) are orthogonal on any interval over a period T0 = 1/f0.

- Since {𝜙n(x)} is an orthogonal set on [a,b], each term on right-hand side is zero except m=n.

- In this case we have

![datacommunication14](/assets/images/2023-10-17-2/datacommunication14.png)

- Example of Fourier Transform

![datacommunication15](/assets/images/2023-10-17-2/datacommunication15.png)

### Fourier Transform: Properties

![datacommunication16](/assets/images/2023-10-17-2/datacommunication16.png)

1, 2, 7, 9, 10, 12

### Composite Signals

- Simple sine waves have many applications in daily life.

- We can send a single sine wave to carry electric energy from one place to another.

- For example,

  - The power company sends a single sine wave with a frequency of 60 Hz to distribute electric energy to houses and businesses.
  - The sine wave is carrying energy

- As another example,

  - We can use a single sine wave to send an alarm to a security center when a burglar opens a door or window in the house.
  - The sine wave is a signal of danger.

- A single-frequency sine wave is not useful in data communications

  - We need to send a composite signal, a signal made of many simple sine waves

- According to Fourier analysis, any composite signal is a combination of simple sine waves with different frequencies, amplitudes, and phases

- If the composite signal is `periodic`,

  - The decomposition gives a series of signals with discrete frequencies;

- If the composite signal is `nonperiodic`,

  - The decomposition gives a combination of sine waves with continuous frequencies.

- Example
  - The following figure shows a `periodic composite signal` with `frequency`. This type of signal is not typical of those found in data communications. We can consider it to be three alarm systems, each with a different frequency. The analysis of this signal can give us a good understanding of how to `decompose signals`.

![datacommunication17](/assets/images/2023-10-17-2/datacommunication17.png)

- The following figure shows the result of decomposing the above signal in both the time and frequency domains.

![datacommunication18](/assets/images/2023-10-17-2/datacommunication18.png)

- Example
  - The following figure shows a `nonperiodic composite signal`. It can be the signal created by a `microphone` or a `telephone set` when a word or two is pronounced. In this case, the composite signal connot be periodic, because that implies that we are repating the same word or words with exactly the same tone.
  - Time and frequency domain of a nonperiodic signal

![datacommunication19](/assets/images/2023-10-17-2/datacommunication19.png)

### Bandwidth

- The range of frequencies contained in a composite signal is its bandwidth.

- The `bandwidth`is normally a difference between two numbers.

- For example,

  - If a composite signal contains frequencies between 1000 and 5000, its bandwidth is 5000 - 1000, or 4000.

- The range of frequencies contained in a composite signal is its bandwidth.

- The `bandwidth` is normally a difference between two numbers.

- For example,

  - If a composite signal contains frequencies between 1000 and 5000, its bandwidth is 5000 - 1000, or 4000.

- The bandwidth of periodic and nonperiodic composite signals

![datacommunication20](/assets/images/2023-10-17-2/datacommunication20.png)
