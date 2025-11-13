# Cryptography and Information Security - Complete Remaining Topics
## Comprehensive Study Material for Exam Preparation

**Author**: Complete Study Notes
**Date**: 2025-11-13
**Student**: siddharth786s1

---

## TABLE OF CONTENTS

### PART 1: ADVANCED STREAM CIPHERS & CRYPTANALYSIS
1. Stream Ciphers (RC4, ChaCha20, One-Time Pad, LFSR)
2. Cryptanalysis Techniques (Differential, Linear, Meet-in-the-Middle, Birthday)

### PART 2: AUTHENTICATION PROTOCOLS
3. Kerberos Authentication Protocol
4. OAuth 2.0 and OpenID Connect
5. SAML (Security Assertion Markup Language)
6. Multi-Factor Authentication (MFA)

### PART 3: PKI & CERTIFICATES
7. X.509 Digital Certificates (Detailed)
8. Certificate Lifecycle Management
9. Certificate Revocation (CRL, OCSP)
10. Public Key Infrastructure Operations

### PART 4: VPN & NETWORK SECURITY
11. VPN Technologies (PPTP, L2TP, OpenVPN, WireGuard)
12. IPSec Deep Dive
13. SSL/TLS Protocol Detailed
14. Secure Shell (SSH) Protocol

### PART 5: ADVANCED TOPICS
15. Blockchain and Cryptocurrency
16. Post-Quantum Cryptography
17. Side-Channel Attacks
18. Secure Software Development
19. Mobile Security
20. Cloud Security

### PART 6: PRACTICAL PROBLEMS & EXAM PREPARATION
21. Numerical Problems with Solutions
22. Exam-Style Questions
23. Quick Revision Notes

---

## PART 1: ADVANCED STREAM CIPHERS & CRYPTANALYSIS

### 1. Stream Ciphers - Complete Coverage

#### 1.1 Linear Feedback Shift Register (LFSR)

**LFSR Concept**:
```
A shift register with feedback function
Generates pseudorandom bit sequences

Structure:
┌───┬───┬───┬───┐
│ S₀│ S₁│ S₂│ S₃│  ← Shift Register (n bits)
└─┬─┴─┬─┴─┬─┴─┬─┘
  │   │   │   │
  └───┴───┴───┴─→ Output Bit
        ↓
    Feedback Function
    (XOR of selected bits)
```

**Example: 4-bit LFSR**:
```
Feedback Polynomial: x⁴ + x + 1
Taps: positions 4 and 1

Initial State: [1][0][1][1]

Clock | State    | Output | Feedback (S₃ ⊕ S₀)
------|----------|--------|--------------------
0     | 1011     | 1      | 1⊕1 = 0
1     | 0101     | 1      | 0⊕1 = 1
2     | 1010     | 0      | 1⊕0 = 1
3     | 1101     | 1      | 1⊕1 = 0
4     | 0110     | 0      | 0⊕0 = 0
5     | 0011     | 1      | 0⊕1 = 1
6     | 1001     | 1      | 1⊕1 = 0
7     | 0100     | 0      | 0⊕0 = 0
8     | 0010     | 0      | 0⊕0 = 0
9     | 0001     | 1      | 0⊕1 = 1
10    | 1000     | 0      | 1⊕0 = 1
11    | 1100     | 0      | 1⊕0 = 1
12    | 1110     | 0      | 1⊕0 = 1
13    | 1111     | 1      | 1⊕1 = 0
14    | 0111     | 1      | 0⊕1 = 1
15    | 1011     | 1      | 1⊕1 = 0  ← Back to initial state

Output sequence: 110100100101101
Period = 15 = 2⁴ - 1 (maximal-length)
```

**LFSR Properties**:
```
Period:
- Maximum period = 2ⁿ - 1 (for n-bit LFSR)
- Requires primitive polynomial
- State [0][0]...[0] forbidden (locks to all zeros)

Primitive Polynomials (examples):
n=3: x³ + x + 1
n=4: x⁴ + x + 1
n=5: x⁵ + x² + 1
n=8: x⁸ + x⁴ + x³ + x² + 1

Advantages:
✓ Simple hardware implementation
✓ Fast
✓ Known period

Disadvantages:
✗ Cryptographically weak alone
✗ Linear (easy to break with known plaintext)
✗ Predictable (knowing 2n consecutive bits reveals all future bits)

Berlekamp-Massey Algorithm:
- Can find shortest LFSR for sequence
- Needs 2n consecutive bits
- Recovers feedback polynomial
```

**Breaking LFSR** (Example):
```
Known plaintext attack:

Given:
- Ciphertext: C = P ⊕ K (K is LFSR keystream)
- Plaintext: P (some portion)

Recover keystream:
K = C ⊕ P

If LFSR has n bits, knowing 2n consecutive keystream bits
allows recovery of entire sequence using Berlekamp-Massey.

Example:
LFSR: 4-bit (n=4)
Need: 8 bits of keystream
Result: Can predict all future keystream bits

Defense:
- Use multiple LFSRs (combine outputs)
- Non-linear combination function
- Larger LFSR (but still weak alone)
```

#### 1.2 RC4 Stream Cipher (Detailed)

**RC4 Algorithm**:
```
Designer: Ron Rivest (1987)
Key Size: 40-2048 bits (variable)
State: 256-byte array

Two phases:
1. Key Scheduling Algorithm (KSA)
2. Pseudo-Random Generation Algorithm (PRGA)
```

**KSA (Key Scheduling Algorithm)**:
```c
// Initialize S array
for i = 0 to 255:
    S[i] = i

// Permute S based on key
j = 0
for i = 0 to 255:
    j = (j + S[i] + Key[i mod keylen]) mod 256
    swap(S[i], S[j])
```

**PRGA (Pseudo-Random Generation Algorithm)**:
```c
i = 0
j = 0

// Generate keystream byte
while generating keystream:
    i = (i + 1) mod 256
    j = (j + S[i]) mod 256
    swap(S[i], S[j])
    t = (S[i] + S[j]) mod 256
    keystream_byte = S[t]
    output keystream_byte
```

**RC4 Example (Hand Calculation)**:
```
Key: "Key" = [75, 101, 121] (ASCII)
Simplified: Use 8-byte S array instead of 256 for example

Initial S array:
S = [0, 1, 2, 3, 4, 5, 6, 7]

KSA:
i=0: j=(0+0+75)%8=3,  swap(S[0],S[3]) → S=[3,1,2,0,4,5,6,7]
i=1: j=(3+1+101)%8=1, swap(S[1],S[1]) → S=[3,1,2,0,4,5,6,7]
i=2: j=(1+2+121)%8=4, swap(S[2],S[4]) → S=[3,1,4,0,2,5,6,7]
... (continue for all 8 positions)

PRGA (generate first byte):
i=1, j=(0+S[1])%8
t=(S[i]+S[j])%8
output S[t]

Encryption:
Plaintext byte ⊕ Keystream byte = Ciphertext byte
```

**RC4 Weaknesses**:
```
1. Biased Output:
   - First bytes of keystream not random
   - Patterns in first 256 bytes
   - Fix: Discard first 256-3072 bytes

2. Related Key Attacks:
   - Similar keys produce similar keystreams
   - WEP vulnerability (IV prepended to key)

3. Statistical Bias:
   - Position 256 has probability 2/256 for byte 0
   - Should be 1/256 (random)

4. Invariance Weakness:
   - Certain key-IV combinations produce weak keys

Real-World Failures:
- WEP (WiFi): Broken due to IV reuse + RC4 weaknesses
- WPA (TKIP): Improved but still uses RC4
- SSL/TLS: RC4 cipher suites deprecated (2015)

Status: ❌ NO LONGER SECURE
RFC 7465 (2015): Prohibit RC4 in TLS
```

#### 1.3 ChaCha20 (Modern Stream Cipher)

**ChaCha20 Overview**:
```
Designer: Daniel J. Bernstein (2008)
Based on: Salsa20
Key: 256 bits
Nonce: 96 bits (12 bytes)
Counter: 32 bits (4 bytes)
Output: 64 bytes per block

Advantages:
✓ Fast in software (no hardware acceleration needed)
✓ Constant-time (immune to timing attacks)
✓ No statistical weaknesses found
✓ Simple and secure
✓ Used in TLS 1.3, WireGuard, SSH

Security: Better than AES in software contexts
```

**ChaCha20 State**:
```
16 words (32 bits each) = 64 bytes total

Initial State Layout:
┌─────────────────────────────────────┐
│ "expa"  "nd 3"  "2-by"  "te k"      │ ← Constants (magic values)
├─────────────────────────────────────┤
│  Key₀    Key₁    Key₂    Key₃       │ ← 256-bit key (8 words)
│  Key₄    Key₅    Key₆    Key₇       │
├─────────────────────────────────────┤
│ Counter  Nonce₀  Nonce₁  Nonce₂     │ ← Counter (1) + Nonce (3)
└─────────────────────────────────────┘

Constants: ASCII for "expand 32-byte k"
0x61707865, 0x3320646e, 0x79622d32, 0x6b206574
```

**ChaCha20 Quarter Round**:
```
Quarter Round Function QR(a,b,c,d):
  a += b; d ^= a; d <<<= 16;  // <<< is rotate left
  c += d; b ^= c; b <<<= 12;
  a += b; d ^= a; d <<<= 8;
  c += d; b ^= c; b <<<= 7;

Operations: Addition, XOR, Rotation (ARX cipher)
20 rounds total (10 column rounds + 10 diagonal rounds)
```

**ChaCha20 Block Function**:
```python
def chacha20_block(key, counter, nonce):
    # Initialize state
    state = [
        0x61707865, 0x3320646e, 0x79622d32, 0x6b206574,  # Constants
        key[0], key[1], key[2], key[3],                  # Key (part 1)
        key[4], key[5], key[6], key[7],                  # Key (part 2)
        counter, nonce[0], nonce[1], nonce[2]            # Counter + Nonce
    ]
    
    working_state = state.copy()
    
    # 20 rounds (10 iterations of 2 rounds each)
    for i in range(10):
        # Column rounds
        QR(working_state, 0, 4, 8, 12)
        QR(working_state, 1, 5, 9, 13)
        QR(working_state, 2, 6, 10, 14)
        QR(working_state, 3, 7, 11, 15)
        
        # Diagonal rounds
        QR(working_state, 0, 5, 10, 15)
        QR(working_state, 1, 6, 11, 12)
        QR(working_state, 2, 7, 8, 13)
        QR(working_state, 3, 4, 9, 14)
    
    # Add original state (prevents reversibility)
    for i in range(16):
        working_state[i] = (working_state[i] + state[i]) & 0xFFFFFFFF
    
    return working_state  # 64 bytes of keystream
```

**ChaCha20 Encryption**:
```python
def chacha20_encrypt(key, nonce, plaintext):
    counter = 0
    ciphertext = []
    
    for i in range(0, len(plaintext), 64):
        # Generate 64 bytes of keystream
        keystream_block = chacha20_block(key, counter, nonce)
        
        # XOR with plaintext block
        block = plaintext[i:i+64]
        for j in range(len(block)):
            ciphertext.append(block[j] ^ keystream_block[j])
        
        counter += 1
    
    return ciphertext

# Usage
key = [random 256-bit key]
nonce = [random 96-bit nonce]  # MUST be unique for each message
plaintext = "Hello, World!"

ciphertext = chacha20_encrypt(key, nonce, plaintext)

# Decryption is identical (XOR is symmetric)
plaintext = chacha20_encrypt(key, nonce, ciphertext)
```

**ChaCha20-Poly1305 (AEAD)**:
```
Combination: ChaCha20 (encryption) + Poly1305 (authentication)

Purpose: Authenticated encryption with associated data

Structure:
1. Encrypt plaintext with ChaCha20
2. Compute Poly1305 MAC over:
   - Associated data (AD)
   - Ciphertext
   - Lengths of AD and ciphertext

Output:
- Ciphertext
- Authentication tag (16 bytes)

Verification:
1. Recompute MAC
2. Compare with received tag (constant-time)
3. If match, decrypt; else reject

Use Cases:
- TLS 1.3 (default cipher suite)
- SSH
- VPNs (WireGuard)
- Google's QUIC protocol

Advantages:
✓ Fast (especially on mobile)
✓ Secure
✓ Simple implementation
✓ No timing attacks
```

#### 1.4 One-Time Pad (Perfect Secrecy)

**One-Time Pad Theorem**:
```
Shannon's Theorem (1949):
OTP provides perfect secrecy if:
1. Key is truly random
2. Key length ≥ message length
3. Key used only once
4. Key kept completely secret

Perfect Secrecy:
P(M|C) = P(M) for all messages M and ciphertexts C

Meaning: Ciphertext reveals ZERO information about plaintext
```

**OTP Example**:
```
Plaintext:  "HELLO"
ASCII:      72  69  76  76  79

Random Key: 43  91  12  54  88  (truly random!)

XOR Encryption:
72 ⊕ 43 = 115  → 's'
69 ⊕ 91 = 26   → (non-printable)
76 ⊕ 12 = 84   → 'T'
76 ⊕ 54 = 122  → 'z'
79 ⊕ 88 = 23   → (non-printable)

Ciphertext: [115, 26, 84, 122, 23]

XOR Decryption (same operation):
115 ⊕ 43 = 72  → 'H'
26  ⊕ 91 = 69  → 'E'
84  ⊕ 12 = 76  → 'L'
122 ⊕ 54 = 76  → 'L'
23  ⊕ 88 = 79  → 'O'

Recovered: "HELLO" ✓
```

**Perfect Secrecy Proof**:
```
Given ciphertext C, all plaintexts are equally likely

Example:
Ciphertext: 115

Possible plaintexts (with appropriate keys):
Plaintext 'H' (72)  with key 43  → 72⊕43 = 115 ✓
Plaintext 'X' (88)  with key 27  → 88⊕27 = 115 ✓
Plaintext 'a' (97)  with key 18  → 97⊕18 = 115 ✓
... (256 possibilities)

Since key is random, each plaintext equally probable.
Attacker learns NOTHING from ciphertext alone.
```

**Why OTP is Impractical**:
```
1. Key Distribution Problem:
   - Need to securely share key as long as message
   - If you can securely share long key, why not share message?
   
   Example:
   Message: 1GB video
   Key needed: 1GB random data
   How to securely transfer 1GB key? (Same problem!)

2. Key Storage:
   - Must store massive keys securely
   - One-time use = constant new keys
   
   Example:
   Daily 100MB encrypted traffic = 36.5GB keys/year

3. Synchronization:
   - Sender and receiver must stay synchronized
   - Lost byte = all future decryption fails
   - No error recovery

4. Key Reuse Disaster:
   P₁ ⊕ K = C₁
   P₂ ⊕ K = C₂  (same key K reused!)
   
   Attack:
   C₁ ⊕ C₂ = (P₁ ⊕ K) ⊕ (P₂ ⊕ K) = P₁ ⊕ P₂
   
   Now attacker has P₁ ⊕ P₂ (no key involved!)
   Statistical analysis can recover both P₁ and P₂

5. No Authentication:
   - OTP provides confidentiality only
   - No integrity check
   - Attacker can flip bits without detection

Historical Use:
- Moscow-Washington hotline (Cold War)
- High-value diplomatic messages
- Spies with physical key books
```

**OTP vs Stream Ciphers**:
```
OTP:
Key: Truly random, same length as message
Security: Perfect (information-theoretic)
Practical: No (key distribution impossible)

Stream Cipher (e.g., ChaCha20):
Key: Short (256 bits), expands to keystream
Security: Computational (depends on algorithm strength)
Practical: Yes (key distribution feasible)

Stream Cipher ≈ "Practical OTP"
- Generates pseudorandom keystream from short key
- Security not perfect but sufficient in practice
```

---

### 2. Advanced Cryptanalysis Techniques

#### 2.1 Differential Cryptanalysis

**Concept**:
```
Analyze how differences in input affect differences in output

For block cipher E with key K:
Let ΔP = P₁ ⊕ P₂ (input difference)
Let ΔC = C₁ ⊕ C₂ (output difference)

where C₁ = E(K, P₁) and C₂ = E(K, P₂)

Goal: Find input difference ΔP that produces predictable output difference ΔC
with probability significantly different from random (1/2ⁿ for n-bit output)

If such pattern exists, it's a "differential characteristic"
```

**Differential Cryptanalysis Steps**:
```
Step 1: Find Differential Characteristic
- Analyze S-boxes for high-probability differentials
- Example: ΔP = 0x0040 → ΔC = 0x4000 with probability 1/4

Step 2: Collect Chosen Plaintexts
- Generate many pairs (P, P⊕ΔP)
- Encrypt all plaintexts
- Get ciphertext pairs (C, C')

Step 3: Analyze Output Differences
- Compute ΔC = C ⊕ C'
- Count frequency of each ΔC
- If characteristic holds, one ΔC appears more often

Step 4: Key Recovery
- Work backwards from output difference
- Guess partial keys
- Eliminate impossible keys
- Repeat for different characteristics
```

**Example: Simplified DES (S-DES)**:
```
S-Box differential:
Input diff 0x01 → Output diff 0x02 with probability 6/16

Attack:
1. Choose 100 plaintext pairs with ΔP = 0x01
2. Encrypt all plaintexts
3. Count output differences
4. If ΔC = 0x02 appears ~38 times (6/16 × 100),
   characteristic confirmed
5. Use this to reduce key space

Full DES:
- Requires 2⁴⁷ chosen plaintexts
- Better than brute force (2⁵⁶)
- Not practical but demonstrates weakness
```

**Defense Against Differential Cryptanalysis**:
```
1. S-Box Design:
   - Minimize maximum differential probability
   - DES S-boxes: max probability ≈ 1/4
   - AES S-box: max probability = 2⁻⁶ (very low)

2. More Rounds:
   - Each round diffuses differences
   - More rounds = exponentially harder attack
   - DES: 16 rounds (differential attack needs >16)
   - AES: 10/12/14 rounds (safe margin)

3. Wide Trail Strategy (AES):
   - Guarantees minimum number of active S-boxes
   - Each round guarantees difference spread
   - Combined probability becomes negligible

4. Avoid Weak S-Boxes:
   - Test all possible input/output differences
   - Reject S-boxes with high-probability differentials
```

#### 2.2 Linear Cryptanalysis

**Concept**:
```
Find linear approximations that hold with probability ≠ 1/2

Linear Approximation:
P[i₁,i₂,...] ⊕ C[j₁,j₂,...] = K[k₁,k₂,...]

Where:
- P[i] = bit i of plaintext
- C[j] = bit j of ciphertext  
- K[k] = bit k of key
- ⊕ = XOR operation

Goal: Find approximation that holds with probability p ≠ 0.5
If p = 0.5, approximation is useless (random)
If p ≠ 0.5, can be exploited
```

