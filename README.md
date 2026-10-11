# Network-Security-lab

## Project Title: Network Security Lab: Integrity, Encryption, and Packet Analysis
**Description: A hands-on demonstration of file integrity using SHA-256, file encryption using GPG, and analyzing the transmission of secure vs. insecure files over HTTP using Wireshark.**

### 📌 Project Overview
This project demonstrates the fundamental concepts of Confidentiality and Integrity in cybersecurity. It simulates a scenario where a user transmits sensitive data over a local network. We compare the transmission of a plaintext file against a GPG-encrypted file, intercepting the traffic with Wireshark to analyze what an attacker could see.

### 🛠️ Tools Used
-OS: Linux (Ubuntu/Debian based)

-Integrity: sha256sum

-Encryption: gpg (GNU Privacy Guard)

-Server: Python 3 http.server

-Analysis: Wireshark

### 🚀 Step-by-Step Implementation
1. Creating the "Secret" Data
First, we create a sensitive file and verify its integrity using a SHA-256 hash. This ensures that if the file is modified in transit, the hash will change, alerting the receiver.

`bash
echo "This is a highly confidential message containing secret keys." > secret.txt
sha256sum secret.txt
Output Hash: ee9a4be92efc841b993454012a95bd0b7601cfd53be2063ba6003c546c968485 `

2. Encrypting the File (Confidentiality)
We use GPG to encrypt the file. This renders the file unreadable to anyone who does not possess the private key.

`bash
gpg -c secret.txt`
** This generates secret.txt.gpg **

3. Hosting the Files (The Python Server)
To simulate a transmission, we host the directory using a simple Python HTTP server.

`bash
python3 -m http.server 8000`
4. Capturing Traffic (The Attack Simulation)
We use Wireshark to capture traffic on the loopback interface (lo). We then use curl to request the files from the server.

** Requesting the Plaintext File:**

`bash
curl http://127.0.0.1:8000/secret.txt`
Result in Wireshark: The plaintext message is fully visible.

This is a highly confidential message containing secret keys.

**Requesting the Encrypted File:**

`bash
curl http://127.0.0.1:8000/secret.txt.gpg --output downloaded_secret.gpg`
Result in Wireshark: The payload is scrambled and unreadable (ciphertext).

### 🔍 Analysis & Results
The Vulnerability of HTTP
As seen in the Wireshark capture (tcp.stream eq 38), transmitting raw text over HTTP is dangerous. The Content-Type is text/plain, and the payload is human-readable.

The Protection of Encryption
When transmitting the .gpg file (tcp.stream eq 34), the Content-Type changes to application/octet-stream. The payload appears as random bytes (binary data). Even if an attacker intercepts this packet, they cannot read the contents without the GPG passphrase.

### 🏁 Conclusion
This lab proves that while hashing ensures data Integrity (detecting changes), encryption ensures Confidentiality (preventing unauthorized reading). Transmitting plaintext over a network is equivalent to sending a postcard; anyone handling it can read it. GPG encryption turns that postcard into a locked safe.
