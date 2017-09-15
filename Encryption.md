# Wallet Encryption

This file explains, how wallet encryption should be implemented.

## Password hashing
When trying to unlock wallet, password should be obtained from the user. Key for AES encryption is derived from password using `sha256` hash function, salted with bytes which are hard-coded into all ElectronPass applications. Multiple hash iterations are used.

**TODO:** Be specific (how many iterations? mabye include pseudocode?)

After hashing, password string should be deleted, so the chance of stealing user's password is minimal. When wallet is locked, all sensitive data should be deleted (this includes AES key and also all data stored in wallet).

## IV generation
IV must not be reused for two message encryptions with the same key. Therefore IV should be randomly generated, using a [cryptographically secure pseudorandom number generator](https://en.wikipedia.org/wiki/Cryptographically_secure_pseudorandom_number_generator). When using 32 byte IV, `2^256` different IVs can be generated, which means that chance of collision for two following encryptions is less than `10^-77`.

## Encryption
AES using CBC mode is used for encryption.
**TODO**

## Authentication
After plain text is padded to fit into blocks for AES (and before it is encrypted), `sha256` hash of this plain text is calculated. After encryption, this hash is added to end of encrypted string.

When decrypting, success of encryption is checked by calculating hash of a decrypted message, which should be the same as hash calculated before encryption.
