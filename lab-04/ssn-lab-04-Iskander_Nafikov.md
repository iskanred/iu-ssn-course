# SSN Lab-04

* **Name**: Iskander Nafikov
* **E-mail**: i.nafikov@innopolis.university
* **System**: MacOS

---
## Task 1
> What is PKI, and what is the purpose of PKI with OpenVPN?

* A public key infrastructure, or **PKI**, is the sum total of everything required to securely use public keys in the real world. More formally, The Public key infrastructure (PKI) is the set of hardware, software, policies, processes, and procedures required to create, manage, distribute, use, store, and revoke digital certificates and public-keys. Any PKI must deal with the following issues:
	* Key generation and management
	* Certificate authorities (**CA**s)
	* Certificate revocation
* PKI uses digital certificates to authenticate users and devices. When a client tries to connect to an OpenVPN server, it presents its certificate, which can be verified against a trusted Certificate Authority (CA). This ensures that only authorized users can connect. At the same time a client can use digital certificates to verify the server!

> Distinguish between a master certificate authority (CA) and a separate certificate authority (CA).

The answer below is well-structured, well-explained and brief, so although it was generated by ChatGPT, I will put it here.
###### Master Certificate Authority (CA)
1. **Hierarchy**: A Master CA is typically at the top of a hierarchical PKI structure. It is responsible for issuing and managing the certificates for subordinate (or child) CAs. This established hierarchy can enhance security and manageability of the PKI.
2. **Trust Anchor**: The Master CA acts as a trust anchor in the PKI. Its root certificate is trusted by clients, systems, and applications, forming the foundation of trust in the PKI environment.
3. **Certificate Issuance**: The Master CA generally issues a limited number of highly trusted root certificates and manages the certificate policies and practices. It may not issue end-user or device certificates directly but delegates that authority to subordinate CAs.
4. **Long-Term Security**: The Master CA’s private key is usually stored securely and is rarely used, as it is crucial to the overall security of the PKI. It may be kept offline most of the time to mitigate risks.
5. **Scope and Authority**: It operates with wide authority and generally covers a broad scope across various systems and applications.
###### Separate Certificate Authority (CA)
1. **Hierarchy**: A Separate CA usually refers to subordinate or intermediate CAs within the PKI hierarchy. These CAs are certified by the Master CA and operate under its policies.
2. **Specific Roles**: A Separate CA is responsible for issuing certificates to end entities (like users, devices, or applications). They may specialize in certain types of certificates or services but follow the policies set by the Master CA.
3. **Certificate Issuance**: Unlike the Master CA, a Separate CA actively issues end-user certificates, managing their lifecycle including issuance, renewal, and revocation.
4. **Less Sensitive Key Management**: The private key of a Separate CA may be used more frequently compared to the Master CA, and it is generally kept more accessible for ease of issuing certificates while still within a secure environment.
5. **Localized Trust**: A Separate CA may serve a specific organization or business unit, providing certificates for internal resources, and it operates under the trust extended by the Master CA.

In summary, a **Master** CA is responsible for maintaining the overall integrity and trust.
## Task 2
> Install, build, and initialize a Public Key Infrastructure for an installed OpenVPN

* **Input**
	```shell
	./easyrsa init-pki
	```
* **Output**
	```text
	init-pki complete; you may now create a CA or requests.
	Your newly created PKI dir is: /etc/openvpn/easy-rsa/pki
	
	```
* **Input**
	```shell
	./easyrsa build-ca
	```
* **Output**
	```text
	Using SSL: openssl OpenSSL 3.0.2 15 Mar 2022 (Library: OpenSSL 3.0.2 15 Mar 2022)

	Enter New CA Key Passphrase:
	Re-Enter New CA Key Passphrase: 
	You are about to be asked to enter information that will be incorporated
	into your certificate request.
	What you are about to enter is what is called a Distinguished Name or a DN.
	There are quite a few fields but you can leave some blank
	For some fields there will be a default value,
	If you enter '.', the field will be left blank.
	-----
	Common Name (eg: your user, host, or server name) [Easy-RSA CA]:srvuser
	
	CA creation complete and you may now import and sign cert requests.
	Your new CA certificate file for publishing is at:
	/etc/openvpn/easy-rsa/pki/ca.crt
	```
> What did you notice about the prompt when creating a CA? What is the reason behind creating and using a passphrase?

I was suggested to enter the passphrase and the name of certificate's owner. The primary purpose of a passphrase is to add an additional layer of security to the private key. Even if an attacker gains access to the private key file, they would still need to know the passphrase to use that key. This greatly reduces the risk of unauthorized access.

