# Data Communication 05

## Digital Transmission & Analog-to-Digital Conversion

#### 2023.10.18.(수)

## Digital Transmission

### : Line Coding, Block coding, Transmission mode

### Digital Transmission: Digital-to-Digital Conversion

- Previously, we learned that

  - `Data` can be either digital or analog
  - `Signals` that represent data can also be digital or analog.

- Now, we learn how we can represent digital data by using digital signals.

- The conversion involves three techniques
  - Line coding
  - Block coding
  - Scrambling

### Line Coding

- Line coding

* The process of converting digital data to digital signals.
* Line coding converts a sequence of bits to a digital signal
  - Data, in the form of text, numbers, graphical images, audio, or video, are stored in computer memery as sequences of bits
  - At the sender, digital data are encoded into a digital signal; at the receiver, the digital data are recreated by decoding the digital signal.

![datacommunication1](/assets/images/2023-10-18/datacommunication1.png)

- Signal Elements Versus Data Element

  - A signal element carries data elements
  - Data element: the smallest entity that can represent a piece of information; this is the bit
  - Signal element: the shortest unit (timewise) of a digital signal
  - Define a ratio r which is the number of data elements carried by each signal element
    - The extra signal element is needed to guarantee synchronization

- Signal elements versus data elements

![datacommunication2](/assets/images/2023-10-18/datacommunication2.png)

- Data Rate Versus Signal Rate
  - Data rate
  - The number of data elements (bits) sent in 1s.
  - The unit is bits per second (bps)
  - Signal rate (or baud rate)
  - The number of signal elements sent in 1s.
  - The unit is the baud.
  - Relationship between data rate (N) and signal rate (S)

> S = N/r

- Bandwidth

  - `Bandwidth` of a nonperiodic signal is continuous with an infinite range
  - Although the actual bandwidth of a digital signal is infinite, the effective bandwidth is finite
  - The baud rate, not the bit rate, determines the required bandwidth for a digital signal

- Baseline Wandering

  - Baseline: a running average of the received signal power
  - A long string of 0s or 1s can cause a drift in the baseline (baseline wandering) and make if difficult for the receiver to decode correctly.

- DC Components

  - Frequencies around zero, called DC (direct-current) components, present problems for a system that cannot pass low frequencies
    - E.g., a telephone line cannot pass frequencies below 200 Hz

- Effect of lack of synchronization

![datacommunication3](/assets/images/2023-10-18/datacommunication3.png)

- Example
  - In a digital transmission, the receiver clock is 0.1 percent faster than the sender clock. How many extra bits per second does the receiver receive if the data rate is 1 kbps? How many if the data rate is 1 Mbps?
  - Solution
    - At 1 kbps, the receiver receives 1001 bps instead of 1000 bps.

> 1000 bits send -> 1001 bits received -> 1 extra bps

    - At 1 Mbps, the receiver receives 1,001,000 bps instead of 1,000,000 bps.

> 1,000,000 bits sent -> 1,001,000 bits received -> 1000 extra bps

- Built-in Error Detecting

  - `Built-in error-detecting` capability in the generated code to detect some or all of the erros that occured during transmission.

- Immunity to Noise and Interference

* Immune to noise and other interferences

- Complexity
  - A complex scheme is more costly to implement than a simple one

### Line Coding Schemes

- Roughly divide line coding schemes into five broad categories, as shown like

![datacommunication4](/assets/images/2023-10-18/datacommunication4.png)

- There are several schemes in each category.

- Unipolar Scheme

  - All the signal levels are on one side of the time axis, either above or below

- Unipolar Scheme: NRZ (Non-Return-to-Zero)

  - Signal does not return to zero at the middle of the bit

- Unipolar NRZ scheme

![datacommunication5](/assets/images/2023-10-18/datacommunication5.png)

- Polar Schemes

  - Voltages are on both sides of the time axis.

