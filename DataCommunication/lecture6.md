# Data Communication 06

## Analog Transmission & Multiplexing

#### 2023.10.19.(목)

### Analog Transmission

#### : Digital to Analog, Analog to Analog

### Digital Data to Analog Signal (Digital to Analog Modulation)

- Digital data to analog conversion is the process of changing one of the characteristics of an analog signal based on the information in digital data.

- Digital data to analog conversion

![datacommunication1](/assets/images/2023-10-19/datacommunication1.png)

- Types of digital to analog conversion: `Modulation`

![datacommunication2](/assets/images/2023-10-19/datacommunication2.png)

- Before we discuss specific methods of digital-to-analog modulation, two basic issues must be reviewed
  - Bit and baud rates
  - Carrier signal

### Bit and Baud Rates

- Data rate (bit rate, N bps) & signal rate (baud rate, S baud/sec)
  - If r = N/S, i.e., the number of data elements carried in one signal element

> N = S x r

- The baud rate is less than or equal to the bit rate

* Example
  - An analog signal carries 4 bits per signal element. If 1000 signal elements are sent per second, find the bit rate.
  - Solution
    - In this case, r = 4, S = 1000, and N is unknown. We can find the value of N from

> S = N x (1/r) or N = S x r = 1000 x 4 = 4000 bps

### Data Elements vs. Signal Elements

- Example
  - An analog signal has a bit rate of 8000 bps and a baud rate or 1000 baud. How many data elements are carried by each signal element? How many levels a single signal element should distinguish?
  - Solution

> S = N x 1/r -> r = N/S = 8000/10,000 = 8 bits/baud
> r = log2(L) -> L = 2^r = 2^8 = 256

### Bandwidth and Carrier signal

- Bandwidth

  - The `required bandwidth` for analog transmission of digital data is proportional to the signal rate except for FSK

- Carrier Signal
  - In analog transmission, the sending device produces a high-frequency signal that acts as a base for the informatioin signal
  - The receiving device is tuned to the frequency of the carrier signal that it expects from the sender
  - Digital information then changes the carrier signal by modifying one or more of its characteristics (amplitude, frequency, or phase)
  - This kind of modification is called `modulation (shift keying)`.

### Amplitude Shift Keying

- In amplitude shift keying (ASK),

  - The amplitude of the carrier signal is varied to create signal elements.
  - Both frequency and phase remain constant while the amplitude changes.

- Binary ASK (BASK)
  - Binary amplitude shift keying or on-off keying (OOK)

![datacommunication3](/assets/images/2023-10-19/datacommunication3.png)

- Implementation of binary ASK

![datacommunication4](/assets/images/2023-10-19/datacommunication4.png)

- Example

  - In data communications, we normally use full-duplex links with communication in both directions. We need to divide the bandwidth into two with two carrier frequencies, as shown below. The figure shows the position of two carrier frequencies and the bandwidths. The available bandwidth for each direction is now 50 kHz, which leaves us with a data rate of 25 kbps in each direction

  - Bandwidth of a full-duplex ASK in this example

![datacommunication5](/assets/images/2023-10-19/datacommunication5.png)

### Frequency Shift Keying

- In frequency shift keying (FSK),

  - The frequency of the carrier signal is varied to represent data.
  - The frequency of the modulated signal is constant for the duration of one signal element, but changes for the next signal element if the data element changes.
  - Both `peak amplitude` and `phase` remain constant for all signal elements.
  - Wider bandwidth required

- Binary FSK (BFSK)
  - Two carrier frequencies, f1 and f2

![datacommunication6](/assets/images/2023-10-19/datacommunication6.png)

- Implementation

  - Noncoherent BFSK: discontinuity in the phase when one signal element ends and the next begins
  - Coherent BFSK: continuous phase through the boundary of two signal elements
  - Coherent BFSK can be implemented by using one `voltage-controlled oscillator (VCO)`that changes its frequency according to the input voltage

- Implementation of BFSK

![datacommunication7](/assets/images/2023-10-19/datacommunication7.png)

- Multilevel FSK

  - Not uncommon with the FSK method
  - Using more than two frequencies
  - MFSK uses more bandwidth than the other techniques; it should be used when noise is a serious issue

- Bandwidth of MFSK

![datacommunication8](/assets/images/2023-10-19/datacommunication8.png)

### Phase Shift Keying

- In phase shift keying (PSK),

  - The phase of the carrier is varied to represent two or more different signal elements.
  - Both peak amplitude and frequency remain constant as the phase changes.

- Today, PSK is more common than ASK for FSK.

* However, we will see shortly that QAM, which combines ASK and PSK, is the dominant method of digital-to-analog modulation.

- Binary PSK (BPSK)
  - Only two signal elements, one with a phase of 0°, and the other with a phase of 180°
  - Binary PSK is as simple as binary ASK with one big advantage
  - PSK is less susceptible to noise than ASK
    - Noise can change the amplitude easier than it can change the phase.
  - PSK is superior to FSK because we do not need two carrier signals.
  - However, PSK needs more sophisticated hardware to be able to distinguish between phases.

