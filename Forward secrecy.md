# Forward secrecy** (**FS**)
- Also known as **perfect forward secrecy** (**PFS**)
- It's a feature of specific [key agreement protocols](https://en.wikipedia.org/wiki/Key-agreement_protocol "Key-agreement protocol") that gives assurances that [session keys](https://en.wikipedia.org/wiki/Session_key "Session key") will not be compromised even if long-term secrets used in the session key exchange are compromised.
- Generate a unique session key for every session a user initiates
- A long-term secret compromise does not affect the security of past session keys

## Value
### Protects past communication
This reduces the motivation for attackers to compromise keys. For instance, if an attacker learns a long-term key, but the compromise is detected and the long-term key is revoked and updated, relatively little information is leaked in a forward secure system.

## Limit
### Diffie-Hellman Key Exchange
It's often signed by the server using a static signing key, if the adversary can obtain the key though legal or illegal actions, the adversary can masquerade as the server/client and implement a classic man-in-the-Middle attack.

### RNG
If the adversary is capable to attack the server/client and modify/predict its RNG, without getting detected, all future conversations are compromised.