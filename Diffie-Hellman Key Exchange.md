# Diffie-Hellman Key Exchange
It's a method of securely exchanging [cryptographic keys](https://en.wikipedia.org/wiki/Cryptographic_key "Cryptographic key") over a public channel.

It allows two parties that have no prior knowledge of each other to jointly establish a [shared secret](https://en.wikipedia.org/wiki/Shared_secret "Shared secret") key over an [insecure channel](https://en.wikipedia.org/wiki/Insecure_channel "Insecure channel"). This key can then be used to encrypt subsequent communications using a [symmetric key](https://en.wikipedia.org/wiki/Symmetric_key "Symmetric key") [cipher](https://en.wikipedia.org/wiki/Cipher "Cipher").

Research published in October 2015 suggests that the parameters in use for many DH Internet applications at that time are not strong enough to prevent compromise by very well-funded attackers, such as the security services of some countries.[\[3\]](https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange#cite_note-imperfectfs-4)


Itself is a non-authenticated [key-agreement protocol](https://en.wikipedia.org/wiki/Key-agreement_protocol "Key-agreement protocol"), it provides the basis for a variety of authenticated protocols, and is used to provide [[forward secrecy]] in [Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security "Transport Layer Security")'s [ephemeral](https://en.wikipedia.org/wiki/Ephemeral_key "Ephemeral key") modes (referred to as EDH or DHE depending on the [cipher suite](https://en.wikipedia.org/wiki/Cipher_suite "Cipher suite")).
The method was followed shortly afterwards by [RSA](https://en.wikipedia.org/wiki/RSA_(algorithm) "RSA (algorithm)"), an implementation of public-key cryptography using asymmetric algorithms.

[![](https://upload.wikimedia.org/wikipedia/commons/thumb/4/4c/Public_key_shared_secret.svg/250px-Public_key_shared_secret.svg.png)](https://en.wikipedia.org/wiki/File:Public_key_shared_secret.svg)
In the Diffie–Hellman key exchange scheme, each party generates a public/private key pair and distributes the public key. After obtaining an authentic copy of each other's public keys, Alice and Bob can compute a shared secret offline. The shared secret can be used, for instance, as the key for a [symmetric cipher](https://en.wikipedia.org/wiki/Symmetric-key_algorithm "Symmetric-key algorithm").


# Description

[![](https://upload.wikimedia.org/wikipedia/commons/thumb/4/46/Diffie-Hellman_Key_Exchange.svg/250px-Diffie-Hellman_Key_Exchange.svg.png)](https://en.wikipedia.org/wiki/File:Diffie-Hellman_Key_Exchange.svg)

Illustration of the concept behind Diffie–Hellman key exchange

---
>https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange