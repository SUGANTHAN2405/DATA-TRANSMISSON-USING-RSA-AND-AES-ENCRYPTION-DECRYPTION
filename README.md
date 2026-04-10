# CryptoBridge

A browser-based hybrid encryption system that enables secure message transmission between two separate machines using **AES-256-CBC** and **RSA-OAEP** cryptography.

---

## 🔐 What is CryptoBridge?

CryptoBridge implements a **hybrid encryption protocol** — the same cryptographic pattern used in HTTPS, PGP email, and the Signal protocol. It allows a Sender on one machine to encrypt a message, and a Receiver on a completely different machine to decrypt it — with zero data ever leaving the browser.

The Sender and Receiver are two standalone HTML files that work together.

---

## 📁 Project Files

| File | Description |
|------|-------------|
| `sender.html` | Open on the **Sender's machine** — generates keys, encrypts the message, exports a Transfer Package |
| `receiver.html` | Open on the **Receiver's machine** — paste the Private Key and Transfer Package to decrypt |

---

## ⚙️ How It Works

### Step-by-Step Flow

```
SENDER MACHINE                          RECEIVER MACHINE
──────────────────                      ──────────────────
1. Generate RSA Key Pair
   ├── Public Key ──────────────────────► (included in package)
   └── Private Key ────[USB/secure]────► Paste into receiver.html

2. Type secret message

3. Generate AES-256 Session Key + IV
   (random, single-use)

4. Encrypt:
   ├── AES-CBC encrypts the message
   └── RSA-OAEP wraps the AES key

5. Build Transfer Package (JSON)
   { ciphertext, encryptedAESKey, iv }
   └────────────[any channel]─────────► Paste into receiver.html

                                        6. Load Private Key
                                        7. Paste Transfer Package
                                        8. Click Decrypt → message revealed ✓
```

### Why Hybrid Encryption?

| Algorithm | Role | Why |
|-----------|------|-----|
| **AES-256-CBC** | Encrypts the message | Fast — handles any message size |
| **RSA-OAEP** | Encrypts the AES key | Solves key distribution — no pre-shared secret needed |

AES alone requires both parties to share a secret key in advance (risky). RSA alone is too slow for large data. Hybrid encryption combines the best of both — AES encrypts the data, RSA encrypts the AES key.

---

## 🚀 How to Use

### Requirements
- Any modern web browser (Chrome, Firefox, Edge, Safari)
- No installation, no server, no internet connection needed after page load

### Setup

1. **Download** both `sender.html` and `receiver.html`
2. **Open `sender.html`** in a browser on the Sender's machine
3. **Open `receiver.html`** in a browser on the Receiver's machine

### Sending a Message

**On the Sender's machine (`sender.html`):**
1. Click **Generate RSA Key Pair**
2. Copy the **Private Key** → transfer it to the Receiver securely (USB drive recommended)
3. Type your secret message
4. Click **Confirm Message**
5. Click **Generate AES Key + IV**
6. Click **Encrypt Message**
7. Click **Build Transfer Package** → copy or download the JSON
8. Send the JSON package to the Receiver (email, chat, USB — any channel is fine)

**On the Receiver's machine (`receiver.html`):**
1. Paste the **Private Key** received from the Sender
2. Click **Load Private Key**
3. Paste the **Transfer Package JSON**
4. Click **Parse Package**
5. Click **Decrypt Message** → original message appears

---

## 🔒 Security Properties

| Property | Detail |
|----------|--------|
| **Confidentiality** | Only the holder of the private key can decrypt the message |
| **No server involved** | All cryptographic operations run in the browser via Web Crypto API (`crypto.subtle`) |
| **Zero data leakage** | No keys, messages, or ciphertext are sent to any server |
| **Forward secrecy** | A fresh AES key is generated per session — no key reuse |
| **Safe to intercept** | The Transfer Package (JSON) is mathematically useless without the private key |

---

## 🛠️ Algorithms

- **AES-256-CBC** — Advanced Encryption Standard, 256-bit key, Cipher Block Chaining mode
- **RSA-OAEP / SHA-256** — Optimal Asymmetric Encryption Padding, 2048-bit key
- **CSPRNG** — Keys and IV generated using `crypto.getRandomValues()` (browser CSPRNG)
- **Web Crypto API** — All operations use the browser-native `crypto.subtle` — hardware-accelerated and timing-attack resistant

---

## 📌 Real-World Applications of This Pattern

- **HTTPS / TLS** — Every secure website uses hybrid encryption
- **PGP Email** — Encrypts email using the same AES + RSA pattern
- **SSH** — Uses RSA for key exchange, AES for session data
- **Signal Protocol** — End-to-end encryption for messaging

---

## 👨‍💻 Developed By

**Suganthan M & Yashwanth Sakthi M**  
Department of ECE — Networking Engineers

---

*All cryptographic operations run entirely in the browser. No data is transmitted to any external server.*
