# Trivial File Transfer Protocol

This is a page about TFTP, its specifications and applications.

### Table of Contents:
- [Overview](#overview)
- [Specification](#specification)
- [Implementations](#implementations)
- [Other Links](#other)

<a name="overview"></a>

## Overview

Trivial Transfer File Protocol, or TFTP is a lightweight file transfer protocol designed to be simple and easy to implement. For this reason, it lacks nearly all the features of [traditional FTP](https://en.wikipedia.org/wiki/File_Transfer_Protocol).

TFTP is often implemented on top of [User Datagram Protocol](https://en.wikipedia.org/wiki/User_Datagram_Protocol), though it can utilize others. The paradigm of TFTP recommends a high speed, high risk protocol, as TFTP's [lockstep](https://en.wikipedia.org/wiki/Lockstep_(computing)) system nullifies the need for a more secure protocol like [TCP](https://en.wikipedia.org/wiki/Transmission_Control_Protocol). TFTP is often used for the delivery of configuration and firmware to embedded units over LAN, although it can also be used over a wide area.

### Lockstep

TFTP describes a lockstep system for transfering files in which the next data packet is only sent after the previous one has been acknowledged as received. Therefore, the acknowledgment of a `DATA ` packet is an `ACK`, and the acknowledgment for an `ACK` is the next `DATA` packet.

Each `DATA` packet is contains a block of 512 bytes. If the block is less than 512 bytes, it signals the end of the file. Error packets can also be sent by sender or receiver. Some of these errors signal the early end of a transfer, while others may merely notify.

### Connection

Connection to a TFTP server takes place over port 69 by default. The server is listening for a Read Request (RRQ) or a Write Request (WRQ) on that port to initiate a transfer. The client should select a random UDP source port to connect from, which will be the port it will listen on from then on. Similarly, once the server has received the RRQ or WRQ, it should select a random UDP source port to send DATA / ACKs from, which will be the port it listens on for the remainder. These random ports are called Transfer Identifiers (TIDs). Packets received with the wrong TID during a transfer are ignored.

### Packets

The formula for a TFTP packet is: [Opcode: 2B] | [packet options] | [Null terminated string].  
opc: Opcode  
blk: Block number

`RRQ/WRQ`:  
```
  opc         filename                   mode
  2B      null term string         null term string
| 01  |  f.txt[null terminator] | octet [null term]
\0\x01  \x66\x2e\x74\x78\x74\0   \x6f\x63\x74\x65\x74\0

hex string: 0001662e747874006f6374657400
```

`DATA`:  
```
  opc   blk       filedata  
  2B    2B  
| 03  | 01 |      text | 0  
\0\x03\0\x01 \x74\x65\x78\x74 \0  

hex string: 000300017465787400
```

`ACK`:  
```
   opc   blk
   2B    2B
|  04  | 01 | 
\0\x04 \0\x01

hex string: 00040001
```

### Reference

#### Opcodes  
1. RRQ - Read Request
2. WRQ - Write Request
3. DATA - Data packet
4. ACK - Acknowlegement
5. ERROR - Error packet
6. OACK - Option Acknowlegement


<a name="specification"></a>

## Specification

TFTP is an open standard currently maintained by [IETF](https://www.ietf.org/about/), codified in the following RCF's:

#### [RFC 1350](https://tools.ietf.org/html/rfc1350)  
RFC 1350 specifies the core of TFTP. It replaced [RFC 783](https://tools.ietf.org/html/rfc783) as the specification for the TFTP protocol.

#### [RFC 2347](https://tools.ietf.org/html/rfc2347)  
RFC 2347 specifies an option extension for TFTP, allowing a client to negotiate options with a server before transferring a file. It superceded [RFC 1782](https://tools.ietf.org/html/rfc1782), the option extension to the original TFTP specification. The option extension consists in appending null terminated option pairs to an RRQ/WRQ and waiting for confirmation of those options. The server can confirm them by an `OACK`, which simply lists the options back in the same form. Some rules apply:

1. The server cannot list anything in an OACK which the client didn't specify
2. The server should only list options it can support
3. The server *can* change the values and list them in the OACK.

If a client finds a value changed in the OACK, it can signal an end of transfer with an error packet, or it can carry on with the server's value.

Options communicated as follows:  
\[RRQ / OACK] option_name_a | 0 | option_value_a | 0 | option_name_a | 0 | option_value_a | 0 |

This format is followed for both the RRQ.




<a name="implementations"></a>

## Implementations


<a name="other"></a>

## Other
