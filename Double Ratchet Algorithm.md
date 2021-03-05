# Double Ratchet Algorithm
In [cryptography](https://en.wikipedia.org/wiki/Cryptography "Cryptography"), the **Double Ratchet Algorithm** (previously referred to as the **Axolotl Ratchet**[\[1\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-Perrin-2016-03-30-1)[\[2\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-signal-inside-and-out-2)) is a [key](https://en.wikipedia.org/wiki/Key_(cryptography) "Key (cryptography)") management algorithm that was developed by [Trevor Perrin](https://en.wikipedia.org/w/index.php?title=Trevor_Perrin&action=edit&redlink=1 "Trevor Perrin (page does not exist)") and [Moxie Marlinspike](https://en.wikipedia.org/wiki/Moxie_Marlinspike "Moxie Marlinspike") in 2013. It can be used as part of a [cryptographic protocol](https://en.wikipedia.org/wiki/Cryptographic_protocol "Cryptographic protocol") to provide [end-to-end encryption](https://en.wikipedia.org/wiki/End-to-end_encryption "End-to-end encryption") for [instant messaging](https://en.wikipedia.org/wiki/Instant_messaging "Instant messaging"). After an initial [key exchange](https://en.wikipedia.org/wiki/Key-agreement_protocol "Key-agreement protocol") it manages the ongoing renewal and maintenance of short-lived session keys. It combines a cryptographic so-called "ratchet" based on the [[Diffie-Hellman Key Exchange]](DH) and a ratchet based on a [key derivation function](Key%20Derivation%20Function) (KDF), such as a [hash function](https://en.wikipedia.org/wiki/Hash_function "Hash function"), and is therefore called a double ratchet.

The algorithm is considered self-healing because under certain conditions it prevents an attacker from accessing the cleartext of future messages after having compromised one of the user's keys.[\[3\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-advanced-ratcheting-3) New session keys are exchanged after a few rounds of communication. This effectively forces the attacker to [intercept](https://en.wikipedia.org/wiki/Man-in-the-middle_attack "Man-in-the-middle attack") _all_ communication between the honest parties, since they lose access as soon as a key exchange occurs that is not intercepted. This property was later named _Future Secrecy_, or _Post-Compromise Security_.[\[4\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-4)

## Etymology


![A gif of a ratchet moving showing that the mechanism can only move in one direction](https://upload.wikimedia.org/wikipedia/commons/thumb/1/17/Ratchet_example.gif/125px-Ratchet_example.gif)(https://en.wikipedia.org/wiki/File:Ratchet_example.gif)
A mechanical ratchet

"[Axolotl](https://en.wikipedia.org/wiki/Axolotl "Axolotl")" was in reference to the salamander's self-healing properties. The term "ratchet" in cryptography is used in analogy to a [mechanical ratchet](https://en.wikipedia.org/wiki/Ratchet_(device) "Ratchet (device)"). In the mechanical sense, a ratchet only allows advancement in one direction; a cryptographic ratchet only allows keys to be generated from the previous key. Unlike a mechanical ratchet, however, **each state is unique**.

## Properties

The Double Ratchet Algorithm features properties that have been commonly available in end-to-end encryption systems for a long time: encryption of contents on the entire way of transport as well as [authentication](https://en.wikipedia.org/wiki/Authentication "Authentication") of the remote peer and protection against manipulation of messages. As a hybrid of [DH](https://en.wikipedia.org/wiki/Diffie%E2%80%93Hellman_key_exchange "Diffie–Hellman key exchange") and [KDF](https://en.wikipedia.org/wiki/Key_derivation_function "Key derivation function") ratchets, it combines several desired features of both principles. From [OTR](https://en.wikipedia.org/wiki/Off-the-Record_Messaging "Off-the-Record Messaging") messaging it takes the properties of [forward secrecy](https://en.wikipedia.org/wiki/Forward_secrecy "Forward secrecy") and automatically reestablishing secrecy in case of compromise of a session key, forward secrecy with a compromise of the secret persistent main key, and [plausible deniability](https://en.wikipedia.org/wiki/Deniable_encryption "Deniable encryption") for the authorship of messages. Additionally, it enables session key renewal without interaction with the remote peer by using secondary KDF ratchets. An additional key-derivation step is taken to enable retaining session keys for out-of-order messages without endangering the following keys.

It is said\[_[by whom?](https://en.wikipedia.org/wiki/Wikipedia:Manual_of_Style/Words_to_watch#Unsupported_attributions "Wikipedia:Manual of Style/Words to watch")_\] to detect reordering, deletion and replay of sent messages and improve forward secrecy properties in comparison to OTR messaging.

Combined with [public key infrastructure](https://en.wikipedia.org/wiki/Public_key_infrastructure "Public key infrastructure") for the retention of pregenerated one-time keys (prekeys), it allows for the initialization of messaging sessions without the presence of the remote peer ([asynchronous communication](https://en.wikipedia.org/wiki/Asynchronous_communication "Asynchronous communication")). The usage of triple Diffie–Hellman key exchange (3-DH) as initial key exchange method improves the deniability properties. An example of this is the Signal Protocol, which combines the Double Ratchet Algorithm, prekeys, and a 3-DH handshake.[\[6\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-Unger-2015-p241-6) The protocol provides confidentiality, integrity, authentication, participant consistency, destination validation, forward secrecy, backward secrecy (aka future secrecy), causality preservation, message unlinkability, message repudiation, participation repudiation, and asynchronicity.[\[7\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-Unger-2015-p239-7) It does not provide anonymity preservation, and requires servers for the relaying of messages and storing of public key material.[\[7\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-Unger-2015-p239-7)

## Functioning

[![](https://upload.wikimedia.org/wikipedia/commons/thumb/6/64/Axolotl_ratchet_scheme%2C_legend.svg/220px-Axolotl_ratchet_scheme%2C_legend.svg.png)](https://en.wikipedia.org/wiki/File:Axolotl_ratchet_scheme,_legend.svg)

[![](https://upload.wikimedia.org/wikipedia/commons/thumb/d/d5/Axolotl_ratchet_scheme.svg/220px-Axolotl_ratchet_scheme.svg.png)](https://en.wikipedia.org/wiki/File:Axolotl_ratchet_scheme.svg)

Diagram of the working principle

A client renews session key material in interaction with the remote peer using Diffie–Hellman ratchet whenever possible, otherwise independently by using a hash ratchet. Therefore, with every message a client using the double ratchet advances one of two hash ratchets (one for sending, one receiving) which get seeded with a common secret from a DH ratchet. At the same time it tries to use every opportunity to provide the remote peer with a new public DH value and advance the DH ratchet whenever a new DH value from the remote peer arrives. As soon as a new common secret is established, a new hash ratchet gets initialized.

As cryptographic primitives, the Double Ratchet Algorithm uses

#### for the DH ratchet
Elliptic curve Diffie–Hellman (ECDH) with [Curve25519](https://en.wikipedia.org/wiki/Curve25519 "Curve25519"),

#### for [message authentication codes](https://en.wikipedia.org/wiki/Message_authentication_code "Message authentication code") (MAC, authentication)
[Keyed-Hash Message Authentication Code](https://en.wikipedia.org/wiki/Hash-based_message_authentication_code "Hash-based message authentication code") (HMAC) based on [SHA-256](https://en.wikipedia.org/wiki/SHA-256 "SHA-256"),

#### for symmetric encryption
the [Advanced Encryption Standard](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard "Advanced Encryption Standard") (AES), partially in Cipher Block Chaining [mode](https://en.wikipedia.org/wiki/Block_cipher_mode_of_operation "Block cipher mode of operation") (CBC) with [padding](https://en.wikipedia.org/wiki/Padding_(cryptography) "Padding (cryptography)") as per [PKCS](https://en.wikipedia.org/wiki/PKCS "PKCS") #5 and partially in Counter mode (CTR) without padding,

#### for the hash ratchet
HMAC.[\[8\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-Frosch-2014-8)

## Applications

The following is a list of applications that use the Double Ratchet Algorithm or a custom implementation of it:

-   [ChatSecure](https://en.wikipedia.org/wiki/ChatSecure "ChatSecure")[\[a\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-OMEMO-9)
-   [Conversations](https://en.wikipedia.org/w/index.php?title=Conversations_(software)&action=edit&redlink=1 "Conversations (software) (page does not exist)")[\[a\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-OMEMO-9)
-   [Cryptocat](https://en.wikipedia.org/wiki/Cryptocat "Cryptocat")[\[a\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-OMEMO-9)[\[9\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-10)
-   [Facebook Messenger](https://en.wikipedia.org/wiki/Facebook_Messenger "Facebook Messenger")[\[b\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-11)[\[c\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-SIGNAL-12)[\[10\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-13)
-   [G Data](https://en.wikipedia.org/wiki/G_Data_Software "G Data Software") Secure Chat[\[c\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-SIGNAL-12)[\[11\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-G_Data-14)[\[12\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-15)
-   [Gajim](https://en.wikipedia.org/wiki/Gajim "Gajim")[\[a\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-OMEMO-9)[\[d\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-Plugin-16)
-   [GNOME Fractal](https://en.wikipedia.org/wiki/GNOME_Fractal "GNOME Fractal")[\[e\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-Matrix-17)
-   [Google Allo](https://en.wikipedia.org/wiki/Google_Allo "Google Allo")[\[f\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-18)[\[c\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-SIGNAL-12)[\[13\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-Greenberg-2016-05-18-19)
-   [Haven](https://en.wikipedia.org/wiki/Haven_(software) "Haven (software)")[\[c\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-SIGNAL-12)[\[14\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-20)[\[15\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-21)
-   Pond[\[16\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-Pond-22)
-   [Element](https://en.wikipedia.org/wiki/Element_(software) "Element (software)")[\[e\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-Matrix-17)[\[17\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-23)
-   [Signal](https://en.wikipedia.org/wiki/Signal_(software) "Signal (software)")[\[c\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-SIGNAL-12)
-   [Silent Phone](https://en.wikipedia.org/wiki/Silent_Circle_(software) "Silent Circle (software)")[\[g\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-zina-24)[\[18\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-libzina-25)
-   [Skype](https://en.wikipedia.org/wiki/Skype "Skype")[\[h\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-26)[\[c\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-SIGNAL-12)[\[19\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-27)
-   [Viber](https://en.wikipedia.org/wiki/Viber "Viber")[\[i\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-28)[\[20\]](https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm#cite_note-29)

---
> https://en.wikipedia.org/wiki/Double_Ratchet_Algorithm