# Cryptography and Information Security (CS-703-A)
## Detailed Study Notes for College Exam Preparation

---

## UNIT I: Mathematical Background and Classical Cryptography

### 1. Mathematical Background for Cryptography

#### 1.1 Abstract Algebra
**Definition**: Abstract algebra is the study of algebraic structures such as groups, rings, and fields.

**Groups**:
- A **group** (G, •) consists of a set G with a binary operation • that satisfies:
  - **Closure**: For all a, b ∈ G, a • b ∈ G
  - **Associativity**: (a • b) • c = a • (b • c)
  - **Identity Element**: There exists e ∈ G such that a • e = e • a = a
  - **Inverse Element**: For each a ∈ G, there exists a⁻¹ such that a • a⁻¹ = e

**Rings**:
- A **ring** is a set R with two operations (+ and ×) where:
  - (R, +) is an abelian group
  - Multiplication is associative
  - Distributive laws hold

**Fields**:
- A **field** is a ring where every non-zero element has a multiplicative inverse
- Examples: Real numbers (ℝ), Complex numbers (ℂ), Finite fields GF(p)

#### 1.2 Number Theory

**Prime Numbers**:
- Numbers divisible only by 1 and themselves
- Fundamental to cryptography (RSA, key generation)

**Modular Arithmetic**:
- Operations performed with a modulus n
- a ≡ b (mod n) means n divides (a - b)
- **Properties**:
  - (a + b) mod n = ((a mod n) + (b mod n)) mod n
  - (a × b) mod n = ((a mod n) × (b mod n)) mod n

**Modular Inverse**:
- For a and n coprime, a⁻¹ (mod n) exists such that a × a⁻¹ ≡ 1 (mod n)
- Found using Extended Euclidean Algorithm

#### 1.3 Extended Euclidean Algorithm
- Finds GCD of two numbers and expresses it as linear combination
- **Algorithm**:
  ```
  For finding gcd(a, b) and ax + by = gcd(a,b):
  1. If b = 0, return (a, 1, 0)
  2. Recursively compute gcd(b, a mod b)
  3. Update x and y accordingly
  ```

#### 1.4 Fermat's Little Theorem
- **Statement**: If p is prime and a is not divisible by p, then:
  - a^(p-1) ≡ 1 (mod p)
- **Applications**: 
  - Primality testing
  - Computing modular inverses
  - RSA algorithm foundation

#### 1.5 Euler Phi-Function (φ)
- **Definition**: φ(n) counts positive integers ≤ n that are coprime to n
- **Properties**:
  - φ(p) = p - 1 (for prime p)
  - φ(p × q) = (p-1)(q-1) (for distinct primes p, q)
  - Used extensively in RSA

#### 1.6 Euler's Theorem
- **Statement**: If gcd(a, n) = 1, then a^φ(n) ≡ 1 (mod n)
- Generalizes Fermat's Little Theorem

### 2. Introduction to Cryptography

#### 2.1 Principles of Cryptography
**Definition**: Cryptography is the science of securing communication by transforming information into an unreadable format.

**Key Objectives**:
1. **Confidentiality**: Only authorized parties can read the data
2. **Integrity**: Data hasn't been altered during transmission
3. **Authentication**: Verify identity of sender/receiver
4. **Non-repudiation**: Sender cannot deny sending the message

**Basic Terminology**:
- **Plaintext**: Original readable message
- **Ciphertext**: Encrypted message
- **Encryption**: Converting plaintext to ciphertext
- **Decryption**: Converting ciphertext back to plaintext
- **Key**: Secret parameter used in encryption/decryption

#### 2.2 Classical Cryptosystems

##### 2.2.1 Caesar Cipher
**Description**: Substitution cipher where each letter is shifted by fixed positions.

**Algorithm**:
- Encryption: C = (P + k) mod 26
- Decryption: P = (C - k) mod 26
- Where k is the shift value (key)

**Example**:
- Plaintext: HELLO
- Key: 3
- Ciphertext: KHOOR

**Weakness**: Only 25 possible keys; vulnerable to brute force

##### 2.2.2 Monoalphabetic Substitution Cipher
**Description**: Each letter is replaced by another letter (one-to-one mapping).

**Features**:
- 26! possible keys (approximately 4 × 10²⁶)
- Preserves letter frequency
- **Vulnerable to frequency analysis**

**Frequency Analysis Attack**:
- Most common letters in English: E, T, A, O, I, N
- Analyze ciphertext letter frequencies
- Match with known language statistics

##### 2.2.3 Playfair Cipher
**Description**: Digraph substitution cipher using 5×5 matrix.

**Algorithm**:
1. Create 5×5 key matrix (I and J share position)
2. Break plaintext into digraphs (pairs)
3. Apply rules:
   - Same row: Take letters to the right
   - Same column: Take letters below
   - Rectangle: Take letters at opposite corners

**Example**:
```
Key Matrix with keyword "MONARCHY":
M O N A R
C H Y B D
E F G I/J K
L P Q S T
U V W X Z
```

**Advantages**: More secure than monoalphabetic ciphers

##### 2.2.4 Hill Cipher
**Description**: Polygraphic substitution using linear algebra.

**Encryption**:
- Plaintext vector P multiplied by key matrix K
- C = K × P (mod 26)

**Example**:
```
Key Matrix K = [3 3]
               [2 5]

Plaintext: "HE" = [7, 4]
Ciphertext: K × [7, 4]ᵀ mod 26
```

**Requirements**: Key matrix must be invertible mod 26

##### 2.2.5 Vigenère Cipher
**Description**: Polyalphabetic substitution cipher using keyword.

**Algorithm**:
- Use keyword to determine shift for each letter
- Cᵢ = (Pᵢ + Kᵢ mod m) mod 26
- Where Kᵢ cycles through keyword letters

**Example**:
```
Plaintext:  ATTACKATDAWN
Keyword:    LEMONLEMONLE
Ciphertext: LXFOPVEFRNHR
```

**Kasiski Examination**: Method to break Vigenère by finding key length

### 3. Block Ciphers

#### 3.1 Data Encryption Standard (DES)

**Overview**:
- Developed by IBM in 1970s
- Block size: 64 bits
- Key size: 56 bits (64 bits with parity)
- 16 rounds of processing

