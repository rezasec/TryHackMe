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

In secure communication, four core principles must be guaranteed:  

- **Authentication**: Confirm the identity of the party you’re communicating with.  
- **Authenticity**: Verify that the message genuinely comes from the claimed sender.  
- **Integrity**: Ensure that data is not altered or tampered with during transmission.  
- **Confidentiality**: Restrict access so that only authorized parties can read the data.  

### Everyday Analogy  
- Meeting a business partner in person:  
  - Seeing/hearing them → authentication.  
  - Knowing the words are theirs → authenticity.  
  - Ensuring their message isn’t altered → integrity.  
  - Speaking privately in a low voice → confidentiality.  

### In the Cyber Realm  
- **Symmetric (Private Key) Cryptography**: Primarily ensures confidentiality.  
- **Asymmetric (Public Key) Cryptography**: Provides strong mechanisms for authentication, authenticity, and integrity.  

Public key cryptography extends beyond secrecy; it underpins trust in digital communication.  

---

## Common Use of Asymmetric Encryption  

Asymmetric encryption is slower than symmetric encryption, so its primary role is to **securely exchange keys** for faster symmetric encryption.  

### Analogy (Lock & Box)  
- **Secret Code** → Symmetric encryption cipher and key.  
- **Lock** → Public key.  
- **Lock’s Key** → Private key.  

Process:  
1. You ask your friend (server) for a lock (public key).  
2. You place the secret code (symmetric key) inside a box and lock it.  
3. Only your friend, who has the matching private key, can unlock the box.  
4. From then on, both of you communicate securely using symmetric encryption.  

### Real-World Application  
- Asymmetric encryption is used once during the handshake to establish a shared key.  
- Symmetric encryption takes over for efficiency during the session.  
- To ensure you’re really talking to the correct party, **digital signatures and certificates** are used (covered later).  

Asymmetric cryptography solves the **key distribution problem**, while symmetric cryptography ensures **fast, ongoing communication**.  

---

## RSA  

RSA is a public-key encryption algorithm designed for secure communication over insecure channels. It is based on the mathematical difficulty of **factoring large numbers**.  

### Core Idea  
- Multiplying two large prime numbers → easy.  
- Factoring their product (n) back into primes → extremely hard and computationally infeasible with large values.  
- Security relies on the difficulty of this factorization problem.  

### Key Concepts  
- **p, q**: Large prime numbers.  
- **n** = p × q → modulus used in keys.  
- **ϕ(n)** = (p − 1)(q − 1) → Euler’s totient function.  
- **e**: Public exponent (relatively prime to ϕ(n)).  
- **d**: Private exponent, calculated so that (e × d) mod ϕ(n) = 1.  
- **Public key**: (n, e).  
- **Private key**: (n, d).  
- **Encryption**: c = m^e mod n.  
- **Decryption**: m = c^d mod n.  

### Simplified Example  
- p = 157, q = 199 → n = 31243.  
- ϕ(n) = 30888.  
- e = 163, d = 379 (since e × d ≡ 1 mod ϕ(n)).  
- Public key = (31243, 163).  
- Private key = (31243, 379).  
- Message m = 13 → Ciphertext c = 16341.  
- Decryption restores m = 13.  

### RSA in CTFs  
- Common challenge variables:  
  - **p, q** → primes.  
  - **n** → modulus (p × q).  
  - **e** → public exponent.  
  - **d** → private exponent.  
  - **m** → plaintext.  
  - **c** → ciphertext.  
- Tools:  
  - **RsaCtfTool**  
  - **rsatool**  
- Typical task: Given some values, break RSA or recover the plaintext flag.  

RSA demonstrates the power of public-key cryptography, but in practice, extremely large primes are required to ensure security.  

---

## Diffie-Hellman Key Exchange  

One challenge of symmetric encryption is securely sharing the key. **Diffie-Hellman (DH)** solves this by allowing two parties to establish a shared secret over an insecure channel, without prior secrets and without an eavesdropper being able to derive the key.  

### Process  
1. **Public agreement**: Both parties agree on a large prime number `p` and a generator `g` (both public).  
2. **Private keys**:  
   - Alice chooses a private key `a`.  
   - Bob chooses a private key `b`.  