- Polar Schemes: Non-Return-to-Zero (NRZ)
  - NRZ-L (NRZ-Level) and NRZ-I (NRZ-Invert)
  - In NRZ-L, the level of the voltage determines the value of the bit. In NRZ-I, the inversion or the lack of inversion determines the value of the bit.
  - NRZ-L and NRZ-I both have a `DC component problem`

![datacommunication6](/assets/images/2023-10-18/datacommunication6.png)

- Return-to-Zero (RZ)
  - The main problem with NRZ encoding occurs when the sender and receiver clocks are not synchronized.
  - The receiver does not know when on ebit has ended and the next bit is starting
  - One solution is the return-to-zero (RZ) scheme, which uses three values: positive, negative, and zero
  - In RZ, the signal changes not between bits but during the bit.
  - No DC component problem
  - Disadvantages
    - RZ uses three levels of voltage, which is more complex to create and discern.
  - Not used today

![datacommunication7](/assets/images/2023-10-18/datacommunication7.png)

- Biphase: `Manchester` and `Differential Manchester`
  - The idea of RZ (transition at the middle of the bit) and the idea of NRZ-L are combined into the Manchester scheme
  - Manchester
  - The voltage remains at one level during the first half and moves to the other level in the second half.
  - The transition at the middle of the bit provides synchronization.

![datacommunication8](/assets/images/2023-10-18/datacommunication8.png)

- Bipolar Schemes

  - In bipolar encoding (sometimes called multilevel binary), we use three levels: positive, zero, and negative.

- Bipolar Schemes: AMI and Pseudotenary
  - Alternate mark inversion (AMI)
    - Alternate 1 inversion
    - A neutral zero voltage represents binary 0
    - Binary 1s are represented by alternating positive and negative voltages
  - Pseudoternary
    - A variation of AMI encoding
    - The 1 bit is encoded as a zero voltage
    - The 0 bit is encoded as alternating positive and negative voltages

![datacommunication9](/assets/images/2023-10-18/datacommunication9.png)

- Bipolar Schemes: AMI and Pseudoternary (continued)

  - No DC component
  - A sequence of 1's: alternating -> no DC
  - A sequence of 0's: zero voltage -> no DC
  - AMI is commonly used for long-distance communication, but it has a synchronization problem when a long sequence of 0s is present in the data
  - A `scrambling` technique can solve this problem

- Multilevel Schemes

  - The goal is to increase the number of bits per baud by encoding a pattern of m data elements into a pattern of n signal elements.
  - Group of m data elements can produce a combination of 2^m data patterns.
  - If 2^m = L^n, then
    - Each data pattern is encoded into one signal pattern
  - If 2^m < L^n,
    - The subset can be carefully designed to prevent baseline wandering, to provide synchronization, and to detect errors that occured during data transmission.
  - If 2^m > L^n not possible
  - In mBnL schemes
    - A pattern of m data elements is encoded as a pattern of n signal elements in which 2^m <= L^n
    - m: length of binary pattern, B: binary data
    - n: length of signal pattern, L: number of levels in signaling
    - In place of L: B (binary) for L = 2, T (ternary) for L = 3, and Q (quaternary) for L = 4

- Multilevel Schemes: 2B1Q
  - Two binary, one quaternary (2B1Q), uses data patterns of size 2 and encodes the 2-bit patterns as one signal element belonging to a four-level signal.
  - The 2B1Q scheme is used in DSL (Digital Subscriber Line) technology to provide a high-speed connection to the Internet by using subscriber telephone lines.

Rules:
00 -> -3  
01 -> -1  
10 -> +3  
11 -> +1

![datacommunication10](/assets/images/2023-10-18/datacommunication10.png)

- Multilevel Schemes: 8B6T

  - Eight binary, six ternary (8B6T)
  - Used with 100BASE-4T cable
  - 3^6 - 2^8 = 473 redundant signal elements that provide synchronization, error detection, and DC balance
  - Each signal pattern has a weight of 0 or +1 DC values

- Multilevel: 8B6T

![datacommunication11](/assets/images/2023-10-18/datacommunication11.png)

