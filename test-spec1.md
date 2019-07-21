# Tests Specification for OSCORE Group Communication

[//]: # (use Pandoc : pandoc spec.md -o spec.html)

## Table of Contents
1. [Notes](#notes)
2. [Security Contexts and Resources](#security-contexts-and-resources)
    1. [Security Context](#group-sec)
    2. [Resources](#resources)
3. [Summary of Tests](#summary)


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

### Modifications from draft -05

1.  Section 3.1.2 - The fields OSCORE_option and options are swapped in the aad_array structure.  This only changes the external_aad for signing and not for encryption.

2.  Section 9.2 - The items are wrapped in an array when more than one elemet exists.  This is reflected in the fields below.

3. Dynamic derivation is optional

4. Error message for signature validation is 4.00

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

3: Perform an OSCORE Group transaction, where the client receives a Recipient Id that it does not have in memory, and succesfully derives the Recipient Context for it. (optional)

4: Perform an OSCORE Group transaction, where the server receives a Recipient Id that it does not have in memory, and succesfully derives the Recipient Context for it. (optional)

5: Perform an OSCORE Group transaction where the client receives a response but decryption fails. Optionally, it does not have in memory the Recipient Id, and derives the Recipient Context for it.

6: Perform an OSCORE Group transaction where the server receives a request but decryption fails. Optionally, it does not have in memory the Recipient Id, and derives the Recipient Context for it.

7: Perform an OSCORE Group transaction where the client receives a response but signature verification fails. Optionally, it does not have in memory the Recipient Id, and derives the Recipient Context for it.

8: Perform an OSCORE Group transaction where the server receives a request but signature verification fails. Optionally, it does not have in memory the Recipient Id, and derives the Recipient Context for it.

9: 2 clients and 1 server with both clients sending the same sequence number, SEQ1, client 1 sending it once and client 2 sending it twice. Server detects replay for client 2 and accept the message from client 1.

10: replay handling on the client - client getting 2 responses from the same server (fresh response or replay) it discards the second.