**Linear Cryptanalysis Process**:
```
Step 1: Find Linear Approximations for S-boxes
Example S-box approximation:
X[1] ⊕ X[3] = Y[0] ⊕ Y[2] with probability 0.75

Step 2: Chain Approximations Across Rounds
Combine multiple S-box approximations
Use piling-up lemma to calculate overall probability

Piling-up Lemma:
If X₁ has bias ε₁, X₂ has bias ε₂, ...
Then X₁ ⊕ X₂ ⊕ ... has bias 2ⁿ⁻¹ · ε₁ · ε₂ · ...

Bias = |probability - 0.5|

Step 3: Collect Known Plaintexts
Need approximately (1/ε²) known plaintext-ciphertext pairs
where ε = bias of linear approximation

Step 4: Count Matches
For each known pair, check if approximation holds
Count how many times it's true vs false

Step 5: Key Recovery
If count significantly deviates from 50%, 
approximation is correct and reveals key bits
Repeat for different approximations to get full key
```

**DES Linear Cryptanalysis Example**:
```
Best known linear approximation for DES:
Probability ≈ 0.5 + 2⁻²¹

Bias ε = 2⁻²¹

Data Required:
N ≈ 1/ε² = 1/(2⁻²¹)² = 2⁴² plaintexts

Result:
- Requires 2⁴³ known plaintexts
- Faster than brute force (2⁵⁶)
- First practical attack on DES (1993)

Time Complexity:
- Data collection: 2⁴³ encryptions
- Analysis: 2⁴³ operations
- Total: ~2⁴³ work
- vs Brute force: 2⁵⁶ work

Improvement: 2¹³ times faster than brute force
```

**Defense Against Linear Cryptanalysis**:
```
1. S-Box Non-linearity:
   - Design S-boxes with low linear probability
   - AES S-box: maximum linear probability = 2⁻³
   - Very low correlation

2. Multiple Rounds:
   - Each round reduces linear probability
   - Combined probability becomes negligible

3. Diffusion:
   - Spread influence of plaintext bits
   - Makes chaining approximations harder

4. AES Design:
   - Wide Trail Strategy guarantees minimum active S-boxes
   - Provable bounds on linear/differential attacks
```

#### 2.3 Meet-in-the-Middle Attack

**Concept**:
```
Time-Memory Trade-off

Instead of trying all possible keys sequentially,
split the search space and meet in the middle.

Applies to:
- Multiple encryption
- Hash function attacks
- Some public key systems
```

**Classic Example: Double DES**:
```
Double DES:
C = E(K₂, E(K₁, P))

Naive Attack (Brute Force):
Try all (K₁, K₂) pairs
Key space: 56 + 56 = 112 bits
Work: 2¹¹² operations

Meet-in-the-Middle Attack:

Step 1: Compute Forward
For all possible K₁:
    X = E(K₁, P)
    Store (X, K₁) in table
    
Operations: 2⁵⁶
Memory: 2⁵⁶ entries

Step 2: Compute Backward
For all possible K₂:
    X' = D(K₂, C)
    Check if X' in table
    If match found: Candidate (K₁, K₂)
    
Operations: 2⁵⁶
Memory: 2⁵⁶ (from step 1)

Step 3: Verify
Test candidate on another plaintext-ciphertext pair
Eliminates false positives

Total:
Time: 2⁵⁶ + 2⁵⁶ ≈ 2⁵⁷
Memory: 2⁵⁶
vs Brute Force: 2¹¹² time, 1 memory

Reduction: 2⁵⁵ times faster!
```

**Why Triple DES (3DES)**:
```
Because Double DES vulnerable to meet-in-the-middle!

3DES: C = E(K₃, D(K₂, E(K₁, P)))

Even with meet-in-the-middle:

Two-Key 3DES (K₁ = K₃):
Effective security: ~112 bits
(not full 112 due to advanced attacks, but sufficient)

Three-Key 3DES:
Key space: 168 bits
Meet-in-the-middle: still ~112 bit security
(limited by birthday bound in advanced scenarios)

Conclusion: 3DES safe from meet-in-the-middle
```

**Meet-in-the-Middle on Hash Functions**:
```
Find collision in hash function H

Birthday Attack (Basic):
Compute √(2ⁿ) hashes
Store all values
Find collision when match occurs

Meet-in-the-Middle (Optimization):
Split into two halves
Generate √(2ⁿ/²) values in each half
Total: 2·√(2ⁿ/²) = 2·2ⁿ/⁴ operations

Example:
Hash: 128-bit
Birthday: 2⁶⁴ operations, 2⁶⁴ memory
Meet-in-middle: 2·2³² = 2³³ operations
(but requires more complex algorithm)
```

#### 2.4 Birthday Attack (Detailed)

**Birthday Paradox**:
```
Question: How many people needed for 50% probability 
that two share a birthday?

Intuition: 365/2 ≈ 183?
Reality: 23 people!

Calculation:
P(no collision) = 365/365 · 364/365 · 363/365 · ... · (365-n+1)/365

For n=23: P(no collision) ≈ 0.493
Therefore: P(collision) ≈ 0.507 (>50%)

Approximation:
P(collision) ≈ 1 - e^(-n²/2m)

where m = total possibilities (365 days)
      n = number of samples

For 50% collision:
1 - e^(-n²/2m) ≈ 0.5
e^(-n²/2m) ≈ 0.5
-n²/2m ≈ ln(0.5) ≈ -0.693
n² ≈ 1.386m
n ≈ 1.177√m

For birthdays: n ≈ 1.177√365 ≈ 22.5 ≈ 23
```

**Application to Cryptography**:
```
Hash Function with n-bit output:
Total possible hashes: m = 2ⁿ

Collisions after:
n = 1.177 · √(2ⁿ) = 1.177 · 2^(n/2) ≈ 2^(n/2)

Examples:
- 64-bit hash: 2³² ≈ 4 billion hashes
- 128-bit hash: 2⁶⁴ ≈ 18 quintillion hashes
- 256-bit hash: 2¹²⁸ ≈ 10³⁸ hashes (infeasible!)

This is why:
❌ MD5 (128-bit): Collisions found (2⁶⁴ work)
❌ SHA-1 (160-bit): Collisions found (2⁸⁰ work, reduced to 2⁶³)
✓ SHA-256: Safe (2¹²⁸ work, computationally infeasible)
```

**Birthday Attack Example**:
```
Goal: Create two contracts with same hash

Step 1: Create variations of legitimate contract A
- Add/remove spaces
- Rephrase sentences (same meaning)
- Generate 2^(n/2) variations
- Store hash of each

Step 2: Create variations of fraudulent contract B
- Similar modifications
- Generate 2^(n/2) variations
- Compute hash of each

Step 3: Find match
- Compare hashes from step 2 with stored hashes from step 1
- Birthday paradox: high probability of collision

Step 4: Execute attack
- Get signature on legitimate variation A
- Claim signature applies to fraudulent variation B
- Both have same hash!

For 64-bit hash:
- Need ~2³² variations of each contract
- Feasible with computer
- This is why 64-bit hashes are insecure!

For 256-bit hash:
- Need ~2¹²⁸ variations
- More than atoms in universe
- Infeasible!
```

