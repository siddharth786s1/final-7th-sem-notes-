# Cryptography and Information Security - Additional Complete Topics
## Remaining and Advanced Concepts for Exam Preparation

**Author**: Complete Study Material
**Date**: 2025-11-13
**Student**: siddharth786s1

---

## TABLE OF CONTENTS

1. Stream Ciphers
2. Advanced Cryptanalysis
3. Kerberos Authentication Protocol
4. Digital Certificates (X.509) in Detail
5. Certificate Authority and PKI Operations
6. VPN Technologies
7. Authentication Protocols (OAuth, SAML, OpenID)
8. Multi-Factor Authentication
9. Blockchain and Cryptography
10. Post-Quantum Cryptography
11. Secure Coding Practices
12. Cryptographic Protocols (Detailed)
13. Advanced Network Security
14. Mobile Security
15. Cloud Security
16. Additional Practice Problems

---

## UNIT I (EXTENDED): STREAM CIPHERS

### 1. Stream Ciphers Fundamentals

**Definition**: Stream ciphers encrypt plaintext one bit or byte at a time using a pseudorandom keystream.

**Block Cipher vs Stream Cipher**:

| Aspect | Block Cipher | Stream Cipher |
|--------|--------------|---------------|
| **Encryption Unit** | Fixed blocks (64/128 bits) | Bit or byte |
| **Speed** | Slower | Faster |
| **Memory** | More (needs buffering) | Less |
| **Error Propagation** | High (entire block) | Low (single bit) |
| **Padding** | Required | Not required |
| **Use Case** | File encryption | Real-time streams |
| **Examples** | AES, DES | RC4, ChaCha20 |

**Stream Cipher Operation**:
```
Keystream Generator → Keystream (K)
                          ↓
Plaintext (P) ⊕ Keystream (K) = Ciphertext (C)
Ciphertext (C) ⊕ Keystream (K) = Plaintext (P)

Key Properties:
- Keystream appears random
- Same key produces same keystream
- Keystream must never repeat with same key
```

### 2. Linear Feedback Shift Register (LFSR)

**What is LFSR?**
- Core component of many stream ciphers
- Generates pseudorandom bit sequences
- Hardware efficient

**LFSR Structure**:
```
Initial State: [1][0][1][1]
              ↓  ↓  ↓  ↓
              ⊕---------┘
              ↓
           Output bit

Feedback polynomial: x^4 + x + 1
```

**Example 4-bit LFSR**:
```
Step | State  | Output | Feedback
-----|--------|--------|----------
0    | 1011   | 1      | 1⊕1=0
1    | 0101   | 1      | 0⊕1=1
2    | 1010   | 0      | 1⊕0=1
3    | 1101   | 1      | 1⊕1=0
4    | 0110   | 0      | 0⊕0=0
5    | 0011   | 1      | 0⊕1=1

Period: 15 (for maximal-length LFSR with 4 bits: 2^4-1)
```

**Properties**:
```
Maximum Period: 2^n - 1 (for n-bit LFSR)

Good Properties:
- Fast in hardware
- Predictable
- Known period

Bad Properties:
- Not cryptographically secure alone
- Can be broken with known plaintext
- Need combination for security
```

### 3. RC4 Stream Cipher

**RC4 Algorithm**:

**Key Scheduling Algorithm (KSA)**:
```
Initialize S array (0-255):
for i = 0 to 255:
    S[i] = i

j = 0
for i = 0 to 255:
    j = (j + S[i] + Key[i mod keylen]) mod 256
    swap(S[i], S[j])
```

**Pseudo-Random Generation Algorithm (PRGA)**:
```
i = 0
j = 0

Generate keystream byte:
    i = (i + 1) mod 256
    j = (j + S[i]) mod 256
    swap(S[i], S[j])
    t = (S[i] + S[j]) mod 256
    output = S[t]
```

**Example RC4 Encryption**:
```
Key: "Key" (ASCII: 75, 101, 121)
Plaintext: "Hi" (ASCII: 72, 105)

After KSA, generate keystream:
Keystream[0] = 143
Keystream[1] = 67

Encryption:
C[0] = 72 ⊕ 143 = 215
C[1] = 105 ⊕ 67 = 38

Ciphertext: [215, 38]
```

**RC4 Vulnerabilities**:
```
1. Biased output: First bytes not random
   Fix: Discard first 256-3072 bytes

2. Related key attacks
   Problem: WEP uses RC4 with related keys

3. Plaintext recovery
   With enough ciphertexts, patterns emerge

Status: ❌ Deprecated (RFC 7465)
Replacement: ChaCha20, AES-GCM
```

### 4. ChaCha20 Stream Cipher

**ChaCha20 Overview**:
```
Designer: Daniel J. Bernstein
Based on: Salsa20
Key Size: 256 bits
Nonce: 96 bits (unique per message)
Counter: 32 bits

Advantages:
- Fast in software (no hardware requirements)
- Constant-time (resistant to timing attacks)
- Better security margin than RC4
- Used in TLS 1.3, SSH, VPNs
```

**ChaCha20 Structure**:
```
Input State (16 words of 32 bits each):

Constants | Key      | Key      | Key
Key       | Counter  | Nonce    | Nonce
Nonce     | Nonce    | ←------- | 

Apply 20 rounds of quarter-round operations
Output 64 bytes of keystream per block
```

**ChaCha20 Quarter Round**:
```
Operations on (a, b, c, d):
a += b; d ^= a; d <<<= 16
c += d; b ^= c; b <<<= 12
a += b; d ^= a; d <<<= 8
c += d; b ^= c; b <<<= 7

(<<< = circular left shift)
```

**Usage Example**:
```
Key: 256-bit secret key
Nonce: 96-bit unique value (never reuse with same key!)
Counter: Starts at 0, increments per block

Encryption:
for each 64-byte block:
    keystream = ChaCha20(key, counter, nonce)
    ciphertext = plaintext ⊕ keystream
    counter++
```

### 5. One-Time Pad (OTP)

**Perfect Secrecy**:
```
Definition: Ciphertext gives no information about plaintext

Conditions for OTP:
1. Key is truly random
2. Key is same length as message
3. Key is used only once
4. Key is kept completely secret

Security Proof:
For any ciphertext C, all plaintexts equally likely
P(M|C) = P(M) for all messages M
```

**OTP Example**:
```
Plaintext:  HELLO
ASCII:      72 69 76 76 79

Random Key: 43 91 12 54 88
(truly random, same length)

XOR:
C = P ⊕ K
C = [115, 26, 84, 122, 23]

Decryption:
P = C ⊕ K
P = [72, 69, 76, 76, 79] = HELLO ✓
```