- Multilevel Schemes: 4D-PAM5

  - Four-dimensional five level pulse amplitude modulation (4D-PAM5)
    - The 4D means that data is sent over four wires at the same time
    - It uses five voltage levels, such as -2, -1, 0, 1, and 2.
      - 0 for FEC
  - Similar to 8B4Q
    - An 8-bit word is translated to a signal element of four different levels.
  - Gigabit LANs use this technique to send 1-Gbps data over four copper cables that can handle 125 Mbaud

- Multilevel: 4D-PAM5 scheme

![datacommunication12](/assets/images/2023-10-18/datacommunication12.png)

- Summary of line coding schemes

![datacommunication13](/assets/images/2023-10-18/datacommunication13.png)

### Block Coding

- We need redundancy to ensure synchronization and to provide some kind of inherent error detecting.

- Block coding can give us this redundancy and improve the performance of line coding.

- In general, block coding changes a block of m bits into a block of n bits, where n is larger than m.

- Block coding is normally referred to as mB/nB coding

  - It replaces each m-bit group with an n-bit group.

- Block coding concept

![datacommunication14](/assets/images/2023-10-18/datacommunication14.png)

- 4B/5B

  - The four binary/five binary (4B/5B) coding scheme was designed to be used in combination with NRZ-I.
  - A long sequence of 0s can make the receiver clock lose synchronization
    - One solution is to change the bit stream, prior to encoding with NRZ-I, so that it does not have a long stream of 0s.
    - The block-coded stream does not have more than three consecutive 0s

- Using block coding 4B/5B with NRZ-I line coding scheme

![datacommunication15](/assets/images/2023-10-18/datacommunication15.png)

- In 4B/5B, the 5-bit output that replaces the 4-bit input has no more than one leading zero (left bit) and no more than two trailing zeros (right bits)
  - Never more than three consecutive 0s
- Some of unused groups are used for control purposes; the others are not used at all

![datacommunication16](/assets/images/2023-10-18/datacommunication16.png)

- Substitution in 4B/5B block coding
  - The redundant bits add 20 percent more baud.
    - Still, the result is less than the biphase scheme which has a signal rate of 2 times that of NRZ-I.
  - Not solve the DC component problem of NRZ-I. If a DC component is unacceptable, we need to use biphase or bipolar encoding.

![datacommunication17](/assets/images/2023-10-18/datacommunication17.png)

- 8B/10B
  - Eight binary/ten binary (8B/10B) encoding
  - Greater Error detection capability than 4B/5B.
  - The 8B/10B block coding is actually a combination of 5B/6B and 3B/4B encoding
    - The split is done to simplify the mapping table.

![datacommunication18](/assets/images/2023-10-18/datacommunication18.png)

### Scrambling

- We modify line and block coding to include scrambling, as shown below
  - AMI used with scrambling

![datacommunication19](/assets/images/2023-10-18/datacommunication19.png)

- Note that `scrambling`, as opposed to block coding, is done at the same time as encoding.

- The system needs to insert the required pulses based on the defined scrambling rules.

- One of common scrambling techniques is B8ZS.

- B8ZS

  - Bipolar with 8-zero substitution (B8ZS)
  - Eight consecutive zero-level voltages are replaced by the sequence 000VB0VB
    - V: violation, B: bipolar

- Two cases of B8ZS scrambling technique

![datacommunication20](/assets/images/2023-10-18/datacommunication20.png)

### Transmission Mode

- Of primary concern when we are considering the transmission of data from one device to another is the wiring, and of primary concern when we are considering the wiring is the data stream.

- Do we send 1 bit at a time; or do we group bits into larger groups and, if so, how?

* The transmission of binary data accross a link can be accomplished in either parallel or serial mode and isochronous.

![datacommunication21](/assets/images/2023-10-18/datacommunication21.png)

### Parallel Transmission

- Line coding is the process of converting digital data to digital signals.

- We assume that data, in the form of text, numbers, graphical images, audio, or video, are stored in computer memory as sequences of bits.

- Line coding converts a sequence of bits to a digital signal.

  - At the sender, digital data are encoded into a digital signal
  - At the receiver, the digital data are recreated by decoding the digital signal.

