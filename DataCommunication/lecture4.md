# Data Communication 04

### Introduction to Physical Layer 2

#### : Transmission & Performances

#### 2023.10.18.(수)

## Transmission & Performance

### : Transmission & Performances

### Transmission of Digital Signals

- The previous discussion asserts that a digital signal, periodic or nonperiodic, is a composite analog signal with frequencies between zero and infinity.

  - A digital signal is a composite analog signal with an infinite bandwidth

- The fundamental question is,

  - How can we send a digital signal from point A to point B?

- We can transmit a digital signal by using one of two different approaches: baseband transmission or broadband transmission (using modulation).

- Baseband transmission
  - Sending digital signal over a channel without changing the digital signal to an analog signal

![datacommunication28](/assets/images/2023-10-17-2/datacommunication28.png)

- Low-pass channel
  - A channel with a bandwidth that starts from zero
  - It can ben used for baseband communication
  - In real life, impossible to have a low-pass channel with infinite bandwidth

![datacommunication29](/assets/images/2023-10-17-2/datacommunication29.png)

- Case 1: Low-Pass Channel with Wide Bandwidth

  - Continuous range of frequencies between zero and infinity
  - Not possible between two devices
  - Only very good accuracy

- Baseband transmission using a dedicated medium

![datacommunication30](/assets/images/2023-10-17-2/datacommunication30.png)

- Baseband transmission of a digital signal that preserves the shape of the digital signal is possible only if we have a low-pass channel with an infinite or very wide bandwidth.

- For better approximation
  - By adding more harmonics of the frequencies
  - In baseband transmission, the required bandwidth is proportional to the bit rate;
    - If we need to send bits faster, we need more bandwidth.
  - Simulating a digital signal with first three harmonics

![datacommunication31](/assets/images/2023-10-17-2/datacommunication31.png)

- Simulating a digital signal with first three harmonics (part 2)

![datacommunication32](/assets/images/2023-10-17-2/datacommunication32.png)

- Baseband transmission (using modulation)

  - Changing the digital signal to an analog signal for transmission

- Modulation allows us to use a bandpass channel - a channel with a bandwidth that does not start from zero

  - Note that a low-pass channel can be considered a bandpass channel with the lower frequency starting at zero.

- Bandwidth of a band-pass channel

![datacommunication33](/assets/images/2023-10-17-2/datacommunication33.png)

- If the available channel is a bandpass channel, we cannot send the digital signal directly to the channel; we need to convert the digital signal to an analog signal before transmission.

- Modulation of a digital signal for transmission on band-pass channel

![datacommunication34](/assets/images/2023-10-17-2/datacommunication34.png)

### Transmission Impairment

- Signals travel through transmission media, which are not perfect.

- The imperfection causes signal impairment.

  - This means that the signal at the beginning of the medium is not the same as the signal at the end of the medium.
  - What is sent is not what is received.

- Causes of impairment

![datacommunication35](/assets/images/2023-10-17-2/datacommunication35.png)

### Attenuation

- Attenuation means a loss of energy.

  - When a signal, simple or composite, travels through a medium, it loses some of its energy in overcoming the resistance of the medium.
  - That is why a wire carrying electric signals gets warm, if not hot, after a while.
  - Some of the electrical energy in the signal is converted to heat. To compensate for this loss, amplifiers are used to amplify the signal.

- Attenuation and amplification.

![datacommunication36](/assets/images/2023-10-17-2/datacommunication36.png)

- Decibel
  - The decibel (dB) measures the relative strengths of two signals or one signal at two different points.
  - The decibel is negative if a signal is attenuated and positive if a signal is amplified.

> dB = 10 log10(P2/P1)

- Variables P1 and P2 are the powers of a signal at points 1 and 2, respectively.
  - Note that some engineering books define the decibel in terms of voltage instead of power

> dB = 20 log10(V1/V2)

- Example
  - Suppose a signal travels through a transmission medium and its power is reduced to one half. This means that P2 = 0.5 P1. In this case, the attenuation (loss of power) can be calculated as

> 10 log10(P2/P1) = 10 log10(0.5P1 / p1) = 10 log10 (0.5) = 10 x (-0.3) = -3 dB.

- A loss of 3 dB (-3 dB) is equivalent to losing one-half the power.

- A signal travels through an amplifier, and its power is increased 10 times. This means that P2 = 10P1. In this case, the amplification (gain of power) can be calculated as

