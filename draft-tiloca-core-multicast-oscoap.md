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

  I-D.ietf-cose-msg:
  RFC2119:
  RFC7252:
  RFC7641:

informative:

  I-D.selander-ace-object-security:
  RFC7228:

--- abstract

This document describes how to enable secure communication in a group of devices that exchange (multicast) CoAP messages.

The proposed approach relies on Object Security for CoAP (OSCOAP) {{I-D.selander-ace-object-security}}, with reference to the "one request - one response" model. Only minor extensions or changes to OSCOAP are required.

Furthermore, the proposed approach provides source authentication of group messages, by means of digital signatures produced through asymmetric private keys and embedded in the secure messages.


--- middle

# Introduction # {#intro}

TODO: motivation

Even in the presence of an intermediary proxy, the use of multicast OSCOAP makes it possible:

1. on the listener side, to understand which is the specific broadcaster originating a multicast request message, so decoupling message security and IP addressing;
2. on the broadcaster side, to understand which is the specific listener originating a unicast response message as a reply to a multicast request message, so decoupling message security and IP addressing;
3. possibly adopt DTLS to protect hop-by-hop communication between a broadcaster and a proxy (and vice versa), and between a proxy and a listener (and vice versa), together with the adoption of secure group communication based on OSCOAP.

## Terminology ## {#terminology}

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be interpreted as described in {{RFC2119}}. These words may also appear in this document in lowercase, absent their normative meanings.

Readers are expected to be familiar with the terms and concepts described in {{RFC7252}} and {{RFC7641}}.

Terminology for constrained environments, such as "constrained device", "constrained-node network", is defined in {{RFC7228}}.

TODO: OSCOAP terminology.

# Scope description # {#scope}

An endpoint joins a group by explicitly interacting with a responsible Group Manager. An endpoint member of a group can behave as a broadcaster and/or as a listener. As a broadcaster, it can transmit multicast request messages to the group. As a listener, it receives multicast request messages from a broadcaster, and possibly replies by transmitting unicast response messages. Upon joining the group, endpoints are not required to know how many and what endpoints are active in the group.

According to the multicast CoAP specification {{RFC7252}}, any possible proxy entity is supposed to know about the broadcasters in the group and to not perform aggregation of response messages. Also, every broadcaster expects and is able to handle multiple unicast response messages associated to a same multicast request message.

An endpoint that is member of a group is identified by an endpoint ID, which is associated to the group and shared among all its members.
Endpoint IDs are necessary in order to identify a given endpoint with no commitment to any protocol-relevant identifiers as IP addresses. The Group Manager generates and manages endpoint IDs in order to ensure their uniqueness within a same multicast group. That is, there cannot be multiple endpoints that belong to the same group and are associated to a same endpoint ID.

An endpoint needs to have other data, in order to participate in the group communication, such for example the shared keying material to protect and verify the message and the public keys of the broadcasters of the groups, in order to verify digital signatures of secure messages and ensure their source authenticity. These pieces of information are provided by the Group Manager through out-of-band means or other pre-established secure channels; further details about establishment, revocation and renewal of the group context and key material is out of the scope of this document.

# Security context # {#sec-context}

To support multicast communication secured with OSCOAP, each endpoint maintains a security context as defined in section 3 of {{I-D.selander-ace-object-security}}. In particular, each endpoint in a group stores:

1. one Common Context, containing the Context Identifier and the Base Key used to derive the keying material (Section 3.2 of {{I-D.selander-ace-object-security}});
2. one Sender Context, used to secure outgoing messages;
3. one Recipient Context for each different broadcaster from which messages are received, used to process such incoming secure messages.

In addition to what is defined in {{I-D.selander-ace-object-security}}, the Sender Context stores the endpoint's own asymmetric public-private key pair, and each Recipient Context stores the other broadcaster's public key.

# Message exchange # {#mess-exchange}

The message exchanged is protected as a non-multicast message, with the following addition: the Sender ID of the endpoint must be sent explicitely. The Sender ID MUST be sent in the COSE object, as defined in Section TBD of {{I-D.selander-ace-object-security}}).

# Binding between requests and responses # {#req-resp-binding}

The mapping between a unicast response message and the associated multicast request message relies on the same principle adopted in OSCOAP {{I-D.selander-ace-object-security}}. That is, it is based on the Transaction Identifier, included by listener endpoints in the COSE object conveyed in their own unicast response message to a multicast request message.

# Message processing  # {#mess-processing} 

The processing for securing multicast requests and unicast response with OSCOAP is the same as in non-multicast communication, with this difference: the sending endpoint uses its own private key to create a counter signature of the COSE_Encrypt0 object (Appendix C.4 of {{I-D.ietf-cose-msg}}). The recipient uses the corresponding public key to verify the counter signature before proceding with the verification of the message.

# Acknowledgments #

TODO.

--- back

# Appendix #



--- fluff



