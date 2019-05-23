# Tests Specification for OSCORE Group Communication

[//]: # (use Pandoc : pandoc spec.md -o spec.html)

## Table of Contents
1. [Notes](#notes)
2. [Security Contexts and Resources](#security-contexts-and-resources)
    1. [Security Context A1: Client](#client-sec-A1)
    2. [Security Context A2: Client](#server-sec-A2)
    3. [Security Context A3: Client](#client-sec-A3)
    4. [Security Context A4: Client](#server-sec-A4)
    5. [Security Context B1: Server](#server-sec-B1)
    6. [Security Context B2: Server](#server-sec-B2)
    7. [Security Context B3: Server](#server-sec-B3)
    8. [Security Context B4: Server](#server-sec-B4)
    9. [Resources](#resources)
3. [Set up the environment](#env-setup)
    1. [Test 0a](#test-0a)
    2. [Test 0b](#test-0b)
4. [Correct OSCORE use](#correct-oscore-use)
    1. [test](#client)
        1. [Test 1a](#test-1a)
        2. [Test 1b](#test-1b)
        3. [Test 2a](#test-2a)
        4. [Test 2b](#test-2b)
        5. [Test 3a](#test-3a)
        6. [Test 3b](#test-3b)
        7. [Test 4a](#test-4a)
        8. [Test 4b](#test-4b)


## 1. Notes

CoAP Version is 1 in all the tests.

The client and server may optionally display external_aad and COSE object (before and after compression) to simplify debugging.

It is recommended to have tested OSCORE functionalities (see TBD link) before running these tests.

Note that Group Communication (TBD link) mandates the use of NON for requests. This specification recommends the use of NON for responses as well.

Note that the CoAP Token timeout needs to be configured to run the tests with multiple responses (see TBD). This specification recommends using a timeout of appr. 30 seconds.

Most of the tests can be run between 2 nodes. A few tests require 3/4 nodes.

Although these specifications test the implementation of group OSCORE, this version does not rely on multicast communication.
The test specified below can in fact be run on unicast between the nodes, to test the group OSCORE implementation features.

## 2. Configuration, Security Contexts and Resources

### Security Context A1: Client {#client-sec-a1}

* Common Context:
    - Master Secret: 0x0102030405060708090a0b0c0d0e0f10 (16 bytes)
    - Master Salt: 0x9e7ca92223786340 (8 bytes)
    - Common IV: 0x0x2ca58fb85ff1b81c0b7181b85e (13 bytes)
    - ID Context: 0x37cbf3210017a2d3 (8 bytes)
* Sender Context:
    - Sender ID: 0xa1 (0 byte)
    - Sender Key: 0xaf2a1300a5e95788b356336eeecd2b92 (16 bytes)
    - Sender Seq Number: 00
    - Sender IV: 0x2ca58fb85ff1b81c0b7181b85e (using Partial IV: 00)
    - Private Key TBD
* Recipient Context 1:
    - Recipient ID: 0xb1 (1 byte)
    - Recipient Key: 0xe39a0c7c77b43f03b4b39ab9a268699f (16 bytes)
    - Recipient IV: 0x2da58fb85ff1b81d0b7181b85e (using Partial IV: 00)
    - Public Key TBD
* Recipient Context 2:
    - Recipient ID: 0xb2 (1 byte)
    - Recipient Key: TODO (16 bytes)
    - Recipient IV: TODO (using Partial IV: TODO)
    - Public Key TBD

### Security Context A2: Client {#client-sec-a2}

* Common Context:
    - Master Secret: 0x0102030405060708090a0b0c0d0e0f10 (16 bytes)
    - Master Salt: 0x9e7ca92223786340 (8 bytes)
    - Common IV: 0x0x2ca58fb85ff1b81c0b7181b85e (13 bytes)
    - ID Context: 0x37cbf3210017a2d3 (8 bytes)
* Sender Context:
    - Sender ID: 0xa2 (0 byte)
    - Sender Key: TBD (16 bytes)
    - Sender Seq Number: 00
    - Sender IV: TBD (using Partial IV: 00)
    - Private Key TBD
* Recipient Context 1:
    - Recipient ID: 0xb1 (1 byte)
    - Recipient Key: 0xe39a0c7c77b43f03b4b39ab9a268699f (16 bytes)
    - Recipient IV: 0x2da58fb85ff1b81d0b7181b85e (using Partial IV: 00)
    - Public Key TBD

### Security Context A3: Client {#client-sec-a3}

(incorrect sec ctx) modified Sender Key

* Common Context:
    - Master Secret: 0x0102030405060708090a0b0c0d0e0f10 (16 bytes)
    - Master Salt: 0x9e7ca92223786340 (8 bytes)
    - Common IV: 0x0x2ca58fb85ff1b81c0b7181b85e (13 bytes)
    - ID Context: 0x37cbf3210017a2d3 (8 bytes)
* Sender Context:
    - Sender ID: 0xa3 (0 byte)
    - Sender Key: TBD (16 bytes)
    - Sender Seq Number: 00
    - Sender IV: TBD (using Partial IV: 00)
    - Private Key TBD
* Recipient Context 1:
    - Recipient ID: 0xb1 (1 byte)
    - Recipient Key: 0xe39a0c7c77b43f03b4b39ab9a268699f (16 bytes)
    - Recipient IV: 0x2da58fb85ff1b81d0b7181b85e (using Partial IV: 00)
    - Public Key TBD

### Security Context A4: Client {#client-sec-a4}

(incorrect sec ctx) modified Private Key

* Common Context:
    - Master Secret: 0x0102030405060708090a0b0c0d0e0f10 (16 bytes)
    - Master Salt: 0x9e7ca92223786340 (8 bytes)
    - Common IV: 0x0x2ca58fb85ff1b81c0b7181b85e (13 bytes)
    - ID Context: 0x37cbf3210017a2d3 (8 bytes)
* Sender Context:
    - Sender ID: 0xa4 (0 byte)
    - Sender Key: TBD (16 bytes)
    - Sender Seq Number: 00
    - Sender IV: TBD (using Partial IV: 00)
    - Private Key TBD
* Recipient Context 1:
    - Recipient ID: 0xb1 (1 byte)
    - Recipient Key: 0xe39a0c7c77b43f03b4b39ab9a268699f (16 bytes)
    - Recipient IV: 0x2da58fb85ff1b81d0b7181b85e (using Partial IV: 00)
    - Public Key TBD

### Security Context B1: Server {#server-sec-b1}

* Common Context:
    - Master Secret: 0x0102030405060708090a0b0c0d0e0f10 (16 bytes)
    - Master Salt: 0x9e7ca92223786340 (8 bytes)
    - Common IV: 0x2ca58fb85ff1b81c0b7181b85e (13 bytes)
    - ID Context: 0x37cbf3210017a2d3 (8 bytes)
* Sender Context:
    - Sender Id: 0xb1 (1 byte)
    - Sender Key: 0xe39a0c7c77b43f03b4b39ab9a268699f (16 bytes)
    - Sender Seq Number: 00
    - Sender IV: 0x2da58fb85ff1b81d0b7181b85e (using Partial IV: 00)
    - Private Key TBD
* Recipient Context 1:
    - Recipient Id: 0xa1 (0 byte)
    - Recipient Key: 0xaf2a1300a5e95788b356336eeecd2b92 (16 bytes)
    - Recipient IV: 0x2ca58fb85ff1b81c0b7181b85e (using Partial IV: 00)
    - Public Key TBD
* Recipient Context 2:
    - Recipient ID: 0xa2 (1 byte)
    - Recipient Key: TODO (16 bytes)
    - Recipient IV: TODO (using Partial IV: TODO)
    - Public Key TBD

### Security Context B2: Server {#server-sec-b2}

* Common Context:
    - Master Secret: 0x0102030405060708090a0b0c0d0e0f10 (16 bytes)
    - Master Salt: 0x9e7ca92223786340 (8 bytes)
    - Common IV: 0x2ca58fb85ff1b81c0b7181b85e (13 bytes)
    - ID Context: 0x37cbf3210017a2d3 (8 bytes)
* Sender Context:
    - Sender Id: 0xb2 (1 byte)
    - Sender Key: TBD (16 bytes)
    - Sender Seq Number: 00
    - Sender IV: TBD (using Partial IV: 00)
    - Private Key TBD
* Recipient Context:
    - Recipient Id: 0xa1 (0 byte)
    - Recipient Key: 0xaf2a1300a5e95788b356336eeecd2b92 (16 bytes)
    - Recipient IV: 0x2ca58fb85ff1b81c0b7181b85e (using Partial IV: 00)
    - Public Key TBD

### Security Context B3: Server {#server-sec-b3} 

(incorrect sec ctx) modified Sender Key

* Common Context:
    - Master Secret: 0x0102030405060708090a0b0c0d0e0f10 (16 bytes)
    - Master Salt: 0x9e7ca92223786340 (8 bytes)
    - Common IV: 0x2ca58fb85ff1b81c0b7181b85e (13 bytes)
    - ID Context: 0x37cbf3210017a2d3 (8 bytes)
* Sender Context:
    - Sender Id: 0xb3 (1 byte)
    - Sender Key: TBD (16 bytes) *** Key needs to be manually modified
    - Sender Seq Number: 00
    - Sender IV: TBD (using Partial IV: 00)
    - Private Key TBD
* Recipient Context:
    - Recipient Id: 0xa1 (0 byte)
    - Recipient Key: 0xaf2a1300a5e95788b356336eeecd2b92 (16 bytes)
    - Recipient IV: 0x2ca58fb85ff1b81c0b7181b85e (using Partial IV: 00)
    - Public Key TBD

### Security Context B4: Server {#server-sec-b4} 

(incorrect sec ctx) different Private Key

* Common Context:
    - Master Secret: 0x0102030405060708090a0b0c0d0e0f10 (16 bytes)
    - Master Salt: 0x9e7ca92223786340 (8 bytes)
    - Common IV: 0x2ca58fb85ff1b81c0b7181b85e (13 bytes)
    - ID Context: 0x37cbf3210017a2d3 (8 bytes)
* Sender Context:
    - Sender Id: 0xb4 (1 byte)
    - Sender Key: TBD (16 bytes) 
    - Sender Seq Number: 00
    - Sender IV: TBD (using Partial IV: 00)
    - Private Key TBD *** Key does not match Sec Ctx A Public Key
* Recipient Context:
    - Recipient Id: 0xa1 (0 byte)
    - Recipient Key: 0xaf2a1300a5e95788b356336eeecd2b92 (16 bytes)
    - Recipient IV: 0x2ca58fb85ff1b81c0b7181b85e (using Partial IV: 00)
    - Public Key TBD

### Resources

The list of resources the OSCORE-aware server must implement is the following:

* /oscore/hello/coap : authorized method: GET, returns the string "Hello World!" with content-format 0 (text/plain)
* /oscore/hello/1 : protected resource, authorized method: GET, returns the string "Hello World!" with content-format 0 (text/plain)

------

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

### 4.1 Correct Group OSCORE Use

#### 4.1.1. Identifier: TEST_1a {#test-1a}

**Objective** : Perform an OSCORE Group transaction, where the client and server retrieve a recipient context that they have stored in memory. (Client side)

**Configuration** :

_client security context_: [Security Context A1](#client-sec-a1), with:

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
|      |          | and decrypts the message: OSCORE verification succeeds   |
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

_server security context_: 
[Security Context B1](#server-sec-b1), with:

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
|      |          | and decrypts the message: OSCORE verification succeeds   |
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
|      |          | received Recipient Id = 0x02 ; Client decrypts           |
|      |          | the message: OSCORE verification succeeds                |
+------+----------+----------------------------------------------------------+
| 6    | Check    | Client parses the decrypted response and continues the   |
|      |          | CoAP processing; expected 2.05 Content Response with:    |
|      |          |                                                          |
|      |          | - Content-Format = 0 (text/plain)                        |
|      |          | - Payload = "Hello World!"                               |
+------+----------+----------------------------------------------------------+
| 7    | Verify   | Client displays the received packet                      |
+------+----------+----------------------------------------------------------+

#### 4.1.4. Identifier: TEST_2b {#test-2b}

**Objective** : Perform an OSCORE Group transaction, where the client receive a Recipient Id that it does not have in memory, and derives the Recipient Context for it. (Server side)

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
|      |          | and decrypts the message: OSCORE verification succeeds   |
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

#### 4.1.3. Identifier: TEST_3a {#test-3a}

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
|      |          | and decrypts the message: OSCORE verification succeeds   |
+------+----------+----------------------------------------------------------+
| 6    | Check    | Client parses the decrypted response and continues the   |
|      |          | CoAP processing; expected 2.05 Content Response with:    |
|      |          |                                                          |
|      |          | - Content-Format = 0 (text/plain)                        |
|      |          | - Payload = "Hello World!"                               |
+------+----------+----------------------------------------------------------+
| 7    | Verify   | Client displays the received packet                      |
+------+----------+----------------------------------------------------------+

#### 4.1.4. Identifier: TEST_3b {#test-3b}

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
|      |          | received Recipient Id = 0x00 ; Client decrypts           |
|      |          | the message: OSCORE verification succeeds                |
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

#### 4.2.1. Identifier: TEST_3a {#test-3a}

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
|      |          | received Recipient Id = 0x03 ; Client tries to decrypt   |
|      |          | the message: OSCORE verification fail                    |
+------+----------+----------------------------------------------------------+
| 6    | Verify   | Client displays the received packet                      |
+------+----------+----------------------------------------------------------+

#### 4.2.2. Identifier: TEST_3b {#test-3b}

**Objective** : Perform an OSCORE Group transaction where the client receive a Recipient Id that it does not have in memory, and derives the Recipient Context for it, but decryption fails. (Server side)

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
|      |          | and decrypts the message: OSCORE verification succeeds   |
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

***** Server

#### 4.2.1. Identifier: TEST_3a {#test-3a}

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
|      |          | received Recipient Id = 0x03 ; Client tries to decrypt   |
|      |          | the message: OSCORE verification fail                    |
+------+----------+----------------------------------------------------------+
| 6    | Verify   | Client displays the received packet                      |
+------+----------+----------------------------------------------------------+

#### 4.2.2. Identifier: TEST_3b {#test-3b}

**Objective** : Perform an OSCORE Group transaction where the client receive a Recipient Id that it does not have in memory, and derives the Recipient Context for it, but decryption fails. (Server side)

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
|      |          | and decrypts the message: OSCORE verification succeeds   |
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

#### 4.1.3. Identifier: TEST_4a {#test-4a}

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
|      |          | received Recipient Id = 0x04 ; Client decrypts           |
|      |          | the message successfully; Client verifies signature:     |
|      |          | signature verification fails                             |
+------+----------+----------------------------------------------------------+
| 6    | Verify   | Client displays the received packet                      |
+------+----------+----------------------------------------------------------+

#### 4.1.4. Identifier: TEST_4b {#test-4b}

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
|      |          | and decrypts the message: OSCORE verification succeeds   |
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

**********

