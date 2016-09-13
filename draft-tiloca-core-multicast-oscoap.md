---
title: Secure group communication for CoAP
# abbrev: 
docname: draft-tiloca-core-multicast-oscoap-latest

# stand_alone: true

ipr: trust200902
area: Applications
wg: CoRE Working Group
kw: Internet-Draft
cat: std

coding: us-ascii
pi:    # can use array (if all yes) or hash here
#  - toc
#  - sortrefs
#  - symrefs
  toc: yes
  sortrefs:   # defaults to yes
  symrefs: yes

author:
      -
        ins: M. Tiloca
        name: Marco Tiloca
        org: SICS
        street: TBD
        city: Kista
        code: TBD
        country: Sweden
        email: marco@sisc.se

normative:

#  I-D.ietf-cose-msg:
  RFC2119:
  RFC7252:
  RFC7641:

informative:

  I-D.selander-ace-object-security:
  RFC7228:

--- abstract

This document describes how to enable secure communication in a group of devices that exchange (multicast) CoAP messages.

The proposed approach relies on Object Security for CoAP (OSCOAP) {{I-D.selander-ace-object-security}}, with reference to the "one request - one response" model. No extensions or changes to OSCOAP are required.

Furthermore, the proposed approach provides source authentication of group messages, by means of digital signatures produced through asymmetric private keys and embedded in the secure messages.


--- middle

# Introduction # {#intro}

 Insert introduction here.

## Terminology ## {#terminology}

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in {{RFC2119}}. These words may also appear in this document in lowercase, absent their normative meanings.

Readers are expected to be familiar with the terms and concepts described in {{RFC7252}} and {{RFC7641}}.

Terminology for constrained environments, such as "constrained device", "constrained-node network", is defined in {{RFC7228}}.

# Assumptions and requirements # {#assumptions-and-requirements}

A node N joins a group G by explicitly interacting with a responsible Group Manager. Upon joining a group, a node receives a unique node ID IDN with respect to a certain context, which is associated to the group G and shared among all its members. These pieces of information are provided by the Group Manager through out-of-band means or other pre-established secure channels (further details are out of scope).

Node IDs are necessary in order to identify a given node with no commitment to any protocol-relevant identifiers as IP addresses. The Group Manager generates and manages node IDs in order to ensure their uniqueness within a same multicast group. That is, there cannot be multiple nodes that belong to the same group and are associated to a same node ID.

On the other hand, the group context includes, among other security attributes:

1. a symmetric group key KG;
2. a unique context identifier IDGC.

In particular, two distinct group contexts must be associated with different group context identifiers. Since different groups can be managed by different Group Managers, a possible way to ensure uniqueness of group context identifiers is to consider the format IDGC = \{IDG \|\| IPGM\}, where i) IDG is the group identifier of group G, and is unique in the pool of groups including G and managed by that Group Manager; ii) and IPGM is the IP address of the Group Manager. Possible alternative ways to ensure uniqueness of group context identifiers are out of the scope of this document.

In order to verify digital signatures of secure messages and ensure their source authenticity, every node in the group is supposed to be pre-provisioned with the necessary public keys associated to message originators, or to get such keys from dedicated entities when it becomes necessary.

A group member can behave as sender node and/or listener node. As a sender node, it can transmit multicast request messages to the group. As a listener node, it receives multicast request messages from a sender node, and possibly replies by transmitting unicast response messages. Upon joining the group, nodes are not required to know how many and what nodes are active in the group.

We assume that there are no internal adversaries in the multicast group, i.e. group members are not compromised and are not interested in inspecting and/or tampering with messages sent to / received from other members in the group. Establishment, revocation and renewal of the group context and key material is out of the scope of this document.

According to the multicast CoAP specification {{RFC7252}}, any possible proxy entity is supposed to know about the sender nodes in the group and to not perform aggregation of response messages. Also, every sender node expects and is able to handle multiple unicast response messages associated to a same multicast request message.

# Support for multicast communication # {#multicast-communication}

OSCOAP messages exchanged in a group G convey a COSE object whose header includes also:

1. The group context identifier IDGC as "kid" parameter. The most significant bit of IDGC is conventionally set to '1', in order to recognize it as a group context identifier (rather than a unicast context identifier), and to enable the correct parsing of the rest of the COSE object.

2. The ID of the sender node, if the OSCOAP message is a multicast request message.

3. The ID of the listener node, if the OSCOAP message is a unicast response to a multicast request.

4. The Group transaction identifier (GTid), if the OSCOAP message is a unicast response to a multicast request. The GTid is defined as {IDGC , IDS , SN}, where IDS is the ID of the sender node that sent the associated multicast request message, and SN is the write sequence number considered by the sender node when securing and transmitting that request message.

Even in the presence of an intermediary proxy, this makes it possible:

1. on the listener side, to understand which is the specific sender node originating a multicast request message, so decoupling message security and IP addressing;

2. on the sender node, to understand which is the specific listener node originating a unicast response message as a reply to a multicast request message, so decoupling message security and IP addressing;

3. possibly adopt DTLS to protect hop-by-hop communication between a sender node and a proxy (and vice versa), and between a proxy and a listener node (and vice versa), together with the adoption of secure group communication based on OSCOAP.

# Binding between requests and responses # {#req-resp-binding}

