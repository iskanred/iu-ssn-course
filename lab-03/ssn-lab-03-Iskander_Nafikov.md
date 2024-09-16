# SSN Lab-03

* **Name**: Iskander Nafikov
* **E-mail**: i.nafikov@innopolis.university
* **System**: MacOS

---
## Task 1
> Create a 2048-bit RSA key pair using OpenSSL. Write your full name in a text file and encrypt it with your private key. Using OpenSSL, extract the public modulus and the exponent from the public key. Publish your public key and the base64-formatted encrypted text in your report. Verify that it is enough for decryption.
1. First, let' create a key pair of size 2048:
	- **Input**:
		```shell
		openssl genrsa -out key.pem 2048
		```
	- **Output**:
		```shell
		
		```
2. Then, let's extract the public key:
	- **Input**:
		```shell
		openssl rsa -in key.pem -outform PEM -pubout -out public.pem
		```
	- **Output**:
		```text
		writing RSA key
		```
3. Let's check the key files are formed:
	- **Input**:
		```shell
		ls
		```
	- **Output**:
		```text
		key.pem public.pem
		```
	Actually, the public key can be found here:
	- **Input**:
		```shell
		cat public.pem
		```
	- **Output**:
		```text
		-----BEGIN PUBLIC KEY-----
		MIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEAvjfqgp1Y8/tUvhXO+xTq
		nDOA5bAs0OujXqE3KY0KX8HcycZJhfWTrvjDOXzBImk4+S2uut5t6XUrVTSY6lnX
		YrmJMSsyrw5oos3Fus8j9kC/0e1rDZ4HDJNDnWv57KC3E/YmQwAAtwADlvllm/AX
		xMQHwFgWk8MOGCk7gNxHCf3BT/lq2OSuS4eFbYOh4dCXwuZ2l5wn2K9XefLBeTEb
		EoMd7S2clA0ohz7tcPvppY1Axgm1yHLWqJjB6tebbXRN6U32hJpse9MEtptgWiZr
		JLTiAxfFrhaWJ2TvPn0xNFiOvDgxxDkmiMK9lFWpCFzns66nak6TUHdUeF5olGcC
		/wIDAQAB
		-----END PUBLIC KEY-----
		```
4. Let's create the `plaintext.txt` with my full name inside. After, we can sign it using the private key `key.pem`, the resulting ciphertext will be located inside the `ciphertext.txt` file.
	- **Input**:
		```shell
		openssl pkeyutl -sign -inkey key.pem -in plaintext.txt -out ciphertext.txt
		```
	- **Output**:
		```text
		```
5. Now we can check  the file in the base64 format
	- **Input**:
		```shell
		cat ciphertext.txt | base64
		```
	- **Output**:
		```text
		giRcNG1rG+mBL0rP526g8C5CFW47pF0NFVBfBP+LlkrwH5ogFTbKHcESN1wA1QDeo8i+8Wo0QYQdNTH3iVFn2YOHUhPR/jOfhaiO3VvAXk5yQbvm0GGSGiE+LkUQZESfSVQwT/Bpy8kpEjUHdl6h9HU/xcaP6qIUY+hf0ANC5jtxrFUW2EVyx0IT+AiDq6448h5RLpOspXPdPYkWfdyiYFuSP7cn4ZIqyaiHx4wW97Ld2+XW3gq7Li4+lkxqsK9r7Qzqpc6OmeXfz8f7eYasTSMIKrE7YIuVfRv6cM55MoHNkzegDEK4ROULzc2tv7MtpBossXkPqjrmlvElzkiRnA==
		```
6. Okay, now let's extract the modulus and the exponent from the public key:
	- **Input**:
		```shell
		openssl rsa -pubin -inform PEM -text -noout < public.pem
		```
	- **Output**:
		```text
		Public-Key: (2048 bit)
		Modulus:
		    00:be:37:ea:82:9d:58:f3:fb:54:be:15:ce:fb:14:
		    ea:9c:33:80:e5:b0:2c:d0:eb:a3:5e:a1:37:29:8d:
		    0a:5f:c1:dc:c9:c6:49:85:f5:93:ae:f8:c3:39:7c:
		    c1:22:69:38:f9:2d:ae:ba:de:6d:e9:75:2b:55:34:
		    98:ea:59:d7:62:b9:89:31:2b:32:af:0e:68:a2:cd:
		    c5:ba:cf:23:f6:40:bf:d1:ed:6b:0d:9e:07:0c:93:
		    43:9d:6b:f9:ec:a0:b7:13:f6:26:43:00:00:b7:00:
		    03:96:f9:65:9b:f0:17:c4:c4:07:c0:58:16:93:c3:
		    0e:18:29:3b:80:dc:47:09:fd:c1:4f:f9:6a:d8:e4:
		    ae:4b:87:85:6d:83:a1:e1:d0:97:c2:e6:76:97:9c:
		    27:d8:af:57:79:f2:c1:79:31:1b:12:83:1d:ed:2d:
		    9c:94:0d:28:87:3e:ed:70:fb:e9:a5:8d:40:c6:09:
		    b5:c8:72:d6:a8:98:c1:ea:d7:9b:6d:74:4d:e9:4d:
		    f6:84:9a:6c:7b:d3:04:b6:9b:60:5a:26:6b:24:b4:
		    e2:03:17:c5:ae:16:96:27:64:ef:3e:7d:31:34:58:
		    8e:bc:38:31:c4:39:26:88:c2:bd:94:55:a9:08:5c:
		    e7:b3:ae:a7:6a:4e:93:50:77:54:78:5e:68:94:67:
		    02:ff
		Exponent: 65537 (0x10001)
		```