**DES Structure**:
1. **Initial Permutation (IP)**: Rearrange 64 input bits
2. **16 Rounds of Feistel Function**:
   - Divide block into Left (L) and Right (R) halves
   - Lᵢ = Rᵢ₋₁
   - Rᵢ = Lᵢ₋₁ ⊕ f(Rᵢ₋₁, Kᵢ)
3. **Final Permutation (FP)**: Inverse of IP

**Feistel Function f(R, K)**:
1. **Expansion (E)**: Expand 32 bits to 48 bits
2. **Key Mixing**: XOR with 48-bit round key
3. **Substitution (S-boxes)**: 8 S-boxes, each converts 6 bits to 4 bits
4. **Permutation (P)**: Rearrange 32 bits

**Key Schedule**:
- 56-bit key divided into two 28-bit halves
- Each round: Left circular shift and permutation
- Generates 16 round keys (48 bits each)

**Weaknesses**:
- 56-bit key too small (vulnerable to brute force)
- Differential and linear cryptanalysis possible

#### 3.2 Triple DES (3DES)
**Description**: Apply DES three times with different keys.

**Modes**:
1. **DES-EEE3**: Encrypt-Encrypt-Encrypt with 3 keys (K1, K2, K3)
2. **DES-EDE3**: Encrypt-Decrypt-Encrypt with 3 keys
3. **DES-EDE2**: Encrypt-Decrypt-Encrypt with 2 keys (K1, K2, K1)

**Effective Key Length**: 112 or 168 bits

**Formula**: C = E(K3, D(K2, E(K1, P)))

#### 3.3 Modes of Operation

##### Electronic Codebook (ECB)
- Each block encrypted independently
- **Advantage**: Parallel processing possible
- **Disadvantage**: Identical plaintext blocks → identical ciphertext blocks

##### Cipher Block Chaining (CBC)
- Each plaintext block XORed with previous ciphertext block
- Cᵢ = E(K, Pᵢ ⊕ Cᵢ₋₁)
- Requires Initialization Vector (IV)
- **Advantage**: Same plaintext produces different ciphertext

##### Cipher Feedback (CFB)
- Converts block cipher to stream cipher
- Cᵢ = Pᵢ ⊕ E(K, Cᵢ₋₁)

##### Output Feedback (OFB)
- Generates keystream independent of plaintext
- Oᵢ = E(K, Oᵢ₋₁)
- Cᵢ = Pᵢ ⊕ Oᵢ

##### Counter (CTR)
- Encrypts counter values to generate keystream
- Cᵢ = Pᵢ ⊕ E(K, Counter + i)
- **Advantages**: Parallel processing, random access

### 4. Stream Ciphers

**Definition**: Encrypts data bit-by-bit or byte-by-byte using keystream.

**Formula**: Cᵢ = Pᵢ ⊕ Kᵢ

**Characteristics**:
- Faster than block ciphers
- No padding required
- Keystream must never be reused with same key

**RC4 Stream Cipher**:
- Variable key size (40-2048 bits)
- Uses permutation of 256 bytes
- **Key Scheduling Algorithm (KSA)**: Initialize state array
- **Pseudo-Random Generation Algorithm (PRGA)**: Generate keystream

---

## UNIT II: Advanced Encryption and Public Key Cryptography

### 1. Advanced Encryption Standard (AES)

#### 1.1 AES Overview
**Background**:
- Adopted in 2001 to replace DES
- Also known as Rijndael cipher
- Block size: 128 bits
- Key sizes: 128, 192, or 256 bits
- Number of rounds: 10, 12, or 14 (depending on key size)

#### 1.2 AES Structure

**State Array**: 4×4 matrix of bytes (16 bytes total)

**Round Transformation (except last round)**:
1. **SubBytes**: Non-linear substitution using S-box
2. **ShiftRows**: Cyclic shift of rows
3. **MixColumns**: Matrix multiplication in GF(2⁸)
4. **AddRoundKey**: XOR with round key

**Last Round**: SubBytes, ShiftRows, AddRoundKey (no MixColumns)

#### 1.3 Detailed AES Operations

**1.3.1 SubBytes**:
- Each byte replaced using S-box (substitution table)
- S-box provides confusion
- Computed using multiplicative inverse in GF(2⁸)

**1.3.2 ShiftRows**:
- Row 0: No shift
- Row 1: Shift left by 1 byte
- Row 2: Shift left by 2 bytes
- Row 3: Shift left by 3 bytes

**1.3.3 MixColumns**:
- Each column multiplied by fixed matrix in GF(2⁸)
- Provides diffusion
```
[02 03 01 01]   [s₀]
[01 02 03 01] × [s₁]
[01 01 02 03]   [s₂]
[03 01 01 02]   [s₃]
```

**1.3.4 AddRoundKey**:
- XOR state with round key
- Round keys derived from cipher key using key expansion

#### 1.4 AES Key Expansion
- Generates round keys from cipher key
- Uses RotWord, SubWord, and Rcon (round constant)
- For 128-bit key: Generates 11 round keys (176 bytes)

**Advantages of AES**:
- Highly secure against known attacks
- Efficient in both software and hardware
- Flexible key sizes

### 2. Public Key Cryptography

#### 2.1 Introduction to Public Key Cryptosystem

**Concept**:
- Two keys: Public key (encryption) and Private key (decryption)
- Public key can be shared openly
- Private key kept secret

**Advantages**:
- No need for secure key exchange
- Enables digital signatures
- Supports non-repudiation

**Differences from Symmetric Cryptography**:
| Symmetric | Asymmetric |
|-----------|------------|
| Same key for encryption/decryption | Different keys |
| Faster | Slower |
| Key distribution problem | No key distribution issue |
| Examples: AES, DES | Examples: RSA, ECC |

#### 2.2 Discrete Logarithm Problem

**Definition**: Given g, p (prime), and y = gˣ mod p, find x.

**Difficulty**: Computationally infeasible for large p (basis of security)

**Applications**: Diffie-Hellman, ElGamal, DSA

#### 2.3 Diffie-Hellman Key Exchange

**Purpose**: Securely establish shared secret over insecure channel.

