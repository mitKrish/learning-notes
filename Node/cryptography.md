Encryption and hashing are both cryptographic techniques used to secure data, but they serve fundamentally different purposes and have distinct characteristics. Here's a breakdown of each:

**Encryption**

* **Purpose:** To protect the **confidentiality** of data. The goal is to transform data into an unreadable format (ciphertext) so that only authorized parties with the correct decryption key can access the original data (plaintext).
* **Key Characteristic:** **Reversible**. Encrypted data can be converted back to its original form using a decryption key.
* **Process:** An encryption algorithm (cipher) is used along with a key to transform plaintext into ciphertext. The same key (in symmetric encryption) or a different but related key (in asymmetric encryption) is used for decryption.
* **Use Cases:**
    * Securing data in transit (e.g., HTTPS for web browsing, email encryption).
    * Protecting data at rest (e.g., encrypting files on a hard drive, database encryption).
    * Secure communication where only the sender and receiver should understand the message.
* **Types of Encryption:**
    * **Symmetric Encryption:** Uses the same key for both encryption and decryption (e.g., AES, DES, 3DES). It's generally faster but requires a secure way to share the key.
    * **Asymmetric Encryption:** Uses two separate but mathematically linked keys: a public key for encryption and a private key for decryption (e.g., RSA, ECC). The public key can be shared, but the private key must be kept secret. It's slower than symmetric encryption but simplifies key management.

**Hashing**

* **Purpose:** To ensure the **integrity** of data and for **authentication**. It creates a fixed-size "fingerprint" (hash value or message digest) of the input data. Any change to the original data, no matter how small, will result in a completely different hash value.
* **Key Characteristic:** **One-way (Irreversible)**. It is computationally infeasible to reverse a hash value back to its original input data.
* **Process:** A hash function takes an input (data of any size) and produces a fixed-size output (the hash value). The same input will always produce the same hash output.
* **Use Cases:**
    * **Password Storage:** Websites typically store hashed versions of passwords instead of the plaintext. When a user tries to log in, their entered password is hashed, and the result is compared to the stored hash.
    * **Data Integrity Verification:** Checking if a file has been altered during download or transmission. The hash of the original file can be compared to the hash of the received file.
    * **Digital Signatures:** Hashing is used to create a digest of a document, which is then digitally signed. This ensures both the authenticity and integrity of the document.
    * **Cryptocurrencies (e.g., Bitcoin):** Hashing is a fundamental part of blockchain technology, used to link blocks of transactions securely.
* **Properties of Cryptographic Hash Functions:**
    * **Pre-image resistance:** It should be hard to find the original input given only the hash value.
    * **Second pre-image resistance:** It should be hard to find a different input that produces the same hash as a given input.
    * **Collision resistance:** It should be hard to find two different inputs that produce the same hash value. Strong cryptographic hash functions aim for high collision resistance.
* **Common Hashing Algorithms:** SHA-256, SHA-3, BLAKE2, MD5 (MD5 is now considered cryptographically weak and should not be used for security-sensitive applications).

**Key Differences Summarized:**

| Feature          | Encryption                                  | Hashing                                      |
| ---------------- | ------------------------------------------- | -------------------------------------------- |
| **Primary Goal** | Confidentiality                             | Integrity and Authentication                 |
| **Reversibility** | Reversible (using a key)                    | Irreversible (one-way function)              |
| **Output Size** | Typically the same size or larger than input | Fixed size, usually much smaller than input |
| **Key Usage** | Requires a key for both encryption/decryption | No key is used in the hashing process itself |
| **Main Use Cases**| Protecting data in transit and at rest      | Password storage, data integrity checks, digital signatures |

In essence, **encryption is about hiding information**, while **hashing is about creating a unique and verifiable representation of information**. They are both crucial tools in cryptography but serve distinct security needs. Sometimes, they are even used together to achieve both confidentiality and integrity. For example, a message might be encrypted for confidentiality, and a hash of the message could be included to ensure its integrity upon decryption.

**Encryption**

```javascript
const crypto = require('crypto');

// Define a secret key (in a real application, this should be securely generated and managed)
const secretKey = 'thisIsAStrongSecretKey1234567890123456'; // 32 bytes for AES-256

// Define an initialization vector (IV) - should be unique and unpredictable
const iv = crypto.randomBytes(16); // 16 bytes for AES-256-CBC

function encrypt(text) {
  const cipher = crypto.createCipheriv('aes-256-cbc', Buffer.from(secretKey), iv);
  let encrypted = cipher.update(text, 'utf8', 'hex');
  encrypted += cipher.final('hex');
  return iv.toString('hex') + ':' + encrypted; // Return IV with the encrypted text
}

function decrypt(encryptedText) {
  const textParts = encryptedText.split(':');
  const ivDecrypt = Buffer.from(textParts.shift(), 'hex');
  const encryptedPayload = textParts.join(':');
  const decipher = crypto.createDecipheriv('aes-256-cbc', Buffer.from(secretKey), ivDecrypt);
  let decrypted = decipher.update(encryptedPayload, 'hex', 'utf8');
  decrypted += decipher.final('utf8');
  return decrypted;
}

const originalText = 'This is some secret information!';
console.log('Original Text:', originalText);

const encryptedText = encrypt(originalText);
console.log('Encrypted Text:', encryptedText);

const decryptedText = decrypt(encryptedText);
console.log('Decrypted Text:', decryptedText);

// Verify if decryption was successful
if (originalText === decryptedText) {
  console.log('Encryption and decryption successful!');
} else {
  console.error('Decryption failed!');
}
```


**Hashing**

You can use the built-in `crypto` module in Node.js or the Web Crypto API in browsers to create cryptographic hashes.

**Node.js Example (using `crypto` module):**

```javascript
const crypto = require('crypto');

function createHash(data, algorithm = 'sha256') {
  const hash = crypto.createHash(algorithm);
  hash.update(data);
  return hash.digest('hex'); // 'hex' for a hexadecimal representation
}

const myData = 'This is the data to be hashed.';
const sha256Hash = createHash(myData);
const md5Hash = createHash(myData, 'md5');

console.log('Original Data:', myData);
console.log('SHA-256 Hash:', sha256Hash);
console.log('MD5 Hash:', md5Hash);
```

**Common Hashing Algorithms:**

* **SHA-256 (Secure Hash Algorithm 256-bit):** A widely used strong cryptographic hash function.
* **SHA-1 (Secure Hash Algorithm 1):** Older and considered less secure for many applications due to known collision vulnerabilities.
* **MD5 (Message Digest Algorithm 5):** Also older and considered cryptographically weak due to collision vulnerabilities. Often used for non-security purposes like data integrity checks.