- Parallel transmission

![datacommunication22](/assets/images/2023-10-18/datacommunication22.png)

- The advantage of parallel transmission is `speed`.

- But, expensive.
  - Parallel transmission requires n communication lines (wires in the example) just to transmit the data stream

### Serial Transmission

- In `serial transmission` one bit follows another, so we need only one communication channel rather than n to transmit data between two communicating devices

- Serial transmission

![datacommunication23](/assets/images/2023-10-18/datacommunication23.png)

### Asynchronous Transmission

- Asynchronous Transmission
  - The timing of a signal is unimportant.
  - Instead, information is received and translated by agreed upon patterns.
  - In asynchronous transmission, we send 1 start bit(0) at the beginning and 1 or more stop bits (1s) at the end of each byte. THere may be a gap between bytes.

* Asynchronous here means "asynchronous at the byte level", but the bits are still synchronized; their durations are the same.
* Cheap and effective
* Slower than forms of transmission that can operate without the addition of control information
* E.g., the connection of a keyboard to a computer is a natural application for asynchronous transmission.

- Asynchronous transmission

![datacommunication24](/assets/images/2023-10-18/datacommunication24.png)

### Synchronous Transmission

- Synchronous Transmission
  - The bit stream is combined into longer "frames", which may contain multiple bytes.
  - In synchronous transmission, we send bits one after another without start or stop bits or gaps.
    - It is the responsibility of the receiver to group the bits.
  - The advantage of synchronous transmission is speed
    - More useful for high-speed applications such as the transmission of data from one computer to another
  - There may be uneven gaps between frames.

![datacommunication25](/assets/images/2023-10-18/datacommunication25.png)

## Analog-to-Digital Conversion

### : PCM, Delta modulation

### Analog-to-Digital Conversion (ADC)

- We have an analog signal such as one created by a microphone or camera.

- We have seen that a digital signal is superior to an analog signal.

- The tendency today is to change an analog signal to digital data.

- Now, we describe two techniques, pulse code modulation and delta modulation.

### Pulse Code Modulation (PCM)

- The most common technique to change an analog signal to digital data (digitization) is called `pulse code modulation (PCM)`.

- A PCM encoder has three processes, as shown in the next slide.

  1. The analog signal is sampled.
  2. The sampled signal is quantized.
  3. The quantized values are encoded as streams of bits.

- Components of PCM encoder

![datacommunication26](/assets/images/2023-10-18/datacommunication26.png)

- Sampling
  - The first step in PCM is sampling
  - T(s) is the sample interval or period.
  - Sampling rate or sampling frequency, denoted by f(s^t) where f(s) = 1/T(s)
  - The most common sampling method: sample and hold
  - Sometimes referred to as pulse amplitude modulation (PAM).

![datacommunication27](/assets/images/2023-10-18/datacommunication27.png)

- Sampling theorem

  - According to the `Nyquist theorem`, the sampling rate must be at least 2 times the highest frequency contained in the signal.

- Nyquist sampling rate for low-pass and bandpass signals

![datacommunication28](/assets/images/2023-10-18/datacommunication28.png)

- Example

  - For an intuitive example of the Nyquist theorem, let us sample a simple sine wave at three sampling rates: f(S) = 4f (2 times the Nyquist rate), f(s) = 2f (Nyquist rate), and f(s) = f (one-half the Nyquist rate).
  - The figure in the next slide shows the sampling and the subsequent recovery of the signal.
  - It can be seen that sampling at the Nyquist rate can create a good approximation of the original sine wave (part a).
  - `Oversampling` in part b can also create the same approximation, but it is redundant and unnecessary.
  - Sampling below the Nyquist rate (part c) does not produce a signal that looks like the original sine wave.

- Recovery of a sine wave with different sampling rates.

![datacommunication29](/assets/images/2023-10-18/datacommunication29.png)

- Example

  - Telephone companies digitize voice by assuming a maximum frequency of 4000 Hz. The sampling rate therefore is 8000 samples per second.

