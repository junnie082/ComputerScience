# Data Communication 09

## Error Detection and Correction

#### 2023.12.11.(월)

### Types of Errors

- Whenever bits flow from one point to another, unpredictable changes because of interference

- Single-bit error

  - Only 1 bit of given data unit (such as a byte, character, or packet) is changed from 1 to 0 or from 0 to 1

- Burst error

  - 2 or more bits in the data unit have changed from 1 to 0 or from 0 to 1

- A burst error is more likely to occur than a single-bit error

  - Because the duration of the noise signal is normally longer than the duration of 1 bit

- Single-bit and burst error

![dc3](/assets/images/2023-12-11/dc3.png)

### Redundancy

- Redundancy

* To be able to detect or correct errors, we need to send some extra bits with our data.

### Detection versus Correction

- The correction of errors is more difficult than the detection

- In error detection

* Only looking to see if any error has occurred

- In error correction

* Need to know the exact number of bits that are corrupted and, more importantly, their location in the message

### Coding

- Redundancy is acheived through various coding schemes

  - A relationship between the redundant bits and the actual data bits

- Important factors in any coding scheme

  - Ratio of redundant bits to data bits
  - Robustness of the process

- Two broad categories of coding schemes
  - `Block` coding
  - `Convolution` coding

### Block Coding

- In block coding

  - Divide our message into blocks, each of k bits, called `datawords`
  - Add `r redundant bits` to each block to make the length `n = k + r`.
    - The resulting n-bit blocks are called `codewords`

- The number of possible codewords is larger than the number of possible datawords

  - n > k

- one-to-one coding process

  - The same dataword is always encoded as the same codeword

- Existence of these invalid codes
  - 2^n - 2^k codewords that are not used
  - If the receiver receives an invalid codeword, this indicates that the data was corrupted during transmission.

### Error Detection

- If the following two conditions are met, the receiver can detect a change in the original codeword

1. The receiver has (or can find) a list of valid codewords
2. The original codeword has changed to an invalid one

- An error-detecting code can detect only the types of errors for which it is designed; other types of errors may remain undetected.

  - If the received codeword is not valid
    - It is discarded.
  - However, if the codeword is corrupted during transmission but the received word still matches a valid codeword,
    - The error remains undetected

- Example

  - Let us assume that k = 2 and n = 3. Table 10.1 shows the list of datawords and codewords. Later, we will see how to derive a codeword from a dataword.
  - A code for error detection in Example

  | Dataword | Codeword | Dataword | Codeword |
  | :------: | :------: | :------: | :------: |
  |    00    |   000    |    10    |   101    |
  |    01    |   011    |    11    |   110    |

  - Solution
  - Assume the sender encodes the dataword 01 as 011 and sends it to the receiver. Consider the following cases:

  1.  The receiver receives 011. It is a valid codeword. The receiver extracts the dataword 01 from it.
  2.  The codeword is corrupted during transmission, and 111 is received (the leftmost bit is corrupted). This is not a valid codeword and is discarded.
  3.  The codeword is corrupted during transmission, and 000 is received (the right two bits are corrupted). This is a valid codeword. The receiver incorrectly extracts the dataword 00. Two corrupted bits have made the error undetectable.

- `Hamming Distance` between two words (of the same size)

  - The number of differences between the corresponding bits
  - Hamming distance between two words x and y: d(x, y)
  - E.g., d(00000, 01101) = 3

- Hamming distance between the received codeword and the sent codeword is the number of bits that are corrupted during transmission

- The Hamming distance can easily be found

  - If we apply the XOR operation (⊕) on the two words and count the number of 1s in the result.

- Example

  - Let us find the Hamming distance between two pairs of words.
    1. The Hamming distance d(000, 011) is 2 because (000 ⊕ 011) is 011 (two 1s).
    2. The Hamming distance d(10101, 11110) is 3 because (10101 ⊕ 11110) is 01011 (three 1s).

- Minimum Hamming Distance for Error Detection

  - In a set of codewords, the minimum Hamming distance is the smallest Hamming distance between all possible pairs of codewords
  - If our system is to detect up to s errors, the minimum distance between the valid codes must be (s+1), so that the received codeword does not match a valid codeword.
  - To guarantee the detection of up to s errors in all cases, the minimum Hamming distance in a block code must be d(min) = s + 1.

- Geometric concept explaining d(min) in error detection
  - d(min) must be an integer greater than s or d(min) = s + 1.

![dc4](/assets/images/2023-12-11/dc4.png)

- Linear Block Codes

  - A linear block code is a code in which the exclusive OR (addition modulo-2) of two valid codewords creates another valid codeword.