7. Finally, let's verify that we can decrypt the `ciphetext.txt` using the `public.pem`
	- **Input**:
		```shell
		openssl rsautl -verify -inkey public.pem -pubin -in ciphertext.txt
		```
	- **Output**:
		```text
		Iskander Nafikov

		```
	So, it is exactly the content `plaintext.txt`
	*Note*: We have not used any padding algorithm or hashing digests as it should be because we have not studied it yet
## Task 2
> Assuming that you are generating a 1024-bit RSA key and the prime factors have a 512-bit length, what is the probability of picking the same prime factor twice?

According to [\[1\]](https://homes.cerias.purdue.edu/~ssw/shortage.pdf) "the number of 512-bit primes is" $\pi(2^{512}) − \pi(2^{511}) > 0.3 \times \frac{2^{512}}{{(511 \ln{2})}} \approx 11.36 \times 10^{150}$ which is smaller than usual approximation ($18.85 \times 10^{150}$) because of the facts that are discussed in the article. 
Therefore, we can conduct that if the primes were chosen uniformly and randomly, the chance of picking the same `p` and `q` or picking the same factors after several tries is negligible.
## Task 3
> Explain why using a good RNG is crucial for the security of RSA. Provide one reference to a real-world case where a poor RNG leads to a security vulnerability.
###### Answer
Using a good Random Number Generator (**RNG**) is crucial for the security of RSA and other cryptographic systems because random numbers play a significant role in key generation, prime number generation, and other critical operations. Randomness is essential to ensure the unpredictability and uniqueness of keys, making it challenging for adversaries to guess or calculate them.

For example, if some RNG would generate RSA prime factors badly only in some set of values, let's say 10_000 values only, then the large "numbers" do not play any role since Trudy can perform a forward-search attack and find such factors. Another bad case could be if the prime factors are generated predictably.
###### Real word case
As for a real-world case, one notable example is the case of the [Dual_EC_DRBG (Dual Elliptic Curve Deterministic Random Bit Generator)](https://en.wikipedia.org/wiki/Dual_EC_DRBG), which was a cryptographic random number generator standardised by the National Institute of Standards and Technology (NIST) in 2007. It was later discovered that the Dual_EC_DRBG contained a backdoor that allowed an adversary to compute the generated random numbers with a secret key. This backdoor made it possible to break the security of systems relying on this RNG, potentially compromising the security of implementations using RSA or other cryptographic schemes.

The revelation of the backdoor in Dual_EC_DRBG highlighted the significance of using a good RNG and raised concerns about the trustworthiness of cryptographic standards. It serves as a cautionary example of the potential security vulnerabilities that can arise from the use of a poor RNG in cryptographic algorithms.
## Task 4
> [Here](https://docs.google.com/document/d/1pMiWS12Po3v3VG64TogFqxM4wcM0B6YS9ouTdA3kuII/edit), you can find the modulus (public information) of two related 1024bit RSA keys. Your keys are numbered using the [list](https://docs.google.com/document/d/1qf28zZFMi7b_AR5GZs3FKzjvueYLZkmr9-HpJWYrg7c/edit). Your task is to factor them i.e. retrieve p and q. You may use any tools for this. Explain your approach.

1. First, in [list](https://docs.google.com/document/d/1qf28zZFMi7b_AR5GZs3FKzjvueYLZkmr9-HpJWYrg7c/edit) we can find the ciphered information. It is really similar to be ciphered with Caesar cipher. However, trying to decipher it with Caesar did not give me anything meaningful. Then I decided to remember the materials from the [[ssn-lab-01-Iskander_Nafikov]]. We studied the Vigenere cipher. Then I visited the Vigenere [decoder online](https://www.dcode.fr/vigenere-cipher) website and tried perform  the automatic decryption. I have found that the list is the list of numbered students in our group and the key for Vigenere is pretty simple: `KEY`.
2. My number according to list is 15 (Iskander Nafikov), so my modulus of two related 1024-bit RSA key are:
	```text
	0xbad833fc4e84da0e46409037fecf6fd931f3e49bfd720c84b396f97c1c5f6bf9c02c141fe96198e0d93aa102fbcc583ae5777508c713c9d490fab0e7b5c0fc422eef5c15e0a750f737256ca2e49e5ceb7a589c163d68851a98dd80a8a03b7dfb358f45f617a50125957180e7f3277be6f125068de46e9055e457039feef3b871
	
	0x9f6333a71576b492cb07f1d0e72011bcf1431465e175d7b1f51485716eb9d2799c177159831e642e0658a43fdfd6e6a5734bcf9eb38bb4a7de3c1a7da007096ac25a6195587ada25a4324ec958dd5978e7478408c2322248092999a5233be6397f5b46d3f04f7911d5f74750b0ddb18f38e04ac262e5f4395a8c3934d104f271
	```
3. Two different RSA keys can share the same prime factors (obviously, one of them for keys to be different). Let's suppose a prime factor `p` is common for both of two modulus. Then, we need to find $q_{1}$ and $q_{2}$ prime factors correspondingly. We can do this by finding `p` first. Since `p` is a common factor for both of the modulus we can assume that it can equal to $p = GCD(N_{1}, N_{2})$, where $N_{1}$ and $N_{2}$ are public modulus:
	```python
	import math

	N1 = int("0xbad833fc4e84da0e46409037fecf6fd931f3e49bfd720c84b396f97c1c5f6bf9c02c141fe96198e0d93aa102fbcc583ae5777508c713c9d490fab0e7b5c0fc422eef5c15e0a750f737256ca2e49e5ceb7a589c163d68851a98dd80a8a03b7dfb358f45f617a50125957180e7f3277be6f125068de46e9055e457039feef3b871", 16)
	N2 = int("0x9f6333a71576b492cb07f1d0e72011bcf1431465e175d7b1f51485716eb9d2799c177159831e642e0658a43fdfd6e6a5734bcf9eb38bb4a7de3c1a7da007096ac25a6195587ada25a4324ec958dd5978e7478408c2322248092999a5233be6397f5b46d3f04f7911d5f74750b0ddb18f38e04ac262e5f4395a8c3934d104f271", 16)
	
	p = math.gcd(N1, N2)
	print(p)
	print(N1 // p)  # q1
	print(N2 // p)  # q2
	```
4. Finally, using the script above we can obtain the values:
	```text
	p = 11271593473146056501304298827298484391816437473484078884144725104239771896412622327816582457083786784510837971257569523563791772662477718353715289835522059

	q1 = 11640474842510317474648353739025199864115448893921604572161062661021962244156208060225515240442325641511886000690809203693963822934857192797973508969775859

	q2 = 9929892691656207179161824570127757833963312942187513410651011617572879374352907428364906460408753063973638534542937000429136145519846675431088501038690547
	```
## Task 5
> Now that you have the p and q for both keys, recreate the first public and private key using [this script](https://gist.github.com/cosu/4058678). Encrypt your name with the private key, and post the public key and the base64-formatted encrypted data in your report.

Using the script we can obtain the following info
* Public key:
	```text
	-----BEGIN PUBLIC KEY-----
	MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQC62DP8ToTaDkZAkDf+z2/ZMfPk
	m/1yDISzlvl8HF9r+cAsFB/pYZjg2TqhAvvMWDrld3UIxxPJ1JD6sOe1wPxCLu9c
	FeCnUPc3JWyi5J5c63pYnBY9aIUamN2AqKA7ffs1j0X2F6UBJZVxgOfzJ3vm8SUG
	jeRukFXkVwOf7vO4cQIDAQAB
	-----END PUBLIC KEY-----
	```
* Base64-encoded encrypted data: 
	```text
	Yui2H4dModtICdx3NNKDVWKxHK7vdD2HphGqWHIODZH4FAfvMsn4yfbTfAXk205PLlpqqSWkoN/mZbb2Q6+8KqjDlfQPNViwaCHqG9A+c9vRwSmqy0VhZk6xHx4Q6MaBZeZnRGOo832Nmi8HWBsVRT3WFYLawpm+n1yCpfYivKk
	```
---
## References
1. [Is there a shortage of primes for cryptography?](https://homes.cerias.purdue.edu/~ssw/shortage.pdf)
* https://docs.openssl.org/master/man1/openssl-rsautl/
* https://en.wikipedia.org/wiki/Dual_EC_DRBG
* https://www.dcode.fr/vigenere-cipher