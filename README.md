# Kali Linux Cryptography Lab

A professional lab project demonstrating how to update a Kali Linux system and use OpenSSL for symmetric and asymmetric encryption to protect sensitive data.

## Objectives
* Update and prepare the software packages in a Kali Linux environment.
* Implement symmetric encryption with a password to secure a text file.
* Deploy asymmetric encryption with public and private keys to protect data.

---

## Lab Implementation Steps

### 1. System Update
The Kali Linux environment was updated and upgraded to ensure all security tools and software packages were current.
```bash
sudo apt update && sudo apt upgrade -y
```

**Evidence:**
[ DROP YOUR "UPDATED KALI SYSTEM" SCREENSHOT HERE ]

---

### 2. Creating the Confidential File
A text file named `confidential.txt` was created containing fake simulation data, including mock administrative logins, passwords, and server IP addresses to act as the target sensitive data.
```bash
echo -e "--- INTERNAL NETWORK CONFIDENTIAL ---\nSYS_ADMIN_USER=net_admin\nSYS_ADMIN_HASH=\$6\$fakehash\$z8X2...\nINTERNAL_DB_IP=10.0.15.42\nAPI_KEY=sh_live_9a8b7c6d5e4f3g2h1i0j" > confidential.txt

cat confidential.txt
```

**Evidence:**
[ DROP YOUR "ORIGINAL FILE" SCREENSHOT HERE ]

---

### 3. Symmetric Encryption and Decryption
The text file was encrypted using the AES-256-CBC standard with a password to scramble the content. Afterward, the same password was used to decrypt the file back into readable text.

* **Symmetric Encryption Command:**
  ```bash
  openssl enc -aes-256-cbc -salt -pbkdf2 -in confidential.txt -out symmetric_encrypted.enc
  cat symmetric_encrypted.enc
  ```

**Evidence:**
[ DROP YOUR "SYMMETRICALLY ENCRYPTED FILE" SCREENSHOT HERE ]

* **Symmetric Decryption Command:**
  ```bash
  openssl enc -aes-256-cbc -d -salt -pbkdf2 -in symmetric_encrypted.enc -out symmetric_decrypted.txt
  cat symmetric_decrypted.txt
  ```

**Evidence:**
[ DROP YOUR "SYMMETRICALLY DECRYPTED FILE" SCREENSHOT HERE ]

---

### 4. Asymmetric Encryption and Decryption
A matching 2048-bit RSA key pair consisting of a private key (to unlock data) and a public key (to lock data) was generated. The public key was used to encrypt the file, and the private key was used to decrypt it.

* **Key Generation Command:**
  ```bash
  openssl genpkey -algorithm RSA -out private_key.pem -pkeyopt rsa_keygen_bits:2048
  openssl pkey -in private_key.pem -pubout -out public_key.pem
  ls -l *.pem
  ```

**Evidence:**
[ DROP YOUR "GENERATED PUBLIC/PRIVATE KEY PAIR" SCREENSHOT HERE ]

* **Asymmetric Encryption (Using Public Key):**
  ```bash
  openssl pkeyutl -encrypt -pubin -inkey public_key.pem -in confidential.txt -out asymmetric_encrypted.enc
  cat asymmetric_encrypted.enc
  ```

**Evidence:**
[ DROP YOUR "ASYMMETRICALLY ENCRYPTED FILE" SCREENSHOT HERE ]

* **Asymmetric Decryption (Using Private Key):**
  ```bash
  openssl pkeyutl -decrypt -inkey private_key.pem -in asymmetric_encrypted.enc -out asymmetric_decrypted.txt
  cat asymmetric_decrypted.txt
  ```

**Evidence:**
[ DROP YOUR "ASYMMETRICALLY DECRYPTED FILE" SCREENSHOT HERE ]

---

## Technical Challenges Faced

### Network Error during System Update (400 Bad Request)
* **Problem:** While running `sudo apt update`, the terminal threw multiple "400 Bad Request" errors and failed to download packages. This occurred because Kali tries to download multiple files simultaneously by default, which overwhelmed the internet connection network.
* **Resolution:** This issue was resolved by adding a configuration rule (`-o Acquire::http::Pipeline-Depth=0`) to the command line. This forced the system to download files sequentially, completely resolving the network bottleneck and allowing the system update to finish successfully.