**Why OTP is Impractical**:
```
Problems:
1. Key Distribution:
   - Need to securely share key
   - Key is as long as message
   - If you can share long key securely, 
     why not share message itself?

2. Key Storage:
   - Need to store huge keys
   - One-time use means constant new keys

3. Synchronization:
   - Sender and receiver must be synchronized
   - Lost synchronization = lost message

4. Authentication:
   - OTP provides confidentiality only
   - No message integrity or authentication

Real-world use:
- Hotline between Washington and Moscow (Cold War)
- High-security diplomatic communications
- Now mostly replaced by modern ciphers
```

---

## UNIT II (EXTENDED): ADVANCED CRYPTANALYSIS

### 1. Cryptanalysis Types

**Classification by Known Information**:

```
1. Ciphertext-Only Attack (COA):
   - Only ciphertext available
   - Hardest for attacker
   - Example: Frequency analysis on classical ciphers

2. Known-Plaintext Attack (KPA):
   - Some plaintext-ciphertext pairs known
   - Find key or decrypt other messages
   - Example: Breaking Enigma with "WEATHER"

3. Chosen-Plaintext Attack (CPA):
   - Attacker can choose plaintexts to encrypt
   - Get corresponding ciphertexts
   - Example: Differential cryptanalysis

4. Chosen-Ciphertext Attack (CCA):
   - Attacker can choose ciphertexts to decrypt
   - Get corresponding plaintexts
   - Example: RSA attacks without padding

5. Adaptive Chosen-Plaintext/Ciphertext:
   - Can choose based on previous results
   - Most powerful attack
```

### 2. Differential Cryptanalysis

**Concept**:
```
Analyze how differences in input affect differences in output

For block cipher:
P1 ⊕ P2 = ΔP (input difference)
C1 ⊕ C2 = ΔC (output difference)

Goal: Find relationship between ΔP and ΔC to recover key
```

**Differential Cryptanalysis Process**:
```
Step 1: Choose input difference ΔP
Step 2: Encrypt many pairs (P, P⊕ΔP)
Step 3: Analyze output differences
Step 4: Find characteristic with high probability
Step 5: Use characteristic to recover key bits

Example (simplified):
If ΔP = 0x4000 leads to ΔC = 0x8000 with probability 1/4,
this characteristic can be exploited.
```

**Defense Against Differential Cryptanalysis**:
```
1. More rounds:
   - Each round diffuses differences
   - DES: 16 rounds resistant

2. S-boxes design:
   - Non-linearity
   - No high-probability differentials

3. Wide Trail Strategy:
   - Used in AES
   - Guarantees minimum active S-boxes
```

### 3. Linear Cryptanalysis

**Concept**:
```
Find linear approximations between plaintext, ciphertext, and key bits

For DES:
P[i1, i2, ...] ⊕ C[j1, j2, ...] = K[k1, k2, ...]

Where:
- P[i] = plaintext bit i
- C[j] = ciphertext bit j
- K[k] = key bit k

Goal: Find approximation that holds with probability ≠ 0.5
```

**Linear Cryptanalysis Process**:
```
Step 1: Find linear approximation for S-boxes
Step 2: Combine approximations for full cipher
Step 3: Collect many plaintext-ciphertext pairs
Step 4: Use piling-up lemma to find key bits

Data Required:
If probability = 0.5 + ε,
Need approximately 1/ε² known plaintexts

Example for DES:
- Best linear approximation: probability ≈ 0.5 + 2^-21
- Need ≈ 2^43 known plaintexts
- Better than brute force (2^56)
```

**Defense Against Linear Cryptanalysis**:
```
1. S-box Design:
   - High non-linearity
   - Low maximum linear probability

2. Mixing:
   - Good diffusion
   - Multiple rounds

3. Modern Ciphers:
   - AES designed with both differential and linear cryptanalysis in mind
   - Wide Trail Strategy bounds linear probability
```

### 4. Meet-in-the-Middle Attack

**Concept**:
```
Time-Memory Trade-off Attack

For double encryption:
C = E(K2, E(K1, P))

Naive attack: Try all (K1, K2) pairs = 2^(2n) operations

Meet-in-the-Middle:
1. Compute X = E(K1, P) for all K1 → Store in table
2. Compute X = D(K2, C) for all K2 → Check if in table
3. If match found, test (K1, K2) on another plaintext-ciphertext pair

Complexity: 2^(n+1) operations, 2^n memory
```

**Example: Double DES**:
```
DES key: 56 bits
Double DES: Two 56-bit keys

Brute force: 2^112 operations
Meet-in-the-Middle: 2^57 operations, 2^56 memory

This is why Triple DES is used instead of Double DES!
```

**Triple DES (3DES) Defense**:
```
3DES: C = E(K3, D(K2, E(K1, P)))

Even with meet-in-the-middle:
- Two-key 3DES: ~112-bit security
- Three-key 3DES: ~168-bit security

Sufficient to prevent meet-in-the-middle
```

### 5. Birthday Attack on Hash Functions

**Birthday Paradox**:
```
Question: How many people needed for 50% probability 
that two share a birthday?

Answer: 23 people (surprisingly low!)

Formula:
P(collision) ≈ 1 - e^(-n²/2m)

Where:
- n = number of items (people)
- m = total possibilities (365 days)

For 50% probability: n ≈ 1.177 × √m
```

**Application to Hash Functions**:
```
Hash function output: n bits
Total possible hashes: 2^n

Birthday attack:
Compute √(2^n) = 2^(n/2) hashes
50% probability of finding collision

Example:
- 64-bit hash: 2^32 ≈ 4 billion hashes (feasible!)
- 128-bit hash: 2^64 ≈ 18 quintillion (infeasible)
- 256-bit hash: 2^128 (completely infeasible)

This is why:
MD5 (128-bit) is broken → 2^64 operations found collisions
SHA-1 (160-bit) is deprecated → 2^80 operations
SHA-256 is secure → 2^128 operations (infeasible)
```

**Birthday Attack Example**:
```
Goal: Create fraudulent contract

Step 1: Create two contracts
Contract A: Legitimate
Contract B: Fraudulent (with same hash)

Step 2: Generate variations
- Make tiny changes to both
  (spaces, wording, formatting)
- Try to find variation of A and B with same hash

Step 3: Get signature
- Get legitimate contract A signed
- Claim signature applies to fraudulent B

Defense:
- Use hash with sufficient length (256+ bits)
- Sign original document, not just hash
- Timestamp and notarize
```

---

## UNIT III (EXTENDED): KERBEROS AUTHENTICATION

### 1. Kerberos Protocol Detailed

**Overview**:
```
Purpose: Network authentication using symmetric key cryptography
Name: From Greek mythology (three-headed dog guarding Hades)
Version: Kerberos V5 (current), V4 (obsolete)

Key Concept: Tickets
- Ticket = Encrypted credential
- Used instead of passwords
- Time-limited
- Cannot be reused
```

