# Summary
* Substitution cipher
* Permutations
* Making good ciphers
* Data Encryption Standard (DES)
* Advanced Encryption Standard (AES)
# History
* Been around forever
* 513BC Histiaeus of Miletus shaved the slave's head, tattooed the message on it, let the hair grow to encrypt a message
- Pencil and Paper
	- Caesar's Cipher (shifting letters)
	- Cryptoquote: Simple substitution cipher, permutations of 26 letters
		- Using the dictionary and frequencies, this is also easy to break
- Mechanical Era
	- Enigma machine --> supposedly unbreakable
	- Got cracked, german messages were transparent to enemies towards end of way
	- Tunny - german invented code (Lorenz cipher system)
	- The key stream was XOR/d with plain text p to produce the cipher **c**: c = p $\oplus$ ks
	- Cracked by british cryptographers in Jan 1942 
- Modern Era
	- First major theoretical development in crypto after WWII was Shannon's Information Theory
	- This theory introduced the **one-time pad** and presented theoretical analysis of the code
	- The modern era started around the 70s
	- Driven by banks and military system requirements
	- NIST developed a set of standards for the banks, the DES
- Crypto principles see growing usage in information protection
- A locking approach
# The Role of Cryptography
- Terminology
	- Cryptology: cryptography and cryptoanalysis
	- Cryptography: art/science of keeping messages secure
	- Cryptanalysis: art/science of breaking ciphertext
	- Basic Cryptographic Scheme
		- ![[Pasted image 20250828164330.png]]
		- P is a set of plaintext, Pi is the i-th char of P
		- P = "DO NOT TELL ANYBODY", p1 = D, p2 = O...
		- By convention, cleartext in uppercase
		- C is a set of ciphertext, ci is the char within the cipher
		- C = "ep opu ufmm bozcpez" c1 = "e" ...
		- By convention, ciphertext in lowercase
	- If mathematically will take years to crack, it's generally regarded as cryptographically secure
- Benefits of Cryptography
	- Improvement, not a solution!
		- Minimizes problems
		- Doesn't solve them
			- However, there is no solution
	- Adds an *envelope* (encoding) to an open postcard (plaintext or cleartext)
- Formal Notation (following image above)
	- C=E(P)
	- P = D(C)
	- We need a cryptosystem where:
		- P = D(C) = D(E(P))
			- i.e. able to get back the original message
- Cryptography in Practice
	- Sending a secure message to hostile environment
	- Receiving a secure message from hostile environment
	- ![[Pasted image 20250828164805.png]]
- Crypto System with Keys
	- C=E(Ke, P)
		- E = set of encryption algorithms, Ke selects Ei within E
	- P = D(Kd, C)
		- D = set of decryption algorithms, Kd selects Di within D
	- We need $P = D(K_D, E(K_E,P))$ 
- Classification of Cryptosystems w.r.t. Keys
	- Keyless cryptosystems (i.e. Caesar's cipher)
		- less secure
	- Symmetric cryptosystems $K_E = K_D$
		- Classic
		- Encipher and decipher using the same key, or one key is easily derived from other
	- Asymmetric cryptosystems: $K_E \ne K_D$
		- Public key system
		- Encipher and decipher using different keys
			- Computationally infeasible to derive one from another
- Cryptanalysis
	- Goals:
		- Break a single message
		- Recognize patterns in encrypted messages, to be able to break the subsequent ones
		- Infer meaning w/o breaking encryption
			- Unusual volume of messages between enemy troops may indicate a coming attack
			- Busiest node may be enemy headquarters, etc.
		- Deduce the key to facilitate breaking subsequent messages
		- Find vulnerabilities in implementation or environment of an encryption algorithm
		- Find a general weakness in an encryption system
	- Info
		- Intercepted encrypted messages
		- Known encryption algorithms
		- Intercepted plaintext
		- Data known or suspected to be ciphertext
		- Math or statistical tool and techniques
		- Properties of natural languages (especially enemy's natural language)
		- Properties of computer systems
	- Role of ingenuity / luck
	- **There are no rules**
- Stopped at 1=grams for english method