# Tests Specification for OSCORE

[//]: # (use Pandoc : pandoc spec.md -o spec.html)

## Table of Contents
1. [Notes](#notes)
2. [Security Contexts and Resources](#security-contexts-and-resources)
    1. [Security Context A: Client](#client-sec)
    2. [Security Context B: Server](#server-sec)
    3. [Resources](#resources)
3. [Set up the environment](#env-setup)
    1. [Test 0a](#test-0a)
    2. [Test 0b](#test-0b)
4. [Correct OSCORE use](#correct-oscore-use)
    1. [GET test](#get)
        1. [Test 1a](#test-1a)
        2. [Test 1b](#test-1b)


## 1. Notes

CoAP Version is 1 in all the tests.

The client and server may optionally display external_aad and COSE object (before and after compression) to simplify debugging.

When non-indicated, CoAP messages can be NON or CON (implementer's choice).

To be able to run Test 16, the implementer must run an OSCORE-unaware server.

The number used as Object-Security option number is set to 9 in this document.

Although these specifications test the implementation of group oscore, this version does not rely on multicast communication.
The test specified below are in fact to be run on unicast between two nodes, to test the group OSCORE implementation features.

## 2. Security Contexts and Resources

### Security Context A: Client {#client-sec}

* Common Context:
    - Master Secret: 0x0102030405060708090a0b0c0d0e0f10 (16 bytes)
    - Master Salt: 0x9e7ca92223786340 (8 bytes)
    - Common IV: 0x0x2ca58fb85ff1b81c0b7181b85e (13 bytes)
    - ID Context: 0x37cbf3210017a2d3 (8 bytes)
* Sender Context:
    - Sender ID: 0x (0 byte)
    - Sender Key: 0xaf2a1300a5e95788b356336eeecd2b92 (16 bytes)
    - Sender Seq Number: 00
    - Sender IV: 0x2ca58fb85ff1b81c0b7181b85e (using Partial IV: 00)
* Recipient Context:
    - Recipient ID: 0x01 (1 byte)
    - Recipient Key: 0xe39a0c7c77b43f03b4b39ab9a268699f (16 bytes)
    - Recipient IV: 0x2da58fb85ff1b81d0b7181b85e (using Partial IV: 00)

### Security Context B: Server {#server-sec}

* Common Context:
    - Master Secret: 0x0102030405060708090a0b0c0d0e0f10 (16 bytes)
    - Master Salt: 0x9e7ca92223786340 (8 bytes)
    - Common IV: 0x2ca58fb85ff1b81c0b7181b85e (13 bytes)
    - ID Context: 0x37cbf3210017a2d3 (8 bytes)
* Sender Context:
    - Sender Id: 0x01 (1 byte)
    - Sender Key: 0xe39a0c7c77b43f03b4b39ab9a268699f (16 bytes)
    - Sender Seq Number: 00
    - Sender IV: 0x2da58fb85ff1b81d0b7181b85e (using Partial IV: 00)
* Recipient Context:
    - Recipient Id: 0x (0 byte)
    - Recipient Key: 0xaf2a1300a5e95788b356336eeecd2b92 (16 bytes)
    - Recipient IV: 0x2ca58fb85ff1b81c0b7181b85e (using Partial IV: 00)

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

## 4. Correct OSCORE use

### 4.1 GET Tests {#get}

#### 4.1.1. Identifier: TEST_1a {#test-1a}

**Objective** : Perform a simple GET transaction using OSCORE, Content-Format and Uri-Path option (Client side)

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
|      |          | - Object-Security option                                 |
|      |          | - Payload: ciphertext including:                         |
|      |          |       * Code: GET                                        |
|      |          |       * Uri-Path : /oscore/hello/1                       |
+------+----------+----------------------------------------------------------+
| 3    | Verify   | Client displays the sent packet                          |
+------+----------+----------------------------------------------------------+
| 4    | Check    | Client parses the response; expected:                    |
|      |          | 2.04 Changed Response with:                              |
|      |          |                                                          |
|      |          | - Object-Security option                                 |
|      |          | - Payload                                                |
+------+----------+----------------------------------------------------------+
| 5    | Verify   | Client decrypts the message: OSCORE verification succeeds|
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

**Objective** : Perform a simple GET transaction using OSCORE, Content-Format and Uri-Path option (Server side)

**Configuration** :

_server security context_: 
[Security Context B](#server-sec), with:

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
|      |          | - Object-Security option                                 |
|      |          | - Payload                                                |
+------+----------+----------------------------------------------------------+
| 4    | Verify   | Server decrypts the message: OSCORE verification succeeds|
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
|      |          | - Object-Security option                                 |
|      |          | - Payload: ciphertext including:                         |
|      |          |     * Code: 2.05 Content Response                        |
|      |          |     * Content-Format = 0 (text/plain)                    |
|      |          |     * Payload = "Hello World!"                           |
+------+----------+----------------------------------------------------------+
| 8    | Verify   | Server displays the sent packet                          |
+------+----------+----------------------------------------------------------+

