# Trivial File Transfer Protocol

This is a page about TFTP, its specifications and applications.

### Table of Contents:
- [Overview](#overview)
- [Specification](#specification)
- [Implementations](#implementations)
- [Other Links](#other)

<a name="overview"></a>

## Overview

Trivial Transfer File Protocol, or TFTP is a lightweight file transfer protocol designed to be simple and easy to implement. For this reason, it lacks nearly all the features of a [traditional FTP](https://en.wikipedia.org/wiki/File_Transfer_Protocol) server.

TFTP is often implemented on top of [User Datagram Protocol](https://en.wikipedia.org/wiki/User_Datagram_Protocol), though it can utilize other methods. The paradigm of TFTP recommends a high speed, high risk protocol, as TFTP's [lockstep](https://en.wikipedia.org/wiki/Lockstep_(computing)) system nullifies the need for a more secure protocol like [TCP](https://en.wikipedia.org/wiki/Transmission_Control_Protocol).

### Lockstep

TFTP describes a lockstep system for transfering files in which the next data packet is only sent after the previous one has been acknowledged as being received. Therefore, the acknowledgment of a `DATA ` packet is an `ACK`, and the acknowledgment for an `ACK` is the next `DATA` packet.

Each `DATA` packet is contains a block of 512 bytes. If the block is less than 512 bytes, it signals the end of the file.

<a name="specification"></a>

## Specification

TFTP is an open standard currently maintained by [IETF](https://www.ietf.org/about/), codified in the following RCF's:

#### [RFC 1350](https://tools.ietf.org/html/rfc1350)
RFC 1350 specifies the core of TFTP.

#### [RFC 1350](https://tools.ietf.org/html/rfc1350)




<a name="implementations"></a>

## Implementations


<a name="other"></a>

## Other
