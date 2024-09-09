# SSN Lab-01

* **Name**: Iskander Nafikov
* **E-mail**: i.nafikov@innopolis.university

---

## Task  1 - Firmware Databases
1. Extract the Microsoft certificate that belongs to the key referred to in Step 1 from the UEFI firmware, and show its text representation.
	I have checked all my certificates in the storage of `db` variable and figured out that the `dbcert-3.der` file contains `Microsoft Corporation UEFI CA 2011` certificate.
	Also, `dbcert-4.der` contains `Microsoft Windows Production PCA 2011` .
	* **Input**:
		```shell
		efi-readvar -v db -o db.esl
		sig-list-to-certs db.esl dbcert
		openssl x509 -in dbcert-4.der -text -noout 
		```
	* **Output**:
		```text
		Certificate:
		    Data:
		        Version: 3 (0x2)
		        Serial Number:
		            61:08:d3:c4:00:00:00:00:00:04
		        Signature Algorithm: sha256WithRSAEncryption
		        Issuer: C = US, ST = Washington, L = Redmond, O = Microsoft Corporation, CN = Microsoft Corporation Third Party Marketplace Root
		        Validity
		            Not Before: Jun 27 21:22:45 2011 GMT
		            Not After : Jun 27 21:32:45 2026 GMT
		        Subject: C = US, ST = Washington, L = Redmond, O = Microsoft Corporation, CN = Microsoft Corporation UEFI CA 2011
		        Subject Public Key Info:
		            Public Key Algorithm: rsaEncryption
		                RSA Public-Key: (2048 bit)
		                Modulus:
		                    00:a5:08:6c:4c:c7:45:09:6a:4b:0c:a4:c0:87:7f:
		                    06:75:0c:43:01:54:64:e0:16:7f:07:ed:92:7d:0b:
		                    b2:73:bf:0c:0a:c6:4a:45:61:a0:c5:16:2d:96:d3:
		                    f5:2b:a0:fb:4d:49:9b:41:80:90:3c:b9:54:fd:e6:
		                    bc:d1:9d:c4:a4:18:8a:7f:41:8a:5c:59:83:68:32:
		                    bb:8c:47:c9:ee:71:bc:21:4f:9a:8a:7c:ff:44:3f:
		                    8d:8f:32:b2:26:48:ae:75:b5:ee:c9:4c:1e:4a:19:
		                    7e:e4:82:9a:1d:78:77:4d:0c:b0:bd:f6:0f:d3:16:
		                    d3:bc:fa:2b:a5:51:38:5d:f5:fb:ba:db:78:02:db:
		                    ff:ec:0a:1b:96:d5:83:b8:19:13:e9:b6:c0:7b:40:
		                    7b:e1:1f:28:27:c9:fa:ef:56:5e:1c:e6:7e:94:7e:
		                    c0:f0:44:b2:79:39:e5:da:b2:62:8b:4d:bf:38:70:
		                    e2:68:24:14:c9:33:a4:08:37:d5:58:69:5e:d3:7c:
		                    ed:c1:04:53:08:e7:4e:b0:2a:87:63:08:61:6f:63:
		                    15:59:ea:b2:2b:79:d7:0c:61:67:8a:5b:fd:5e:ad:
		                    87:7f:ba:86:67:4f:71:58:12:22:04:22:22:ce:8b:
		                    ef:54:71:00:ce:50:35:58:76:95:08:ee:6a:b1:a2:
		                    01:d5
		                Exponent: 65537 (0x10001)
		        X509v3 extensions:
		            1.3.6.1.4.1.311.21.1: 
		                .....
		            1.3.6.1.4.1.311.21.2: 
		                ....k..wSJ.%7.N.&{. p.
		            X509v3 Subject Key Identifier: 
		                13:AD:BF:43:09:BD:82:70:9C:8C:D5:4F:31:6E:D5:22:98:8A:1B:D4
		            1.3.6.1.4.1.311.20.2: 
		                .
		.S.u.b.C.A
		            X509v3 Key Usage: 
		                Digital Signature, Certificate Sign, CRL Sign
		            X509v3 Basic Constraints: critical
		                CA:TRUE
		            X509v3 Authority Key Identifier: 
		                keyid:45:66:52:43:E1:7E:58:11:BF:D6:4E:9E:23:55:08:3B:3A:22:6A:A8
		
		            X509v3 CRL Distribution Points: 
		
		                Full Name:
		                  URI:http://crl.microsoft.com/pki/crl/products/MicCorThiParMarRoo_2010-10-05.crl
		
		            Authority Information Access: 
		                CA Issuers - URI:http://www.microsoft.com/pki/certs/MicCorThiParMarRoo_2010-10-05.crt
		
		    Signature Algorithm: sha256WithRSAEncryption
		         35:08:42:ff:30:cc:ce:f7:76:0c:ad:10:68:58:35:29:46:32:
		         76:27:7c:ef:12:41:27:42:1b:4a:aa:6d:81:38:48:59:13:55:
		         f3:e9:58:34:a6:16:0b:82:aa:5d:ad:82:da:80:83:41:06:8f:
		         b4:1d:f2:03:b9:f3:1a:5d:1b:f1:50:90:f9:b3:55:84:42:28:
		         1c:20:bd:b2:ae:51:14:c5:c0:ac:97:95:21:1c:90:db:0f:fc:
		         77:9e:95:73:91:88:ca:bd:bd:52:b9:05:50:0d:df:57:9e:a0:
		         61:ed:0d:e5:6d:25:d9:40:0f:17:40:c8:ce:a3:4a:c2:4d:af:
		         9a:12:1d:08:54:8f:bd:c7:bc:b9:2b:3d:49:2b:1f:32:fc:6a:
		         21:69:4f:9b:c8:7e:42:34:fc:36:06:17:8b:8f:20:40:c0:b3:
		         9a:25:75:27:cd:c9:03:a3:f6:5d:d1:e7:36:54:7a:b9:50:b5:
		         d3:12:d1:07:bf:bb:74:df:dc:1e:8f:80:d5:ed:18:f4:2f:14:
		         16:6b:2f:de:66:8c:b0:23:e5:c7:84:d8:ed:ea:c1:33:82:ad:
		         56:4b:18:2d:f1:68:95:07:cd:cf:f0:72:f0:ae:bb:dd:86:85:
		         98:2c:21:4c:33:2b:f0:0f:4a:f0:68:87:b5:92:55:32:75:a1:
		         6a:82:6a:3c:a3:25:11:a4:ed:ad:d7:04:ae:cb:d8:40:59:a0:
		         84:d1:95:4c:62:91:22:1a:74:1d:8c:3d:47:0e:44:a6:e4:b0:
		         9b:34:35:b1:fa:b6:53:a8:2c:81:ec:a4:05:71:c8:9d:b8:ba:
		         e8:1b:44:66:e4:47:54:0e:8e:56:7f:b3:9f:16:98:b2:86:d0:
		         68:3e:90:23:b5:2f:5e:8f:50:85:8d:c6:8d:82:5f:41:a1:f4:
		         2e:0d:e0:99:d2:6c:75:e4:b6:69:b5:21:86:fa:07:d1:f6:e2:
		         4d:d1:da:ad:2c:77:53:1e:25:32:37:c7:6c:52:72:95:86:b0:
		         f1:35:61:6a:19:f5:b2:3b:81:50:56:a6:32:2d:fe:a2:89:f9:
		         42:86:27:18:55:a1:82:ca:5a:9b:f8:30:98:54:14:a6:47:96:
		         25:2f:c8:26:e4:41:94:1a:5c:02:3f:e5:96:e3:85:5b:3c:3e:
		         3f:bb:47:16:72:55:e2:25:22:b1:d9:7b:e7:03:06:2a:a3:f7:
		         1e:90:46:c3:00:0d:d6:19:89:e3:0e:35:27:62:03:71:15:a6:
		         ef:d0:27:a0:a0:59:37:60:f8:38:94:b8:e0:78:70:f8:ba:4c:
		         86:87:94:f6:e0:ae:02:45:ee:65:c2:b6:a3:7e:69:16:75:07:
		         92:9b:f5:a6:bc:59:83:58

		```