![datacommunication9](/assets/images/2023-10-19/datacommunication9.png)

- Bandwidth

  - The same as that for binary ASK, but less than that for BFSK

- Implementation
  - The implementation of BPSK is as simple as that for ASK.
    - The reason is that the signal element with phase 180° can be seen as the complement of the signal element with phase 0
    - The reason is that the signal element with phase 180° can be seen as the complement of the signal element with phase 0°

![datacommunication10](/assets/images/2023-10-19/datacommunication10.png)

- Quadrature PSK (QPSK)

  - Use 2 bits at a time in each signal element, thereby decreasing the baud rate and eventually the required bandwidth
  - Two separate BPSK modulations
    - One is in-phase, the other quadrature (out-of-phase).
  - The two composite signals created by each multiplier are sine waves with the same frequency, but different phases

- QPSK and its implementation

![datacommunication11](/assets/images/2023-10-19/datacommunication11.png)

- Constellation Diagram

* Useful when we are dealing with multilevel ASK, PSK, or QAM

- Concept of a constellation diagram

![datacommunication12](/assets/images/2023-10-19/datacommunication12.png)

- Example

* Show the constellation diagrams for ASK (OOK), BPSK, and QPSK signals.
* Solution
  - The followings present the three constellation diagrams. Let us analyze each case separately:

![datacommunication13](/assets/images/2023-10-19/datacommunication13.png)

### Quadrature Amplitude Modulation

- PSK is limited by the ability of the equipment to distinguish small differences in phase.

  - This factor limits its potential bit rate.

- So far, we have been altering only one of the three characteristics of a sine wave at a time; but what if we alter two? Why not combine ASK and PSK?

- The idea of using two carries, one in-phase and the other quadrature, with different amplitude levels for each carrier is the concept behind quadrature amplitude modulation (QAM).

- Bandwidth for QAM

  - The minimum bandwidth required for QAM transmission is the same as that required for ASK and PSK transmission.
  - QAM has the same advantages as PSK over ASK

- Constellation diagrams for some QAMs

![datacommunication14](/assets/images/2023-10-19/datacommunication14.png)

### Detection in AWGN

- `AWGN`: additive white Gaussian noise

> y = u + w

- y: received signal, Tx signal u, u(B), w ~ N(0, N0 / 2)

* Detection: either of uA or uB, we conclude that our signal is uA if

![datacommunication15](/assets/images/2023-10-19/datacommunication15.png)

### [Suppl.] LDA: Classify to Highest Density

- Linear Discriminant Analysis (LDA)

![datacommunication16](/assets/images/2023-10-19/datacommunication16.png)

- We classify a new point according to which density is highest

- When the priors are different, we take them into account as well, and compare 𝜋𝑘𝑓𝑘(𝑥)
  - On the right, we favor the pink class -> the decision boundary has shifted to the left

### [Suppl.] LAD w/ 2 features and K = 3 Classes

![datacommunication17](/assets/images/2023-10-19/datacommunication17.png)

- Here 𝜋1 = 𝜋2 = 𝜋3 = 1/3

### [Suppl.] Quadratic Discriminant Analysis (QDA)

![datacommunication18](/assets/images/2023-10-19/datacommunication18.png)

### Analog to Analog Modulation

- Analog-to-analog conversion, or `analog modulation`,is the representation of analog information by analog signal.

- One may ask why we need to modulate an analog signal; it is already analog.

- Modulation is needed if the medium is bandpass in nature or if only a bandpass channel is available to us.

- Analog-to-analog conversion can be accomplished in three ways: `AM`, `FM`, and `PM`.

- Types of analog-to-analog modulation

![datacommunication19](/assets/images/2023-10-19/datacommunication19.png)

### Amplitude Modulation (AM)

- In `AM` transmission, the carrier signal is modulated so that its amplitude varies with the changing amplitudes of the modulating signal.

* The frequency and phase of the carrier remain the same; only the amplitude changes to follow variations in the information.

- Amplitude modulation

  - The modulating signal is the envelope of the carrier.
  - `AM` is normally implemented by using a simple multiplier because the amplitude of the carrier signal needs to be changed according to the amplitude of the modulating signal.

- Figure: Amplitude modulation

![datacommunication20](/assets/images/2023-10-19/datacommunication20.png)

- The total bandwidth required for AM can be determined from the bandwidth of the audio signal:

  > B(AM) = 2B

- Standard Bandwidth Allocation for AM Radio

* The bandwidth of an audio signal (speech and music) is usually 5 kHz.
* Federal Communications Commission (FCC) allows 10 kHz for each AM station

- AM band allocation

![datacommunication21](/assets/images/2023-10-19/datacommunication21.png)

### Frequency Modulation (FM)