**Protocol**:
1. Agree on prime p and generator g (public)
2. Alice chooses secret a, computes A = gᵃ mod p, sends A
3. Bob chooses secret b, computes B = gᵇ mod p, sends B
4. Alice computes K = Bᵃ mod p
5. Bob computes K = Aᵇ mod p
6. Both have same secret K = gᵃᵇ mod p

**Security**: Based on discrete logarithm problem

**Vulnerability**: Susceptible to Man-in-the-Middle attack (mitigated by authentication)

#### 2.4 Computational & Decisional Diffie-Hellman Problem

**Computational DH (CDH)**: Given gᵃ and gᵇ, compute gᵃᵇ

**Decisional DH (DDH)**: Distinguish (gᵃ, gᵇ, gᵃᵇ) from (gᵃ, gᵇ, gᶜ)

### 3. RSA Cryptosystem

#### 3.1 RSA Algorithm

**Key Generation**:
1. Choose two large primes p and q
2. Compute n = p × q
3. Compute φ(n) = (p-1)(q-1)
4. Choose e such that gcd(e, φ(n)) = 1 and 1 < e < φ(n)
5. Compute d = e⁻¹ mod φ(n)
6. Public key: (e, n), Private key: (d, n)

**Encryption**: C = Mᵉ mod n

**Decryption**: M = Cᵈ mod n

**Example**:
```
p = 61, q = 53
n = 3233, φ(n) = 3120
e = 17 (coprime to 3120)
d = 2753 (17 × 2753 ≡ 1 mod 3120)

Plaintext M = 123
Ciphertext C = 123¹⁷ mod 3233 = 855
Decrypted M = 855²⁷⁵³ mod 3233 = 123
```

#### 3.2 RSA Assumptions & Cryptosystem

**RSA Assumption**: Factoring large n into p and q is computationally hard

**Security Requirements**:
- p and q should be large (≥1024 bits each)
- p and q should be randomly chosen
- |p - q| should not be small
- e commonly chosen as 65537 (2¹⁶ + 1)

#### 3.3 RSA Signatures & Schnorr Identification Schemes

**RSA Signature**:
- Sign: S = Mᵈ mod n (using private key)
- Verify: M = Sᵉ mod n (using public key)

**Schnorr Identification**:
1. Prover generates random r, sends x = gʳ mod p
2. Verifier sends challenge c
3. Prover responds with y = r + c×s (s is secret)
4. Verifier checks if gʸ = x × (public key)ᶜ

### 4. Primality Testing

#### 4.1 Miller-Rabin Primality Test

**Purpose**: Probabilistic test to determine if number is prime.

**Algorithm**:
1. Write n-1 = 2ᵏ × m (m odd)
2. Choose random a (2 ≤ a ≤ n-2)
3. Compute b = aᵐ mod n
4. If b = 1 or b = n-1, n is probably prime
5. For i = 1 to k-1:
   - b = b² mod n
   - If b = n-1, n is probably prime
6. If none passed, n is composite

**Accuracy**: Multiple rounds increase confidence

### 5. Elliptic Curve Cryptography

#### 5.1 Elliptic Curve over the Reals

**Equation**: y² = x³ + ax + b (where 4a³ + 27b² ≠ 0)

**Point Addition**:
- Geometric interpretation: Line through two points intersects curve at third point
- Reflect third point across x-axis

**Properties**:
- Identity element: Point at infinity (O)
- Inverse: (x, y) and (x, -y)

#### 5.2 Elliptic Curve Modulo a Prime

**Equation**: y² ≡ x³ + ax + b (mod p)

**Point Operations** (all mod p):
- Point addition: P + Q
- Point doubling: P + P = 2P
- Scalar multiplication: kP = P + P + ... + P (k times)

**Advantages**:
- Smaller key sizes for equivalent security
- Faster computations
- Lower bandwidth requirements

**ECC Comparison**:
| RSA Key Size | ECC Key Size | Security Level |
|--------------|--------------|----------------|
| 1024 bits | 160 bits | Low |
| 2048 bits | 224 bits | Medium |
| 3072 bits | 256 bits | High |
| 15360 bits | 512 bits | Very High |

#### 5.3 ECC Applications

**ECDH (Elliptic Curve Diffie-Hellman)**:
- Alice sends aG, Bob sends bG
- Shared secret: abG

**ECDSA (Elliptic Curve Digital Signature Algorithm)**:
- Used in Bitcoin, TLS, cryptocurrencies
- More efficient than RSA signatures

### 6. Chinese Remainder Theorem (CRT)

**Statement**: System of congruences has unique solution mod product of moduli.

**Given**:
- x ≡ a₁ (mod m₁)
- x ≡ a₂ (mod m₂)
- ...where m₁, m₂ are coprime

**Solution**:
```
M = m₁ × m₂ × ... × mₙ
Mᵢ = M / mᵢ
yᵢ = Mᵢ⁻¹ mod mᵢ
x = Σ(aᵢ × Mᵢ × yᵢ) mod M
```

**RSA-CRT**: Speeds up RSA decryption by ~4x using CRT

---

## UNIT III: Hash Functions and Message Authentication

### 1. Message Authentication

#### 1.1 Introduction

**Purpose**: Verify message integrity and authenticity.

**Requirements**:
- Detect modification, insertion, deletion, replay
- Verify sender's identity
- Ensure message ordering (sequence)

**Approaches**:
1. Message Authentication Codes (MAC)
2. Hash functions
3. Digital signatures

#### 1.2 Message Authentication Codes (MAC)

**Definition**: Cryptographic checksum using secret key.

**MAC Generation**: MAC = Cₖ(M)
- C: MAC algorithm
- K: Secret key
- M: Message

**Verification**: Compare received MAC with computed MAC

**Properties**:
- Deterministic
- Fixed output size
- Requires shared secret key
- Provides integrity and authentication (not non-repudiation)

### 2. Digital Signature

#### 2.1 Digital Signature Basics

**Purpose**: Provides authentication, integrity, and non-repudiation.

**Process**:
1. **Signing**: Signature = Sign(Private Key, Message)
2. **Verification**: Valid = Verify(Public Key, Message, Signature)

**Properties**:
- Only signer can create signature (private key)
- Anyone can verify (public key)
- Signature tied to specific message
- Cannot be repudiated

**Differences from MAC**:
| MAC | Digital Signature |
|-----|-------------------|
| Symmetric key | Asymmetric keys |
| Both parties can generate | Only signer can generate |
| No non-repudiation | Non-repudiation |
| Faster | Slower |