**Kerberos Components**:
```
1. Key Distribution Center (KDC):
   - Trusted third party
   - Contains two services:
     a) Authentication Server (AS)
     b) Ticket Granting Server (TGS)

2. Client (User):
   - Wants to access service
   - Has password

3. Service Server (SS):
   - Provides service (file server, email, etc.)
   - Trusts KDC

4. Database:
   - Stores user passwords (hashed)
   - Stores service keys
```

**Kerberos Detailed Flow**:

**Phase 1: Authentication (Get TGT)**:
```
Client → AS:
- Username
- Requested service: TGS

AS → Client:
- TGT (Ticket Granting Ticket)
  Encrypted with TGS secret key:
  {Username, TGS_Session_Key, Timestamp, Lifetime}
  
- Session Key for TGS
  Encrypted with user's password:
  {TGS_Session_Key, Timestamp, Lifetime}

Client:
- Decrypts with password to get TGS_Session_Key
- Stores TGT (can't decrypt it, but will use it)
```

**Phase 2: Get Service Ticket**:
```
Client → TGS:
- TGT (from Phase 1)
- Requested service name
- Authenticator:
  {Username, Timestamp} encrypted with TGS_Session_Key

TGS:
- Decrypts TGT with its secret key
- Verifies authenticator
- Checks timestamp (prevent replay)

TGS → Client:
- Service Ticket
  Encrypted with Service's secret key:
  {Username, Service_Session_Key, Timestamp, Lifetime}
  
- Service Session Key
  Encrypted with TGS_Session_Key:
  {Service_Session_Key, Timestamp, Lifetime}

Client:
- Decrypts to get Service_Session_Key
- Stores Service Ticket
```

**Phase 3: Access Service**:
```
Client → Service:
- Service Ticket (from Phase 2)
- Authenticator:
  {Username, Timestamp} encrypted with Service_Session_Key

Service:
- Decrypts ticket with its secret key
- Gets Service_Session_Key
- Decrypts authenticator
- Verifies username and timestamp

Service → Client (optional mutual authentication):
- {Timestamp + 1} encrypted with Service_Session_Key

Client:
- Verifies server's response
- Confirms server is authentic
```

**Complete Kerberos Flow Diagram**:
```
   Client                AS                  TGS              Service
      |                   |                   |                  |
  [1] |---- Username ---->|                   |                  |
      |                   |                   |                  |
  [2] |<--- TGT + Key ----|                   |                  |
      |(encrypted)        |                   |                  |
      |                   |                   |                  |
  [3] |------------- TGT + Auth ----------->|                  |
      |                   |                   |                  |
  [4] |<---- Service Ticket + Key -----------|                  |
      |                   |                   |(encrypted)       |
      |                   |                   |                  |
  [5] |------------- Service Ticket + Auth ------------------>|
      |                   |                   |                  |
  [6] |<------------ Service Response -------------------------|
      |                   |                   |                  |
```

**Kerberos Advantages**:
```
1. Single Sign-On (SSO):
   - Login once, access multiple services
   - TGT valid for multiple service tickets

2. No Password Transmission:
   - Password never sent over network
   - Only used locally to decrypt

3. Mutual Authentication:
   - Client verifies server
   - Server verifies client

4. Time-Limited Tickets:
   - Automatic expiration
   - Reduces replay attack window

5. Centralized Management:
   - Single point for user management
   - Easy to revoke access
```

**Kerberos Limitations**:
```
1. Single Point of Failure:
   - If KDC down, no authentication
   - Solution: Replicate KDC

2. Time Synchronization:
   - All systems must have synchronized clocks
   - Timestamps prevent replay attacks
   - Solution: NTP (Network Time Protocol)

3. Initial Authentication:
   - Still relies on password
   - Vulnerable to password attacks
   - Solution: Use strong passwords, multi-factor

4. No Protection Against Compromised Client:
   - If client machine compromised, tickets exposed
   - Solution: Short ticket lifetimes

5. Scalability:
   - Cross-realm authentication complex
   - Large deployments need careful planning
```

**Kerberos Security**:
```
Threats:
1. Password Guessing:
   - Offline attack on encrypted message
   - Defense: Strong passwords, rate limiting

2. Replay Attack:
   - Reuse old ticket
   - Defense: Timestamps, short lifetimes

3. Man-in-the-Middle:
   - Intercept and modify messages
   - Defense: Encryption, authenticators

4. Ticket Theft:
   - Steal ticket from memory
   - Defense: Short lifetimes, secure systems

Security Measures:
- Encryption: All sensitive data encrypted
- Timestamps: Prevent replay (5-minute window typical)
- Lifetimes: Tickets expire (8-10 hours typical)
- Authenticators: Fresh proof of possession
- Nonces: Random values prevent replay
```

**Kerberos Example Session**:
```
User: alice
Service: fileserver
Password: alicePass123

Step 1: Alice logs in
- Client sends: "alice", "TGS"
- AS responds with TGT (encrypted for TGS)
- Client decrypts session key with password hash

Step 2: Alice requests file server access
- Client sends TGT + authenticator to TGS
- TGS responds with fileserver ticket
- Client decrypts service session key

Step 3: Alice accesses file server
- Client sends fileserver ticket + authenticator
- Fileserver verifies and grants access
- Alice can now read/write files

All subsequent file operations use service session key
until ticket expires (e.g., 8 hours)
```

---

## UNIT IV (EXTENDED): DIGITAL CERTIFICATES & PKI

### 1. X.509 Digital Certificates

**X.509 Certificate Structure** (Detailed):
```
Certificate:
    Version Number: v3 (most common)
    Serial Number: Unique ID from CA
    Signature Algorithm: RSA-SHA256, ECDSA-SHA256, etc.
    
    Issuer: CA information
        - Country (C)
        - Organization (O)
        - Organizational Unit (OU)
        - Common Name (CN)
    
    Validity:
        - Not Before: Start date/time
        - Not After: Expiration date/time
    
    Subject: Certificate holder information
        - Country (C)
        - State/Province (ST)
        - Locality (L)
        - Organization (O)
        - Organizational Unit (OU)
        - Common Name (CN) ← Domain name for websites
        - Email Address
    
    Subject Public Key Info:
        - Algorithm: RSA, ECDSA, etc.
        - Public Key: Actual key value
        - Key Size: 2048, 3072, 4096 bits
    
    Issuer Unique ID (optional)
    Subject Unique ID (optional)
    
    Extensions (v3):
        - Key Usage
        - Extended Key Usage
        - Subject Alternative Name (SAN)
        - Basic Constraints
        - CRL Distribution Points
        - Authority Key Identifier
        - Subject Key Identifier
    
Certificate Signature Algorithm: (repeated)
Certificate Signature: Digital signature by CA
```

