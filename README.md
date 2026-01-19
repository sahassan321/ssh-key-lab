# SSH Keys in Practice: Authentication, Encryption, and Git Commit Integrity

This repository demonstrates hands-on use of SSH keys across real-world cybersecurity use cases using an Ubuntu system on AWS EC2.

The project covers:
- SSH key generation and management
- Asymmetric encryption and decryption with OpenSSL
- Git commit signing using SSH keys
- Secure key-handling best practices

> 🔐 Security Note: Any key material, fingerprints, or identifying host information has been intentionally redacted from screenshots to follow secure key-management best practices.

---

## Environment
- OS: Ubuntu (AWS EC2)
- Tools: OpenSSH, OpenSSL, Git
- Access: RDP + Terminal

---

## Part 1 — SSH Key Generation

SSH keys were generated using RSA with a 4096-bit key size.

Command used:
```bash
ssh-keygen -t rsa -b 4096 -C "CYB101 Ubuntu Key" 

```
<img width="1439" height="214" alt="cyb-pic1 1" src="https://github.com/user-attachments/assets/922132ae-8dab-4bdb-b005-92acb4b687da" />

---

## Part 2 — Asymmetric Encryption with OpenSSL

In this section, an additional RSA key pair is generated using OpenSSL and used to encrypt and decrypt a text file.
This demonstrates asymmetric encryption, where the public key encrypts data and the private key decrypts it.

### Generate RSA keys with OpenSSL

An RSA key pair was generated using OpenSSL.  
The **private key** is kept secret, while the **public key** is used to encrypt data.

Command used:
```bash
openssl genrsa -out ~/.ssh/privatekey.pem 2048
openssl rsa -in ~/.ssh/privatekey.pem -out ~/.ssh/publickey.pem -pubout

```
📸 OpenSSL key generation  
<img width="1425" height="251" alt="cyb-pic1 3" src="https://github.com/user-attachments/assets/f1acaf76-a2da-4d04-a6bf-e728c7e1fa3a" />

### Encrypting a file using the public key

A plaintext file was encrypted using the RSA public key.  
Only the corresponding private key can decrypt the file.

Command used:
```bash
openssl pkeyutl -encrypt -pubin \
-inkey ~/.ssh/publickey.pem \
-in secret.txt \
-out secret.txt.encrypted

```
📸 File encrypted using the public key  
<img width="1425" height="163" alt="cyb-pic1 4" src="https://github.com/user-attachments/assets/8a2bea01-203f-4391-b623-454e00e001bf" />

### Decrypting the file using the private key

The encrypted file was decrypted using the RSA private key, restoring the original plaintext.

Command used:
```bash
openssl pkeyutl -decrypt \
-inkey ~/.ssh/privatekey.pem \
-in secret.txt.encrypted \
-out secret.txt.decrypted

```
📸 File decrypted using the private key  
<img width="1425" height="275" alt="cyb-pic1 5" src="https://github.com/user-attachments/assets/635e6c5b-768e-43ac-84e9-f3b8c3ca7aec" />

## Part 3 — Git Commit Signing with SSH Keys

In this section, SSH keys are loaded into the SSH agent and used to cryptographically sign Git commits.
This ensures commit authenticity and verifies the identity of the author.

The SSH agent was started to manage private keys securely in memory for authentication and signing.


Command used:
```bash
eval "$(ssh-agent -s)"
```
📸 SSH agent started  
<img width="668" height="58" alt="Untitled 2" src="https://github.com/user-attachments/assets/24af89e6-f0dd-4dd5-9364-6ccfecf175e5" />