**Collision Attack vs Pre-image Attack**:
```
Collision Attack (Birthday):
Find any two messages M₁ ≠ M₂ where H(M₁) = H(M₂)
Difficulty: 2^(n/2) operations

Pre-image Attack:
Given hash h, find message M where H(M) = h
Difficulty: 2ⁿ operations

Second Pre-image Attack:
Given M₁, find M₂ ≠ M₁ where H(M₁) = H(M₂)
Difficulty: 2ⁿ operations

Comparison (for 256-bit hash):
Collision: 2¹²⁸ ops (hard but why we need 256 bits)
Pre-image: 2²⁵⁶ ops (essentially impossible)
Second pre-image: 2²⁵⁶ ops (essentially impossible)

Signature Requirement:
Must resist collision attacks (2^(n/2))
Therefore, need 2n-bit hash for n-bit security level

Example:
For 128-bit security: Use SHA-256 (or better)
For 192-bit security: Use SHA-384
For 256-bit security: Use SHA-512
```

---

## PART 2: AUTHENTICATION PROTOCOLS

### 3. Kerberos Authentication Protocol (Complete)

#### 3.1 Kerberos Architecture

**Kerberos v5 Components**:
```
1. Key Distribution Center (KDC):
   a) Authentication Server (AS)
      - Verifies user identity
      - Issues Ticket Granting Ticket (TGT)
   
   b) Ticket Granting Server (TGS)
      - Issues service tickets
      - Uses TGT for authentication

2. Principals:
   - Users (alice@REALM.COM)
   - Services (HTTP/web.example.com@REALM.COM)

3. Realm:
   - Administrative domain
   - Example: COMPANY.COM

4. Service Servers:
   - Provide actual services
   - Verify service tickets

5. Database:
   - Stores principal keys
   - User passwords (hashed)
   - Service keys
```

**Kerberos Ticket Structure**:
```
Ticket:
{
  Version: 5
  Realm: COMPANY.COM
  Service Principal: HTTP/web.example.com@COMPANY.COM
  Client Principal: alice@COMPANY.COM
  Session Key: <random key>
  Timestamp: 2025-11-13T09:00:00Z
  Lifetime: 8 hours (28800 seconds)
  Flags: forwardable, renewable
  Authorization Data: <optional>
}

Encrypted with: Service's secret key
Cannot be read by client, only by service

Authenticator:
{
  Client Principal: alice@COMPANY.COM
  Timestamp: 2025-11-13T09:00:03Z
  Microseconds: 123456
  Sequence Number: 12345  (optional, for replay detection)
}

Encrypted with: Session key
Fresh proof that client possesses session key
```

#### 3.2 Kerberos Protocol Flow (Detailed)

**Phase 1: Initial Authentication (AS Exchange)**:
```
Client → AS:
{
  Message Type: AS-REQ
  Client Principal: alice@COMPANY.COM
  Service Principal: krbtgt/COMPANY.COM@COMPANY.COM  (TGS)
  Requested Lifetime: 10 hours
  Nonce: <random value>
  Supported Encryption: AES256, AES128, DES3
  Pre-authentication Data: {Timestamp} encrypted with user's key
}

Pre-authentication prevents offline password guessing

AS → Client:
{
  Message Type: AS-REP
  
  TGT (Ticket Granting Ticket):
  {
    Service: krbtgt/COMPANY.COM
    Client: alice@COMPANY.COM
    Session Key: K_TGS
    Expiration: 10 hours from now
    Flags: renewable, forwardable
  } encrypted with TGS's secret key
  
  Session Info:
  {
    Session Key: K_TGS  (same as in TGT)
    Expiration: 10 hours from now
    Nonce: <same as request>
    Service: krbtgt/COMPANY.COM
  } encrypted with alice's password hash
}

Client:
1. Decrypts Session Info with password
2. Extracts K_TGS (TGS session key)
3. Stores TGT (can't decrypt it, but will present it)
4. Forgets password (only needed for this step)
```

**Phase 2: Service Ticket Request (TGS Exchange)**:
```
Client → TGS:
{
  Message Type: TGS-REQ
  
  TGT from Phase 1 (encrypted with TGS key)
  
  Authenticator:
  {
    Client: alice@COMPANY.COM
    Timestamp: <current time>
  } encrypted with K_TGS
  
  Requested Service: HTTP/web.example.com@COMPANY.COM
  Requested Lifetime: 8 hours
  Nonce: <new random value>
}

TGS:
1. Decrypts TGT with its secret key
2. Extracts K_TGS from TGT
3. Decrypts Authenticator with K_TGS
4. Verifies:
   - Client name in TGT matches Authenticator
   - Timestamp is recent (within 5 minutes)
   - TGT not expired
   - Authenticator not replayed (checks timestamp cache)

TGS → Client:
{
  Message Type: TGS-REP
  
  Service Ticket:
  {
    Service: HTTP/web.example.com
    Client: alice@COMPANY.COM
    Session Key: K_Service
    Expiration: 8 hours from now
    Authorization Data: group memberships, etc.
  } encrypted with service's secret key
  
  Session Info:
  {
    Session Key: K_Service
    Expiration: 8 hours from now
    Nonce: <same as request>
    Service: HTTP/web.example.com
  } encrypted with K_TGS
}

Client:
1. Decrypts Session Info with K_TGS
2. Extracts K_Service
3. Stores Service Ticket
```

**Phase 3: Service Access (AP Exchange)**:
```
Client → Service:
{
  Message Type: AP-REQ
  
  Service Ticket from Phase 2 (encrypted with service key)
  
  Authenticator:
  {
    Client: alice@COMPANY.COM
    Timestamp: <current time>
    Checksum: <hash of application data>
  } encrypted with K_Service
  
  Options: mutual-authentication-required
}

Service:
1. Decrypts Service Ticket with its secret key
2. Extracts K_Service
3. Decrypts Authenticator with K_Service
4. Verifies:
   - Client name matches
   - Timestamp recent
   - Ticket not expired
   - Checksum (if present) matches data

Service → Client (if mutual authentication requested):
{
  Message Type: AP-REP
  
  {
    Timestamp: <from client's authenticator>
    Microseconds: <from client's authenticator>
    Sequence Number: <optional>
  } encrypted with K_Service
}

Client:
1. Decrypts response with K_Service
2. Verifies timestamp matches what was sent
3. Confirms server is authentic

Now: Secure channel established using K_Service
```

