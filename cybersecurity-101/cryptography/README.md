## Part 1 - Cryptography Basics

Cryptography is the foundation of secure communication in the digital world.  

- **Purpose**: Ensures that data exchanged over networks cannot be read or altered by unauthorized parties.  
- **Authentication**: Confirms that we are communicating with the legitimate server or entity.  
- **Impact**: Enables trust in online interactions, from messaging apps to web browsers.  

---

## Importance of Cryptography  

Cryptography ensures **secure communication** even in the presence of adversaries by protecting:  
- **Confidentiality** – prevents unauthorized disclosure of data.  
- **Integrity** – ensures data cannot be altered without detection.  
- **Authenticity** – verifies the identity of the communicating parties.  

### Real-World Uses
- **Login credentials**: Encrypted when logging into platforms like TryHackMe.  
- **SSH**: Establishes encrypted tunnels to prevent eavesdropping.  
- **Online banking**: Certificates confirm you’re connecting to the real bank server.  
- **File downloads**: Hash functions validate files are intact and unaltered.  

### Compliance & Standards
- **PCI DSS**: Requires credit card data encryption at rest and in transit.  
- **Healthcare**: Regulations like HIPAA, HITECH (US), GDPR (EU), and DPA (UK) enforce encryption for medical records.  

Cryptography is embedded in daily digital life, often invisible to the user, but essential for secure systems and legal compliance.  

---

## Plaintext to Ciphertext  

Encryption transforms readable information (**plaintext**) into scrambled, unreadable data (**ciphertext**) using a cipher and a key. Decryption reverses the process with the correct key.  

### Key Terms
- **Plaintext**: Original readable data (text, images, files, etc.).  
- **Ciphertext**: Encrypted, unreadable version of the message.  
- **Cipher**: Algorithm that converts plaintext ↔ ciphertext. Publicly known but secure only when combined with a secret key.  
- **Key**: String of bits used in encryption/decryption. Must remain secret (except for public keys in asymmetric encryption).  
- **Encryption**: Process of turning plaintext into ciphertext using a cipher and key.  
- **Decryption**: Process of converting ciphertext back into plaintext with the correct key.  

Without the proper key, recovering the original plaintext from ciphertext should be computationally infeasible.  

---

## Historical Ciphers  

Cryptography dates back thousands of years, with early systems relying on simple substitution and transposition methods.  

### Caesar Cipher (1st century BCE)
- **Method**: Shifts each letter of the alphabet by a fixed number.  
- **Example**:  
  - Plaintext: `TRYHACKME`  
  - Key: `3` (right shift)  
  - Ciphertext: `WUBKDFNPH`  
- **Decryption**: Reverse the shift (left shift by the same key).  
- **Weakness**: Only 25 possible keys → easily broken with brute force.  
- **Status**: Considered insecure by modern standards.  

### Other Historical Ciphers
- **Vigenère Cipher (16th century)** – polyalphabetic substitution using a keyword.  
- **Enigma Machine (WWII)** – electro-mechanical cipher device used by the Germans.  
- **One-Time Pad (Cold War)** – theoretically unbreakable if used correctly (random key as long as the message, used only once).  

Historical ciphers illustrate the evolution of cryptography but are insecure by today’s standards.  

---

## Types of Encryption  

Encryption methods fall into two main categories: **symmetric** and **asymmetric**.  

### Symmetric Encryption (Private-Key Cryptography)  
- **Key usage**: Same key for encryption and decryption.  
- **Challenge**: Securely sharing the key with recipients (key distribution problem).  
- **Examples**:  
  - **DES** – 56-bit key; broken in less than 24 hours by 1999 → insecure.  
  - **3DES** – Triple application of DES, 168-bit key (effective ~112 bits); deprecated in 2019.  
  - **AES** – Standard since 2001; key sizes: 128, 192, 256 bits → modern and secure.  
- **Use case**: Password-protected files, encrypted documents, etc.  

### Asymmetric Encryption (Public-Key Cryptography)  
- **Key usage**: Two keys – **public key** (encrypt) and **private key** (decrypt).  
- **Examples**:  
  - **RSA** – 2048-bit minimum, also 3072-bit and 4096-bit.  
  - **Diffie-Hellman** – 2048-bit minimum, larger sizes for stronger security.  
  - **Elliptic Curve Cryptography (ECC)** – Provides equivalent security with shorter keys (e.g., ECC 256-bit ≈ RSA 3072-bit).  
- **Properties**:  
  - More secure against interception, but slower than symmetric encryption.  
  - Relies on hard mathematical problems (e.g., factoring, discrete logs).  
  - “Practically infeasible” to reverse without the private key.  

### Key Terms  
- **Alice & Bob**: Standard characters used in cryptography examples.  
- **Symmetric Encryption**: One shared secret key for both encryption and decryption.  
- **Asymmetric Encryption**: Public key (shared) and private key (kept secret).  

Symmetric encryption is **fast but struggles with key distribution**, while asymmetric encryption **solves distribution issues but is slower**. Modern systems often combine both.  

---

## Basic Math  

Modern cryptography relies heavily on mathematical operations. Two core building blocks are **XOR** and **Modulo**.  

### XOR Operation (⊕)  
- **Definition**: Compares two bits → returns `1` if different, `0` if the same.  
- **Truth Table**:  
  - 0 ⊕ 0 = 0  
  - 0 ⊕ 1 = 1  
  - 1 ⊕ 0 = 1  
  - 1 ⊕ 1 = 0  
- **Properties**:  
  - A ⊕ A = 0  
  - A ⊕ 0 = A  
  - Commutative: A ⊕ B = B ⊕ A  
  - Associative: (A ⊕ B) ⊕ C = A ⊕ (B ⊕ C)  
- **Use in Cryptography**:  
  - Acts as a simple symmetric cipher.  
  - If **P** = plaintext, **K** = key, then:  
    - Encryption: C = P ⊕ K  
    - Decryption: C ⊕ K = P  

 Effective only if the secret key is as long as the plaintext (like a one-time pad).  

### Modulo Operation (mod or %)  
- **Definition**: Returns the remainder after division.  
- **Examples**:  
  - 25 % 5 = 0 → (25 = 5 × 5 + 0)  
  - 23 % 6 = 5 → (23 = 3 × 6 + 5)  
  - 23 % 7 = 2 → (23 = 3 × 7 + 2)  
- **Properties**:  
  - Always returns a value between 0 and n − 1 (inclusive).  
  - Not reversible → multiple inputs can give the same result.  
- **Use in Cryptography**: Essential for algorithms involving large numbers (RSA, Diffie-Hellman, ECC).  

XOR provides **bitwise transformations**, while modulo underpins **number theory operations** in modern encryption. 

---------------------------------

## Part 2 - Public Key Cryptography Basics 