> Generate Diffie Hellman parameters and key pair for the server. Show the location(path) of the key pair generated
* **Input**
	```shell
	./easyrsa gen-dh
	```
* **Output**
	```text
	Using SSL: openssl OpenSSL 3.0.2 15 Mar 2022 (Library: OpenSSL 3.0.2 15 Mar 2022)
	Generating DH parameters, 2048 bit long safe prime
	..........................................................................................................+...............................................................................................................................................................................................................................................................................+....................................++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*++*
	
	DH parameters of size 2048 created at /etc/openvpn/easy-rsa/pki/dh.pem
	```

The location is `/etc/openvpn/easy-rsa/pki/dh.pem`.

> What is the size of the Diffie Hellman parameters and their locations?

The size according to output is 2048 bits and the location is `/etc/openvpn/easy-rsa/pki/dh.pem`

> Create a certificate for the server. What is the commonName value, and how many days are the certificates signed on the server?

* **Input**
	```shell
	./easyrsa sign-req server myservername
	```
* **Output**
	```text
	Using SSL: openssl OpenSSL 3.0.2 15 Mar 2022 (Library: OpenSSL 3.0.2 15 Mar 2022)
	You are about to sign the following certificate.
	Please check over the details shown below for accuracy. Note that this request
	has not been cryptographically verified. Please be sure it came from a trusted
	source or that you have verified the request checksum with the sender.
	
	Request subject, to be signed as a server certificate for 825 days:
	
	subject=
	    commonName                = srvuser
	
	
	Type the word 'yes' to continue, or any other input to abort.
	  Confirm request details: yes
	
	Using configuration from /etc/openvpn/easy-rsa/pki/easy-rsa-5292.6ZemZJ/tmp.MUEARc
	Enter pass phrase for /etc/openvpn/easy-rsa/pki/private/ca.key:
	40B760B3F57F0000:error:0700006C:configuration file routines:NCONF_get_string:no value:../crypto/conf/conf_lib.c:315:group=<NULL> name=unique_subject
	Check that the request matches the signature
	Signature ok
	The Subject's Distinguished Name is as follows
	commonName            :ASN.1 12:'srvuser'
	Certificate is to be certified until Dec 27 19:25:52 2026 GMT (825 days)
	
	Write out database with 1 new entries
	Data Base Updated
	
	Certificate created at: /etc/openvpn/easy-rsa/pki/issued/srvuser.crt

	```

We have created and signed the server's certificate. The value of `commonName` is `srvuser`. The certificate is signed for 825 days.

> Create a key for the client and also a client certificate. What is the expiration date of the certificate?
* **Input**
	```shell
	./easyrsa gen-req client nopass
	```
* **Output**
	```text
	Using SSL: openssl OpenSSL 3.0.2 15 Mar 2022 (Library: OpenSSL 3.0.2 15 Mar 2022)
	.....+...+......+.....+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*..+...............+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*.+..+...............+......+.+...+.....+...+....+...+......+...........+...+.+..+...+......+..........+.....+......+.............+......+.........+..+....+...+...............+.....+.+............+.....+..........+........+......+.+...+............+............+......+..............+.+..........................+...+.......+..................+..+....+.........+..+.+...+..+.......+..+......+...+.+...+...+...+..+...+....+.....+................+............+..+......+......+.........+................+.....................+........+......+....+....................+.........+......+.+...+...+........+....+.....+................+..+....+...+...+..+..........+...+..+.+......+.....+.........+.+.....+......+....+..........................+....+.....+...+.......+...+........+....+.........+......+......+..+............+.........+.+.........+..+...+..........+......+...+......+..+...................+...+..+..........+.....+...+...+..........+..+......+.......+.................+...+....+...+..+....+..+....+...+......+..+....+.....+....+.....+...............+.+.....+...+......+.+......+...+.........+...+...+...........+...+.+..+..........+..+.+.....+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++......+.+...+.........+......+.........+........+...+.+...........+...+.+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*....+.....+......+.+..+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++*...+......+......+.......+..+....+.....+....+...........................+.....+....+..+....+...+........+...............+...............+............+...+...+.........+......+...............+.+.........+..+....+...+..+++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
	-----
	You are about to be asked to enter information that will be incorporated
	into your certificate request.
	What you are about to enter is what is called a Distinguished Name or a DN.
	There are quite a few fields but you can leave some blank
	For some fields there will be a default value,
	If you enter '.', the field will be left blank.
	-----
	Common Name (eg: your user, host, or server name) [client]:client  
	
	Keypair and certificate request completed. Your files are:
	req: /etc/openvpn/easy-rsa/pki/reqs/client.req
	key: /etc/openvpn/easy-rsa/pki/private/client.key

	```