#### 3.3 Kerberos Security Features

**Replay Attack Prevention**:
```
1. Timestamp Windows:
   - Authenticator timestamp must be within 5 minutes
   - Requires synchronized clocks (NTP)
   - Expired authenticators rejected

2. Timestamp Cache:
   - Server stores recent authenticator timestamps
   - Rejects duplicate timestamps within window
   - Cache flushed after 5 minutes (window size)

3. Sequence Numbers (optional):
   - Incrementing counter in each message
   - Detect out-of-order or replayed messages

4. Ticket Lifetimes:
   - TGT: typically 10 hours
   - Service tickets: typically 8 hours
   - Renewable for longer periods
```

**Cross-Realm Authentication**:
```
Two realms: COMPANY-A.COM and COMPANY-B.COM

Setup: Establish trust
- Create cross-realm principal: krbtgt/COMPANY-B.COM@COMPANY-A.COM
- Share secret key between realms

User alice@COMPANY-A.COM accessing service@COMPANY-B.COM:

Step 1: Get TGT from COMPANY-A.COM (normal)

Step 2: Get cross-realm TGT
Client → TGS-A: Request ticket for krbtgt/COMPANY-B.COM@COMPANY-A.COM
TGS-A → Client: Cross-realm TGT encrypted with shared secret

Step 3: Get service ticket from COMPANY-B.COM
Client → TGS-B: Present cross-realm TGT
TGS-B → Client: Service ticket for service@COMPANY-B.COM

Step 4: Access service (normal)

Transitive Trust:
A ← → B ← → C
User from A can access C through B
Ticket path: TGT-A → Cross-TGT-AB → Cross-TGT-BC → Service-C
```

**Kerberos Delegation**:
```
Scenario: Client → Web Server → Database Server
Problem: Web server needs to access database on behalf of client

Solutions:

1. Proxy (Non-Forwardable) Ticket:
   - Web server gets proxy ticket for database
   - Ticket marked as proxy
   - Limited delegation scope

2. Forwardable Ticket:
   - Client's TGT marked "forwardable"
   - Web server can get new tickets as if it were client
   - Full delegation (more risk)

3. Constrained Delegation (S4U2Self, S4U2Proxy):
   - Web server can only get tickets for specific services
   - Configured in KDC
   - Safer than forwardable

Flags in ticket:
- forwardable: Can be forwarded to another service
- forwarded: This ticket was forwarded
- proxiable: Can be used to get proxy ticket
- proxy: This is a proxy ticket
```

#### 3.4 Kerberos Weaknesses and Mitigations

**Known Attacks**:
```
1. Password Guessing (Offline):
   Attack: Capture AS-REP, try passwords offline
   Mitigation: 
   - Pre-authentication (encrypt timestamp with password)
   - Strong password policy
   - Account lockout after failures

2. Golden Ticket:
   Attack: If krbtgt key compromised, forge any ticket
   Impact: Complete domain compromise
   Mitigation:
   - Protect KDC (isolate, harden)
   - Regular krbtgt password rotation
   - Monitor for anomalous tickets

3. Silver Ticket:
   Attack: If service key compromised, forge tickets for that service
   Impact: Access to specific service
   Mitigation:
   - Regular service password rotation
   - Monitor service access patterns

4. Pass-the-Ticket:
   Attack: Steal ticket from memory, reuse
   Impact: Impersonate user until ticket expires
   Mitigation:
   - Short ticket lifetimes
   - Secure client systems
   - Memory protection (Credential Guard on Windows)

5. Kerberoasting:
   Attack: Request service tickets, crack offline
   Target: Services with weak passwords
   Mitigation:
   - Strong service account passwords (30+ random characters)
   - Managed service accounts
   - Monitor TGS-REQ for unusual patterns
```

**Kerberos Best Practices**:
```
1. Time Synchronization:
   - Use NTP on all systems
   - Max 5-minute drift
   - Monitor clock skew

2. Encryption Types:
   - Use AES256 or AES128
   - Disable DES, RC4 (weak)
   - Upgrade all clients/servers

3. Password Policy:
   - Minimum 14 characters for users
   - 30+ characters for services
   - Regular rotation (90 days users, 180 days services)

4. Monitoring:
   - Log all authentications
   - Alert on:
     - Failed pre-auth
     - Ticket requests for disabled accounts
     - Unusual service ticket requests
     - Golden/silver ticket indicators

5. Hardening:
   - Isolate KDC (separate VLAN)
   - No internet access for KDC
   - Minimal services on KDC
   - Regular security patches

6. krbtgt Rotation:
   - Rotate at least yearly
   - After any potential compromise
   - Two-step process (compatibility)
```

---

## PART 3: PKI & DIGITAL CERTIFICATES (Extended)

### 7. X.509 Digital Certificates (Complete Details)

#### 7.1 Certificate Structure (In-Depth)

**Complete X.509 v3 Certificate**:
```
Certificate:
    Data:
        Version: 3 (0x2)
        Serial Number: 
            04:9a:2f:87:d6:e5:4b:92:3f:c1
        Signature Algorithm: sha256WithRSAEncryption
        
        Issuer:
            Country Name (C): US
            Organization Name (O): DigiCert Inc
            Organizational Unit Name (OU): www.digicert.com
            Common Name (CN): DigiCert SHA2 Secure Server CA
        
        Validity:
            Not Before: Nov  1 00:00:00 2024 GMT
            Not After : Dec  1 12:00:00 2025 GMT
        
        Subject:
            Country Name (C): US
            State or Province Name (ST): California
            Locality Name (L): San Francisco
            Organization Name (O): Example Corp
            Common Name (CN): www.example.com
        
        Subject Public Key Info:
            Public Key Algorithm: rsaEncryption
                Public-Key: (2048 bit)
                Modulus:
                    00:b5:d9:7c:e5:7b:64:3f:9a:8e:4f:12:3d:a7:84:
                    ...  (256 bytes for 2048-bit key)
                Exponent: 65537 (0x10001)
        
        X509v3 extensions:
            X509v3 Key Usage: critical
                Digital Signature, Key Encipherment
            
            X509v3 Extended Key Usage:
                TLS Web Server Authentication, TLS Web Client Authentication
            
            X509v3 Basic Constraints: critical
                CA:FALSE
            
            X509v3 Subject Key Identifier:
                A4:6B:0F:A6:3C:4D:89:56:17:9E:81:F5:B3:29:D7:3A:45:8F:12:34
            
            X509v3 Authority Key Identifier:
                keyid:0F:80:61:1C:82:31:61:D5:2F:28:E7:8D:46:38:B4:2C:E1:C6:D9:E2
            
            Authority Information Access:
                OCSP - URI:http://ocsp.digicert.com
                CA Issuers - URI:http://cacerts.digicert.com/DigiCertSHA2SecureServerCA.crt
            
            X509v3 Subject Alternative Name:
                DNS:www.example.com
                DNS:example.com
                DNS:*.example.com
                DNS:mail.example.com
            
            X509v3 Certificate Policies:
                Policy: 2.16.840.1.114412.1.1
                    CPS: https://www.digicert.com/CPS
            
            X509v3 CRL Distribution Points:
                Full Name:
                  URI:http://crl3.digicert.com/sha2-ha-server-g6.crl
                Full Name:
                  URI:http://crl4.digicert.com/sha2-ha-server-g6.crl
            
            CT Precertificate SCTs:
                Signed Certificate Timestamp:
                    Version   : v1 (0x0)
                    Log ID    : pLkJ...
                    Timestamp : Nov  1 10:00:00.123 2024 GMT
                    Extensions: none
                    Signature : ecdsa-with-SHA256
                                30:45:...
    
    Signature Algorithm: sha256WithRSAEncryption
         6f:85:3a:28:6f:47:94:a4:ce:69:f3:b6:85:82:e8:99:a5:91:
         ...  (256 bytes)
```

