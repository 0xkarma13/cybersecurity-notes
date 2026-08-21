# Cryptography Basics — My Learning Notes

I just finished a cryptography room, and I want to share what I learned, what I already knew, and the extra stuff I searched for along the way. Basically, I want you to know what I now know and enjoy this pile of information as much as I did.

## The CIA Triad

In security, there are three important things we need to protect:

- **Integrity**
- **Confidentiality**
- **Availability**

**Confidentiality** means only authorized people can access the information, nobody else. For example, a picture containing sensitive info that ends up on the internet, or a student's grade of 12 that shouldn't be changed to 20 without authorization.

**Availability** means being able to access something when you actually need it. For example, your money is in the bank and it's secure, but if the bank's website goes down, you can't do anything with your money anymore.

**Integrity** means information shouldn't be changed without authorization — it means knowing the data wasn't tampered with along the way.

These three are the prerequisite for talking about cryptography.

## Why We Need Cryptography

If I want to send something to my friend, I have to consider that if someone sits between me and my friend, they can hear everything we say — and maybe even change my message. This problem is solvable with cryptography.

I need to send my message in a way that changes it, so the original message can't be recognized from it anymore — it has to look scrambled.

## Basic Concepts

Before diving in, there are a few more concepts to know:

- **Plaintext**: the plain message, my original message
- **Ciphertext**: the encrypted text; a version of the text that's scrambled and the original message can't be identified from it at all
- **Key**: the secret factor that controls how encryption and decryption happen
- **Algorithm**: the procedure or method that tells you how to use the key to get back to the original message

The security of a system relies on keeping the key secret. Encryption algorithms are usually public and get reviewed and tested by experts worldwide. What has to stay secret is the key.

### The Box Analogy

If I want to use an analogy: imagine a box.

- The **Algorithm** is the way the box is locked — if someone sees it, it's not a problem and doesn't threaten us
- The **Key** is your personal metal key — nobody else should have it, because if someone does, they can open the box, so we need to protect it
- **Plaintext** is the letter or thing we put inside the box
- **Ciphertext** is the encrypted text — it's the box itself, sent through the mail to its destination

Broadly speaking, cryptography is about constructing and analyzing protocols that prevent third parties or the general public from reading private messages. Cryptography is the use of mathematical methods to secure information; at its core it's the science of changing a message or data using a secret key and an algorithm.

## A Bit of History and the Origin of the Word

In the past, the main focus was on confidentiality — that is, preventing **eavesdropping**: two people are talking, and a third party overhears the conversation and figures out what they're discussing. With cryptography, even if someone sees or hears the conversation, they still can't understand what it means, and the receiver uses their key to turn it back into the original message.

(A well-known example: a picture of the encryption device Germans used during World War II to encrypt important messages.)

The word **Cryptography** comes from two Greek words: *Kryptos*, meaning "hidden," and *Graphien*, meaning "to write."

## Real-World Applications

Cryptography isn't just a technology for hackers and security professionals — it's infrastructure present in banking, SSH, the web, file downloads, medicine, and pretty much all modern digital communication. That's why different standards exist for it, for example:

- **PCI DSS**: for online cards and online payments
- **HIPAA**: protects patients' private and medical information

Or when we download a file, digital certificates or **hashing** come into play too (which is a whole separate topic on its own).

Cryptography today isn't just about hiding a text message — it also covers:

- **Confidentiality**: only authorized people can view the information
- **Authentication**: making sure the other party really is who they claim to be
- **Digital Signature**: confirming that a message was actually sent by the key's owner

## Caesar Cipher

Named after Julius Caesar, who is said to have used this method to encrypt messages at the time. In this type of encryption, each letter shifts a fixed number of positions in the alphabet — that fixed number is the key.

That number — how many positions forward we go — is the key, and it needs to stay secure. The algorithm — "shift by a fixed number" — is public knowledge and isn't secret at all. The scrambled output is the **ciphertext**; the original text we feed in is the **plaintext**.

Example: our key is 3, and our input text is "Hello":

```
H → K
E → H
L → O
L → O
O → R
```

So it becomes: **KHOOR**

This is encryption, but it's not secure at all, and modern security doesn't use it — if someone doesn't have the key (3), they can brute-force it by trying all 24 possible shifts. So this is really just for learning; it's how things used to be done.

---

Now that we've covered the basics, let's get to the main topic. There are two main types of encryption: **symmetric** and **asymmetric**.