The mapping between a unicast response message and the associated multicast request message relies on the same principle adopted in OSCOAP {{I-D.selander-ace-object-security}}. That is, it is based on the Group Transaction Identifier (GTid), included by listener nodes in the COSE object conveyed in their own unicast response message to a multicast request message.

# Security material # {#sec-material}

Each node maintains data structures named read/write group communication states. In particular, each node N in a group G stores:

1. a single write group communication state, considered to secure outgoing multicast request messages and outgoing unicast response messages to received multicast requests;

2. a read group communication state for each different node that originated i) a received multicast request message; or ii) a unicast response message to a multicast request previously sent by node N itself. Read group communication states are considered to process such incoming secure messages.

Once it has fully joined a group G, a node N creates a write communication state where it stores:

1. a write sequence number SNWN initialized to 0

2. a node key KN as KN = HKDF\_Expand(KG , IDN \|\| "Key" \|\| Key\_lenght)

3. a node initialization vector IVN as IVN = HKDF\_Expand(IVG , IDN \|\| "IV" \|\| IV_lenght)

4. its own asymmetric private key K-N

# Message exchange and processing # {#coap-headers-and-options} 

TODO.

## Step 1 - Transmission of multicast request messages ##

A sender node S refers to its own write group communication state and proceeds as follows.

First, node S produces a signature X of the "Plaintext" and "Additional authenticated data" defined in {{I-D.selander-ace-object-security}}, by means of its own asymmetric private key K-S.

Second, node S considers its node key KS and its node initialization vector IVS to protect the outgoing multicast request message. To this end, it produces a secure OSCOAP message {{I-D.selander-ace-object-security}}, computing the "Cipher text" from the "Plaintext", the "Additional authenticated data", and the signature X. In particular, node S considers the write sequence number SNWS from its own write group communication state to generate a unique IV for the AEAD.

Finally, node S increments the value of SNWS by 1 and transmits the multicast request message to the group G.

## Step 2 - Reception and processing of multicast request messages ##

A listener node L processeses a multicast request message as such, because of the multicast IP destination address specified in the message IP header. If the multicast request message has the most significant bit of the "kid" parameter in the COSE object's header set to 0, node L discards the message as invalid.

Otherwise, node L retrieves the group context identifier IDGC, the sender identifier IDS, and the write sequence number SNWS, from the COSE object's header.

If this is the very first multicast request message received from that sender node (i.e. node L is not maintaining a read group communication state associated to node S), then node L:

{{I-D.selander-ace-object-security}} derives the sender node key KS = HKDF\_Expand(KG , IDS \|\| "Key" \|\| Key\_lenght) and the sender node initialization vector IVS = HKDF\_Expand(IVG , IDS \|\| "IV" \|\| IV_lenght) associated to node S;

2. creates a read group communication state associated to node S, where it stores KS, IVS, SNWS, and node S' public key K+S.

Then, node L considers the sender node key KS, the sender node initialization vector IVS, and the write sequence number SNWS to unsecure the received multicast request message, according to the OSCOAP procedure described in {{I-D.selander-ace-object-security}}. After that, it verifies the signature X conveyed in the COSE object, by means of sender S' public key K+S.

If the multicast request message is successfully decrypted and verified, node L updates the sequence number stored in the considered read group communication state, according to the adopted anti-replay policies.

## Step 3 - Transmission of unicast response messages ##

In order to reply to a multicast request message sent by node S, the listener node L refers to its own write group communication state and builds a secure unicast response message as follows.

First, node L produces a signature Y of the "Plaintext" and "Additional authenticated data" defined in {{I-D.selander-ace-object-security}}, by means of its own asymmetric private key K-L.

Second, node L considers its node key KL and its node initialization vector IVL to protect the outgoing unicast response message. To this end, it produces a secure OSCOAP message {{I-D.selander-ace-object-security}}, computing the "Cipher text" from the "Plaintext", the "Additional authenticated data", and the signature Y. In particular, node L considers the write sequence number SNWL from its own write group communication state to generate a unique IV for the AEAD.

Finally, node L increments the value of SNWL by 1 and transmits the unicast response message to the sender node S.

## Step 4 - Processing of unicast response messages on sender nodes ##

A sender node S processes a unicast response message to one of its own previously sent multicast request messages as follows.

Node S retrieves the group context identifier IDGC and the write sequence number SNWL, from the COSE object's header. In particular, IDGC is retrieved from the group transaction identifier GTid.

If this is the very first unicast response message received from node L as a reply to one of S' multicast request messages (i.e. node S is not maintaining a read group communication state associated to node L), then node S:

1. derives the node key KL = HKDF\_Expand(KG , IDL \|\| "Key" \|\| Key\_lenght) and the node initialization vector IVL = HKDF\_Expand(IVG , IDL \|\| "IV" \|\| IV\_lenght) associated to node L.

2. creates a read group communication state associated to node L, where it stores KL, IVL, SNWL, and node L's public key K+L.

Then, node S considers the listener node key KL, the listener node initialization vector IVL, and the write sequence number SNWL to unsecure the received unicast response message, according to the OSCOAP procedure described in {{I-D.selander-ace-object-security}}. After that, it verifies the signature Y conveyed in the COSE object, by means of listener L's public key K+L.

If the unicast response message is successfully decrypted and verified, node S updates the sequence number stored in the considered read group communication state, according to the adopted anti-replay policies.

# Acknowledgments #

TODO.

--- back

# Appendix # {#appendix-a}

TODO.

--- fluff