2. Is this certificate the root certificate in the chain of trust? What is the role of the Platform Key (PK) and other variables?
	* We can see that the Issuer and Subject are different. Hence, this certificate is not root in the chain of trust. 
	* `PK` variable stores the key to the platform. The key as any RSA key consists of public and private keys and is stored in the NVRAM as a self-signed certificate. It is generated by a platform vendor and it is necessary to access the storage of `KEK` keys.
	* The `KEK` (Key Exchange Key) is used to update the signature database and is stored in the KEK variable. It may be used either to update the current signature databases (`db` and `dbx`) or to sign binaries for valid execution.
	* The signature database is used to validate signed efi binaries and loadable roms when the platform is operating in secure mode. It is stored in the `db` variable. The `db` variable may contain a mixed set of keys, signatures or hashes. In secure boot mode, the signature stored in the efi binary (or the SHA-256 hash if there is no signature) is compared against the entries in the database. The image will be executed if either:
		1. the image is unsigned and a SHA-256 hash of the image is in the database.
		2. the image is signed and the signature itself is in the database.
		3. the image is signed and the signing key is in the database (and the signature is valid).
	* The forbidden signatures database is used to invalidate efi binaries and loadable roms when the platform is operating in secure mode. It is stored in the `dbx` variable. The `dbx` variable may contain either keys, signatures or hashes. In secure boot mode, the signature stored in the efi binary (or computed using SHA-256 if the binary is unsigned) is compared against the entries in the database. Execution is refused if either
		1. The binary is unsigned and the SHA-256 hash of the binary is in `dbx`.
		2. The image is signed and the signature matches an entry in `dbx` .
		3. The image is signed and the key used to create the signature matches an entry in `dbx`. 