- Minimum Distance for Linear Block Codes

  - The number of 1s in the nonzero valid codeword with the smallest number of 1s.

- Example

| Dataword | Codeword | Dataword | Codeword |
| :------: | :------: | :------: | :------: |
|    00    |   000    |    10    |   101    |
|    01    |   011    |    11    |   110    |

- The number of 1s in the nonzero codewords are 2, 2, and 2.
- So the minimum Hamming distance is d(min) = 2

* `Parity-Check` Code
  - This code is a linear block for even case
  - k-bit dataword is changed to an n-bit codeword where n = k + 1
  - Parity bit
    - Selected to make the total number of 1s in the codeword even or odd
  - The minimum Hamming distance for this category is d(min) = 2
    - The code is a single-bit error-detecting code

- A parity-check code can detect an odd number or errors

* Simple parity-check code C(5,4)

![dc5](/assets/images/2023-12-11/dc5.png)

- Encoder and decoder for simple parity-check code

![dc6](/assets/images/2023-12-11/dc6.png)

- Sender
  - Calculation is done in modular arithmetic

```
r0 = a3 + a2 + a1 + a0 (modulo-2)
```

- If the number of 1s is even, the result is 0
- If the number of 1s is odd, the result is 1

* Receiver
  - The result which is called the syndrome, is just 1 bit.

```
s0 = b3 + b2 + b1 + b0 + q0  (modulo-2)
```

    - The syndrome is 0 when the number of 1s in the received codeword is even
    - Otherwise, it is 1.

- The syndrome is 0, there is no detectable error
- If the syndrome is 1, the data portion of the received codeword is discarded

* Example

  - Let us look at some transmission scenarios. Assume the sender sends the dataword `1011`. The codeword created from this dataword is `10111`, which is sent to the receiver. We examine five cases:

  1. No error occurs; the received codeword is 10111. The syndrome is 0. The dataword 1011 is created.

  2. One-single-bit error changes a1. The received codeword is 10011. The syndrome is 1. No dataword is created.

  3. One single-bit error changes r0. The received codeword is 10110. The syndrome is 1. No dataword is created. Note that although none of the dataword bits are corrupted, no dataword is created because the code is not sophisticated enouth to show the position of the corrupted bit.

  4. An error changes r0 and a second error changes a3. The received codeword is 00110. The syndrome is 0. The dataword 0011 is created at the receiver. Note that here the dataword is wrongly created due to the syndrome value. The simple parity-check decoder connot detect an even number of errors. The errors cancel each other out and give the syndrome a value of 0.

  5. Three bits-a3, a2 and a1-are changed by errors. The received codeword is 01011. The syndrome is 1. The dataword is not created. This shows that the simple parity check, guaranteed to detect one single error, can also find any odd numbers of errors.

### Cyclic Codes

- `Cyclic` codes are special linear block codes with one extra property.

- In a cyclic code,
  - If a codeword is cyclically shifted (rotated), the result is another codeword

```
b1 = a0  b2 = a1  b3 = a2  b4 = a3  b5 = a4  b6 = a5  b0 = a6
```

- The last bit of the first word is wrapped around and becomes the first bit of the second word.

### Cyclic Redundancy Check

- Cyclic redundancy check (CRC)

* Used iin networks such as LANs and WANs

- Encoding

* Dataword has k bits (4 here), codeword has n bits (7 here)
* Divisor of size n - k + 1 (4 here)
* Modulo-2 division
  - The quotient of the division is discarded
  - The remainder (r2r1r0) is appended to the dataword to create the codeword

- Decoding

  - The remainder produced by the checker is a syndrome of n - k (3 here)
    - If the syndrome bits are all 0s, the 4 leftmost bits of the codeword are accepted as the dataword (interpreted as no error)
    - Otherwise, the 4 bits are discarded (error).

- A CRC code with C(7,4)
  - Both the linear and cyclic properties

![dc7](/assets/images/2023-12-11/dc7.png)

- CRC encoder and decoder

![dc8](/assets/images/2023-12-11/dc8.png)

- Division in CRC encoder

![dc9](/assets/images/2023-12-11/dc9.png)

- Division in the CRC decoder for two cases

### Polynomials

- Represent cyclic codes `as polynomials`

  - A pattern of 0s and 1s can be represented as a polynomial with coefficients of 0 and 1

- A polynomial to represent a binary word
  - A 7-bit pattern can be replaced by three terms

![dc11](/assets/images/2023-12-11/dc11.png)

- Degree of a Polynomial

  - The highest power in the polynomial