**Example X.509 Certificate** (simplified):
```
Certificate:
    Version: 3 (0x2)
    Serial Number: 04:xx:xx:xx:xx:xx:xx:xx:xx
    Signature Algorithm: sha256WithRSAEncryption
    
    Issuer: CN=Let's Encrypt Authority X3, O=Let's Encrypt, C=US
    
    Validity:
        Not Before: Jan  1 00:00:00 2024 GMT
        Not After : Apr  1 23:59:59 2024 GMT
    
    Subject: CN=www.example.com
    
    Subject Public Key Info:
        Algorithm: RSA
        Public-Key: (2048 bit)
        Modulus:
            00:b5:d9:7c:... (2048-bit public key)
        Exponent: 65537 (0x10001)
    
    X509v3 Extensions:
        X509v3 Key Usage: critical
            Digital Signature, Key Encipherment
        X509v3 Extended Key Usage:
            TLS Web Server Authentication
        X509v3 Subject Alternative Name:
            DNS:www.example.com, DNS:example.com
        X509v3 Basic Constraints: critical
            CA:FALSE
    
    Signature Algorithm: sha256WithRSAEncryption
        4f:85:3a:... (256-byte RSA signature)
```

### 2. Certificate Extensions (Important)

**Key Usage**:
```
Defines what certificate can be used for:

- digitalSignature: For digital signatures
- nonRepudiation: For signing (can't deny)
- keyEncipherment: Encrypt symmetric keys
- dataEncipherment: Encrypt data directly
- keyAgreement: Key exchange (Diffie-Hellman)
- keyCertSign: Sign other certificates (CA only)
- cRLSign: Sign CRLs (CA only)
- encipherOnly: Only encrypt (with keyAgreement)
- decipherOnly: Only decrypt (with keyAgreement)

Example:
Server certificate: digitalSignature, keyEncipherment
CA certificate: keyCertSign, cRLSign
```

**Extended Key Usage** (EKU):
```
More specific purposes:

- serverAuth: TLS/SSL server
- clientAuth: TLS/SSL client
- codeSigning: Sign software/code
- emailProtection: S/MIME email
- timeStamping: Timestamp signatures
- OCSPSigning: Sign OCSP responses

Example:
Web server: serverAuth
Code signing cert: codeSigning
```

**Subject Alternative Name** (SAN):
```
Additional identities for certificate:

Types:
- DNS: example.com, www.example.com, *.example.com
- IP Address: 192.168.1.1
- Email: user@example.com
- URI: https://example.com/resource

Use Case: One certificate for multiple domains
Example:
DNS:example.com
DNS:www.example.com
DNS:blog.example.com
DNS:shop.example.com
```

**Basic Constraints**:
```
CA: TRUE/FALSE
pathLenConstraint: Number

If CA:TRUE → Can issue certificates
pathLenConstraint: Max intermediate CAs in chain

Example:
Root CA: CA:TRUE, pathLen:2
Intermediate CA: CA:TRUE, pathLen:1
End-entity: CA:FALSE

This allows:
Root → Intermediate1 → Intermediate2 → End-entity (valid)
Root → Intermediate1 → Intermediate2 → Intermediate3 → End-entity (invalid, exceeds pathLen)
```

**CRL Distribution Points**:
```
URL where Certificate Revocation List published

Example:
URI:http://crl.example.com/ca.crl

Client can download CRL to check if certificate revoked
```

**Authority Information Access** (AIA):
```
CA Issuers: Where to get CA certificate
OCSP: Where to check revocation status online

Example:
CA Issuers - URI:http://cert.example.com/ca.crt
OCSP - URI:http://ocsp.example.com
```

### 3. Certificate Chain Validation

**Certificate Chain**:
```
End-Entity Certificate (example.com)
        ↓ signed by
Intermediate CA Certificate
        ↓ signed by
Root CA Certificate (self-signed)
        ↓ in
Trusted Root Store (OS/Browser)
```

**Validation Process** (Step-by-Step):
```
Step 1: Verify Digital Signature
- Extract signature from certificate
- Decrypt signature with issuer's public key
- Compute hash of certificate
- Compare: hash = decrypted signature?
- If NO → Invalid certificate, STOP
- If YES → Continue

Step 2: Check Validity Period
- Current date/time ≥ Not Before?
- Current date/time ≤ Not After?
- If outside range → Expired/Not yet valid, STOP
- If within range → Continue

Step 3: Check Revocation Status
Option A: CRL (Certificate Revocation List)
- Download CRL from CRL Distribution Point
- Check if certificate serial number in list
- If YES → Revoked, STOP

Option B: OCSP (Online Certificate Status Protocol)
- Send request to OCSP responder
- Response: Good / Revoked / Unknown
- If Revoked → STOP

Step 4: Verify Chain to Trusted Root
- Get issuer certificate
- Repeat steps 1-3 for issuer
- Continue until reaching root
- Check if root in trusted store
- If NOT → Untrusted chain, STOP

Step 5: Check Name/Domain
- Certificate Common Name or SAN matches domain?
- Wildcard certificates: *.example.com matches www.example.com
- If NO match → Wrong certificate, STOP

Step 6: Check Key Usage
- Is certificate allowed for this purpose?
- Server cert must have serverAuth EKU
- If wrong usage → STOP

Step 7: Check Constraints
- Basic Constraints: CA flag correct?
- Path Length not exceeded?
- If violated → STOP

If all steps pass → Certificate VALID ✓
```

**Example Validation**:
```
Website: https://www.example.com

Certificate Chain:
1. www.example.com (End-entity)
   Issued by: DigiCert TLS RSA SHA256 2020 CA1
   Valid: 2024-01-01 to 2025-01-01
   
2. DigiCert TLS RSA SHA256 2020 CA1 (Intermediate)
   Issued by: DigiCert Global Root CA
   Valid: 2020-01-01 to 2030-01-01
   
3. DigiCert Global Root CA (Root)
   Self-signed
   Valid: 2006-11-10 to 2031-11-10
   In trusted root store ✓

Validation:
✓ Signature of (1) verified with public key from (2)
✓ Signature of (2) verified with public key from (3)
✓ Certificate (3) in trusted store
✓ All certificates within validity period
✓ No certificates revoked (checked via OCSP)
✓ www.example.com matches certificate CN/SAN
✓ Certificate has serverAuth extended key usage
✓ All constraints satisfied

Result: VALID, establish TLS connection
```

### 4. Certificate Revocation

**Why Revoke Certificates?**
```
Reasons:
1. Private key compromised
2. Certificate mis-issued
3. User's affiliation changed
4. CA compromised
5. Superseded by new certificate
6. Certificate holder no longer authorized
```

**Certificate Revocation List (CRL)**:
```
Published by CA
Contains:
- List of revoked certificate serial numbers
- Revocation date
- Revocation reason
- Next update time

Format:
Certificate Revocation List (CRL):
    Version: 2
    Signature Algorithm: sha256WithRSAEncryption
    Issuer: CN=Example CA
    Last Update: Jan 1 00:00:00 2024 GMT
    Next Update: Jan 8 00:00:00 2024 GMT
    
    Revoked Certificates:
        Serial Number: 1A2B3C4D5E6F
            Revocation Date: Dec 25 12:00:00 2023 GMT
            Reason: keyCompromise
        
        Serial Number: 9F8E7D6C5B4A
            Revocation Date: Dec 28 15:30:00 2023 GMT
            Reason: superseded
    
    Signature: (CA's signature)

Problems:
- Can be large (MBs for big CAs)
- Must download entire list
- Updated periodically (not real-time)
- Privacy: Downloading reveals which sites you visit
```

**Online Certificate Status Protocol (OCSP)**:
```
Real-time revocation checking

Request:
Client → OCSP Responder:
- Certificate serial number
- Issuer information

Response:
OCSP Responder → Client:
- Status: Good / Revoked / Unknown
- Response time
- Next update time
- Signature (by OCSP responder)

Example:
Request:
OCSP Request:
    Cert ID:
        Hash Algorithm: sha1
        Issuer Name Hash: f2d2...
        Issuer Key Hash: 90af...
        Serial Number: 04:xx:xx:xx

Response:
OCSP Response:
    Response Status: successful
    Cert Status: good
    This Update: Jan 1 12:00:00 2024 GMT
    Next Update: Jan 1 18:00:00 2024 GMT
    Signature: (signed by OCSP responder)

Advantages over CRL:
+ Real-time status
+ Smaller response (only one certificate)
+ Faster

Disadvantages:
- Privacy: CA knows which sites you visit
- Dependency: If OCSP server down, what to do?
  - Hard-fail: Reject certificate (strict, may break sites)
  - Soft-fail: Accept certificate (lenient, security risk)
```

**OCSP Stapling**:
```
Solution to OCSP privacy and reliability issues

How it works:
1. Server periodically requests OCSP response for its own certificate
2. Server "staples" (attaches) OCSP response to TLS handshake
3. Client verifies stapled response

Benefits:
+ Privacy: Client doesn't contact OCSP server
+ Performance: No additional round-trip
+ Reliability: Not dependent on OCSP server during handshake

TLS Handshake with OCSP Stapling:
Client → Server: ClientHello (with status_request extension)
Server → Client: ServerHello, Certificate, CertificateStatus (OCSP response)
Client: Verifies certificate and OCSP response locally
```

**Certificate Transparency (CT)**:
```
Public logs of all issued certificates

Purpose:
- Detect mis-issued certificates
- Detect compromised CAs
- Monitor certificate issuance

How it works:
1. CA issues certificate
2. CA submits certificate to CT logs
3. CT log returns Signed Certificate Timestamp (SCT)
4. Certificate includes SCT
5. Browsers require SCT for trust

Benefits:
- Public auditing
- Early detection of attacks
- Accountability for CAs

CT Log Entry:
{
  "timestamp": 1609459200000,
  "certificate": "MIIFxDCC...",
  "sct": {
    "version": 0,
    "log_id": "pLkJ...",
    "timestamp": 1609459200000,
    "signature": "BAM..."
  }
}
```

### 5. PKI (Public Key Infrastructure) Operations

**PKI Components**:
```
1. Certificate Authority (CA):
   - Issues certificates
   - Signs certificates with private key
   - Maintains CRL
   - Provides OCSP

2. Registration Authority (RA):
   - Verifies identity of certificate requesters
   - Approves/rejects requests
   - Submits approved requests to CA

3. Certificate Repository:
   - Stores issued certificates
   - Publishes CRLs
   - Public access

4. End Entities:
   - Certificate holders
   - Users, servers, devices

5. Validation Authority (VA):
   - Validates certificates
   - Provides revocation information
   - May run OCSP responder
```

**Certificate Lifecycle**:
```
1. Generation:
   - End entity generates key pair
   - Creates Certificate Signing Request (CSR)
   - CSR contains: public key, subject info

2. Registration:
   - Submit CSR to RA
   - RA verifies identity (domain validation, organization validation, extended validation)
   - RA approves request

3. Issuance:
   - CA receives approved request
   - CA creates certificate
   - CA signs certificate with its private key
   - Certificate published to repository

4. Usage:
   - End entity uses certificate
   - Other parties validate certificate
   - Certificate presented during TLS handshake

5. Renewal:
   - Before expiration, request new certificate
   - May reuse same key pair or generate new

6. Revocation:
   - If compromised, request revocation
   - CA adds to CRL
   - OCSP responds "revoked"

7. Expiration:
   - Certificate automatically invalid after "Not After" date
   - Must renew before expiration
```

**Certificate Signing Request (CSR)**:
```
Format: PKCS#10

Structure:
CertificationRequest:
    Version: 0
    Subject: CN=www.example.com, O=Example Inc, C=US
    SubjectPublicKeyInfo:
        Algorithm: rsaEncryption
        Public Key: (2048 bits)
            00:b5:d9:...
    Attributes:
        challengePassword: (optional password)
        unstructuredName: (optional)
        extensionRequest:
            subjectAltName: DNS:example.com, DNS:www.example.com
    Signature Algorithm: sha256WithRSAEncryption
    Signature: (signed with private key)

Generate CSR (OpenSSL):
openssl req -new -key private.key -out request.csr

Submit CSR to CA
CA verifies and issues certificate
```

**Types of Certificate Validation**:
```
1. Domain Validation (DV):
   - Verify control of domain only
   - Email to admin@example.com
   - Or DNS record verification
   - Or HTTP file verification
   - Issued in minutes
   - Cheapest/free (Let's Encrypt)

2. Organization Validation (OV):
   - Verify organization exists
   - Check business registration
   - Phone verification
   - Takes 1-3 days
   - Shows organization name in certificate

3. Extended Validation (EV):
   - Strictest verification
   - Legal, physical, operational existence
   - Officer authorization
   - Takes days to weeks
   - Green address bar (older browsers)
   - Highest trust level
   - Most expensive

Comparison:
DV:  example.com
OV:  Example Inc [US]
EV:  Example Incorporated [Delaware, US]
```

**CA Hierarchy**:
```
Root CA (offline, highly secure)
    ↓ issues
Intermediate CA 1 (online, issues certificates)
Intermediate CA 2 (online, issues certificates)
    ↓ issue
End-entity certificates

Why intermediate CAs?
1. Security:
   - Root CA kept offline
   - If intermediate compromised, revoke only that CA
   - Root CA still trusted

2. Flexibility:
   - Different intermediates for different purposes
   - Different geographic regions
   - Different validation levels

3. Performance:
   - Distribute load across multiple CAs
```

