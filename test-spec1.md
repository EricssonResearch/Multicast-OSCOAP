# Tests Specification for OSCORE Group Communication

[//]: # (use Pandoc : pandoc spec.md -o spec.html)

## Table of Contents
1. [Notes](#notes)
2. [Security Contexts and Resources](#security-contexts-and-resources)
    1. [Security Context](#group-sec)
    2. [Resources](#resources)
3. [Summary of Tests](#summary)
3. [Set up the environment](#env-setup)
    1. [Test 0a](#test-0a)
    2. [Test 0b](#test-0b)
4. [Correct OSCORE use](#correct-oscore-use)
    1. [Test 1a](#test-1a)
    2. [Test 1b](#test-1b)
    3. [Test 2a](#test-2a)
    4. [Test 2b](#test-2b)
    5. [Test 3a](#test-3a)
    6. [Test 3b](#test-3b)
    7. [Test 4a](#test-4a)
    8. [Test 4b](#test-4b)
4. [Incorrect OSCORE use](#incorrect-oscore-use)
    1. [Test 5a](#test-5a)
    2. [Test 5b](#test-5b)
    3. [Test 6a](#test-6a)
    4. [Test 6b](#test-6b)
    5. [Test 7a](#test-7a)
    6. [Test 7b](#test-7b)
    7. [Test 8a](#test-8a)
    8. [Test 8b](#test-8b)
    9. [Test 9a]
    10. [Test 9b]
    11. [Test 10a]
    12. [Test 10b]


## 1. Notes

CoAP Version is 1 in all the tests.

The client and server may optionally display external_aad and COSE object (before and after compression) to simplify debugging.

It is recommended to have tested OSCORE functionalities (see TBD link) before running these tests.

Note that Group Communication (TBD link) mandates the use of NON for requests. This specification recommends the use of NON for responses as well.

Note that the CoAP Token timeout needs to be configured to run the tests with multiple responses (see TBD). This specification recommends using a timeout of appr. 30 seconds.

Most of the tests can be run between 2 nodes. A few tests require 3/4 nodes.

Although these specifications test the implementation of group OSCORE, this version does not rely on multicast communication.
The test specified below can in fact be run on unicast between the nodes, to test the group OSCORE implementation features.

### Addresses

31.133.136.216 - Jim

31.133.155.197 - Jim

31.133.136.213 - Rikard

31.133.156.244 - Peter

### Jim's Modifications from draft -05

1.  Section 3.1.2 - The fields OSCORE_option and options are swapped in the aad_array structure.  This only changes the external_aad for signing and not for encryption.

2.  Section 9.2 - The items are wrapped in an array when more than one elemet exists.  This is reflected in the fields below.

## 2. Configuration, Security Contexts and Resources

### Group Security Context: EdDSA   {#group-sec}

* Common Context:
    - Master Secret: 0x102030405060708090a0b0c0d0e0f001 (16 bytes)
    - Master Salt: 0xe9c79a2232873604 (8 bytes)
    - ID Context: 0x73bc3f1200712a3d (8 bytes)
    - Common IV: 0x5B188ED0BD4B9B046B818BE37E (13 bytes)
    - Par Countersign: 0x06
    - Par Countersign Key: 0x820106

* Entity #1:
    - Signing Key: {1: 1, -1: 6,
              -2: h'4C5E5A898AFC77D9C90773D9B4F5E7B378605753F9BA9E8A62488C64E1A524B0',
              -4: h'C9AFCF6610BAB69A7E72B78B6D364BE86F12CF293523DA51433B09A799FF0F62'}
    
* Entity #2:
    - Signing Key: {1: 1, -1: 6,
              -2: h'90F28C4CC63A56574F1873B802B587F9CE05E718887B3411E8EC97B9C28E7227',
              -4: h'732BA0EF6CAC00A91E97BDA18E1E4D94C4C75988676BE43B7B7664A1D5B2651F'}

* Entity #3:
    - Signing Key: {1: 1, -1: 6,
              -2: h'91BDA65809E1D37B74E7B9AB5797479D47AF6E8CE6C4940AAA468562F04CE715',
              -4: h'00FC63AD4D5C3C4B645B3DE47E937F419EE3FA58B41BEBE8FB7E7429520AD06B'}

* Entity #4:
    - Signing Key: {1: 1, -1: 6,
              -2: h'2A279191227491C92E9C5AEDCF72F5C73E78E19C7E77172B4FEFCE09018AEFD4',
              -4: h'D744189028C8F2652EBBF3576B4CB740926B25DA087043E978AE570AAD333495'}

* Jim's Client
    - Sender ID: 0xa1 (1 byte)
    - Sender Key: 0x57892057B3A8181989F42C23C3DE2F40 (16 bytes)
    - Sender Seq Number: 00
    - Sender IV: 0x5A188ED0BD4B9BA56B818BE37E (using Partial IV: 00)
    - Signing Key: Entity 1 Signing Key

* Jim's Server 1
    - Sender ID: 0xa2 (1 byte)
    - Sender Key: 0x8FDCB65DB23B63D9338A276DF844A371 (16 bytes)
    - Sender Seq Number: 00
    - Sender IV: 0x5A188ED0BD4B9BA66B818BE37E (using Partial IV: 00)
    - Signing Key: Entity 1 Signing Key

* Jim's Server 2
    - Sender ID: 0xa3 (1 byte)
    - Sender Key: 0xF598A90DBC770E5F4301F99F572F0763 (16 bytes)
    - Sender Seq Number: 00
    - Sender IV: 0x5A188ED0BD4B9BA76B818BE37E (using Partial IV: 00)
    - Signing Key: Entity 1 Signing Key

* Rikard's Client
    - Sender ID: 0xb1 (1 byte)
    - Sender Key: 0xA09B63097B20B43A895B5A65D709BA81 (16 bytes)
    - Sender Seq Number: 00
    - Sender IV: 0x5A188ED0BD4B9BB56B818BE37E (using Partial IV: 00)
    - Signing Key: Entity 2 Signing Key

* Rikard's Server 1
    - Sender ID: 0xb2 (1 byte)
    - Sender Key: 0xE9BB12DE9ED96975D78CEBF59A5F87E7 (16 bytes)
    - Sender Seq Number: 00
    - Sender IV: 0x5A188ED0BD4B9BB66B818BE37E (using Partial IV: 00)
    - Signing Key: Entity 2 Signing Key

* Rikard's Server 2
    - Sender ID: 0xb3 (1 byte)
    - Sender Key: 0xEB999F5EE8F06813B346E937723BDEF4 (16 bytes)
    - Sender Seq Number: 00
    - Sender IV: 0x5A188ED0BD4B9BB76B818BE37E (using Partial IV: 00)
    - Signing Key: Entity 2 Signing Key

* Peter's Client
    - Sender ID: 0xc1 (1 byte)
    - Sender Key: 0xBA4A352BDADDC787226F78030E5062ED (16 bytes)
    - Sender Seq Number: 00
    - Sender IV: 0x5A188ED0BD4B9BC56B818BE37E (using Partial IV: 00)
    - Public Signing Key: 0x508AFC1C29037EF3614D63AF87E1EA31D891D76B1F906098AF8FA39BBE874019 (32 bytes)

* Peter's Server
    - Sender ID: 0xc2 (1 byte)
    - Sender Key: 0x6110B3F88024B485ACC6A0616F10D45E (16 bytes)
    - Sender Seq Number: 00
    - Sender IV: 0x5A188ED0BD4B9BC66B818BE37E (using Partial IV: 00)
    - Public Signing Key: 0x508AFC1C29037EF3614D63AF87E1EA31D891D76B1F906098AF8FA39BBE874019 (32 bytes)


### Resources

The list of resources the OSCORE-aware server must implement is the following:

* /oscore/hello/coap : authorized method: GET, returns the string "Hello World!" with content-format 0 (text/plain)
* /oscore/hello/1 : protected resource, authorized method: GET, returns the string "Hello World!" with content-format 0 (text/plain)

------

## Summary of tests {#summary}

0: Verify that CoAP exchange works. Perform a simple GET transaction using COAP, Content-Format and Uri-Path option.

1: Perform an OSCORE Group transaction, where the client and server retrieve a recipient context that they have stored in memory.

2: Perform an OSCORE Group transaction, where the client sends requests to 2 servers and receives 2 successfully validated responses.

3: Perform an OSCORE Group transaction, where the client receives a Recipient Id that it does not have in memory, and succesfully derives the Recipient Context for it.

4: Perform an OSCORE Group transaction, where the server receives a Recipient Id that it does not have in memory, and succesfully derives the Recipient Context for it.

5: Perform an OSCORE Group transaction where the client receives a Recipient Id that it does not have in memory, and derives the Recipient Context for it, but decryption fails.

6: Perform an OSCORE Group transaction where the server receives a Recipient Id that it does not have in memory, and derives the Recipient Context for it, but decryption fails.

7: Perform an OSCORE Group transaction where the client receives a Recipient Id that it does not have in memory, and derives the Recipient Context for it, but signature verification fails.

8: Perform an OSCORE Group transaction where the server receives a Recipient Id that it does not have in memory, and derives the Recipient Context for it, but signature verification fails.

9: 2 clients and 1 server with both clients sending the same sequence number, SEQ1, client 1 sending it once and client 2 sending it twice. Server detects replay for client 2 and accept the message from client 1.

10: replay handling on the client - client getting 2 responses from the same server (fresh response or replay) it discards the second.

## 3. Set up the environment {#env-setup}

### 3.1. Identifier: TEST_0a {#test-0a}

**Objective** : Verify that CoAP exchange works. Perform a simple GET transaction using COAP, Content-Format and Uri-Path option (Client side)

**Test Sequence**

+------+----------+----------------------------------------------------------+
| Step | Type     | Description                                              |
+======+==========+==========================================================+
| 1    | Stimulus | The client is requested to send a CoAP GET request       |
|      |          | including:                                               |
|      |          |                                                          |
|      |          | - Uri-Path : /oscore/hello/coap                          |
+------+----------+----------------------------------------------------------+
| 2    | Check    | Client serializes the request                            |
+------+----------+----------------------------------------------------------+
| 3    | Verify   | Client displays the sent packet                          |
+------+----------+----------------------------------------------------------+
| 4    | Check    | Client parses the response and continues the CoAP        |
|      |          | processing expected; expected:                           |
|      |          | 2.05 Content Response with:                              |
|      |          |                                                          |
|      |          | - Content-Format = 0 (text/plain)                        |
|      |          | - Payload = "Hello World!"                               |
+------+----------+----------------------------------------------------------+
| 5    | Verify   | Client displays the received packet                      |
+------+----------+----------------------------------------------------------+

#### 3.2. Identifier: TEST_0b {#test-0b}

**Objective** : Verify that CoAP exchange works. Perform a simple GET transaction using COAP, Content-Format and Uri-Path option (Server side)

**Configuration** :

_server resources_:

* /oscore/hello/coap : authorized method: GET, returns the string "Hello World!" with content-format 0 (text/plain)

**Test Sequence**

+------+----------+----------------------------------------------------------+
| Step | Type     | Description                                              |
+======+==========+==========================================================+
| 1    | Stimulus | The client is requested to send a CoAP GET request       |
|      |          | including:                                               |
|      |          |                                                          |
|      |          | - Uri-Path = /oscore/hello/coap                          |
+------+----------+----------------------------------------------------------+
| 2    | Check    | Server parses the request and continues the CoAP         |
|      |          | processing                                               |
+------+----------+----------------------------------------------------------+
| 3    | Verify   | Server displays the received packet                      |
+------+----------+----------------------------------------------------------+
| 4    | Check    | Server serialize the response correctly, which is:       |
|      |          | 2.05 Content Response with:                              |
|      |          |                                                          |
|      |          | - Content-Format = 0 (text/plain)                        |
|      |          | - Payload = "Hello World!"                               |
+------+----------+----------------------------------------------------------+
| 5    | Verify   | Server displays the sent packet                          |
+------+----------+----------------------------------------------------------+

## 4. Several Recipient Contexts on Client {#client}

In these tests, the node has several recipient contexts. The goal is to test the retrieval/derivation of recipient contexts on the client.

For these tests, all parties need to agree on a signature algorithm to be used and from that select the group context to be used.

### 4.1 Correct Group OSCORE Use


#### 4.1.1. Identifier: TEST_1a {#test-1a}

**Objective** : Perform an OSCORE Group transaction, where the client and server retrieve a recipient context that they have stored in memory. (Client side)

**Configuration** :  

_client security context_: Client takes the key for Entity #1

* Sequence number received not in client's replay window

**Test Sequence**

+------+----------+----------------------------------------------------------+
| Step | Type     | Description                                              |
+======+==========+==========================================================+
| 1    | Stimulus | The client is requested to send a CoAP GET request       |
|      |          | protected with OSCORE, including:                        |
|      |          |                                                          |
|      |          | - Object-Security option                                 |
|      |          | - Uri-Path : /oscore/hello/1                             |
+------+----------+----------------------------------------------------------+
| 2    | Check    | Client serializes the request, which is a POST request,  |
|      |          | with:                                                    |
|      |          |                                                          |
|      |          | - Object-Security option: kid = 0xa1                     |
|      |          | - Payload: ciphertext including:                         |
|      |          |                                                          |
|      |          |       * Code: GET                                        |
|      |          |       * Uri-Path : /oscore/hello/1                       |
+------+----------+----------------------------------------------------------+
| 3    | Verify   | Client displays the sent packet                          |
+------+----------+----------------------------------------------------------+
| 4    | Check    | Client parses the response; expected:                    |
|      |          | 2.04 Changed Response with:                              |
|      |          |                                                          |
|      |          | - Object-Security option: kid = 0xb1                     |
|      |          | - Payload                                                |
+------+----------+----------------------------------------------------------+
| 4    | Verify   | Client successfully retrieves correct Recipient Context  |
|      |          | and decrypts the message: OSCORE verification succeeds   |
+------+----------+----------------------------------------------------------+
| 6    | Check    | Client parses the decrypted response and continues the   |
|      |          | CoAP processing; expected 2.05 Content Response with:    |
|      |          |                                                          |
|      |          | - Content-Format = 0 (text/plain)                        |
|      |          | - Payload = "Hello World!"                               |
+------+----------+----------------------------------------------------------+
| 7    | Verify   | Client displays the received packet                      |
+------+----------+----------------------------------------------------------+

#### 4.1.2. Identifier: TEST_1b {#test-1b}

**Objective** : Perform an OSCORE Group transaction, where the client and server retrieve a recipient context that they have stored in memory. (Server side)

**Configuration** :

_server security context_:  Server takes the roll of Entity #2 with:

* Sequence number received not in server's replay window

_server resources_:

* /oscore/hello/1 : protected resource, authorized method: GET, returns the string "Hello World!" with content-format 0 (text/plain)

**Test Sequence**

+------+----------+----------------------------------------------------------+
| Step | Type     | Description                                              |
+======+==========+==========================================================+
| 1    | Stimulus | The client is requested to send a CoAP GET request       |
|      |          | protected with OSCORE, including:                        |
|      |          |                                                          |
|      |          | - Object-Security option                                 |
|      |          | - Uri-Path = /oscore/hello/1                             |
+------+----------+----------------------------------------------------------+
| 2    | Verify   | Server displays the received packet                      |
+------+----------+----------------------------------------------------------+
| 3    | Check    | Server parses the request; expected:                     |
|      |          | 0.02 POST with:                                          |
|      |          |                                                          |
|      |          | - Object-Security option: kid = 0xa1                     |
|      |          | - Payload                                                |
+------+----------+----------------------------------------------------------+
| 4    | Verify   | Server successfully retrieves correct Recipient Context  |
|      |          | and decrypts the message: OSCORE verification succeeds   |
+------+----------+----------------------------------------------------------+
| 5    | Check    | Server parses the request and continues the CoAP         |
|      |          | processing; expected: CoAP GET request, including:       |
|      |          |                                                          |
|      |          | - Uri-Path : /oscore/hello/1                             |
+------+----------+----------------------------------------------------------+
| 6    | Verify   | Server displays the received packet                      |
+------+----------+----------------------------------------------------------+
| 7    | Check    | Server serialize the response correctly, which is:       |
|      |          | 2.04 Changed Response with:                              |
|      |          |                                                          |
|      |          | - Object-Security option: kid = 0xb1                     |
|      |          | - Payload: ciphertext including:                         |
|      |          |                                                          |
|      |          |     * Code: 2.05 Content Response                        |
|      |          |     * Content-Format = 0 (text/plain)                    |
|      |          |     * Payload = "Hello World!"                           |
+------+----------+----------------------------------------------------------+
| 8    | Verify   | Server displays the sent packet                          |
+------+----------+----------------------------------------------------------+


#### 4.1.3. Identifier: TEST_2a {#test-2a}

**Objective** : Perform an OSCORE Group transaction, where the client sends requests to 2 servers and receives 2 successfully validated responses. (Client side)

**Configuration** :  

_client security context_: Client takes the key for Entity #1

* Sequence number received not in client's replay window

**Test Sequence**

+------+----------+----------------------------------------------------------+
| Step | Type     | Description                                              |
+======+==========+==========================================================+
| 1    | Stimulus | The client is requested to send 2 CoAP GET request       |
|      |          | protected with OSCORE, including:                        |
|      |          |                                                          |
|      |          | - Object-Security option                                 |
|      |          | - Uri-Path : /oscore/hello/1                             |
+------+----------+----------------------------------------------------------+
| 2    | Check    | Client serializes the 2 request, which is a POST request |
|      |          | with:                                                    |
|      |          |                                                          |
|      |          | - Object-Security option: kid = 0xa1                     |
|      |          | - Payload: ciphertext including:                         |
|      |          |                                                          |
|      |          |       * Code: GET                                        |
|      |          |       * Uri-Path : /oscore/hello/1                       |
+------+----------+----------------------------------------------------------+
| 3    | Verify   | Client displays the sent packet                          |
+------+----------+----------------------------------------------------------+
| 4    | Check    | Client parses the response; expected:                    |
|      |          | 2.04 Changed Response with:                              |
|      |          |                                                          |
|      |          | - Object-Security option: kid = 0xb1                     |
|      |          | - Payload                                                |
+------+----------+----------------------------------------------------------+
| 4    | Verify   | Client successfully retrieves correct Recipient Contexts |
|      |          | and decrypts the messages: OSCORE verification succeeds  |
+------+----------+----------------------------------------------------------+
| 6    | Check    | Client parses the decrypted responses and continues the  |
|      |          | CoAP processing; expected 2.05 Content Response with:    |
|      |          |                                                          |
|      |          | - Content-Format = 0 (text/plain)                        |
|      |          | - Payload = "Hello World!"                               |
+------+----------+----------------------------------------------------------+
| 7    | Verify   | Client displays the received packet                      |
+------+----------+----------------------------------------------------------+

#### 4.1.4. Identifier: TEST_2b {#test-2b}

**Objective** : Perform an OSCORE Group transaction, where the client sends requests to 2 servers and receives 2 successfully validated responses. (Server side)

**Configuration** :

_server security context_:  Server takes the roll of Entity #2 with:

* Sequence number received not in server's replay window

_server resources_:

* /oscore/hello/1 : protected resource, authorized method: GET, returns the string "Hello World!" with content-format 0 (text/plain)

**Test Sequence**

+------+----------+----------------------------------------------------------+
| Step | Type     | Description                                              |
+======+==========+==========================================================+
| 1    | Stimulus | The client is requested to send a CoAP GET request       |
|      |          | protected with OSCORE, including:                        |
|      |          |                                                          |
|      |          | - Object-Security option                                 |
|      |          | - Uri-Path = /oscore/hello/1                             |
+------+----------+----------------------------------------------------------+
| 2    | Verify   | Server displays the received packet                      |
+------+----------+----------------------------------------------------------+
| 3    | Check    | Server parses the request; expected:                     |
|      |          | 0.02 POST with:                                          |
|      |          |                                                          |
|      |          | - Object-Security option: kid = 0xa1                     |
|      |          | - Payload                                                |
+------+----------+----------------------------------------------------------+
| 4    | Verify   | Server successfully retrieves correct Recipient Context  |
|      |          | and decrypts the message: OSCORE verification succeeds   |
+------+----------+----------------------------------------------------------+
| 5    | Check    | Server parses the request and continues the CoAP         |
|      |          | processing; expected: CoAP GET request, including:       |
|      |          |                                                          |
|      |          | - Uri-Path : /oscore/hello/1                             |
+------+----------+----------------------------------------------------------+
| 6    | Verify   | Server displays the received packet                      |
+------+----------+----------------------------------------------------------+
| 7    | Check    | Server serialize the response correctly, which is:       |
|      |          | 2.04 Changed Response with:                              |
|      |          |                                                          |
|      |          | - Object-Security option: kid = 0xb1                     |
|      |          | - Payload: ciphertext including:                         |
|      |          |                                                          |
|      |          |     * Code: 2.05 Content Response                        |
|      |          |     * Content-Format = 0 (text/plain)                    |
|      |          |     * Payload = "Hello World!"                           |
+------+----------+----------------------------------------------------------+
| 8    | Verify   | Server displays the sent packet                          |
+------+----------+----------------------------------------------------------+

#### 4.1.5. Identifier: TEST_3a {#test-3a}

**Objective** : Perform an OSCORE Group transaction, where the client receives a Recipient Id that it does not have in memory, and succesfully derives the Recipient Context for it. (Client side)

**Configuration** :

_client security context_: [Security Context A](#client-sec), with:

* Sequence number received not in client's replay window

**Test Sequence**

+------+----------+----------------------------------------------------------+
| Step | Type     | Description                                              |
+======+==========+==========================================================+
| 1    | Stimulus | The client is requested to send a CoAP GET request       |
|      |          | protected with OSCORE, including:                        |
|      |          |                                                          |
|      |          | - Object-Security option                                 |
|      |          | - Uri-Path : /oscore/hello/1                             |
+------+----------+----------------------------------------------------------+
| 2    | Check    | Client serializes the request, which is a POST request,  |
|      |          | with:                                                    |
|      |          |                                                          |
|      |          | - Object-Security option: kid = 0x                       |
|      |          | - Payload: ciphertext including:                         |
|      |          |                                                          |
|      |          |       * Code: GET                                        |
|      |          |       * Uri-Path : /oscore/hello/1                       |
+------+----------+----------------------------------------------------------+
| 3    | Verify   | Client displays the sent packet                          |
+------+----------+----------------------------------------------------------+
| 4    | Check    | Client parses the response; expected:                    |
|      |          | 2.04 Changed Response with:                              |
|      |          |                                                          |
|      |          | - Object-Security option: kid = 0x02                     |
|      |          | - Payload                                                |
+------+----------+----------------------------------------------------------+
| 5    | Verify   | Client successfully derives a new Recipient Context from |
|      |          | received Recipient Id = 0x02 ; Client decrypts           |
|      |          | the message: OSCORE verification succeeds                |
+------+----------+----------------------------------------------------------+
| 6    | Check    | Client parses the decrypted response and continues the   |
|      |          | CoAP processing; expected 2.05 Content Response with:    |
|      |          |                                                          |
|      |          | - Content-Format = 0 (text/plain)                        |
|      |          | - Payload = "Hello World!"                               |
+------+----------+----------------------------------------------------------+
| 7    | Verify   | Client displays the received packet                      |
+------+----------+----------------------------------------------------------+

#### 4.1.6. Identifier: TEST_3b {#test-3b}

**Objective** : Perform an OSCORE Group transaction, where the client receive a Recipient Id that it does not have in memory, and successfully derives the Recipient Context for it. (Server side)

**Configuration** :

_server security context_: 
[Security Context C](#server-sec), with:

* Sequence number received not in server's replay window

_server resources_:

* /oscore/hello/1 : protected resource, authorized method: GET, returns the string "Hello World!" with content-format 0 (text/plain)

**Test Sequence**

+------+----------+----------------------------------------------------------+
| Step | Type     | Description                                              |
+======+==========+==========================================================+
| 1    | Stimulus | The client is requested to send a CoAP GET request       |
|      |          | protected with OSCORE, including:                        |
|      |          |                                                          |
|      |          | - Object-Security option                                 |
|      |          | - Uri-Path = /oscore/hello/1                             |
+------+----------+----------------------------------------------------------+
| 2    | Verify   | Server displays the received packet                      |
+------+----------+----------------------------------------------------------+
| 3    | Check    | Server parses the request; expected:                     |
|      |          | 0.02 POST with:                                          |
|      |          |                                                          |
|      |          | - Object-Security option: kid = 0x                       |
|      |          | - Payload                                                |
+------+----------+----------------------------------------------------------+
| 4    | Verify   | Server successfully retrieves correct Recipient Context  |
|      |          | and decrypts the message: OSCORE verification succeeds   |
+------+----------+----------------------------------------------------------+
| 5    | Check    | Server parses the request and continues the CoAP         |
|      |          | processing; expected: CoAP GET request, including:       |
|      |          |                                                          |
|      |          | - Uri-Path : /oscore/hello/1                             |
+------+----------+----------------------------------------------------------+
| 6    | Verify   | Server displays the received packet                      |
+------+----------+----------------------------------------------------------+
| 7    | Check    | Server serialize the response correctly, which is:       |
|      |          | 2.04 Changed Response with:                              |
|      |          |                                                          |
|      |          | - Object-Security option: kid = 0x02                     |
|      |          | - Payload: ciphertext including:                         |
|      |          |                                                          |
|      |          |     * Code: 2.05 Content Response                        |
|      |          |     * Content-Format = 0 (text/plain)                    |
|      |          |     * Payload = "Hello World!"                           |
+------+----------+----------------------------------------------------------+
| 8    | Verify   | Server displays the sent packet                          |
+------+----------+----------------------------------------------------------+

#### 4.1.7. Identifier: TEST_4a {#test-4a}

**Objective** : Perform an OSCORE Group transaction, where the server receives a Recipient Id that it does not have in memory, and succesfully derives the Recipient Context for it. (Client side)

**Configuration** :

_client security context_: [Security Context A2](#client-sec-a), with:

* Sequence number received not in client's replay window

**Test Sequence**

+------+----------+----------------------------------------------------------+
| Step | Type     | Description                                              |
+======+==========+==========================================================+
| 1    | Stimulus | The client is requested to send a CoAP GET request       |
|      |          | protected with OSCORE, including:                        |
|      |          |                                                          |
|      |          | - Object-Security option                                 |
|      |          | - Uri-Path : /oscore/hello/1                             |
+------+----------+----------------------------------------------------------+
| 2    | Check    | Client serializes the request, which is a POST request,  |
|      |          | with:                                                    |
|      |          |                                                          |
|      |          | - Object-Security option: kid = 0x00                     |
|      |          | - Payload: ciphertext including:                         |
|      |          |                                                          |
|      |          |       * Code: GET                                        |
|      |          |       * Uri-Path : /oscore/hello/1                       |
+------+----------+----------------------------------------------------------+
| 3    | Verify   | Client displays the sent packet                          |
+------+----------+----------------------------------------------------------+
| 4    | Check    | Client parses the response; expected:                    |
|      |          | 2.04 Changed Response with:                              |
|      |          |                                                          |
|      |          | - Object-Security option: kid = 0x01                     |
|      |          | - Payload                                                |
+------+----------+----------------------------------------------------------+
| 5    | Verify   | Client successfully retrieves correct Recipient Context  |
|      |          | and decrypts the message: OSCORE verification succeeds   |
+------+----------+----------------------------------------------------------+
| 6    | Check    | Client parses the decrypted response and continues the   |
|      |          | CoAP processing; expected 2.05 Content Response with:    |
|      |          |                                                          |
|      |          | - Content-Format = 0 (text/plain)                        |
|      |          | - Payload = "Hello World!"                               |
+------+----------+----------------------------------------------------------+
| 7    | Verify   | Client displays the received packet                      |
+------+----------+----------------------------------------------------------+

#### 4.1.8. Identifier: TEST_4b {#test-4b}

**Objective** : Perform an OSCORE Group transaction, where the server receive a Recipient Id that it does not have in memory, and derives the Recipient Context for it. (Server side)

**Configuration** :

_server security context_: 
[Security Context B](#server-sec-B), with:

* Sequence number received not in server's replay window

_server resources_:

* /oscore/hello/1 : protected resource, authorized method: GET, returns the string "Hello World!" with content-format 0 (text/plain)

**Test Sequence**

+------+----------+----------------------------------------------------------+
| Step | Type     | Description                                              |
+======+==========+==========================================================+
| 1    | Stimulus | The client is requested to send a CoAP GET request       |
|      |          | protected with OSCORE, including:                        |
|      |          |                                                          |
|      |          | - Object-Security option                                 |
|      |          | - Uri-Path = /oscore/hello/1                             |
+------+----------+----------------------------------------------------------+
| 2    | Verify   | Server displays the received packet                      |
+------+----------+----------------------------------------------------------+
| 3    | Check    | Server parses the request; expected:                     |
|      |          | 0.02 POST with:                                          |
|      |          |                                                          |
|      |          | - Object-Security option: kid = 0x00                     |
|      |          | - Payload                                                |
+------+----------+----------------------------------------------------------+
| 4    | Verify   | Server successfully derives a new Recipient Context from |
|      |          | received Recipient Id = 0x00 ; Client decrypts           |
|      |          | the message: OSCORE verification succeeds                |
+------+----------+----------------------------------------------------------+
| 5    | Check    | Server parses the request and continues the CoAP         |
|      |          | processing; expected: CoAP GET request, including:       |
|      |          |                                                          |
|      |          | - Uri-Path : /oscore/hello/1                             |
+------+----------+----------------------------------------------------------+
| 6    | Verify   | Server displays the received packet                      |
+------+----------+----------------------------------------------------------+
| 7    | Check    | Server serialize the response correctly, which is:       |
|      |          | 2.04 Changed Response with:                              |
|      |          |                                                          |
|      |          | - Object-Security option: kid = 0x01                     |
|      |          | - Payload: ciphertext including:                         |
|      |          |                                                          |
|      |          |     * Code: 2.05 Content Response                        |
|      |          |     * Content-Format = 0 (text/plain)                    |
|      |          |     * Payload = "Hello World!"                           |
+------+----------+----------------------------------------------------------+
| 8    | Verify   | Server displays the sent packet                          |
+------+----------+----------------------------------------------------------+

### 4.2 Incorrect Group OSCORE Use

#### 4.2.1. Identifier: TEST_5a {#test-5a}

**Objective** : Perform an OSCORE Group transaction where the client receives a Recipient Id that it does not have in memory, and derives the Recipient Context for it, but decryption fails. (Client side)

**Configuration** :

_client security context_: [Security Context A](#client-sec), with:

* Sequence number received not in client's replay window

**Test Sequence**

+------+----------+----------------------------------------------------------+
| Step | Type     | Description                                              |
+======+==========+==========================================================+
| 1    | Stimulus | The client is requested to send a CoAP GET request       |
|      |          | protected with OSCORE, including:                        |
|      |          |                                                          |
|      |          | - Object-Security option                                 |
|      |          | - Uri-Path : /oscore/hello/1                             |
+------+----------+----------------------------------------------------------+
| 2    | Check    | Client serializes the request, which is a POST request,  |
|      |          | with:                                                    |
|      |          |                                                          |
|      |          | - Object-Security option: kid = 0x                       |
|      |          | - Payload: ciphertext including:                         |
|      |          |                                                          |
|      |          |       * Code: GET                                        |
|      |          |       * Uri-Path : /oscore/hello/1                       |
+------+----------+----------------------------------------------------------+
| 3    | Verify   | Client displays the sent packet                          |
+------+----------+----------------------------------------------------------+
| 4    | Check    | Client parses the response; expected:                    |
|      |          | 2.04 Changed Response with:                              |
|      |          |                                                          |
|      |          | - Object-Security option: kid = 0x03                     |
|      |          | - Payload                                                |
+------+----------+----------------------------------------------------------+
| 5    | Verify   | Client successfully derives a new Recipient Context from |
|      |          | received Recipient Id = 0x03 ; Client tries to decrypt   |
|      |          | the message: OSCORE verification fail                    |
+------+----------+----------------------------------------------------------+
| 6    | Verify   | Client displays the received packet                      |
+------+----------+----------------------------------------------------------+

#### 4.2.2. Identifier: TEST_5b {#test-5b}

**Objective** : Perform an OSCORE Group transaction where the client receives a Recipient Id that it does not have in memory, and derives the Recipient Context for it, but decryption fails. (Server side)

**Configuration** :

_server security context_: 
[Security Context D](#server-sec-D), with:

* Sequence number received not in server's replay window

_server resources_:

* /oscore/hello/1 : protected resource, authorized method: GET, returns the string "Hello World!" with content-format 0 (text/plain)

**Test Sequence**

+------+----------+----------------------------------------------------------+
| Step | Type     | Description                                              |
+======+==========+==========================================================+
| 1    | Stimulus | The client is requested to send a CoAP GET request       |
|      |          | protected with OSCORE, including:                        |
|      |          |                                                          |
|      |          | - Object-Security option                                 |
|      |          | - Uri-Path = /oscore/hello/1                             |
+------+----------+----------------------------------------------------------+
| 2    | Verify   | Server displays the received packet                      |
+------+----------+----------------------------------------------------------+
| 3    | Check    | Server parses the request; expected:                     |
|      |          | 0.02 POST with:                                          |
|      |          |                                                          |
|      |          | - Object-Security option: kid = 0x                       |
|      |          | - Payload                                                |
+------+----------+----------------------------------------------------------+
| 4    | Verify   | Server successfully retrieve correct Recipient Context   |
|      |          | and decrypts the message: OSCORE verification succeeds   |
+------+----------+----------------------------------------------------------+
| 5    | Check    | Server parses the request and continues the CoAP         |
|      |          | processing; expected: CoAP GET request, including:       |
|      |          |                                                          |
|      |          | - Uri-Path : /oscore/hello/1                             |
+------+----------+----------------------------------------------------------+
| 6    | Verify   | Server displays the received packet                      |
+------+----------+----------------------------------------------------------+
| 7    | Check    | Server serialize the response correctly, which is:       |
|      |          | 2.04 Changed Response with:                              |
|      |          |                                                          |
|      |          | - Object-Security option: kid = 0x03                     |
|      |          | - Payload: ciphertext including:                         |
|      |          |                                                          |
|      |          |     * Code: 2.05 Content Response                        |
|      |          |     * Content-Format = 0 (text/plain)                    |
|      |          |     * Payload = "Hello World!"                           |
+------+----------+----------------------------------------------------------+
| 8    | Verify   | Server displays the sent packet                          |
+------+----------+----------------------------------------------------------+

#### 4.2.3. Identifier: TEST_6a {#test-6a}

**Objective** : Perform an OSCORE Group transaction where the server receives a Recipient Id that it does not have in memory, and derives the Recipient Context for it, but decryption fails. (Client side)

**Configuration** :

_client security context_: [Security Context A3](#client-sec), with:

* Sequence number received not in client's replay window

**Test Sequence**

+------+----------+----------------------------------------------------------+
| Step | Type     | Description                                              |
+======+==========+==========================================================+
| 1    | Stimulus | The client is requested to send a CoAP GET request       |
|      |          | protected with OSCORE, including:                        |
|      |          |                                                          |
|      |          | - Object-Security option                                 |
|      |          | - Uri-Path : /oscore/hello/1                             |
+------+----------+----------------------------------------------------------+
| 2    | Check    | Client serializes the request, which is a POST request,  |
|      |          | with:                                                    |
|      |          |                                                          |
|      |          | - Object-Security option: kid = 0x                       |
|      |          | - Payload: ciphertext including:                         |
|      |          |                                                          |
|      |          |       * Code: GET                                        |
|      |          |       * Uri-Path : /oscore/hello/1                       |
+------+----------+----------------------------------------------------------+
| 3    | Verify   | Client displays the sent packet                          |
+------+----------+----------------------------------------------------------+
| 4    | Check    | Client parses the response; expected:                    |
|      |          | 2.04 Changed Response with:                              |
|      |          |                                                          |
|      |          | - Object-Security option: kid = 0x03                     |
|      |          | - Payload                                                |
+------+----------+----------------------------------------------------------+
| 5    | Verify   | Client successfully derives a new Recipient Context from |
|      |          | received Recipient Id = 0x03 ; Client tries to decrypt   |
|      |          | the message: OSCORE verification fail                    |
+------+----------+----------------------------------------------------------+
| 6    | Verify   | Client displays the received packet                      |
+------+----------+----------------------------------------------------------+

#### 4.2.4. Identifier: TEST_6b {#test-6b}

**Objective** : Perform an OSCORE Group transaction where the server receives a Recipient Id that it does not have in memory, and derives the Recipient Context for it, but decryption fails. (Server side)

**Configuration** :

_server security context_: 
[Security Context D](#server-sec-D), with:

* Sequence number received not in server's replay window

_server resources_:

* /oscore/hello/1 : protected resource, authorized method: GET, returns the string "Hello World!" with content-format 0 (text/plain)

**Test Sequence**

+------+----------+----------------------------------------------------------+
| Step | Type     | Description                                              |
+======+==========+==========================================================+
| 1    | Stimulus | The client is requested to send a CoAP GET request       |
|      |          | protected with OSCORE, including:                        |
|      |          |                                                          |
|      |          | - Object-Security option                                 |
|      |          | - Uri-Path = /oscore/hello/1                             |
+------+----------+----------------------------------------------------------+
| 2    | Verify   | Server displays the received packet                      |
+------+----------+----------------------------------------------------------+
| 3    | Check    | Server parses the request; expected:                     |
|      |          | 0.02 POST with:                                          |
|      |          |                                                          |
|      |          | - Object-Security option: kid = 0x                       |
|      |          | - Payload                                                |
+------+----------+----------------------------------------------------------+
| 4    | Verify   | Server successfully retrieve correct Recipient Context   |
|      |          | and decrypts the message: OSCORE verification succeeds   |
+------+----------+----------------------------------------------------------+
| 5    | Check    | Server parses the request and continues the CoAP         |
|      |          | processing; expected: CoAP GET request, including:       |
|      |          |                                                          |
|      |          | - Uri-Path : /oscore/hello/1                             |
+------+----------+----------------------------------------------------------+
| 6    | Verify   | Server displays the received packet                      |
+------+----------+----------------------------------------------------------+
| 7    | Check    | Server serialize the response correctly, which is:       |
|      |          | 2.04 Changed Response with:                              |
|      |          |                                                          |
|      |          | - Object-Security option: kid = 0x03                     |
|      |          | - Payload: ciphertext including:                         |
|      |          |                                                          |
|      |          |     * Code: 2.05 Content Response                        |
|      |          |     * Content-Format = 0 (text/plain)                    |
|      |          |     * Payload = "Hello World!"                           |
+------+----------+----------------------------------------------------------+
| 8    | Verify   | Server displays the sent packet                          |
+------+----------+----------------------------------------------------------+

#### 4.2.5. Identifier: TEST_7a {#test-7a}

**Objective** : Perform an OSCORE Group transaction where the client receives a Recipient Id that it does not have in memory, and derives the Recipient Context for it, but signature verification fails. (Client side)

**Configuration** :

_client security context_: [Security Context A](#client-sec), with:

* Sequence number received not in client's replay window

**Test Sequence**

+------+----------+----------------------------------------------------------+
| Step | Type     | Description                                              |
+======+==========+==========================================================+
| 1    | Stimulus | The client is requested to send a CoAP GET request       |
|      |          | protected with OSCORE, including:                        |
|      |          |                                                          |
|      |          | - Object-Security option                                 |
|      |          | - Uri-Path : /oscore/hello/1                             |
+------+----------+----------------------------------------------------------+
| 2    | Check    | Client serializes the request, which is a POST request,  |
|      |          | with:                                                    |
|      |          |                                                          |
|      |          | - Object-Security option: kid = 0x                       |
|      |          | - Payload: ciphertext including:                         |
|      |          |                                                          |
|      |          |       * Code: GET                                        |
|      |          |       * Uri-Path : /oscore/hello/1                       |
+------+----------+----------------------------------------------------------+
| 3    | Verify   | Client displays the sent packet                          |
+------+----------+----------------------------------------------------------+
| 4    | Check    | Client parses the response; expected:                    |
|      |          | 2.04 Changed Response with:                              |
|      |          |                                                          |
|      |          | - Object-Security option: kid = 0x04                     |
|      |          | - Payload                                                |
+------+----------+----------------------------------------------------------+
| 5    | Verify   | Client successfully derives a new Recipient Context from |
|      |          | received Recipient Id = 0x04 ; Client decrypts           |
|      |          | the message successfully; Client verifies signature:     |
|      |          | signature verification fails                             |
+------+----------+----------------------------------------------------------+
| 6    | Verify   | Client displays the received packet                      |
+------+----------+----------------------------------------------------------+

#### 4.2.6. Identifier: TEST_7b {#test-7b}

**Objective** : Perform an OSCORE Group transaction where the client receives a Recipient Id that it does not have in memory, and derives the Recipient Context for it, but signature verification fails. (Server side)

**Configuration** :

_server security context_: 
[Security Context E](#server-sec-E), with:

* Sequence number received not in server's replay window

_server resources_:

* /oscore/hello/1 : protected resource, authorized method: GET, returns the string "Hello World!" with content-format 0 (text/plain)

**Test Sequence**

+------+----------+----------------------------------------------------------+
| Step | Type     | Description                                              |
+======+==========+==========================================================+
| 1    | Stimulus | The client is requested to send a CoAP GET request       |
|      |          | protected with OSCORE, including:                        |
|      |          |                                                          |
|      |          | - Object-Security option                                 |
|      |          | - Uri-Path = /oscore/hello/1                             |
+------+----------+----------------------------------------------------------+
| 2    | Verify   | Server displays the received packet                      |
+------+----------+----------------------------------------------------------+
| 3    | Check    | Server parses the request; expected:                     |
|      |          | 0.02 POST with:                                          |
|      |          |                                                          |
|      |          | - Object-Security option: kid = 0x                       |
|      |          | - Payload                                                |
+------+----------+----------------------------------------------------------+
| 4    | Verify   | Server successfully retrieve correct Recipient Context   |
|      |          | and decrypts the message: OSCORE verification succeeds   |
+------+----------+----------------------------------------------------------+
| 5    | Check    | Server parses the request and continues the CoAP         |
|      |          | processing; expected: CoAP GET request, including:       |
|      |          |                                                          |
|      |          | - Uri-Path : /oscore/hello/1                             |
+------+----------+----------------------------------------------------------+
| 6    | Verify   | Server displays the received packet                      |
+------+----------+----------------------------------------------------------+
| 7    | Check    | Server serialize the response correctly, which is:       |
|      |          | 2.04 Changed Response with:                              |
|      |          |                                                          |
|      |          | - Object-Security option: kid = 0x04                     |
|      |          | - Payload: ciphertext including:                         |
|      |          |                                                          |
|      |          |     * Code: 2.05 Content Response                        |
|      |          |     * Content-Format = 0 (text/plain)                    |
|      |          |     * Payload = "Hello World!"                           |
+------+----------+----------------------------------------------------------+
| 8    | Verify   | Server displays the sent packet                          |
+------+----------+----------------------------------------------------------+

#### 4.2.7. Identifier: TEST_8a {#test-8a}

**Objective** : Perform an OSCORE Group transaction where the server receives a Recipient Id that it does not have in memory, and derives the Recipient Context for it, but signature verification fails. (Client side)

**Configuration** :

_client security context_: [Security Context A](#client-sec), with:

* Sequence number received not in client's replay window

**Test Sequence**

+------+----------+----------------------------------------------------------+
| Step | Type     | Description                                              |
+======+==========+==========================================================+
| 1    | Stimulus | The client is requested to send a CoAP GET request       |
|      |          | protected with OSCORE, including:                        |
|      |          |                                                          |
|      |          | - Object-Security option                                 |
|      |          | - Uri-Path : /oscore/hello/1                             |
+------+----------+----------------------------------------------------------+
| 2    | Check    | Client serializes the request, which is a POST request,  |
|      |          | with:                                                    |
|      |          |                                                          |
|      |          | - Object-Security option: kid = 0x                       |
|      |          | - Payload: ciphertext including:                         |
|      |          |                                                          |
|      |          |       * Code: GET                                        |
|      |          |       * Uri-Path : /oscore/hello/1                       |
+------+----------+----------------------------------------------------------+
| 3    | Verify   | Client displays the sent packet                          |
+------+----------+----------------------------------------------------------+
| 4    | Check    | Client parses the response; expected:                    |
|      |          | 2.04 Changed Response with:                              |
|      |          |                                                          |
|      |          | - Object-Security option: kid = 0x04                     |
|      |          | - Payload                                                |
+------+----------+----------------------------------------------------------+
| 5    | Verify   | Client successfully derives a new Recipient Context from |
|      |          | received Recipient Id = 0x04 ; Client decrypts           |
|      |          | the message successfully; Client verifies signature:     |
|      |          | signature verification fails                             |
+------+----------+----------------------------------------------------------+
| 6    | Verify   | Client displays the received packet                      |
+------+----------+----------------------------------------------------------+

#### 4.2.8. Identifier: TEST_8b {#test-8b}

**Objective** : Perform an OSCORE Group transaction where the server receives a Recipient Id that it does not have in memory, and derives the Recipient Context for it, but signature verification fails. (Server side)

**Configuration** :

_server security context_: 
[Security Context E](#server-sec-E), with:

* Sequence number received not in server's replay window

_server resources_:

* /oscore/hello/1 : protected resource, authorized method: GET, returns the string "Hello World!" with content-format 0 (text/plain)

**Test Sequence**

+------+----------+----------------------------------------------------------+
| Step | Type     | Description                                              |
+======+==========+==========================================================+
| 1    | Stimulus | The client is requested to send a CoAP GET request       |
|      |          | protected with OSCORE, including:                        |
|      |          |                                                          |
|      |          | - Object-Security option                                 |
|      |          | - Uri-Path = /oscore/hello/1                             |
+------+----------+----------------------------------------------------------+
| 2    | Verify   | Server displays the received packet                      |
+------+----------+----------------------------------------------------------+
| 3    | Check    | Server parses the request; expected:                     |
|      |          | 0.02 POST with:                                          |
|      |          |                                                          |
|      |          | - Object-Security option: kid = 0x                       |
|      |          | - Payload                                                |
+------+----------+----------------------------------------------------------+
| 4    | Verify   | Server successfully retrieve correct Recipient Context   |
|      |          | and decrypts the message: OSCORE verification succeeds   |
+------+----------+----------------------------------------------------------+
| 5    | Check    | Server parses the request and continues the CoAP         |
|      |          | processing; expected: CoAP GET request, including:       |
|      |          |                                                          |
|      |          | - Uri-Path : /oscore/hello/1                             |
+------+----------+----------------------------------------------------------+
| 6    | Verify   | Server displays the received packet                      |
+------+----------+----------------------------------------------------------+
| 7    | Check    | Server serialize the response correctly, which is:       |
|      |          | 2.04 Changed Response with:                              |
|      |          |                                                          |
|      |          | - Object-Security option: kid = 0x04                     |
|      |          | - Payload: ciphertext including:                         |
|      |          |                                                          |
|      |          |     * Code: 2.05 Content Response                        |
|      |          |     * Content-Format = 0 (text/plain)                    |
|      |          |     * Payload = "Hello World!"                           |
+------+----------+----------------------------------------------------------+
| 8    | Verify   | Server displays the sent packet                          |
+------+----------+----------------------------------------------------------+

* 1 more test with 2 clients and 1 server with 1 client behaving good and one client doing replays (to test that replay windows are independent) (same seq number for both)

* replay handling on the client
    - client getting 2 responses from the same server (fresh response or replay) it should discard the second