- Example

  - A complex low-pass signal has a bandwidth of 200 kHz. What is the minimum sampling rate for this signal?
  - Solution: The bandwidth of a low-pass signal is between 0 and f, where f is the maximum frequency in the signal. Therefore, we can sample this signal at 2 times the highest frequency (200 kHz). The sampling rate is therefore 400,000 samples per second.

- Example

* A complex bandpass signal has a bandwidth of 200 kHz. What is the minimum sampling rate for this signal?
* Solution: We cannot find the minimum sampling rate in this case because we do not know where the bandwidth starts or ends. We do not know the maximum frequency in the signal.

- Quantization
  - The set of amplitudes can be infinite with nonintegral values between the two limits -> finite amplitudes
  - Quantization steps
    1. We assume that the original analog signal has instantaneous amplitudes between V(min) and V(max)
    2. We divide the range into L zones, each of height Δ (delta).
    3. We assign quantized values of 0 to L - 1 to the midpoint of each zone.
    4. We approximate the value of the sample amplitude to the quantized values.
  - The quantization process selects the quantization value from the middle of each zone.
  - Quantization Levels
    - The choice of L, the number of levels, depends on the range of the amplitudes of the analog signal and how accurately we need to recover the signal.
    - In audio digitizing, L is normally chosen to be 256; in video it is normally thousands
  - Quantization Error
    - The error created in the quantization process
    - Quantization is an approximation process
    - The value of the error for any sample is less than Δ/2. In other words, we have -Δ/2 <= error <= Δ/2.
    - The quantization error to the SNR(dB) of the signal
      - n(b): the bits per sample

> SNR(dB) = 6.02nb + 1.76 dB

- Quantization and encoding of a sampled signal

![datacommunication30](/assets/images/2023-10-18/datacommunication30.png)

- Uniform Versus Nonuniform Quantization

* Changes in amplitude often occur more frequently in the lower amplitudes than in the higher ones.
* Better to use nonuniform zones.

- Encoding
  - The last step in PCM is encoding
  - Each sample can be changed to an nb-bit code word
  - If the number of quantization levels is L, the number of bits is nb = log2(L)
  - Bit rate formula

> Bit rate = sampling rate x number of bits per sample = f(s) x n(b)

- Example
  - We want to digitize the human voice. What is the bit rate, assuming 8 bits per samle?
  - Solution
    - The human voice normally contains frequencies from 0 to 4000 Hz. So the sampling rate and bit rate are calculated as follows:

> Sampling rate = 4000 x 2 = 8000 samples/s
> Bit rate = 8000 x 8 = 64,000 bps = 64 kbps

- Original Signal Recovery

  - The recovery of the original signal requires the PCM decoder
  - After the staircase signal is completed, it is passed through a low-pass filter to smooth the staircase signal into an analog signal
  - The filter has the same `cutoff frequency` as the original signal at the sender.
  - If the signal has been sampled at (or greater than) the Nyquist sampling rate and if there are enough quantization levels, the original signal will be recreated.

- Components of a PCM decoder

![datacommunication31](/assets/images/2023-10-18/datacommunication31.png)

### Delta Modulation (DM)

- PCM is a very complex technique.
- Other techniques have been developed to reduce the complexity of PCM.
- The simplest is `delta modulation (DM)`.

  - PCM finds the value of the signal amplitude for each sample;
  - DM finds the change from the previous sample.

- Process of delta modulation
  - Note that there are no code words here; bits are sent one after another.

![datacommunication32](/assets/images/2023-10-18/datacommunication32.png)

- Delta modulation components

* The process records the small positive or negative changes, called delta δ.
  - If the delta is positive, the process records a 1;
  - If it is negative, the process records a 0

![datacommunication33](/assets/images/2023-10-18/datacommunication33.png)

- Demodulator

  - The created analog signal, however, needs to pass through a low-pass filter for smoothing

- Delta demodulation components

![datacommunication34](/assets/images/2023-10-18/datacommunication34.png)