#### 7.2 Certificate Extensions (All Important Ones)

**Key Usage Extension** (Critical):
```
Purpose: Restrict what certificate can be used for

Bit Flags:
0: digitalSignature  - Create digital signatures
1: nonRepudiation    - Content commitment (can't deny)
2: keyEncipherment   - Encrypt keys (RSA key transport)
3: dataEncipherment  - Encrypt data directly (rare)
4: keyAgreement      - Key agreement (Diffie-Hellman)
5: keyCertSign       - Sign certificates (CA only)
6: cRLSign           - Sign CRLs (CA only)
7: encipherOnly      - Only encrypt (with keyAgreement)
8: decipherOnly      - Only decrypt (with keyAgreement)

Examples:
Web Server: digitalSignature, keyEncipherment
Email (S/MIME): digitalSignature, keyEncipherment, dataEncipherment
CA: keyCertSign, cRLSign
Code Signing: digitalSignature, nonRepudiation

ASN.1 Encoding:
KeyUsage ::= BIT STRING {
    digitalSignature        (0),
    nonRepudiation          (1),
    keyEncipherment         (2),
    ...
}

Example (Web Server):
Key Usage: Digital Signature, Key Encipherment
Binary: 1010 0000 0000 (bits 0 and 2 set)
```

**Extended Key Usage (EKU)**:
```
More specific purposes (Object Identifiers)

Common EKUs:
1.3.6.1.5.5.7.3.1  - serverAuth (TLS server)
1.3.6.1.5.5.7.3.2  - clientAuth (TLS client)
1.3.6.1.5.5.7.3.3  - codeSigning
1.3.6.1.5.5.7.3.4  - emailProtection (S/MIME)
1.3.6.1.5.5.7.3.8  - timeStamping
1.3.6.1.5.5.7.3.9  - OCSPSigning

Usage:
- Web Server: Must have serverAuth
- Email: Must have emailProtection
- Code: Must have codeSigning

Certificate can have multiple EKUs
Browser/OS checks appropriate EKU for use case

Example:
Extended Key Usage:
    TLS Web Server Authentication
    TLS Web Client Authentication
```

**Subject Alternative Name (SAN)** (Critical for modern certs):
```
Additional identities for the certificate

Types:
- dNSName: Domain names
- iPAddress: IP addresses
- uniformResourceIdentifier: URLs
- rfc822Name: Email addresses
- directoryName: X.500 directory name
- otherName: Other naming schemes

Example:
X509v3 Subject Alternative Name:
    DNS:www.example.com
    DNS:example.com
    DNS:*.example.com
    DNS:api.example.com
    DNS:cdn.example.com
    IP Address:192.0.2.1
    IP Address:2001:DB8::1

Wildcard Certificates:
*.example.com matches:
    www.example.com     ✓
    api.example.com     ✓
    shop.example.com    ✓
    example.com         ✗ (doesn't match naked domain)
    sub.api.example.com ✗ (only one level)

Modern Requirement:
Common Name (CN) deprecated for domain validation
Must use SAN (even if same as CN)
Browsers check SAN, ignore CN
```

**Basic Constraints** (Critical for CAs):
```
Indicates if certificate is CA or end-entity

Fields:
- cA: TRUE or FALSE
- pathLenConstraint: Maximum intermediate CAs allowed

Examples:

Root CA:
X509v3 Basic Constraints: critical
    CA:TRUE

Intermediate CA:
X509v3 Basic Constraints: critical
    CA:TRUE, pathlen:0

End-Entity (Server/Client):
X509v3 Basic Constraints: critical
    CA:FALSE

Path Length Constraint:
Root (pathlen:2) → Intermediate1 (pathlen:1) → Intermediate2 (pathlen:0) → End-entity ✓
Root (pathlen:1) → Intermediate1 → Intermediate2 → End-entity ✗ (exceeds root's pathlen)

Purpose:
- Prevents end-entity certs from issuing other certs
- Limits depth of CA hierarchy
- Critical for PKI security
```

**Authority Key Identifier (AKI)**:
```
Identifies which CA key signed this certificate

Usually: SHA-1 hash of CA's public key

Purpose:
- CAs may have multiple keys
- Helps select correct key for verification
- Links certificate to issuer

Example:
X509v3 Authority Key Identifier:
    keyid:0F:80:61:1C:82:31:61:D5:2F:28:E7:8D:46:38:B4:2C:E1:C6:D9:E2

Matches Subject Key Identifier in CA's certificate
```

**Subject Key Identifier (SKI)**:
```
Unique identifier for certificate's public key

Usually: SHA-1 hash of public key (excluding algorithm identifier)

Purpose:
- Uniquely identify key
- Referenced by Authority Key Identifier
- Useful when certificate renewed with same key

Example:
X509v3 Subject Key Identifier:
    A4:6B:0F:A6:3C:4D:89:56:17:9E:81:F5:B3:29:D7:3A:45:8F:12:34
```