- In FM transmission,
  - The frequency of the carrier signal is modulated to follow the changing voltage level (amplitude) of the modulating signal.
  - The peak amplitude and phase of the carrier signal remain constant, but as the amplitude of the information signal changes, the frequency of the carrier changes correspondingly.

![datacommunication22](/assets/images/2023-10-19/datacommunication22.png)

- Standard Bandwidth Allocation for FM Radio

  - The bandwidth of an audio signal (speech and music) broadcast in stereo is almost 15 kHz.
  - The FCC allows 200 kHz (0.2 MHz) for each station.
  - Only alternate bandwidth allocations
  - Given 88 to 108 MHz as a range, there are 100 potential FM bandwidths in an area, of which 50 can operate at any one time

- FM band allocation

![datacommunication23](/assets/images/2023-10-19/datacommunication23.png)

## Multiplexing

### : FDM, WDM, TDM, CDM, SDM

### Multiplexing

- `Multiplexing` is the set of techniques that allows the simultaneous transmission of multiple signals across a single data link.

- We can accommodate data increase by continuing to add individual links each time a new channel is needed, or we can install higher-bandwidth links and use each to carry multiple signals.

- Terminologies

  - ` Multiplexer`` (MUX) vs.  `Demultiplexer`` (DEMUX)
  - The word link refers to the physical path.
  - The word `channel` refers to the portion of a link that carries a transmission between a give pair of lines.
  - One link can have many (n) channels.

- Dividing a link into channels

![datacommunication24](/assets/images/2023-10-19/datacommunication24.png)

- Categories of multiplexing
  - Frequency division multiplexing (FDM)
    - Wavelength division multiplexing (WDM)
  - Time division multiplexing (TDM)
  - Code division multiplexing (CDM)
  - Space division multiplexing (SDM)
    - multiple input multiple output (MIMO): multiple antenna technologies

### Frequency-Division Multiplexing

- `Frequency-division multiplexing (FDM)` is an analog technique that can be applied when the bandwidth of a link (in hertz) is greater than the combined bandwidths of the signals to be transmitted.

- In FDM, signals generated by each sending device modulate different carrier frequencies.

- These modulated signals are then combined into a single composite signal that can be transported by the link.

- Multiplexing Process

  - Each source generates a signal of a similar frequency range.
  - Inside the multiplexer, these similar signals modulate different carrier frequencies (f1, f2, and f3)

- FDM Process

![datacommunication25](/assets/images/2023-10-19/datacommunication25.png)

- Example (Old telephone networks)
  - Assume that a voice channel occupies a bandwidth of 4 kHz. We need to combine three voice channels into a link with a bandwidth of 12 kHz, from 20 to 32 kHz. Show the configuration, using the frequency domain. Assume there are no guard bands.
  - Solution
  - We shift (modulate) each of the three voice channels to a different bandwidth, as shown below.

![datacommunication26](/assets/images/2023-10-19/datacommunication26.png)

- Example: `Guard band`

  - Five channels, each with a 100-kHz bandwidth, are to be multiplexed together. What is the minimum bandwidth of the link if there is a need for a guard band of 10 kHz between the channels to prevent interference?

  - Solution
    - For five channels, we need at least four guard bands. This means that the required bandwidth is at least 5 x 100 + 4 x 10 = 540 kHz, as shown below.

![datacommunication27](/assets/images/2023-10-19/datacommunication27.png)

- Applications of FDM
  - AM radio braodcasting
    - 530 to 1700 kHz, 10 kHz of bandwidth
    - Without multiplexing, only one AM station could broadcast to the common link, the air.
  - FM radio broadcasting
    - FM has a wider band of 88 to 108 MHz because each station needs a bandwidth of 200 kHz
  - Television broadcasting
    - Each TV channel has its own bandwidth of 6 MHz
  - The first generation of cellular telephones
    - Two 30-kHz channels, one for sending voice and the other for receiving.
    - The voice signal, which has a bandwidth of 3 kHz (from 300 to 3300 Hz), is modulated by using FM.
      - Remember that an FM signal has a bandwidth 10 times that of the modulating signal, which means each channel has 30 kHz (10 x 3) of bandwidth.

### Wavelength-Division Multiplexing

- `Wavelength-division multiplexing (WDM)` is designed to use the high-data-rate capability of fiber-optic cable.
- The optical fiber data rate is higher than the data rate of metallic transmission cable, but using a `fiber-optic cable` for a single line wastes the available bandwidth.
- Multiplexing allows us to combine several lines into one.
- WDM is conceptually the same as FDM
  - Except that the multiplexing and demultiplexing involve `optical signals` transmitted through fiber-optic chan

* Wavelength-division multiplexing

![datacommunication28](/assets/images/2023-10-19/datacommunication28.png)

- WDM is an analog multiplexing technique to combine optical signals.

* It combines multiple light sources into one single light at the multiplexer and do the reverse at the demultiplexer.
* The combining and splitting of light sources are easily handled by a prism.

- Prisms in wave-length division multiplexing

![datacommunication29](/assets/images/2023-10-19/datacommunication29.png)