## Symmetric Encryption

Symmetric encryption came about long before asymmetric encryption. Symmetric schemes have a long history — early examples are said to date back to around 1900 BCE in ancient Egypt.

Symmetric encryption uses **one key** for both locking and unlocking — the same key that encrypts the data can also decrypt it.

### Example

I want to send my friend Bob a private message using symmetric encryption:

1. I have my message, and I have a key
2. I encrypt my message with that key and send it to Bob
3. Bob, who already has a copy of my key, uses that same key to decrypt the message and read it

Symmetric encryption is fast — it can decrypt a message using a fast algorithm — and it's also very efficient: for encrypting large files, hard drives, or network traffic.

There are several well-known algorithms for this, including:

- **AES**: widely used today
- **DES / 3DES**: outdated, no longer used

### AES (Advanced Encryption Standard)

AES is an advanced standard used to encrypt and decrypt electronic data. Its algorithm is a symmetric block cipher that processes data in 128-bit (16-byte) blocks.

**Block cipher** means: instead of encrypting the whole message at once, we break it into fixed-size chunks called blocks, and encrypt each block separately. AES works on one 128-bit block at a time.

- Key length: 128, 192, or 256 bits
- In 2001, AES replaced DES, selected by **NIST**
- A competition was held that year for encryption algorithms, with more than 15 designs submitted, and AES won
- The standard, originally called **Rijndael**, was developed by two Belgian cryptographers, **Joan Daemen** and **Vincent Rijmen** (the name Rijndael comes from combining the two creators' names)

In AES there's something called the **state**, a 4×4 grid — meaning your data is stored in a 4×4 arrangement, 16 cells total, each cell being one byte, a number from 0 to 255.

AES runs this state through 4 operations multiple times (depending on key length: 10, 12, or 14 rounds):

- **SubBytes**: swaps the value of each cell — just a substitution across those 16 cells, nothing more, no real computation involved
- **ShiftRows**: rotates the rows — each row is either left unchanged, rotated once, or rotated several positions; again, no heavy computation
- **MixColumns**: mixes each column using a fixed mathematical formula
- **AddRoundKey**: XORs the grid with the key — this is the only step that depends on the key

### Modes of Operation

Since AES can only encrypt 16 bytes at a time, for data larger than that we use one of three modes:

- **ECB (Electronic Codebook)**: the simplest and least secure; each block is encrypted independently with the same key (there's a well-known image that shows just how insecure this is)
- **CBC (Cipher Block Chaining)**: chains the blocks together; each block is XORed with the previous block before being encrypted
- **GCM (Galois/Counter Mode)**: the modern mode used today, more secure than the previous two, and does two things at once: encryption and authentication

### The Problem: Key Distribution

But with all these capabilities, we run into a problem called **key distribution**. Since this algorithm is symmetric and uses one key, the other party needs a copy of that same key too. The first time around, if I want to re-encrypt that key, I'd still have to send it to the other party somehow. This is where asymmetric encryption comes in.

## Asymmetric Encryption

Asymmetric encryption is an encryption algorithm that uses **two keys** instead of one:

- **Private key**: nobody else should have this — it's mine alone
- **Public key**: I can share this freely; it's fine if other people have it too

The interesting part:

- If someone encrypts a message with the public key, only I — since I hold the private key — can decrypt it
- Conversely, if I encrypt a message with my own private key, anyone with the public key can decrypt it

### Example

I have two keys: one public, one private (I'll explain later how to generate your own key pair). My friend Bob wants to send me a message for the first time:

1. I give him my public key, or even upload it somewhere so anyone who wants to send me a private message can use it
2. Bob, who now has my public key, encrypts the message with it and sends it to me
3. No one else can open it — only I can decrypt it with my private key; I use my private key to open and read it

Another interesting point: if a text is encrypted with the public key, only the private key can open it — even the public key itself can't open it anymore.

### TLS/HTTPS

Asymmetric encryption isn't only used for data. HTTPS websites — or, more precisely, **TLS** — use this type of encryption too.

Note: TLS is quite similar to PGP (explained further below):

- **TLS** is used to encrypt websites and secure connections
- **PGP** is used to encrypt and decrypt files or messages

Here's how a browser handles it:

1. The browser gets the information needed to authenticate the site and the site's public key
2. The site sends its public key along with a **Certificate**
3. The browser and the site use asymmetric encryption to securely agree on a shared **secret** for that session
4. Then, for transferring the actual bulk of data, symmetric encryption is used because it's much faster

This combination is what's known as **Hybrid** encryption, explained below.

### Certificates

Another thing asymmetric encryption enables is the **Certificate**, or digital certificate. Say I publish my public key on my site or anywhere else — how would other people who want to send me a private message know it's really my public key?

That's where certificates come in: my browser fetches the certificate and checks it against a trusted authority called a **CA**, which signs it. If it checks out, the connection continues; if it's invalid or expired, my browser shows a warning.

### RSA

There's an asymmetric encryption algorithm called **RSA**, named after the first letters of its creators' names: **Rivest-Shamir-Adleman**. The three of them published this algorithm publicly in 1977.

RSA's security relies entirely on a math problem: multiplying two large prime numbers together gives you a result that's public, but reversing that result back into its two original prime factors is extremely hard.

RSA uses a few specific mathematical formulas to generate both the public and private keys. In short:

1. Pick two large prime numbers and multiply them → n
2. Build a mathematical formula that connects e and d
3. Make e and n public (the public key) — keep d secret (the private key)

But despite all this security, RSA has problems:

1. It's slow and heavy, due to the multiplication and heavy math involved
2. The message being encrypted can't be larger than n

For these two reasons, RSA isn't at all suitable for encrypting large amounts of data.

## The Solution: Hybrid Encryption

That's where **Hybrid** encryption comes in — a combined approach. It's not a special or unique method on its own — it just uses the two previous approaches together.

Think of it this way:

- **AES** is fast and encrypts the whole message or file, but uses a single key
- **RSA** is slow and limited, but secure, and uses two keys

The two complement each other nicely. Here's what actually happens: RSA establishes the connection and securely delivers a copy of the AES key to the other party. AES then, now that the key has safely reached the other side via RSA, can transfer the data with both speed and strong security.

At this point, that covers pretty much all of encryption — now let's get to **PGP**.

## PGP (Pretty Good Privacy)

PGP was developed by **Phil Zimmermann** in 1991 for better information security. For more details, check out **RFC 9580**.

Important note: PGP itself doesn't have any proprietary encryption algorithm of its own. Instead, it's a **framework** that combines and coordinates several encryption algorithms to build a complete security system. PGP is a protocol/system, not an encryption algorithm.

We can work with PGP on Linux using the **gpg** tool. Installing it is easy:

```bash
sudo apt install gpg
```

To see your private keys:

```bash
gpg --list-secret-keys
```

To see your public keys:

```bash
gpg --list-keys
```

To generate your own key pair:

```bash
gpg --full-generate-key
```

This command generates a key pair. After running it, you'll be asked a series of questions — for beginners, hitting Enter to accept the defaults is fine.

After that, you can **export** your public key — meaning you can send it to someone or post it somewhere.

Here's my own public key. If you want to practice, search for how to encrypt a message with a public key, and send me something:

```
-----BEGIN PGP PUBLIC KEY BLOCK-----

mDMEaoW9bxYJKwYBBAHaRw8BAQdAiDSs6q8gm7DqEFT8ED4hCmAtKTo/ddEwBWnt
8ZKjWD20BWthcm1hiJMEExYKADsWIQSdrEF6mQy9POa/59xFSGbbql6oNgUCaoW9
bwIbAwULCQgHAgIiAgYVCgkICwIEFgIDAQIeBwIXgAAKCRBFSGbbql6oNlmeAQC1
SY2jH4PJxpjLCX1VpgNJX8efpDWcQfJsMOBV/AopFAEA2rK/OpNUKlyT8+TRXWWd
ABbR2TUh5OO2SPBotlT43Qu4OARqhb1vEgorBgEEAZdVAQUBAQdAVc4OmN7t/ouK
P1KkRjaZdPBbTfdF07wqCH9tKQJXPg8DAQgHiHgEGBYKACAWIQSdrEF6mQy9POa/
59xFSGbbql6oNgUCaoW9bwIbDAAKCRBFSGbbql6oNmymAQDVO3zY38f2SUvKzbna
6TdNDGGRWVQiIfKO/rjpkjOuwgEAyg4UAXX+7AaybMQse2tE29VNU1dr4XV7MSyj
RVU+1gA=
=r6t5
-----END PGP PUBLIC KEY BLOCK-----
```

⚠️ Important: never publish your private key anywhere.



it.karma

---

**Sources:** NIST, TryHackMe — Cryptography Concepts