**PKI Trust Models**:
```
1. Hierarchical (Tree):
   - Single root CA
   - Tree of intermediate CAs
   - Most common
   Example: Root → Intermediate → End-entity

2. Mesh (Web of Trust):
   - Peer-to-peer trust
   - Users sign each other's keys
   - PGP uses this
   Example: Alice trusts Bob, Bob trusts Carol, Alice may trust Carol

3. Bridge:
   - Multiple hierarchies connected
   - Bridge CA connects roots
   - Government/military use
   Example: Hierarchy A ← Bridge → Hierarchy B

4. Hybrid:
   - Combination of above
   - Real-world often uses hybrid
```

---

## UNIT V (EXTENDED): VPN & AUTHENTICATION PROTOCOLS

### 1. Virtual Private Network (VPN)

**VPN Types**:

**Site-to-Site VPN**:
```
Purpose: Connect two networks over internet

Topology:
Office A Network ← Router/VPN Gateway ← Internet → VPN Gateway/Router → Office B Network
192.168.1.0/24                                                          192.168.2.0/24

Characteristics:
- Always-on connection
- Transparent to users
- Gateway-to-gateway encryption
- Branch offices, data centers

Protocols:
- IPSec (most common)
- GRE (Generic Routing Encapsulation)
- MPLS (Multi-Protocol Label Switching)
```

**Remote Access VPN**:
```
Purpose: Connect remote user to corporate network

Topology:
Remote User (Laptop) → Internet → VPN Gateway → Corporate Network

Characteristics:
- On-demand connection
- User initiates
- Client software required
- Remote workers, BYOD

Protocols:
- SSL/TLS VPN (WebVPN)
- IPSec VPN
- OpenVPN
- WireGuard
```

**VPN Protocols Comparison**:

**PPTP (Point-to-Point Tunneling Protocol)**:
```
Developer: Microsoft
Port: TCP 1723
Encryption: MPPE (weak)

Pros:
+ Easy to set up
+ Fast (weak encryption)
+ Built into Windows

Cons:
- Insecure (multiple vulnerabilities)
- Easily blocked by firewalls
- No longer recommended

Status: ❌ Obsolete
```

**L2TP/IPSec**:
```
L2TP: Layer 2 Tunneling Protocol (tunneling)
IPSec: Internet Protocol Security (encryption)

Port: UDP 500 (IKE), UDP 4500 (NAT-T), ESP Protocol 50

How it works:
1. IPSec establishes secure tunnel
2. L2TP creates tunnel inside IPSec
3. Double encapsulation

Pros:
+ Widely supported
+ Secure (with IPSec)
+ Stable

Cons:
- Slower (double encapsulation)
- Can be blocked (uses fixed ports)
- Complex configuration

Status: ✓ Secure but slower
```

**OpenVPN**:
```
Developer: Open-source
Port: Configurable (commonly TCP 443 or UDP 1194)
Encryption: OpenSSL library (AES, etc.)

How it works:
1. Uses SSL/TLS for key exchange
2. Custom protocol for data
3. Can use TCP or UDP

Pros:
+ Highly secure
+ Open source (auditable)
+ Flexible (bypass firewalls)
+ Cross-platform

Cons:
- Requires third-party client
- Can be complex to set up
- Slower than WireGuard

Status: ✓ Recommended

Configuration Example:
client
dev tun
proto udp
remote vpn.example.com 1194
ca ca.crt
cert client.crt
key client.key
cipher AES-256-GCM
auth SHA256
```

**WireGuard**:
```
Developer: Jason A. Donenfeld
Port: UDP 51820 (default)
Encryption: ChaCha20, Curve25519

Design Philosophy:
- Minimal code (< 4,000 lines)
- Modern cryptography
- Simple configuration

Pros:
+ Very fast
+ Simple and auditable
+ Modern crypto (no legacy)
+ Built into Linux kernel
+ Low attack surface

Cons:
- Relatively new
- Stores IPs (privacy concern)
- Less enterprise features

Status: ✓ Latest, fastest

Configuration Example:
[Interface]
PrivateKey = <client private key>
Address = 10.0.0.2/24

[Peer]
PublicKey = <server public key>
Endpoint = vpn.example.com:51820
AllowedIPs = 0.0.0.0/0
```

**IPSec VPN Deep Dive**:

**IPSec Modes**:
```
1. Transport Mode:
   - Only payload encrypted
   - IP header visible
   - End-to-end security
   
   Original:  [IP Header][TCP][Data]
   Transport: [IP Header][ESP][TCP][Data][ESP Trailer]
   
   Use: Host-to-host

2. Tunnel Mode:
   - Entire original packet encrypted
   - New IP header added
   - Gateway-to-gateway
   
   Original: [IP Header][TCP][Data]
   Tunnel:   [New IP Header][ESP][Original Packet][ESP Trailer]
   
   Use: Site-to-site VPNs
```

**IPSec Protocols**:
```
1. AH (Authentication Header):
   - Authentication only
   - Integrity check
   - No encryption
   
   Header:
   [Next Header][Payload Len][Reserved][SPI][Sequence][ICV]
   
   Use: When encryption not needed (rare)

2. ESP (Encapsulating Security Payload):
   - Authentication AND encryption
   - Confidentiality and integrity
   - Most commonly used
   
   Format:
   [ESP Header][Encrypted Payload][ESP Trailer][ESP Auth]
   
   Use: Standard VPN encryption
```

**IKE (Internet Key Exchange)**:
```
Purpose: Establish security associations (SA)

IKEv1 (Phase 1 and 2):
Phase 1: Establish secure channel (ISAKMP SA)
  - Main Mode (6 messages, secure)
  - Aggressive Mode (3 messages, faster, less secure)

Phase 2: Negotiate IPSec SA
  - Quick Mode (3 messages)
  - Actual VPN tunnel parameters

IKEv2 (Improved):
  - Single 4-message exchange (faster)
  - Built-in NAT traversal
  - Mobility support (MOBIKE)
  - Better error handling
  - Mutual authentication required

IKEv2 Exchange:
Client → Server: IKE_SA_INIT (DH exchange, nonces)
Server → Client: IKE_SA_INIT (DH response)
Client → Server: IKE_AUTH (authenticate, create child SA)
Server → Client: IKE_AUTH (respond, confirm)

Status: IKEv2 preferred over IKEv1
```

**VPN Security Considerations**:
```
1. Kill Switch:
   - Blocks internet if VPN drops
   - Prevents data leaks
   - Critical for privacy

2. DNS Leaks:
   - DNS queries bypass VPN
   - Use VPN's DNS servers
   - Test: dnsleaktest.com

3. IPv6 Leaks:
   - IPv6 traffic bypass VPN
   - Disable IPv6 or use IPv6 VPN

4. WebRTC Leaks:
   - Browser reveals real IP
   - Disable WebRTC or use extensions

5. Split Tunneling:
   - Route some traffic through VPN, some direct
   - Improves performance
   - Security risk if misconfigured

6. Logging:
   - No-logs policy important for privacy
   - Verify through audits
   - Jurisdiction matters

7. Encryption:
   - Use AES-256 minimum
   - Perfect forward secrecy (PFS)
   - Strong authentication (SHA-256+)
```

