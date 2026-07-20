#### TELNET

Telnet is a terminal emulation program used to access servers and input commands remotely. It was used in the past before the days of internet when security wasn't as big an issue. Because Telnet sends clear text commands, it was vulnerable to man in the middle attacks. This is why SSH was created in the 90s.

#### SSH

SSH, or Secure Shell, does the same thing that Telnet used to do, however it encrypts the data with an asymmetric encryption. Asymmetric encryption involves a pair of keys: a public key that can be shared with anyone, and a private key that must be kept secret. Anyone with the public key can encrypt a message, but only the owner of the private key can decrypt it.