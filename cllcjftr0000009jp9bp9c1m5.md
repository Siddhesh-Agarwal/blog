---
title: "Encryption and Emojis!"
seoTitle: "Encrypt text as emojis"
seoDescription: "Encrypt text as emojis in Python using Cryptmoji - A library written natively in Python."
datePublished: Tue Aug 15 2023 16:48:40 GMT+0000 (Coordinated Universal Time)
cuid: cllcjftr0000009jp9bp9c1m5
slug: encryption-and-emojis
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1692118030987/a058419d-2cd5-4fa3-afeb-db840583a280.png
tags: python, encryption, emoji, cryptography

---

Almost nine months ago, I published my 3rd Python library - [Cryptmoji](https://pypi.org/projects/cryptmoji). You may have come across a ton of cryptography libraries on PyPI. Many may be relatively safer, but I aimed to use "[**Caesar Cipher**](https://en.wikipedia.org/wiki/Caesar_cipher)" and "**mapping**" to make a fun tool to learn cryptography. To install Cryptmoji, run the following:

```bash
pip install cryptmoji
```

## How to use Cryptmoji

### Encryption

To encrypt text, we use the `encrypt()` function.

```python
>>> from cryptmoji import encrypt
>>> text = "H3LL0 W0RLD"
>>> encrypted = encrypt(text)
'🌾🌜🍂🍂🌙🌉🍍🌙🍈🍂🌺'
```

The output is much different than the regular encryption algorithms, isn't it?

### Decryption

To decrypt text, we use the `decrypt()` function.

```python
>>> from cryptmoji import decrypt
>>> encrypted = "🌾🌜🍂🍂🌙🌉🍍🌙🍈🍂🌺"
>>> decrypt(encrypted)
'H3LL0 W0RLD'
```

And Voila! We have our string back!

## using the `Key` parameter

As you can see, the output starts to become predictable after some time. So we need another parameter to shuffle the characters. This Parameter is `key`. This will drastically change the encrypted string.

For example:

```python
>>> from cryptmoji import encrypt, decrypt
>>> key = "HI_M0M"
>>> encrypted = encrypt("H3LL0 W0RLD", key=key)
>>> encrypted
'🎇🍲🎮🎐🍖🍣🎢🍯🎴🎐🍪'
>>> decrypt(encrypted, key=key)
'H3LL0 W0RLD'
```

For more details, refer to the [documentation](https://siddhesh.tech/cryptmoji/) and [GitHub repo](https://github.com/Siddhesh-Agarwal/cryptmoji/).