### 2. OAuth 2.0

**What is OAuth 2.0?**
```
Purpose: Authorization framework (NOT authentication!)
Use Case: "Login with Google", "Access your Facebook photos"

Key Concept: Delegated access
- User grants app limited access to their data
- App never gets user's password
- User can revoke access anytime

Roles:
1. Resource Owner: User
2. Client: Application requesting access
3. Authorization Server: Issues tokens (e.g., Google)
4. Resource Server: Hosts user data (e.g., Google Drive API)
```

**OAuth 2.0 Grant Types**:

**1. Authorization Code Grant** (Most secure):
```
Use Case: Server-side web apps

Flow:
1. User clicks "Login with Google"
2. Client redirects to authorization server
3. User authenticates and grants permission
4. Authorization server redirects back with CODE
5. Client exchanges CODE for ACCESS TOKEN (server-to-server)
6. Client uses ACCESS TOKEN to access user data

Detailed Flow:
Step 1: Client → User → Authorization Server
https://accounts.google.com/oauth/authorize?
  response_type=code&
  client_id=abc123&
  redirect_uri=https://app.com/callback&
  scope=profile email&
  state=random_state

Step 2: User authenticates and approves

Step 3: Authorization Server → Client
https://app.com/callback?
  code=AUTH_CODE_HERE&
  state=random_state

Step 4: Client → Authorization Server (backend)
POST /oauth/token
{
  grant_type: "authorization_code",
  code: "AUTH_CODE_HERE",
  client_id: "abc123",
  client_secret: "secret123",
  redirect_uri: "https://app.com/callback"
}

Step 5: Authorization Server → Client
{
  access_token: "ACCESS_TOKEN",
  token_type: "Bearer",
  expires_in: 3600,
  refresh_token: "REFRESH_TOKEN"
}

Step 6: Client → Resource Server
GET /api/user
Authorization: Bearer ACCESS_TOKEN

Security:
+ Code only exposed to user's browser (untrusted)
+ Token exchange happens server-to-server (secure)
+ Client secret never exposed to browser
```

**2. Implicit Grant** (Deprecated):
```
Use Case: JavaScript apps (legacy)

Flow:
1. Redirect to authorization server
2. Token returned directly in URL fragment
3. No code exchange step

Security Issues:
- Token exposed in browser
- No client authentication
- Vulnerable to token theft

Status: ❌ Deprecated
Replacement: Authorization Code + PKCE
```

**3. Client Credentials Grant**:
```
Use Case: Machine-to-machine (no user)

Flow:
1. Client sends credentials directly
2. Receives access token
3. Uses token for API calls

POST /oauth/token
{
  grant_type: "client_credentials",
  client_id: "abc123",
  client_secret: "secret123",
  scope: "api.read"
}

Response:
{
  access_token: "ACCESS_TOKEN",
  token_type: "Bearer",
  expires_in: 3600
}

Use: Backend services, cron jobs, microservices
```

**4. Resource Owner Password Credentials** (Not recommended):
```
Use Case: Trusted first-party apps only

Flow:
1. User enters username/password in app
2. App sends credentials to auth server
3. Receives access token

POST /oauth/token
{
  grant_type: "password",
  username: "user@example.com",
  password: "password123",
  client_id: "abc123",
  client_secret: "secret123"
}

Security Issues:
- App handles user password
- Defeats OAuth purpose

Use Only: Legacy migration, trusted apps
Status: ⚠️ Avoid if possible
```

**PKCE (Proof Key for Code Exchange)**:
```
Purpose: Secure authorization code grant for public clients (mobile, SPA)

Problem without PKCE:
- Public clients can't securely store client_secret
- Authorization code can be intercepted

Solution:
1. Client generates random code_verifier
2. Creates code_challenge = SHA256(code_verifier)
3. Sends code_challenge with authorization request
4. Authorization server stores code_challenge
5. Client exchanges code + code_verifier for token
6. Server verifies: SHA256(code_verifier) == code_challenge

Flow:
// Generate
code_verifier = random_string(43-128 chars)
code_challenge = base64url(sha256(code_verifier))

// Authorization request
GET /authorize?
  response_type=code&
  client_id=abc123&
  code_challenge=CHALLENGE&
  code_challenge_method=S256&
  redirect_uri=app://callback

// Token request
POST /token
{
  grant_type: "authorization_code",
  code: "AUTH_CODE",
  code_verifier: "CODE_VERIFIER",
  client_id: "abc123"
}

Server verifies:
base64url(sha256(code_verifier)) == code_challenge

Benefits:
+ No client secret needed
+ Prevents code interception attacks
+ Recommended for all OAuth clients now
```

**OAuth 2.0 Tokens**:

**Access Token**:
```
Purpose: Access protected resources
Format: Usually JWT (JSON Web Token) or opaque string
Lifetime: Short (minutes to hours)
Scope: Limited permissions

Example JWT:
eyJhbGciOiJSUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiaWF0IjoxNTE2MjM5MDIyfQ.SflKxwRJSMeKKF2QT4fwpMeJf36POk6yJV_adQssw5c

Decoded:
Header: {"alg": "RS256", "typ": "JWT"}
Payload: {
  "sub": "1234567890",
  "name": "John Doe",
  "iat": 1516239022,
  "exp": 1516242622,
  "scope": "read write"
}
Signature: (verify with public key)
```

**Refresh Token**:
```
Purpose: Obtain new access token without user interaction
Lifetime: Long (days to months)
Security: More sensitive, must be stored securely

Use:
POST /oauth/token
{
  grant_type: "refresh_token",
  refresh_token: "REFRESH_TOKEN",
  client_id: "abc123",
  client_secret: "secret123"
}

Response:
{
  access_token: "NEW_ACCESS_TOKEN",
  expires_in: 3600,
  refresh_token: "NEW_REFRESH_TOKEN"  // May rotate
}

Rotation:
- Issue new refresh token with each use (recommended)
- Invalidate old refresh token
- Detect token theft (if old token reused)
```

**OAuth 2.0 Scopes**:
```
Purpose: Limit access permissions

Examples:
- read: Read access only
- write: Write access
- email: Access email address
- profile: Access profile info
- openid: OpenID Connect authentication

Usage:
scope=email profile

User sees: "App wants to access your email and profile"

API checks:
if (token.scope.includes('email')) {
  return user.email;
}
```