- Adding and Subtracting Polynomials

  - Addition and subtraction are the same
  - Adding or subtracting is done by combining terms and deleting pairs of identical terms
    - E.g., adding x^5 + x^4 + x^2 and x^6 + x^4 + x^2 gives just x^6 + x^5

- Multiplying of Dividing Terms

  - Multiplying a term by another term: just adding the power
  - For dividing, just subtracting the power of the second term from the power of the first

- Multiplying Two Polynomials
  - Done term by term

```
(x^5 + x^3 + x^2 + x)(x^2 + x + 1) =
x^7 + x^6 + x^5 + x^5 + x^4 + x^3 + x^4 + x^3 + x^2 + x^3 + x^2 + x
= x^7 + x^6 + x^3 + x
```

- Dividing One Polynomial by Another

  - Conceptually the same as the binary division

- Shifting
  - Shifting to the left means adding extra 0s as rightmost bits
  - Shifting to the right means deleting some rightmost bits

```
Shifting left 3 bits: 10011 becomes 10011000
x^4 + x + 1 becomes x^7 + x^4 + x^3

Shifting right 3 bits: 10011 becomes 10
x^4 + x + 1 becomes x
```

### Encoder Using Polynomials

- Creation of a codeword from a dataword

- The divisor in a cyclic code is normally called the generator polynomial or simply the generator

- CRC division using polynomials

![dc12](/assets/images/2023-12-11/dc12.png)

### Cyclic Code Analysis

- Notation

```
Dataword: d(x)
Codeword: c(x)
Generator: g(x)
Syndrome: s(x)
Error: e(x)
```

- In a cyclic code,
  - If s(x) != 0, one or more bits is corrupted.
  - If s(x) = 0, either
  - a. No bit is corrupted, or
  - b. Some bits are corrupted, but the decoder failed to detect them.

```
Received codeword = c(x) + e(x)

Received codeword / g(x) = c(x) / g(x) + e(x) / g(x)
```

- `Syndrome` is actually the remainder of the second term on the right-hand side
- In a cyclic code, those e(x) errors that are divicible by g(x) are not caught.

* `Single-Bit Error`

  - Single-Bit Error
  - A single-bit error is e(x) = x^i
  - If the generator has more than one term and the coefficient of x^0 is 1, all single-bit errors can be caught.

* Example

  - Which of the following g(x) values guarantees that a single-bit is caught? x + 1, x^3 and 1
    - x + 1: Any single-bit error can be caught
    - x^3: All single-bit errors in positions 1 to 3 are caught
    - 1: No single-bit error can be caught

* Two Isolated Single-Bit Errors

- e(x) = x^j + x^i.

* Representation of isolated single-bit errors

![dc13](/assets/images/2023-12-11/dc13.png)

- If a generator cannot divide x^t + 1 (t between 0 and n-1), then all isolated double errors can be detected.

- Example

* Find the status of the following generators related to two isolated, single-bit errors: x + 1, x^4 + 1, and x^7 + x^6 + 1,
* Solution
  - x + 1: This is a very poor choice for a generator. Any two errors next to each other cannot be detected.
  - x^4 + 1: This generator cannot detect two errors that are four positions apart. The two errors can be anywhere, but if their distance is 4, they remain undetected.
  - x^7 + x^6 + 1: This is a good choice for this purpose.

- Odd Numbers of Errors

  - A generator that contains a factor of x + 1 can detect all odd-numbered errors
  - E.g., x^4 + x^2 + x + 1 can catch all odd-numbered errors since it can be written as a product of the two polynomials x + 1 and x^3 + x^2 + 1

- Burst Errors

  - A burst error is of the form e(x) = (x^j + ... + x^i)

- Summary: A good polynomial generator needs to have the following characteristics:

  1. It should have at least two terms.
  2. The coefficient of the term x^0 should be 1.
  3. It should not divide x^t + 1, for t between 2 and n - 1.
  4. It should have the factor x + 1.

- Standard Polynomials

![dc14](/assets/images/2023-12-11/dc14.png)

### Advantages of Cyclic Codes

- `Cyclic codes` have a very good performance in detecting single-bit errors, double errors, an odd number of errors, and burst errors.

- They can easily be implemented in hardware and software.
  - Especially fast when implemented in hardware.
  - This has made cyclic codes a good candidate for many networks.

### Other Cyclic Codes

- There are, however, more powerful polynomials that are based on abstract algebra involving Galois fields.

- One of the most interesting of these codes in the `Reed-Solomon code` used today for both detection and correction.

### Checksum