* **Input**
	```shell
	./easyrsa sign-req client client
	```
* **Output**
	```text
	Using SSL: openssl OpenSSL 3.0.2 15 Mar 2022 (Library: OpenSSL 3.0.2 15 Mar 2022)
	You are about to sign the following certificate.
	Please check over the details shown below for accuracy. Note that this request
	has not been cryptographically verified. Please be sure it came from a trusted
	source or that you have verified the request checksum with the sender.
	
	Request subject, to be signed as a client certificate for 825 days:
	
	subject=
	    commonName                = client
	
	Type the word 'yes' to continue, or any other input to abort.
	  Confirm request details: yes
	Using configuration from /etc/openvpn/easy-rsa/pki/easy-rsa-5443.g9edeQ/tmp.rNhLlK
	Enter pass phrase for /etc/openvpn/easy-rsa/pki/private/ca.key:
	Check that the request matches the signature
	Signature ok
	The Subject's Distinguished Name is as follows
	commonName            :ASN.1 12:'client'
	Certificate is to be certified until Dec 27 19:33:22 2026 GMT (825 days)
	
	Write out database with 1 new entries
	Data Base Updated
	
	Certificate created at: /etc/openvpn/easy-rsa/pki/issued/client.crt
	```

The expiration date for the client's certificate is `Dec 27 19:33:22 2026`

## Task 3
> What is the TLS Authentication (TA) Key, and why is it essential in OpenVPN?

**TLS authentication** requires one or both parties to prove their identity using TLS certificates.

The **TLS Authentication (TA) Key** is a pre-shared key used in OpenVPN to enhance security during the TLS handshake process. By requiring the correct key, it prevents unauthorized access, mitigates DoS attacks, and helps defend against MITM attacks. The TA key ensures that only authenticated packets can access the control channel, where sensitive information, including encryption keys, is exchanged. This protection is crucial for maintaining the integrity and confidentiality of the VPN connection.

> Generate TLS Authentication (server) and show the server key information

* **Input**
	```shell
	sudo openvpn --genkey secret ta.key
	```
* **Output**
	```text
	```
* **Input**
	```shell
	cat ta.key
	```
* **Output**
	```text
	#
	# 2048 bit OpenVPN static key
	#
	-----BEGIN OpenVPN Static key V1-----
	070db675872a8a4936b1647334454afa
	645d84edcd05559ce7a6fbb1f139d954
	d6fb03f5955eda67dd487b715640fd2d
	a05629938bf168d389957d08afb8182b
	058c83455541b5cd9c8c0f488fbab572
	744edf9a22de78a2be583f59f24a6e2d
	3cce4f91be5ee14033bc1478a5cc5ac2
	6db4f73772f4684e91aeda6a90fed65e
	ba9c12790e9d05e59107653ffa9a3fcd
	b979db3af73c8e42db22d6bcf60ec0d2
	95df9958c991e8990a2db286a4d71d29
	bd2933f9d9c0c12e08c60013f5ed9800
	3f810f3ad99886009cbf6148368fea50
	8eb0f0c8ae3db473a7079bfe61a89aff
	4d6182538b915731a9dc2838adb6761d
	db38fc5fcc7ddcb697b229c8958b6ce4
	-----END OpenVPN Static key V1-----

	```

> Generate a TLS Authentication key (client) and show the client key information

Since the key is a shared secret the key is not generated on the client side, but rather securely transferred from the server

## Task 4
> Simulate a communication between the OpenVPN server and the and client. Both the client and the server were installed on the same machine.

I have set up client and server openvpn daemons using the default (example) configurations.
* On the client side I can execute `ip route` and obtain:
	```
	...
	10.8.0.0/24 via 10.8.0.2 dev tun0 
	10.8.0.1 via 10.8.0.5 dev tun1 
	10.8.0.2 dev tun0 proto kernel scope link src 10.8.0.1 
	10.8.0.5 dev tun1 proto kernel scope link src 10.8.0.6
	...
	```