**OAuth 2.0 Security Best Practices**:
```
1. Always use HTTPS
   - Tokens in clear text over HTTP = stolen tokens

2. Validate redirect_uri
   - Prevent open redirect attacks
   - Whitelist exact URIs

3. Use state parameter
   - Prevent CSRF attacks
   - Verify state matches

4. Short-lived access tokens
   - 15-60 minutes typical
   - Limit damage if stolen

5. Secure storage
   - Store tokens securely (encrypted, httpOnly cookies)
   - Never in localStorage (XSS vulnerable)

6. Rotate refresh tokens
   - Issue new with each use
   - Detect and revoke on suspicious activity

7. Use PKCE
   - For all client types now (not just public)
   - Additional security layer

8. Validate JWTs properly
   - Check signature
   - Verify expiration
   - Validate issuer and audience

9. Implement rate limiting
   - Prevent brute force token guessing

10. Revocation
    - Provide token revocation endpoint
    - Allow users to revoke access
```

### 3. OpenID Connect (OIDC)

**What is OpenID Connect?**
```
Purpose: Authentication layer on top of OAuth 2.0
OAuth 2.0 = Authorization ("what can you access?")
OIDC = Authentication ("who are you?")

Key Difference:
OAuth: Get access to user's photos
OIDC: Know that user is alice@example.com

Built on OAuth 2.0:
- Uses OAuth 2.0 flows
- Adds ID Token
- Adds UserInfo endpoint
- Standardized claims
```

**OIDC Flow**:
```
Same as OAuth Authorization Code flow + ID Token

Authorization Request:
GET /authorize?
  response_type=code&
  scope=openid profile email&  ← "openid" required for OIDC
  client_id=abc123&
  redirect_uri=https://app.com/callback&
  state=xyz&
  nonce=random123  ← OIDC-specific

Token Response:
{
  access_token: "ACCESS_TOKEN",
  id_token: "ID_TOKEN",  ← OIDC ID Token (JWT)
  token_type: "Bearer",
  expires_in: 3600
}
```

**ID Token** (JWT):
```
Purpose: Proof of authentication

Structure:
Header:
{
  "alg": "RS256",
  "kid": "key-id-1"
}

Payload:
{
  "iss": "https://accounts.google.com",  // Issuer
  "sub": "1234567890",                   // Subject (user ID)
  "aud": "abc123",                       // Audience (client ID)
  "exp": 1638360000,                     // Expiration
  "iat": 1638356400,                     // Issued at
  "nonce": "random123",                  // Matches request
  
  // Standard claims
  "name": "John Doe",
  "email": "john@example.com",
  "email_verified": true,
  "picture": "https://example.com/photo.jpg"
}

Signature: (verify with provider's public key)

Validation:
1. Verify signature with provider's public key
2. Check iss (issuer) matches expected
3. Check aud (audience) matches client_id
4. Check exp (not expired)
5. Check nonce matches request
6. Extract user info from payload
```

**UserInfo Endpoint**:
```
Purpose: Get additional user information

Request:
GET /userinfo
Authorization: Bearer ACCESS_TOKEN

Response:
{
  "sub": "1234567890",
  "name": "John Doe",
  "given_name": "John",
  "family_name": "Doe",
  "email": "john@example.com",
  "email_verified": true,
  "picture": "https://example.com/photo.jpg",
  "locale": "en-US"
}

When to use:
- ID Token has basic info (fast)
- UserInfo for additional details (extra request)
```

**OIDC Standard Claims**:
```
Profile:
- name: Full name
- given_name: First name
- family_name: Last name
- middle_name: Middle name
- nickname: Casual name
- preferred_username: Username
- profile: Profile page URL
- picture: Profile picture URL
- website: Web page URL
- gender: Gender
- birthdate: Birth date (YYYY-MM-DD)
- zoneinfo: Time zone
- locale: Language/locale
- updated_at: Last update time

Email:
- email: Email address
- email_verified: Is email verified?

Address:
- address: Full address (object)
  - formatted: Full formatted address
  - street_address: Street
  - locality: City
  - region: State/province
  - postal_code: ZIP/postal code
  - country: Country

Phone:
- phone_number: Phone number
- phone_number_verified: Is phone verified?
```

**OIDC vs SAML**:
```
| Aspect | OIDC | SAML |
|--------|------|------|
| Format | JSON (JWT) | XML |
| Use Case | Modern web/mobile | Enterprise SSO |
| Complexity | Simple | Complex |
| Mobile | Excellent | Poor |
| REST API | Yes | No |
| Adoption | Growing | Established |

When to use:
OIDC: Modern apps, mobile, API-driven
SAML: Enterprise, legacy systems
```

### 4. SAML (Security Assertion Markup Language)

**What is SAML?**
```
Purpose: XML-based SSO (Single Sign-On) standard
Use Case: Enterprise applications, federated identity

Roles:
1. Principal: User
2. Identity Provider (IdP): Authenticates user (e.g., corporate AD)
3. Service Provider (SP): Application user wants to access
```

**SAML Flow (SP-Initiated)**:
```
1. User accesses Service Provider
   https://app.example.com

2. SP generates SAML request
   <samlp:AuthnRequest>
     <saml:Issuer>app.example.com</saml:Issuer>
     <saml:NameIDPolicy Format="email"/>
   </samlp:AuthnRequest>

3. SP redirects user to IdP
   https://idp.company.com/saml?SAMLRequest=<base64>

4. User authenticates at IdP
   (username/password, MFA, etc.)

5. IdP generates SAML assertion
   <saml:Assertion>
     <saml:Subject>
       <saml:NameID>john@company.com</saml:NameID>
     </saml:Subject>
     <saml:Conditions NotBefore="..." NotOnOrAfter="..."/>
     <saml:AttributeStatement>
       <saml:Attribute Name="email">
         <saml:AttributeValue>john@company.com</saml:AttributeValue>
       </saml:Attribute>
     </saml:AttributeStatement>
   </saml:Assertion>

6. IdP signs assertion and redirects to SP
   POST to https://app.example.com/saml/consume
   SAMLResponse=<base64 signed assertion>

7. SP validates assertion
   - Verify signature
   - Check conditions (time)
   - Extract attributes

8. SP logs user in
   Creates session for john@company.com
```

**SAML Assertion**:
```xml
<saml:Assertion Version="2.0" ID="_abc123" IssueInstant="2024-01-01T12:00:00Z">
  <saml:Issuer>https://idp.company.com</saml:Issuer>
  
  <ds:Signature>  <!-- Digital signature -->
    <ds:SignedInfo>
      <ds:CanonicalizationMethod/>
      <ds:SignatureMethod Algorithm="RSA-SHA256"/>
      <ds:Reference>
        <ds:DigestMethod Algorithm="SHA256"/>
        <ds:DigestValue>...</ds:DigestValue>
      </ds:Reference>
    </ds:SignedInfo>
    <ds:SignatureValue>...</ds:SignatureValue>
    <ds:KeyInfo>
      <ds:X509Data>
        <ds:X509Certificate>...</ds:X509Certificate>
      </ds:X509Data>
    </ds:KeyInfo>
  </ds:Signature>
  
  <saml:Subject>
    <saml:NameID Format="email">john@company.