> 10 log10(P2/P1) = 10log10(10P1/p1) = 10log10(10) = 10 dB

- One reason that engineers use the decibel to measure the changes in the strength of a signal is that decibel numbers can be added (or subtracted) when we are measuring several points (cascading) instead of just two.

![datacommunication37](/assets/images/2023-10-17-2/datacommunication37.png)

- Example
  - Sometimes the decibel is used to measure signal power in `milliwatts`. In this case, it is referred to as `dBm` and is calculated as dBm = 10 log10 (Pm') where Pm is the power in milliwatts. Calculate the power of a signal if its `dBm = -30`.

> dBm = 10log10 -> dBm = -30 -> log10(Pm) = -3 -> Pm = 10^(-3) mW

- `Base station` transmit power: 46dBm = 40W
- `User equipment` (i.e., mobile station) transit power: 23dBm = 200mW

- The loss in a cable is usually defined in `decibels per kilometer (dB/km)`. If the signal at the beginning of a cable with -0.3 dB/km has a power of 2 mW, what is the power of the signal at 5 km?

> dB = 10log10(P2/P1) = -1.5 -> (P2/P1) = 10^(-0.15) = 0.71
> P2 = 0.71 P1 = 0.7 x 2 mW = 1.4 mW

### Distortion

- `Distortion` means that the signal changes its form or shape.

- Distortion can occur in a composite signal made of different frequencies.

- Each signal component has its own propagation speed (see the next section) through a medium and, therefore, its own delay in arriving at the final destination.

- Differences in delay may create a difference in phase if the delay is not exactly the same as the period duration.

![datacommunication38](/assets/images/2023-10-17-2/datacommunication38.png)

### Noise

- `Noise` is another cause of impairment.

- Several types of noise, such as thermal noise, induced noise, crosstalk (interference), and impulse noise, may corrupt the signal.
  - Thermal noise is the random motion of electrons in a wire, which creates an extra signal not originally sent by the transmitter.
  - Induced noise comes from sources such as motors.
  - Crosstalk (or interference) is the effect of one wire on the other.

![datacommunication39](/assets/images/2023-10-17-2/datacommunication39.png)

- Signal-to-Noise Ratio (SNR)

> SNR = average signal power / average noise power

- Used to find the theoretical bit rate limit
- Need to consider the average signal power and the average noise power because these may change with time
- Because `SNR` is the ratio of two powers, it is often described in decibel units, SNR(dB), defined as

> SNR(dB) = 10log10(SNR)

![datacommunication40](/assets/images/2023-10-17-2/datacommunication40.png)

- Example
  - The power of a signal is 10 mW and the power of the noise is 1 μW; what are the values of SNR and SNR(dB)?
  - Solution
    - The values of SNR and SNR(dB) can be calculated as follows:

> SNR = (10,000 μW)/(1 μW) = 10,000
> SNR(dB) = 10 log10 (10,000) = 10 log10(10^4) = 40

- The values of SNR and SNR(dB) for a noiseless channel are
- Solution
  - The values of SNR and SNR(dB) for a noiseless channel are

> SNR = (signal power)/0 = ∞
> SNR(dB) = 10 log10(∞) = ∞

    - We can never ahieve this ratio in real life; it is an ideal.

### Wireless Channels

- Variations of channel strength over `time` and over `frequency`

![datacommunication41](/assets/images/2023-10-17-2/datacommunication41.png)

- Large-scale fading

  - Due to path loss of signal as a function of distance
  - Due to shadowing by large objects such as buildings and hills
  - Order of the cell size, and typically frequency independent

- Small-scale fading
  - Due to constructive and destructive interference of the multiple signal paths between transmitter and receiver
  - Order of carrier wavelength, and frequency dependent

### Physical Modeling for Wireless Channels

- Wireless communications

* Based on electromagnetic radiation from transmitter to receiver

- Wave length
  - λ: wavelength
  - c: light spee,
  - f: frequency
  - E.g., for 2GHz, wave length is 15 cm

![datacommunication42](/assets/images/2023-10-17-2/datacommunication42.png)

### Large Scale Fading: Path Loss

- Path loss
  - Received power decreases with distance d
  - Free space 1/d^2
  - Friss formula
  - Reflection from ground plane 1/d^2

![datacommunication43](/assets/images/2023-10-17-2/datacommunication43.png)

- Simplified path loss model

![datacommunication44](/assets/images/2023-10-17-2/datacommunication44.png)

- d0: reference distance for antenna far field
- K: constant path loss factor (antenna, average channel attenuation), and sometimes using

![datacommunication45](/assets/images/2023-10-17-2/datacommunication45.png)

- α: path loss exponent

### Some Empirical Results

![datacommunication46](/assets/images/2023-10-17-2/datacommunication46.png)

### [Suppl.] Large Scale Fading: Path Loss

- Path loss for urban microcell environments in ITU-R M.2412

![datacommunication47](/assets/images/2023-10-17-2/datacommunication47.png)

### Large Scale Fading: Indoor Attenuation

- Factors that affect indoor path loss

  - Wall/floor materials
  - Room/hallway/window/open area layouts
  - Obstructing objects' location and materials
  - Room size/floor numbers

- Materials penetration losses for outdoor to indoor building in ITU-R M.2412

![datacommunication48](/assets/images/2023-10-17-2/datacommunication48.png)

### Large Scale Fading: Lognormal Shadowing

- Same Tx-Rx distance usually have different path loss

  - Surrounding environment is different

- Reality: simplified Path-Loss Model represents an "average"

- How to represent the difference between the average and the actual path loss?

- Empirical measurements have shown that
  - It is random (and so is a random variable)
  - Lognormally distributed

### Large Scale Fading: Cell Coverage Area

- Cell coverage area
  - Expected percentage of locations within a cell where received power at these locations is above a given minimum.

![datacommunication49](/assets/images/2023-10-17-2/datacommunication49.png)

### [Suppl.] Large Scale Fading: Lognormal Shadowing

- Lognormal distribution
  - A lognormal distribution is a probability distribution of a random variable whose logarithm is normally distributed:

![datacommunication50](/assets/images/2023-10-17-2/datacommunication50.png)

- x: random variable (linear scale)
- μ, σ^2: mean and variance of the distribution (in dB)

### Large Scale Fading: Lognormal Shadowing

- Expressing the path loss in dB, we have

![datacommunication51](/assets/images/2023-10-17-2/datacommunication51.png)

- Xσ: Describes random shadowing effects
- Xσ ~ N(0,σ2) -> normal distribution with zero mean and σ2 variance
- Same Tx-Rx distance, but different levels of clutter.
- Empirical Studies show that σ ranges from 4 dB to 13 dB in an outdoor channel

* Why is it log-normal distributed?
  - By central limit theorem
    - Let X1, X2, ..., Xn be a sequence of independent and identically distributed random variables each having mean 𝜇 and variance 𝜎^2. Then for n large, the distribution of X1 + ... + Xn is approximately normal with mean n𝜇 and variance n𝜎^2.

### Large Scale Fading & Small Scale Fading

- Path Loss, Shadowing, and Multi-Path

![datacommunication52](/assets/images/2023-10-17-2/datacommunication52.png)

### Small Scale Fading: Statistical Models

- Rayleigh fading: very righ scattering
  - Aggregation of a large number of independent paths -> central limit theorem

![datacommunication53](/assets/images/2023-10-17-2/datacommunication53.png)

- [Suppl.] Magnitude of channel gain ~ Rayleigh random variable with density
  - Composite channel gain ~ CN(0,σ2)
  - Magnitude of channel gain = square root of sum of two squared Gaussian random variables

![datacommunication54](/assets/images/2023-10-17-2/datacommunication54.png)

- Rician fading: LoS (line of sight) path + NLoS (non-LoS) path
  - LoS: deterministic path, NLoS: applying CLT
  - κ: K-factor
  - Channel gain: expressed as complex number

![datacommunication55](/assets/images/2023-10-17-2/datacommunication55.png)

- Magnitude of above gain ~ following Rician distribution

* Squared magnitude of above gain is often approximated as gamma distributed random variable
  - Nakagami-m fading: its squared value follows the Gamma distribution

### Data Rate

- A very important consideration in data communications is how fast we can send data, in bits per second, over a channel

- Data rate depends on three factors
  1. The bandwidth available
  2. The level of the signals we use
  3. The quality of the channel (the level of noise)

### Performance: Bandwidth (~Data rate)

- One characteristic that measures network performance is bandwidth.

- However, the term can be used in two different contexts with two different measuring values:
  - Bandwidth in hertz
    - The range of frequencies in a composite signal or the range of frequencies that a channel can pass.
  - Bandwidth in bits per second.
    - The number of bits per second that a channel, a link, or even a network can transmit.
    - The speed of bit transmission in a channel or link.

### Performance: Throughput (~Data rate)

- The `throughput` is a measure of how fast we can actually send data through a network.

- Although, at first glance, bandwidth in bits per second and throughput seem the same, they are different.

- A link may have a bandwidth of B bps, but we can only send T bps through this link with T always less than B.

- Example 3.44
  - A network with bandwidth of 10 Mbps can pass only an average of 12,000 frames per minute with each frame carrying on average of 10,000 bits. What is the throughput of this network?
  - Solution
    - We can calculate the throuput as

> Throughput = (12,000 x 10,000)/60 = 2 Mbps

    - The throughput is almost one-fifth of the bandwidth in this case.

### Data Rate Limits: Shannon Capacity

- Noise channel

* In reality, we cannot have a noiseless channel; the channel is always noisy.

- In 1944, Claude Shannon introduced a formula, called the Shannon capacity, to determine the theoretical highest data rate for a noisy channel:

> Capacity = bandwidth x log2(1 + SNR)

- SNR: the signal-to-noise ratio
- Capacity: the capacity of the channel in bits per second
- No indication of the signal level
- It defines a charateristic of the channel, not the method of transmission

* Example
  - Consider an extremely noisy channel in which the value of the `signal-to-noise ratio` is almost `zero`. In other words, the noise is so strong that the signal is faint. For this channel the capacity C is calculated as

C = B log2 (1+SNR) = B log2 (1+0) = B log2 (1) = B x 0 = 0

- This means that the capacity of this channel is zero regardless of the bandwidth.
- In other words, we cannot receive any data through this channel.

* Example
  - We can calculate the theoretical highest bit rate of a regular telephone line. A telephone line normally has a bandwidth of 3000 Hz (300 to 3300 Hz) assigned for data communications. The signal-to-noise ratio is usually 3162 (~35dB). For this channel the capacity is calculated as

> C = B log2 (1+SNR) = 3000 log2 (1 + 3162) = 3000 x 11.62 = 34,860 bps

- This means that the highest bit rate for a telephone line is 34.860 kbps. If we want to send data faster than this, we can either increase the bandwidth of the line or improve the signal-to-noise ratio.

* Example
  - The signal-to-noise ratio is often given in decibels. Assume that `SNR(dB) = 36` and the channel bandwidth is 2 MHz. The theoretical channel capacity can be calculated as

> SNR(dB) = 10 log10 (SNR) -> SNR = 10^(SNR(dB)/10) -> SNR = 10^3.6 = 3981
> C = B log2 (1+SNR) = 2 x 10^6 x log2(3982) = 24 Mbps

- Example (LTE-Advanced system for 4G)
  - Bandwidth 40 MHz, SNR = 10dB
  - 40 x 10^6 x log2(1+10^(10/10)) = 138.4 Mbps < 1 Gbps
  - 8 x 138.4 Mps > 1 Gbps -> multiple antenna technology

### [Suppl.] Shannon Capacity: Encoding

- Dimension of Dof (degree of freedom)

  - Roughly speaking (not strictly), independently transmitted signals
    - E.g., different time slot, different frequency, individual (independent) resource etc

- Which better?
  - One signal per one resource
  - Two mixed signals per two resources
  - N mixed signals per N resources when N is large -> N dimensional codeword

![datacommunication56](/assets/images/2023-10-17-2/datacommunication56.png)

### [Suppl.] Shannon Capacity

- Sannon capacity: maximum data rate for a given AWGN

- Each encoded unit is called codeword

- Noise sphere: uncertainty due to noise centered at Tx codeword

![datacommunication57](/assets/images/2023-10-17-2/datacommunication57.png)

- Therefore, Shannon capacity can be interpreted as
  - The number of noise spheres that can be packed
  - That is, maximum number of codewords that can be reliably distinguished

![datacommunication58](/assets/images/2023-10-17-2/datacommunication58.png)

### Intercell Interference in Multicell Networks

- Multicell networks
  - Intercell interference
  - SNR -> SINR (signal to interference plus noise)

![datacommunication59](/assets/images/2023-10-17-2/datacommunication59.png)