## Task2 
3. Verify that the system indeed boots the shim boot loader in the first stage. What is the full path name of this bootloader?
	To verify the shim is loaded I use `efibootmgr`:
	* **Input**:
		```shell
		efibootmgr -v | grep shim
		```
	* **Output**:
		```text
		Boot0006* ubuntu  HD(4,GPT,256f04f1-0d7a-4b70-b0db-6c8c8daa66f6,0x3a220800,0x32000)/File(\EFI\ubuntu\shimx64.efi)
		```
	
	We can see that the current boot option `Boot0006` points to `\EFI\ubuntu\shimx64.efi` which is a full path to the shim boot loader.
	Now we can check `/boot/efi/` directory and fine the `/boot/efi/EFI/ubuntu/shimx64.efi` executable.

4. Verify that the shim bootloader is indeed signed with the “Microsoft Corporation UEFI CA” key
	* Let's check it with help of `sbverify`. Since it uses PEM certificate format to check, let's firstly convert the Microsoft certificate from DER to PEM format. We can perform it using `openssl x509 -in dbcert-3.der -inform DER -out dbcert-3.pem`
	* After that, let's verify the bootloader:
		- **Input**:
			```shell
			sbverify  --cert dbcert-3.pem /boot/efi/EFI/ubuntu/shimx64.efi
			```
		- **Output**:
			```
			warning: data remaining[830784 vs 955656]: gaps between PE/COFF sections?
			Signature verification OK
			```
5. What is the exact name of the part of the binary where the actual signature is stored?
	The Authenticode signature in a PE file is in a PKCS #7 `SignedData` structure, which is stored in `Attribute Certificate Table` section of a PE file. `Attribute Certificate Table` contains binary array of Authenticode signatures. Authenticode signatures can be “embedded” in a Windows PE file, in a location specified by the `Certificate Table` entry in `PE File Header`.`Optional Header` .`Data Directories`. The actual location of the signature is inside the field `SignerInfo`.`encryptedDigest`.
6.  In what standard cryptographic format is the signature data stored?
	 **PKCS #7**
---
## References
* https://wiki.ubuntu.com/UEFI/SecureBoot
* https://blog.hansenpartnership.com/the-meaning-of-all-the-uefi-keys/
* https://habr.com/ru/articles/267953/
* https://habr.com/ru/articles/308032/
* https://www.symbolcrash.com/wp-content/uploads/2019/02/Authenticode_PE-1.pdf