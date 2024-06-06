# Chapter 10 암호 알고리즘: 공개키 암호 RSA

#### 2024.06.04.(화)

### Classification of the Field of Cryptology

Cryptology - Cryptography, Cryptanalysis

Cryptography - Symmetric cipher, Asymmetric cipher, Protocol

Symmetric cipher - Block cipher, Stream cipher

### Classification of the Field of Cryptology

- Cryptology

  - Cryptography + cryptanalysis

- Cryptography

  - In a narrow sense
    - the science and art of creating secret codes
    - Mangling information into apparent unintelligibility
    - Allowing a secret method of un-mangling
  - In a broader sense
    - Mathematical techniques related to information security
    - About secure communication in the presence of adversaries

- Cryptanalysis
  - The science and art of breaking those codes.
  - The study of methods for obtaining the meaning of encrypted information without accessing the secret information

### Cryptography

- The backbone of IT security

HTTP vs. HTTPS

- What's inside the padlock?

### Secure Communication

- Basic Modern Cryptographic (Secure) Communication

### Symmetric-key cryptography

- Symmetric-key cryptosystem

- Symmetric key crypto:

  - it is easy to compute K' from K (and vice versa)
  - usually K' = K
  - two main types:
    - stream ciphers - operate on individual characters of the plaintext
    - block ciphers - process the plaintext in larger blocks of characters

- Public-key crypto:

  - it is hard(computationally infeasible) to compute K' from K
  - encryption key public, decryption key secret (private)

- Symmetric-key cryptosystem

- Goal of the adversary:

  - to systematically recover plaintexts from ciphertexts
  - to deduce the (decryption) key

- Kerckhoff's principle:
  - we must assume that the adversary knows all details of E and D
  - security of the system should be based on the protection of the decryption key