**CRL Distribution Points**:
```
Where to download Certificate Revocation List

Can have multiple URIs (redundancy)

Example:
X509v3 CRL Distribution Points:
    Full Name:
      URI:http://crl.example.com/ca.crl
    Full Name:
      URI:http://crl-backup.example.com/ca.crl
      URI:ldap://ldap.example.com/cn=CA,ou=PKI,o=Example?certificateRevocationList

Partitioned CRLs:
Reasons:
  URI:http://crl.example.com/ca-keyCompromise.crl
  Reasons: keyCompromise
  
  URI:http://crl.example.com/ca-other.crl
  Reasons: (all other reasons)

Reduces CRL download size
```

**Authority Information Access (AIA)**:
```
Pointers to CA certificate and OCSP

Fields:
- CA Issuers: Where to get CA certificate
- OCSP: Where to check revocation online

Example:
Authority Information Access:
    OCSP - URI:http://ocsp.example.com
    OCSP - URI:http://ocsp-backup.example.com
    CA Issuers - URI:http://certs.example.com/ca.crt
    CA Issuers - URI:ldap://ldap.example.com/cn=CA?cACertificate

Use:
- Browsers automatically fetch CA cert if needed
- OCSP check for revocation status
```

**Certificate Policies**:
```
Legal/business policies governing certificate

Example:
X509v3 Certificate Policies:
    Policy: 2.23.140.1.2.2
      CPS: https://www.example.com/cps
      User Notice:
        Explicit Text: This certificate is for testing only

Common OIDs:
2.23.140.1.2.1  - DV (Domain Validation)
2.23.140.1.2.2  - OV (Organization Validation)
2.23.140.1.1    - EV (Extended Validation)

CPS (Certification Practice Statement):
Legal document describing CA's practices
```

**Subject Directory Attributes** (Rare):
```
Additional attributes about subject

Example:
X509v3 Subject Directory Attributes:
    Date of Birth: 19900115000000Z
    Place of Birth: New York, USA
    Gender: M
    Country of Citizenship: US

Rarely used in public PKI
More common in government/national ID certificates
```

**Name Constraints** (CA certificates only):
```
Restrict which names CA can issue certificates for

Permitted Subtrees:
X509v3 Name Constraints:
    Permitted:
      DNS:.example.com
      IP:192.0.2.0/255.255.255.0

Excluded Subtrees:
X509v3 Name Constraints:
    Excluded:
      DNS:.internal.example.com

Use:
- Limits damage if intermediate CA compromised
- Enterprise CAs with specific scope
```

**Policy Constraints** (Advanced):
```
Controls policy processing in certification path

Fields:
- requireExplicitPolicy: Depth where policy required
- inhibitPolicyMapping: Depth where mapping prohibited

Rarely used
Complex certificate path validation
```

#### 7.3 Certificate Validation (Complete Algorithm)

**RFC 5280 Path Validation Algorithm**:
```
Input:
- Certificate to validate
- Current date/time
- Initial policy set
- Trust anchor (root CA)

Steps:

1. Basic Certificate Processing:
   FOR each certificate in path (leaf to root):
   
   a) Verify signature:
      - Extract signature from certificate
      - Get issuer's public key (from next cert in chain)
      - Verify: signature = decrypt(issuer_public_key, signature_bytes)
      - Compute hash of certificate TBS (To-Be-Signed) portion
      - Compare: hash == decrypted_signature
      - If NO match → INVALID (signature verification failed)
   
   b) Check validity period:
      - current_time >= notBefore?
      - current_time <= notAfter?
      - If outside range → INVALID (expired or not yet valid)
   
   c) Check revocation:
      - If CRL: Download CRL, check serial number
      - If OCSP: Query OCSP responder
      - If revoked → INVALID (certificate revoked)
      - If CRL expired: Depends on policy (hard-fail vs soft-fail)
   
   d) Verify issuer matches subject of next certificate:
      - cert[n].issuer == cert[n+1].subject
      - If NO match → INVALID (broken chain)

2. Name and Key Checking:
   
   a) Name constraints:
      - If name constraints exist in CA cert
      - Check end-entity name falls within permitted subtrees
      - Check end-entity name not in excluded subtrees
      - If violated → INVALID
   
   b) Key usage:
      - CA certificates: must have keyCertSign
      - End-entity: check appropriate for use (serverAuth, etc.)
      - If wrong usage → INVALID

3. Path Length Processing:
   
   a) Basic Constraints:
      - End-entity: cA must be FALSE
      - Intermediates: cA must be TRUE
      - If wrong → INVALID
   
   b) Path length:
      - Count intermediate CAs
      - Each CA's pathLenConstraint must be ≥ remaining intermediates
      - If exceeded → INVALID
   
   Example:
   Root (pathlen:2) OK
   ↓
   Int1 (pathlen:1) OK (1 more intermediate allowed)
   ↓
   Int2 (pathlen:0) OK (0 more intermediates allowed)
   ↓
   End-entity (no pathlen) OK (not a CA)
   
   Total intermediate count: 2
   Root allows 2 → VALID

4. Policy Processing (if required):
   
   a) Build policy tree
   b) Prune invalid policies
   c) Check if required policy present
   d) If policy required but not present → INVALID

5. End-Entity Specific Checks:
   
   a) Name checking:
      - For TLS: Check DNS name against CN or SAN
      - Wildcards: *.example.com
      - IDN (Internationalized Domain Names): Check punycode
      - If mismatch → INVALID
   
   b) Extended Key Usage:
      - serverAuth for TLS servers
      - clientAuth for TLS clients
      - codeSigning for code
      - If wrong EKU → INVALID

6. Final Decision:
   
   - If all checks passed → VALID
   - If any check failed → INVALID
   - Return validation result and path
```

**Example Validation (Step-by-Step)**:
```
Certificate Chain:
[0] www.example.com (end-entity)
[1] Example Intermediate CA
[2] Example Root CA (in trust store)

Validating www.example.com:

Step 1a: Verify signature of [0] with public key from [1]
  SHA-256 hash of [0].TBS: a4b3c2d1...
  Decrypt [0].signature with [1].publicKey: a4b3c2d1...
  Match! ✓

Step 1b: Check validity of [0]
  Current time: 2025-11-13 09:00:03
  Not Before: 2024-11-01 00:00:00 ✓
  Not After:  2025-12-01 12:00:00 ✓
  Valid! ✓

Step 1c: Check revocation of [0]
  OCSP URL: http://ocsp.example.com
  Query: Serial 04:9a:2f:87:d6:e5:4b:92:3f:c1
  Response: good ✓

Step 1d: Verify issuer chain