* The logs of the client process
	```
	сен 23 23:36:11 lenovo ovpn-client[6620]: Outgoing Control Channel Authentication: Using 160 bit message hash 'SHA1' for HMAC authentication
	сен 23 23:36:11 lenovo ovpn-client[6620]: Incoming Control Channel Authentication: Using 160 bit message hash 'SHA1' for HMAC authentication
	сен 23 23:36:11 lenovo ovpn-client[6620]: TCP/UDP: Preserving recently used remote address: [AF_INET]127.0.0.1:1194
	сен 23 23:36:11 lenovo ovpn-client[6620]: Socket Buffers: R=[212992->212992] S=[212992->212992]
	сен 23 23:36:11 lenovo ovpn-client[6620]: UDP link local: (not bound)
	сен 23 23:36:11 lenovo ovpn-client[6620]: UDP link remote: [AF_INET]127.0.0.1:1194
	сен 23 23:36:11 lenovo ovpn-client[6620]: TLS: Initial packet from [AF_INET]127.0.0.1:1194, sid=03d1242c 866514be
	сен 23 23:36:11 lenovo ovpn-client[6620]: VERIFY OK: depth=1, CN=srvuser
	сен 23 23:36:11 lenovo ovpn-client[6620]: VERIFY KU OK
	сен 23 23:36:11 lenovo ovpn-client[6620]: Validating certificate extended key usage
	сен 23 23:36:11 lenovo ovpn-client[6620]: ++ Certificate has EKU (str) TLS Web Server Authentication, expects TLS Web Server Authentication
	сен 23 23:36:11 lenovo ovpn-client[6620]: VERIFY EKU OK
	сен 23 23:36:11 lenovo ovpn-client[6620]: VERIFY OK: depth=0, CN=srvuser
	сен 23 23:36:11 lenovo ovpn-client[6620]: Control Channel: TLSv1.3, cipher TLSv1.3 TLS_AES_256_GCM_SHA384, peer certificate: 2048 bit RSA, signature: RSA-SHA256
	сен 23 23:36:11 lenovo ovpn-client[6620]: [srvuser] Peer Connection Initiated with [AF_INET]127.0.0.1:1194
	сен 23 23:36:11 lenovo ovpn-client[6620]: PUSH: Received control message: 'PUSH_REPLY,route 10.8.0.1,topology net30,ping 10,ping-restart 120,ifconfig 10.8.0.6 10.8.0.5,peer-id 0,cipher AES-256-GCM'
	сен 23 23:36:11 lenovo ovpn-client[6620]: OPTIONS IMPORT: timers and/or timeouts modified
	сен 23 23:36:11 lenovo ovpn-client[6620]: OPTIONS IMPORT: --ifconfig/up options modified
	сен 23 23:36:11 lenovo ovpn-client[6620]: OPTIONS IMPORT: route options modified
	сен 23 23:36:11 lenovo ovpn-client[6620]: OPTIONS IMPORT: peer-id set
	сен 23 23:36:11 lenovo ovpn-client[6620]: OPTIONS IMPORT: adjusting link_mtu to 1624
	сен 23 23:36:11 lenovo ovpn-client[6620]: OPTIONS IMPORT: data channel crypto options modified
	сен 23 23:36:11 lenovo ovpn-client[6620]: Data Channel: using negotiated cipher 'AES-256-GCM'
	сен 23 23:36:11 lenovo ovpn-client[6620]: Outgoing Data Channel: Cipher 'AES-256-GCM' initialized with 256 bit key
	сен 23 23:36:11 lenovo ovpn-client[6620]: Incoming Data Channel: Cipher 'AES-256-GCM' initialized with 256 bit key
	сен 23 23:36:11 lenovo ovpn-client[6620]: net_route_v4_best_gw query: dst 0.0.0.0
	сен 23 23:36:11 lenovo ovpn-client[6620]: net_route_v4_best_gw result: via 172.20.10.1 dev wlp2s0
	сен 23 23:36:11 lenovo ovpn-client[6620]: ROUTE_GATEWAY 172.20.10.1/255.255.255.240 IFACE=wlp2s0 HWADDR=e4:aa:ea:ce:18:09
	сен 23 23:36:11 lenovo ovpn-client[6620]: TUN/TAP device tun1 opened
	сен 23 23:36:11 lenovo ovpn-client[6620]: net_iface_mtu_set: mtu 1500 for tun1
	сен 23 23:36:11 lenovo ovpn-client[6620]: net_iface_up: set tun1 up
	сен 23 23:36:11 lenovo ovpn-client[6620]: net_addr_ptp_v4_add: 10.8.0.6 peer 10.8.0.5 dev tun1
	сен 23 23:36:11 lenovo ovpn-client[6620]: net_route_v4_add: 10.8.0.1/32 via 10.8.0.5 dev [NULL] table 0 metric -1
	сен 23 23:36:11 lenovo ovpn-client[6620]: Initialization Sequence Completed
	```
* The logs of the server process - the part of the logs of connection of the client to the server
	```
	сен 23 23:36:11 lenovo ovpn-server[6276]: 127.0.0.1:60219 Outgoing Control Channel Authentication: Using 160 bit message hash 'SHA1' for HMAC authentication
	сен 23 23:36:11 lenovo ovpn-server[6276]: 127.0.0.1:60219 Incoming Control Channel Authentication: Using 160 bit message hash 'SHA1' for HMAC authentication
	сен 23 23:36:11 lenovo ovpn-server[6276]: 127.0.0.1:60219 TLS: Initial packet from [AF_INET]127.0.0.1:60219, sid=16ec04d7 82b318bd
	сен 23 23:36:11 lenovo ovpn-server[6276]: 127.0.0.1:60219 VERIFY OK: depth=1, CN=srvuser
	сен 23 23:36:11 lenovo ovpn-server[6276]: 127.0.0.1:60219 VERIFY OK: depth=0, CN=client
	сен 23 23:36:11 lenovo ovpn-server[6276]: 127.0.0.1:60219 peer info: IV_VER=2.5.9
	сен 23 23:36:11 lenovo ovpn-server[6276]: 127.0.0.1:60219 peer info: IV_PLAT=linux
	сен 23 23:36:11 lenovo ovpn-server[6276]: 127.0.0.1:60219 peer info: IV_PROTO=6
	сен 23 23:36:11 lenovo ovpn-server[6276]: 127.0.0.1:60219 peer info: IV_NCP=2
	сен 23 23:36:11 lenovo ovpn-server[6276]: 127.0.0.1:60219 peer info: IV_CIPHERS=AES-256-GCM:AES-128-GCM:AES-256-CBC
	сен 23 23:36:11 lenovo ovpn-server[6276]: 127.0.0.1:60219 peer info: IV_LZ4=1
	сен 23 23:36:11 lenovo ovpn-server[6276]: 127.0.0.1:60219 peer info: IV_LZ4v2=1
	сен 23 23:36:11 lenovo ovpn-server[6276]: 127.0.0.1:60219 peer info: IV_LZO=1
	сен 23 23:36:11 lenovo ovpn-server[6276]: 127.0.0.1:60219 peer info: IV_COMP_STUB=1
	сен 23 23:36:11 lenovo ovpn-server[6276]: 127.0.0.1:60219 peer info: IV_COMP_STUBv2=1
	сен 23 23:36:11 lenovo ovpn-server[6276]: 127.0.0.1:60219 peer info: IV_TCPNL=1
	сен 23 23:36:11 lenovo ovpn-server[6276]: 127.0.0.1:60219 Control Channel: TLSv1.3, cipher TLSv1.3 TLS_AES_256_GCM_SHA384, peer certificate: 2048 bit RSA, signature: RSA-SHA256
	сен 23 23:36:11 lenovo ovpn-server[6276]: 127.0.0.1:60219 [client] Peer Connection Initiated with [AF_INET]127.0.0.1:60219
	сен 23 23:36:11 lenovo ovpn-server[6276]: client/127.0.0.1:60219 MULTI_sva: pool returned IPv4=10.8.0.6, IPv6=(Not enabled)
	сен 23 23:36:11 lenovo ovpn-server[6276]: client/127.0.0.1:60219 MULTI: Learn: 10.8.0.6 -> client/127.0.0.1:60219
	сен 23 23:36:11 lenovo ovpn-server[6276]: client/127.0.0.1:60219 MULTI: primary virtual IP for client/127.0.0.1:60219: 10.8.0.6
	сен 23 23:36:11 lenovo ovpn-server[6276]: client/127.0.0.1:60219 Data Channel: using negotiated cipher 'AES-256-GCM'
	сен 23 23:36:11 lenovo ovpn-server[6276]: client/127.0.0.1:60219 Outgoing Data Channel: Cipher 'AES-256-GCM' initialized with 256 bit key
	сен 23 23:36:11 lenovo ovpn-server[6276]: client/127.0.0.1:60219 Incoming Data Channel: Cipher 'AES-256-GCM' initialized with 256 bit key
	сен 23 23:36:11 lenovo ovpn-server[6276]: client/127.0.0.1:60219 SENT CONTROL [client]: 'PUSH_REPLY,route 10.8.0.1,topology net30,ping 10,ping-restart 120,ifconfig 10.8.0.6 10.8.0.5,peer-id 0,cipher AES-> lines 2486-2539/2539
	```

> Using a traffic sniffer like Wireshark, inspect the interface of the VPN traffic. Show the information about this traffic.



---
## References
- ChatGPT