#### 2.2 RSA Digital Signature

**Signing**:
1. Compute hash: h = H(M)
2. Sign hash: S = hᵈ mod n (using private key d)
3. Send (M, S)

**Verification**:
1. Compute hash: h = H(M)
2. Compute h' = Sᵉ mod n (using public key e)
3. Accept if h = h'

### 3. Key Management & Key Exchange

#### 3.1 Key Management

**Key Lifecycle**:
1. **Generation**: Create cryptographic keys
2. **Distribution**: Securely deliver keys
3. **Storage**: Protect keys from unauthorized access
4. **Usage**: Apply keys in cryptographic operations
5. **Destruction**: Securely delete expired keys

**Key Distribution Methods**:
- Physical delivery
- Public key encryption
- Key Distribution Centers (KDC)
- Diffie-Hellman exchange

#### 3.2 Key Exchange Protocols

**Requirements**:
- Establish shared secret securely
- Authenticate parties
- Resist man-in-the-middle attacks
- Forward secrecy (compromised long-term keys don't reveal past sessions)

**Station-to-Station (STS) Protocol**:
1. Diffie-Hellman exchange
2. Signed with digital signatures
3. Provides mutual authentication

### 4. Hash Functions

#### 4.1 Hash Function Properties

**Definition**: Function H that maps arbitrary-length input to fixed-length output.

**Properties**:
1. **Preimage Resistance** (One-way): Given h, computationally infeasible to find m such that H(m) = h
2. **Second Preimage Resistance** (Weak collision resistance): Given m₁, infeasible to find m₂ ≠ m₁ such that H(m₁) = H(m₂)
3. **Collision Resistance** (Strong collision resistance): Infeasible to find any m₁ ≠ m₂ such that H(m₁) = H(m₂)

**Applications**:
- Digital signatures
- Message authentication
- Password storage
- Data integrity verification
- Blockchain

#### 4.2 Birthday Attack

**Birthday Paradox**: In group of 23 people, >50% chance two share birthday.

**Application to Hash Functions**:
- For n-bit hash, collision found with √(2ⁿ) attempts
- For 128-bit hash, 2⁶⁴ attempts sufficient (not 2¹²⁸)

**Implication**: Hash output should be ≥256 bits for adequate security

### 5. Cryptographic Hash Functions

#### 5.1 MD5 (Message Digest 5)

**Specifications**:
- Input: Arbitrary length
- Output: 128-bit hash
- Processes 512-bit blocks
- 4 rounds of 16 operations each

**Algorithm**:
1. Padding: Add bits to make length ≡ 448 (mod 512)
2. Append length
3. Initialize MD buffer (4 32-bit words)
4. Process each 512-bit block
5. Output final 128-bit hash

**Status**: **BROKEN** - Collision attacks found; not recommended

#### 5.2 SHA (Secure Hash Algorithm)

**SHA-1**:
- Output: 160 bits
- Similar to MD5 but more secure
- **Status**: Deprecated (collisions found in 2017)

**SHA-2 Family**:
- **SHA-224**: 224-bit output
- **SHA-256**: 256-bit output (most common)
- **SHA-384**: 384-bit output
- **SHA-512**: 512-bit output

**SHA-256 Algorithm**:
1. Padding (same as MD5)
2. Initialize 8 hash values (32 bits each)
3. Process in 512-bit blocks
4. 64 rounds of operations
5. Output 256-bit hash

**SHA-3 (Keccak)**:
- Different structure (sponge construction)
- Output: 224, 256, 384, or 512 bits
- Not a replacement for SHA-2 (alternative)

### 6. Digital Signature Standard (DSS)

#### 6.1 DSS Overview

**Components**:
- Uses Digital Signature Algorithm (DSA)
- Hash function: SHA-256 or better
- Based on discrete logarithm problem

#### 6.2 DSA Algorithm

**Key Generation**:
1. Choose prime p (modulus), q (prime divisor of p-1)
2. Choose g of order q mod p
3. Choose private key x (0 < x < q)
4. Compute public key y = gˣ mod p

**Signature Generation**:
1. Compute h = H(M)
2. Choose random k (0 < k < q)
3. Compute r = (gᵏ mod p) mod q
4. Compute s = k⁻¹(h + xr) mod q
5. Signature: (r, s)

**Signature Verification**:
1. Compute h = H(M)
2. Compute w = s⁻¹ mod q
3. Compute u₁ = hw mod q, u₂ = rw mod q
4. Compute v = ((gᵘ¹ × yᵘ²) mod p) mod q
5. Accept if v = r

### 7. Advanced Cryptanalysis

#### 7.1 Time-Memory Trade-off Attack

**Concept**: Precompute hash tables to speed up attacks.

**Rainbow Tables**:
- Optimized hash tables for password cracking
- Trade memory for computation time
- Countermeasure: **Salting** (add random data before hashing)

#### 7.2 Differential Cryptanalysis

**Method**: Analyze how differences in input affect output differences.

**Process**:
1. Choose input pairs with specific differences
2. Observe output differences
3. Deduce key bits

**Defense**: Design S-boxes resistant to differential cryptanalysis

#### 7.3 Secure Channel and Authentication Systems

**Kerberos**:
- Network authentication protocol
- Uses symmetric key cryptography
- Involves Key Distribution Center (KDC)
- Ticket-based system

**Components**:
- **Authentication Server (AS)**: Verifies user identity
- **Ticket Granting Server (TGS)**: Issues service tickets
- **Service Server (SS)**: Provides requested service

**Kerberos Process**:
1. User → AS: Request for TGS ticket
2. AS → User: TGS ticket encrypted with user's key
3. User → TGS: Request for service ticket
4. TGS → User: Service ticket
5. User → SS: Service ticket for authentication

---

## UNIT IV: Network Security Protocols

### 1. Information Security Fundamentals

#### 1.1 Threats in Networks

**Types of Threats**:
1. **Interception**: Unauthorized access to data (eavesdropping)
2. **Interruption**: Making resources unavailable (DoS)
3. **Modification**: Altering data in transit (man-in-the-middle)
4. **Fabrication**: Creating fake data (spoofing)

**Network-Specific Threats**:
- IP Spoofing
- Session Hijacking
- DNS Poisoning
- ARP Spoofing
- Packet Sniffing
- Port Scanning

#### 1.2 Network Security Controls

**Architecture**:
- **Perimeter Security**: Firewalls, DMZ
- **Network Segmentation**: VLANs, subnets
- **Access Control**: Authentication, authorization
- **Monitoring**: IDS/IPS, logging

**Defense in Depth**: Multiple layers of security controls

### 2. Wireless Security

#### 2.1 Wireless Threats

**Common Attacks**:
- **War Driving**: Searching for vulnerable WiFi networks
- **Evil Twin**: Rogue access point mimicking legitimate one
- **Packet Sniffing**: Capturing wireless traffic
- **WPS Attacks**: Exploiting WiFi Protected Setup vulnerabilities

#### 2.2 Wireless Security Protocols

**WEP (Wired Equivalent Privacy)**:
- **Encryption**: RC4 stream cipher
- **Key**: 40 or 104 bits + 24-bit IV
- **Status**: **BROKEN** - easily cracked
- **Vulnerabilities**: IV reuse, weak key scheduling

**WPA (WiFi Protected Access)**:
- Improvement over WEP
- Uses TKIP (Temporal Key Integrity Protocol)
- Per-packet key mixing
- Message Integrity Check (MIC)
- **Status**: Deprecated

**WPA2**:
- Uses AES-CCMP (Counter Mode with CBC-MAC)
- Mandatory for WiFi certification since 2006
- Personal (PSK) and Enterprise (802.1X) modes
- **Vulnerability**: KRACK attack (2017) - patched

**WPA3**:
- Latest standard (2018)
- Uses SAE (Simultaneous Authentication of Equals)
- Forward secrecy
- Protection against offline dictionary attacks
- 192-bit security for Enterprise

### 3. Honey Pots

**Definition**: Decoy systems designed to attract and detect attackers.

**Types**:
1. **Low-Interaction**: Emulate services, limited attacker interaction
2. **High-Interaction**: Real systems, full attacker interaction
3. **Pure**: Full operating systems
4. **Research Honeypots**: Gather threat intelligence
5. **Production Honeypots**: Protect organizations

**Benefits**:
- Early warning of attacks
- Divert attackers from real systems
- Collect attack methods and malware
- Legal evidence

**Honeypot Examples**:
- Honeyd (low-interaction)
- Kippo (SSH honeypot)
- HoneyDB (database honeypot)

### 4. Traffic Flow Security

**Definition**: Concealing communication patterns and metadata.

**Techniques**:
- **Traffic Padding**: Add dummy traffic
- **Traffic Shaping**: Disguise traffic patterns
- **VPN Tunneling**: Encrypt and encapsulate traffic
- **Tor Network**: Onion routing for anonymity

**Metadata Protection**: Hide source, destination, timing, volume

### 5. Firewalls

#### 5.1 Firewall Concepts

**Definition**: Network security device that monitors and controls traffic.

**Functions**:
- Packet filtering
- Access control
- NAT (Network Address Translation)
- Logging and auditing

#### 5.2 Firewall Design and Types

**1. Packet Filtering Firewall**:
- Operates at Network layer (Layer 3)
- Filters based on: IP addresses, ports, protocols
- Stateless (examines each packet independently)
- **Advantages**: Fast, transparent
- **Disadvantages**: Cannot inspect application data

**2. Stateful Inspection Firewall**:
- Tracks connection state
- Maintains state table
- Allows return traffic automatically
- **Advantages**: More secure than packet filtering
- **Example**: Most modern firewalls

**3. Application Layer Firewall** (Proxy Firewall):
- Operates at Application layer (Layer 7)
- Deep packet inspection
- Can filter based on application content
- **Advantages**: Granular control, prevents direct connections
- **Disadvantages**: Slower, resource-intensive

**4. Next-Generation Firewall (NGFW)**:
- Combines traditional firewall with:
  - Intrusion Prevention System (IPS)
  - Application awareness
  - Deep packet inspection
  - SSL/TLS inspection

**Firewall Architectures**:
- **Screening Router**: Simple packet filter
- **Dual-Homed Host**: System with two NICs
- **Screened Host**: Bastion host behind packet filter
- **Screened Subnet (DMZ)**: Isolated network segment

### 6. Personal Firewalls & IDS

#### 6.1 Personal Firewalls

**Definition**: Software firewall on individual computer.

**Features**:
- Application control
- Outbound connection monitoring
- Process-level filtering
- **Examples**: Windows Defender Firewall, iptables

#### 6.2 Intrusion Detection Systems (IDS)

**Definition**: Monitors network/system for malicious activity.

**Types**:
1. **Network-based IDS (NIDS)**: Monitors network traffic
2. **Host-based IDS (HIDS)**: Monitors single host

**Detection Methods**:
- **Signature-based**: Match known attack patterns
- **Anomaly-based**: Detect deviations from baseline
- **Stateful Protocol Analysis**: Track protocol state

**Popular IDS**:
- Snort
- Suricata
- OSSEC (HIDS)

**IDS vs IPS**:
| IDS | IPS |
|-----|-----|
| Detects and alerts | Detects and prevents |
| Passive | Active |
| Out-of-band | Inline |

### 7. Email Security

#### 7.1 Email Security Threats

**Common Threats**:
- Phishing
- Spam
- Malware attachments
- Email spoofing
- BEC (Business Email Compromise)

#### 7.2 Email Security Services

**Service Security for Email Attacks Through Emails**:
- **Anti-spam filters**: Bayesian filtering, blacklists
- **Anti-malware scanning**: Attachment analysis
- **Sandboxing**: Execute attachments in isolated environment
- **Content filtering**: Block sensitive data

**Email Authentication**:
1. **SPF (Sender Policy Framework)**:
   - DNS record specifying authorized mail servers
   - Prevents domain spoofing

2. **DKIM (DomainKeys Identified Mail)**:
   - Cryptographic signature in email header
   - Verifies sender and message integrity

3. **DMARC (Domain-based Message Authentication)**:
   - Policy for SPF/DKIM failures
   - Reporting mechanism

#### 7.3 Pretty Good Privacy (PGP)

**Overview**:
- Email encryption program
- Created by Phil Zimmermann (1991)
- Hybrid cryptosystem

**PGP Services**:
1. **Confidentiality**: Encrypt message
2. **Authentication**: Digital signature
3. **Integrity**: Hash function
4. **Non-repudiation**: Public key signature

**PGP Operations**:

**Sending Encrypted Email**:
1. Generate random session key
2. Encrypt message with session key (symmetric)
3. Encrypt session key with recipient's public key
4. Send both

**Receiving Encrypted Email**:
1. Decrypt session key with private key
2. Decrypt message with session key

**Digital Signature**:
1. Hash message
2. Encrypt hash with sender's private key (signature)
3. Recipient decrypts signature with sender's public key
4. Compare hashes

**S-MIME (Secure/Multipurpose Internet Mail Extensions)**:
- Standard for email encryption
- Uses X.509 certificates
- Supported by most email clients
- Similar functionality to PGP

### 8. IP Security (IPSec)

#### 8.1 IPSec Overview

**Purpose**: Secure IP communications by authenticating and encrypting each IP packet.

**Operating Modes**:
1. **Transport Mode**: Protects payload only
   - Used for end-to-end communication
2. **Tunnel Mode**: Protects entire IP packet
   - Used for VPNs

#### 8.2 IPSec Protocols

**1. Authentication Header (AH)**:
- Provides authentication and integrity
- No encryption
- Protects entire packet (except mutable fields)
- **Protocol number**: 51

**2. Encapsulating Security Payload (ESP)**:
- Provides encryption, authentication, and integrity
- Can encrypt payload or entire packet
- **Protocol number**: 50

**3. Internet Key Exchange (IKE)**:
- Establishes Security Association (SA)
- Negotiates cryptographic algorithms
- Authenticates peers
- **Uses UDP port 500**

**IKE Phases**:
- **Phase 1**: Establish secure channel (ISAKMP SA)
- **Phase 2**: Negotiate IPSec SA for data transfer

#### 8.3 IPSec Architecture

**Security Association (SA)**:
- One-way relationship between sender and receiver
- Identified by: SPI (Security Parameter Index), Destination IP, Protocol (AH/ESP)
- Parameters: Encryption algorithm, keys, lifetime

**Security Policy Database (SPD)**:
- Defines which traffic to protect
- Actions: Protect, bypass, discard

**Security Association Database (SAD)**:
- Stores active SAs
- Contains cryptographic keys and parameters

### 9. Web Security

#### 9.1 Web Threats

**Common Attacks**:
- Cross-Site Scripting (XSS)
- SQL Injection
- Cross-Site Request Forgery (CSRF)
- Session Hijacking
- Clickjacking
- Directory Traversal

#### 9.2 SSL/TLS Protocol

**Purpose**: Secure communication over networks.

**SSL (Secure Sockets Layer)**:
- Developed by Netscape
- Versions: SSLv2, SSLv3
- **Status**: Deprecated (vulnerabilities found)

**TLS (Transport Layer Security)**:
- Successor to SSL
- Versions: TLS 1.0, 1.1, 1.2, 1.3
- **Current**: TLS 1.2 and 1.3 recommended

**TLS Handshake Process**:
1. **ClientHello**: Supported cipher suites, random number
2. **ServerHello**: Selected cipher suite, certificate
3. **Key Exchange**: Client generates pre-master secret, encrypts with server's public key
4. **Finished**: Both derive session keys, start encrypted communication

**TLS 1.3 Improvements**:
- Faster handshake (1-RTT, 0-RTT)
- Removed weak ciphers
- Forward secrecy mandatory
- Encrypted handshake

#### 9.3 Basic Protocols of Security

**HTTPS (HTTP over TLS)**:
- Port 443
- Encrypts HTTP traffic
- **Benefits**: Confidentiality, integrity, authentication

**HSTS (HTTP Strict Transport Security)**:
- Forces browsers to use HTTPS
- Prevents downgrade attacks

**Certificate Pinning**:
- Application hardcodes expected certificate
- Prevents MITM with rogue certificates

### 10. Encoding & Secure Electronic Transactions

#### 10.1 Encoding for Security

**Base64 Encoding**:
- Encode binary data as ASCII text
- Used in email attachments, certificates
- Not encryption (easily decoded)

**URL Encoding**:
- Encode special characters in URLs
- Prevents injection attacks

**HTML Encoding**:
- Encode special characters in HTML
- Prevents XSS attacks

#### 10.2 Secure Electronic Transaction (SET)

**Purpose**: Secure credit card transactions over internet.

**Participants**:
- Cardholder
- Merchant
- Issuer (cardholder's bank)
- Acquirer (merchant's bank)
- Payment gateway

**SET Features**:
- Confidentiality: Encryption
- Authentication: Digital certificates
- Message integrity: Digital signatures
- Merchant authentication: Cannot see card number

**SET Process**:
1. Customer sends encrypted card details
2. Merchant forwards to payment gateway (cannot decrypt)
3. Payment gateway processes with bank
4. Authorization sent back to merchant

**Status**: Largely replaced by 3D Secure (Verified by Visa, Mastercard SecureCode)

---

## UNIT V: Cryptography and Information Security Tools

### 1. Spoofing Tools

#### 1.1 Network Spoofing

**IP Spoofing**:
- Forge source IP address
- **Tools**: hping3, Scapy
- **Purpose**: DoS attacks, bypassing filters

**ARP Spoofing**:
- Send fake ARP messages
- Redirect traffic to attacker
- **Tools**: arpspoof, Ettercap
- **Detection**: ARP monitoring tools

**DNS Spoofing**:
- Poison DNS cache
- Redirect users to malicious sites
- **Tools**: dnsspoof
- **Defense**: DNSSEC

#### 1.2 Email Spoofing

**Techniques**:
- Forge sender address
- Display name spoofing
- Similar domain names

**Tools**:
- Anonymouse
- Sendfakemail

**Defense**:
- SPF, DKIM, DMARC
- Email authentication

### 2. Vulnerability Scanning Tools

#### 2.1 Purpose

**Vulnerability Scanning**: Automated testing for security weaknesses.

**Types**:
- Network vulnerability scanners
- Web application scanners
- Database scanners

#### 2.2 Popular Tools

**1. Nessus**:
- Comprehensive vulnerability scanner
- Plugin-based architecture
- Commercial (free for home use)
- Detects: Missing patches, misconfigurations, weak passwords

**2. OpenVAS**:
- Open-source alternative to Nessus
- Network Vulnerability Tests (NVTs)
- Web interface

**3. Nmap (Network Mapper)**:
- Network discovery and security auditing
- Port scanning
- OS detection
- Service version detection
- **NSE (Nmap Scripting Engine)**: Vulnerability detection

**Nmap Scan Types**:
- **TCP SYN Scan** (-sS): Stealth scan
- **TCP Connect Scan** (-sT): Full connection
- **UDP Scan** (-sU): UDP ports
- **Service Version Detection** (-sV)
- **OS Detection** (-O)

**4. Nikto**:
- Web server scanner
- Checks for: Outdated software, dangerous files, misconfigurations
- Command-line tool

**5. Burp Suite**:
- Web application security testing
- Proxy, scanner, intruder, repeater
- Professional and community editions

### 3. Net Tools Suite Pack

**Common Network Tools**:

**1. ping**:
- Test connectivity
- Measure latency
- Uses ICMP Echo Request/Reply

**2. traceroute / tracert**:
- Display route packets take
- Identify network hops
- Diagnose routing issues

**3. netstat**:
- Display network connections
- Show routing tables
- Monitor network statistics

**4. nslookup / dig**:
- Query DNS records
- Troubleshoot DNS issues

**5. Wireshark**:
- Packet capture and analysis
- Protocol analyzer
- Deep inspection of hundreds of protocols
- **Filters**: Display and capture filters

**6. tcpdump**:
- Command-line packet analyzer
- Capture and display network traffic
- Powerful filtering capabilities

### 4. Steganography Tools

#### 4.1 Steganography Concepts

**Definition**: Hiding information within other non-secret data.

**Difference from Cryptography**:
| Cryptography | Steganography |
|--------------|---------------|
| Hides content | Hides existence |
| Encrypted message visible | Message hidden |
| Attracts attention | Appears innocuous |

**Types**:
- Image steganography
- Audio steganography
- Video steganography
- Text steganography

#### 4.2 Image Steganography Techniques

**Least Significant Bit (LSB)**:
- Replace LSBs of pixels with message bits
- Imperceptible changes
- Large capacity

**Algorithm**:
1. Convert message to binary
2. For each byte of message:
   - Take 8 pixels
   - Replace LSB of each pixel with one message bit
3. Unchanged pixels remain

**Example**:
```
Original pixel: 10110110
Message bit: 1
Modified pixel: 10110111 (only LSB changed)
```

#### 4.3 Steganography Tools

**1. Steghide**:
- Command-line tool
- Embeds data in JPEG and BMP images
- Supports encryption
- **Usage**: `steghide embed -cf cover.jpg -ef secret.txt`

**2. OpenStego**:
- Open-source GUI tool
- Image steganography and watermarking
- Java-based, cross-platform

**3. StegDetect**:
- Detects steganographic content
- Analyzes statistical properties
- Identifies tools used

**4. OutGuess**:
- Preserves statistical properties
- More resistant to detection
- Unix tool

**5. Xiao Steganography**:
- Windows GUI tool
- Simple interface
- Password protection

#### 4.4 Steganalysis

**Definition**: Detection and extraction of hidden data.

**Methods**:
- **Visual Attacks**: Detect distortions
- **Statistical Analysis**: Detect anomalies in data distribution
- **Structural Attacks**: Exploit format knowledge

**Tools**:
- StegExpose
- StegSecret
- VS_Detector

### 5. DoS Attack Understanding Tools

#### 5.1 Denial of Service (DoS) Attacks

**Purpose**: Make service unavailable to legitimate users.

**Types**:
1. **Volume-based**: Saturate bandwidth (UDP flood, ICMP flood)
2. **Protocol-based**: Exploit protocol weaknesses (SYN flood, Ping of Death)
3. **Application-based**: Exhaust application resources (Slowloris, HTTP flood)

**DDoS (Distributed DoS)**: Attack from multiple sources.

#### 5.2 DoS Tools (Educational Purpose Only)

**1. LOIC (Low Orbit Ion Cannon)**:
- Open-source stress testing tool
- TCP, UDP, HTTP floods
- GUI interface
- **Warning**: Illegal to use against targets without permission

**2. HOIC (High Orbit Ion Cannon)**:
- Successor to LOIC
- More powerful
- Booster scripts for effectiveness

**3. hping3**:
- Command-line packet generator
- Can perform various DoS attacks
- Legitimate network testing tool

**4. Slowloris**:
- Application-layer attack
- Keeps connections open
- Exhausts server connection pool
- Effective against Apache

**5. Memcrashed**:
- Exploits misconfigured memcached servers
- Amplification attack (1:51000 ratio)

#### 5.3 DoS Defense

**Mitigation Techniques**:
- Rate limiting
- Traffic filtering
- Blackhole routing
- Anycast network
- DDoS protection services (Cloudflare, Akamai)

**Tools for Defense**:
- Fail2ban: Bans IPs with malicious behavior
- ModSecurity: Web Application Firewall (WAF)
- IPtables: Firewall rules to limit connections

### 6. Netbios Enumeration Tools

#### 6.1 NetBIOS Overview

**NetBIOS**: Network Basic Input/Output System

**Ports**:
- 137/UDP: Name service
- 138/UDP: Datagram service
- 139/TCP: Session service

**Information Exposed**:
- Computer name
- Workgroup/Domain
- Shared resources
- MAC address
- Logged-in users

#### 6.2 Enumeration Tools

**1. nbtstat** (Windows):
- Display NetBIOS over TCP/IP statistics
- **Commands**:
  - `nbtstat -a <IP>`: Remote machine name table
  - `nbtstat -c`: NetBIOS name cache

**2. nbtscan**:
- Scan networks for NetBIOS information
- Fast, command-line
- **Usage**: `nbtscan 192.168.1.0/24`

**3. enum4linux**:
- Linux tool for enumerating Windows systems
- Uses SMB, NetBIOS, LDAP
- Extracts: Users, shares, groups, password policy

**4. NetBIOS Auditing Tool (NAT)**:
- GUI tool
- Comprehensive NetBIOS enumeration

### 7. Flood & Targa Tools

#### 7.1 Flood Tools

**Purpose**: Generate high volume of traffic (testing or attacks).

**1. Synflood**:
- TCP SYN flood attack
- Exploits three-way handshake
- Exhausts server resources

**2. Smurf Attack**:
- ICMP flood
- Amplification via broadcast
- Spoofed source IP

**3. Fraggle**:
- Similar to Smurf
- Uses UDP instead of ICMP

**4. Land Attack**:
- Spoofed packet with same source and destination
- Causes system crash

#### 7.2 Targa Tools

**Targa**:
- Suite of DoS tools
- Various attack types
- **Components**:
  - bonk, boink, jolt, land, nestea, newtear, syndrop, teardrop, etc.

**Attack Types**:
- **Teardrop**: Overlapping IP fragments
- **Land**: Same source/destination
- **Ping of Death**: Oversized ICMP packets

**Defense**:
- Patch systems
- Implement firewall rules
- Enable SYN cookies

### 8. Some Trouble Tools (UDP)

**UDP-based Attack Tools**:

**1. UDP Flooder**:
- Sends large volume of UDP packets
- Saturates bandwidth
- Random or targeted ports

**2. Pepsi (formerly known as Nemesy)**:
- Windows-based UDP flooder
- GUI interface
- Adjustable packet size and rate

**3. UDP Unicorn**:
- Layer 4 UDP flood
- Randomization features

**Defense Against UDP Floods**:
- Rate limiting
- UDP port filtering
- Network-level DDoS protection

### 9. Crazy Pinger & Pandora's Box

**Crazy Pinger**:
- ICMP flood tool
- Adjustable packet size
- Measure response times
- **Legitimate use**: Network stress testing

**Pandora's Box**:
- Collection of security tools
- Port scanning
- Vulnerability testing
- Network monitoring
- **Educational purpose only**

### 10. FS Max & Flood Tools

**FSMax (Fire Storm Max)**:
- DoS tool
- Multiple attack vectors
- **Features**:
  - SYN flood
  - UDP flood
  - ICMP flood
  - HTTP flood

**Other Flood Tools**:

**1. GoldenEye**:
- HTTP DoS tool
- Keeps HTTP connections active
- Python-based

**2. HULK (HTTP Unbearable Load King)**:
- Web server DoS
- Generates unique requests
- Bypasses caching

**3. R.U.D.Y (R-U-Dead-Yet)**:
- Slow POST attack
- Keeps connections open
- Low bandwidth required

**Legal & Ethical Considerations**:
- **Always get written permission** before testing
- Use only in controlled environments
- Understand legal consequences
- Focus on defensive understanding

---

## Exam Preparation Tips

### 1. **Important Topics to Focus**:
- RSA algorithm (complete process with examples)
- AES structure and operations
- Diffie-Hellman key exchange
- Digital signatures (RSA, DSA)
- Hash functions (properties and applications)
- IPSec (protocols and modes)
- SSL/TLS handshake
- Firewall types and architectures
- Classical ciphers (Caesar, Vigenère, Playfair)
- Mathematical foundations (modular arithmetic, Euler's theorem)

### 2. **Practice Problems**:
- Numerical problems on RSA
- DES encryption/decryption
- Hash function collisions
- Digital signature verification
- Key exchange scenarios

### 3. **Short Answer Preparation**:
- Differences (Symmetric vs Asymmetric, MAC vs Digital Signature, IDS vs IPS)
- Protocol comparisons (WEP vs WPA vs WPA2)
- Security properties (Confidentiality, Integrity, Authentication, Non-repudiation)
- Attack types and defenses

### 4. **Diagram Practice**:
- DES structure
- AES rounds
- Diffie-Hellman exchange
- Digital signature process
- IPSec architecture
- TLS handshake
- Firewall architectures
- Kerberos authentication flow

### 5. **Tool Knowledge**:
- Know purposes and basic usage of tools mentioned
- Understand differences between similar tools
- Learn practical applications

---

## Quick Reference Formulas

### RSA:
- n = p × q
- φ(n) = (p-1)(q-1)
- e × d ≡ 1 (mod φ(n))
- C = M^e mod n
- M = C^d mod n

### Modular Arithmetic:
- (a + b) mod n = ((a mod n) + (b mod n)) mod n
- (a × b) mod n = ((a mod n) × (b mod n)) mod n
- a^-1 mod n: Find using Extended Euclidean Algorithm

### Diffie-Hellman:
- A = g^a mod p (Alice)
- B = g^b mod p (Bob)
- Shared secret = g^ab mod p = B^a mod p = A^b mod p

### ElGamal:
- Public key: (p, g, y = g^x mod p)
- Private key: x
- Encryption: (C1, C2) = (g^k mod p, m × y^k mod p)
- Decryption: m = C2 × (C1^x)^-1 mod p

---

## Study Strategy

1. **Week 1-2**: Mathematical background, classical ciphers
2. **Week 3**: DES, AES, block cipher modes
3. **Week 4**: Public key cryptography (RSA, Diffie-Hellman, ECC)
4. **Week 5**: Hash functions, digital signatures
5. **Week 6**: Network security protocols (IPSec, SSL/TLS)
6. **Week 7**: Firewalls, IDS, email security
7. **Week 8**: Security tools, revision, practice problems

**Daily Study Plan**:
- 1-2 hours: Theory and concepts
- 30 minutes: Practice numerical problems
- 30 minutes: Revise previous topics
- 30 minutes: Diagram practice

---

## Key Definitions to Memorize

1. **Cryptography**: Science of securing information by transforming it into unreadable form
2. **Confidentiality**: Ensuring information is accessible only to authorized parties
3. **Integrity**: Ensuring data hasn't been altered
4. **Authentication**: Verifying identity of sender/receiver
5. **Non-repudiation**: Preventing denial of actions
6. **Kerckhoffs's Principle**: Security should rely on key secrecy, not algorithm secrecy
7. **Perfect Secrecy**: Ciphertext reveals no information about plaintext
8. **Collision Resistance**: Infeasible to find two different inputs producing same hash
9. **Forward Secrecy**: Past session keys remain secure even if long-term key compromised
10. **Zero-Day Vulnerability**: Security flaw unknown to vendor

---

Good luck with your exam preparation! Focus on understanding concepts rather than just memorization, and practice numerical problems regularly.