3. **Public keys**:  
   - Alice computes `A = g^a mod p`.  
   - Bob computes `B = g^b mod p`.  
4. **Key exchange**: Alice and Bob send each other their public keys (A, B).  
5. **Shared secret**:  
   - Alice computes `B^a mod p`.  
   - Bob computes `A^b mod p`.  
   - Both results equal `g^(ab) mod p` → the shared secret.  

### Example (simplified values)  
- p = 29, g = 3  
- Alice: a = 13 → A = 3^13 mod 29 = 19  
- Bob: b = 15 → B = 3^15 mod 29 = 26  
- Shared secret:  
  - Alice: 26^13 mod 29 = 10  
  - Bob: 19^15 mod 29 = 10  
- Final shared key = 10  

### Notes  
- Small values (like above) are insecure; in practice, very large primes are used.  
- **Diffie-Hellman + RSA**:  
  - DH → secure key agreement.  
  - RSA → authentication and digital signatures (proving identity).  
  - Together, they prevent **man-in-the-middle (MITM)** attacks.  
- Widely incorporated into modern security protocols (e.g., TLS).  

Diffie-Hellman allows secure symmetric key setup over insecure channels, forming the basis of many cryptographic protocols.  

---

## SSH  

SSH (Secure Shell) relies on public key cryptography for **server authentication** and **client authentication**.  

### Authenticating the Server  
- On first connection, the SSH client prompts to confirm the server’s **public key fingerprint** (e.g., ED25519).  
- This prevents **man-in-the-middle attacks**, where a malicious server pretends to be the target.  
- Once accepted, the server’s fingerprint is stored in `known_hosts` for future connections.  

### Authenticating the Client  
- Users may log in with **username/password** (less secure) or **public/private key pairs** (best practice).  
- Keys are typically generated with `ssh-keygen`. Supported algorithms include:  
  - **RSA**  
  - **DSA (Digital Signature Algorithm)**  
  - **ECDSA (Elliptic Curve DSA)**  
  - **ECDSA-SK / Ed25519-SK** (hardware-backed)  
  - **Ed25519** (modern, efficient, secure)  

----------------
# Digital Signatures and Certificates  

In the digital world, handwritten signatures and stamps are replaced by **digital signatures** and **certificates**, powered by asymmetric cryptography.  

## Digital Signatures  
- **Purpose**: Verify authenticity (who created/modified a file) and integrity (file not altered).  
- **How it works**:  
  - A sender signs data using their **private key**.  
  - Anyone can verify the signature using the sender’s **public key**.  
  - This proves the sender’s identity, since only they control the private key.  
- **Example**:  
  - Bob signs a message → encrypts its hash with his private key.  
  - Alice verifies by decrypting with Bob’s public key and comparing hashes.  
- **Important Note**: Simply pasting an image of a handwritten signature = not a true digital signature (provides no integrity check).  
- **Legal Value**: In many jurisdictions, digital signatures carry the same legal weight as physical signatures.  

## Certificates (Proof of Identity)  
- Certificates extend digital signatures to prove **who an entity is**.  
- Widely used in **HTTPS** to prove that a web server is authentic.  
- Certificates rely on a **chain of trust**:  
  - Website certificate → signed by an intermediate authority.  
  - Intermediate authority → trusted by a root CA (Certificate Authority).  
  - Root CAs → trusted by your OS/browser by default.  
- This chain ensures your browser can trust the server’s certificate.  

## TLS Certificates  
- Required to enable HTTPS on websites.  
- Options:  
  - Paid certificates from established CAs.  
  - Free certificates via **Let’s Encrypt** (commonly used for modern websites).  
- Without a trusted certificate, browsers will warn users that a site is insecure.  

Digital signatures ensure **integrity and authenticity**, while certificates provide a **trusted identity** for secure communication (e.g., HTTPS).  

---------

# PGP and GPG  

## What They Are  
- **PGP (Pretty Good Privacy)**: Proprietary software for encryption, signing, and secure communication.  
- **GPG (GNU Privacy Guard)**: Open-source implementation of the OpenPGP standard.  

## Uses  
- **Email security**: Encrypt emails for confidentiality.  
- **Digital signatures**: Verify integrity and authenticity of messages.  
- **File encryption**: Protect sensitive documents.  

-------

## Part 3 - Hashing basics

