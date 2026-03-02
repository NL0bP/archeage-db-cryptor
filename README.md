<div id="header" align="center">
  <img src="https://i.imgur.com/NFDY376.png" width="300" alt="AAEMU"/>
  <div id="badges">
   <a href="https://discord.gg/tbQxq5uGAf">
     <img alt="Discord" src="https://img.shields.io/discord/479677351618281472?color=%235865F2&label=Discord&logo=Discord&logoColor=%23FFFFFF">
   </a>
 </div>
</div>


# Compact SQLite Cipher Tool

## Overview

A Java-based tool for decrypting and encrypting `compact.sqlite` files used in certain game versions.

The `compact.sqlite` file is actually an encrypted ZIP archive containing binary database files.

## Features

Decryption support for 3 encryption types:

- `v2` - Game versions 2.x up to 3.0.0.3
- `v3` - Game versions starting from 3.1.x
- `v4` - AAFree game versions

Encryption support: Currently **only** `v2`.

AES key calculation - Derive AES keys when correct key/IV constants are provided.

RSA key formatting - Format and clean up RSA keys from `rsa_keys.txt` file.

## File Structure

The `compact.sqlite` file is not a genuine SQLite database, but an encrypted ZIP archive containing:

* Localization binary database files
* Data binary database files

## Future Development

* v3 encryption support
* v4 encryption support

## Setup

All files must be in the program folder.

1. DB file: `compact.sqlite`;
2. JSON config like: `config_2017_trion.json`;
3. RSA keys parts: `rsa_keys.txt` (optional).

## Config file structure v2

```json
{
  "provider": "Trion",
  "version": "2.0.1.7",
  "aes_first_stage": {
    "key_constant": "HEX",
    "iv_constant": "HEX",
    "key_bit": 128
  },
  "aes_second_stage": {
    "key_constant": "HEX",
    "iv_constant": "HEX",
    "key_bit": 256
  },
  "cipher_mode": "DECRYPT"
}
```

where:

- `provider` - localization provider
- `version` - game version
- `aes_first_stage.key_constant` - 8 length HEX
- `aes_first_stage.iv_constant` - 8 length HEX
- `aes_first_stage.key_bit` - AES key bit (128, 192, 256)
- `cipher_mode` - cipher mode

## Cipher mode

- `DECRYPT` - uses for decryption
- `ENCRYPT` - uses for encryption

## Config file structure v3

```json
{
  "provider": "Kakao",
  "version": "10.8.1.0",
  "aafree": false,
  "aes_first_stage": {
    "key_constant": "HEX",
    "iv_constant": "HEX",
    "key_bit": 256
  },
  "rsa": {
    "d": "HEX",
    "n": "HEX",
    "offset_constant": "HEX",
    "parts": 10,
    "c_length": "HEX",
    "m_length": "HEX"
  },
  "aes_second_stage": {
    "key_constant": "HEX",
    "iv_constant": "HEX",
    "key_bit": 192
  },
  "cipher_mode": "DECRYPT"
}
```

where:

- `aafree` - is AAFree client (required for AAFree DB versions, optionals for officials)
- `rsa.d` - private exponent in HEX
- `rsa.n` - modulus in HEX
- `rsa.constant` - 8 length HEX
- `rsa.parts` - RSA parts
- `rsa.c_length` - 2 length HEX encrypted data length
- `rsa.m_length` - 2 length HEX decrypted data length (always less than `c_length`)

## RSA keys file structure

```text
23FFh
0BE3Eh
1291h
-
36B7h
28DCh
9AEEh
```

## GUI

### Cipher

![function_1.png](images/function_1.png)

### Calculate AES keys bit

![function_2.png](images/function_2.png)

### Format RSA keys

![function_3.png](images/function_3.png)

# Required

- [Java 21+](https://www.oracle.com/java/technologies/javase/jdk21-archive-downloads.html)

# Disclaimer

This tool is intended for educational purposes.

Users are responsible for complying with applicable laws and terms of service. The developers are not responsible for
any misuse of this software.