- `Checksum`

  - An error-detecting technique that can be applied to a message of any length
  - Mostly used at the network and transport layer rather than the data-link layer

- Sender

  - The message is first divided into m-bit units.
  - The generator then creates an extra m-bit unit called the checksum, which is sent with the message

- Receiver

  - If the new checksum is all 0s, the message is accepted
  - Otherwise, the message is discarded

- Checksum unit is not necessarily added at the end of the message; it can be inserted in the middle of the message.

- Checksum

![dc16](/assets/images/2023-12-11/dc16.png)

- Example
  - Suppose the message is a list of five 4-bit numbers that we want to send to a destination. In addition to sending these numbers, we send the sum of the numbers. For example, if the set of numbers is (7, 11, 12, 0, 6), we send (7, 11, 12, 0, 6, 36), where 36 is the sum of the original numbers. The receiver adds the five numbers and compares the result with the sum. If the two are the same, the receiver assumes no error, accepts the five numbers, and discards the sum. Otherwise, there is an error somewhere and the message not accepted.

### Concept

- One's Complement Addition

  - In the previous example, each number can be written as a 4-bit word (each is less than 15) except for the sum
  - Using one's complement arithmetic
    - If the number has more than m bits, the extra leftmost bits need to be added to the m rightmost bits (wrapping).

- Example
  - In the previous example, the decimal number 36 in binary is (100100)2. To change it to a 4-bit number we add the extra leftmost bit to the right four bits as shown below.

```
(10)2 + (0100)2 = (0110)2 -> (6)10
```

- Instead of sending 36 as the sum, we can send 6 as the sum (7, 11, 12, 0, 6, 6). The receiver can add the first five numbers in one's complement arithmetic. If the result is 6, the numbers are accepted; otherwise, they are rejected.
- A: sum of 4-bit words, Q: quotient of A/2^4, R: remainder of A/2^4, A = 2^4 x Q + R
- Checksum: (2^4 - 1) - (Q + R)
- Rx: A + (2^4 - 1) - (Q + R) = (2^4 x Q + R) + (2^4 - 1) - (Q + R) = 2^4 x Q + [(2^4 - 1) - Q] => 2^4 - 1

* One's complement

![dc17](/assets/images/2023-12-11/dc17.png)

- Checksum

  - We can make the job of the receiver easier if we send the complement of the sum, the checksum

- Example

![dc18](/assets/images/2023-12-11/dc18.png)

- Internet Checksum

  - The Internet has used a 16-bit checksum

- Table: Procedure to calculate the traditional checksum

![dc19](/assets/images/2023-12-11/dc19.png)

- Performance
  - The traditional checksum uses a smaller number of bits (16) to detect errors in a message of any size (sometimes thousands of bits).
  - However, it is not as strong as the CRC in error-checking capability
    - For example, if the value of one word is incremented and the value of another word is decremented by the same amount, the two errors cannot be detected because the sum and checksum remain the same
  - The tendency in the Internet, particularly in designing new protocols, is to replace the checksum with a CRC.

### Forward Error Correction

- Retransmission of corrupted and lost packets is not useful for real-time multimedia transmission because it creates an unacceptable delay in reproducing

- Forward error correction (FEC)
  - Correct the error or reproduce the packet immediately

### Using Hamming Distance

- Hamming distance for `error detection`

  - In order to detect s error, d(min) = s + 1

- Hamming distance for error correction

  - In order to correct t error, d(min) = 2t + 1
  - To give an example, consider the famous BCH code.
    - In this code, if data is 99 bits, we need to send 255 bits (extra 156 bits) to correct just 23 possible bit errors.
    - Most of the time we cannot afford such a redundancy

- Hamming distance for error correction

![dc20](/assets/images/2023-12-11/dc20.png)

### Using XOR

- Another recommendation is to use the property of the exclusive OR operation

```
R = P1 ⊕ P2 ⊕ ... ⊕ Pi ⊕ ... ⊕ PN -> Pi = P1 ⊕ P2 ⊕ ... ⊕ R ⊕ ... ⊕ PN
```

- Recreate any of the data items by exclusive-ORing all of the items, replacing the one to be created by the result of the previous operation (R).
  - E.g., If N = 4, it means that we need to send 25 percent extra data and be able to correct the data if only one out of four chunks is lost.

### Chunk Interleaving

- Another way to achieve FEC in multimedia is to allow some small chunks to be missing at the receiver

  - If the packet is lost, we miss only one chunk in each packet, which is normally acceptable in multimedia communication

- Interleaving

![dc21](/assets/images/2023-12-11/dc21.png)
