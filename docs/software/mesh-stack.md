# Meshtastic mesh protocol stack

## Protocol stack explained

The protocol stack (https://en.wikipedia.org/wiki/Protocol_stack)is used to simlify the protocol design, and purpose of the protocols.

## OSI model explained

The OSI model (https://en.wikipedia.org/wiki/OSI_model) is used here to describe the functions of Meshtastic mesh networking. 

## The layers of Meshtastic mesh network

* L1 Physical/Virtualized layer (raw data, bits)
* L2 Data link layer (frames)
* L3 Networking layer (packets)
* L4 Transport layer (datagram, segments)
* L5 Session layer (interhost cummunication: determining transport layer, forwarded encrypted data)
* L6 Presentation layer (encryption, encrypted data)
* L7 Application layer (messages, plain data)

Layers L1-L3 are called *media layers*, and they are responsible of transfering the data to the intended destination. Layers L4-L7 are called *host layers*, and they are responsible for accurate data delivery between end users.

A Meshtastic node, set to *Repeater Mode*, does need only Media layers (L1-L3) to be able to route Meshtastic mesh packets. This applies only, if the packet is routed within the same physical layer. When the routing process do need the a physical layer to be changed from one type to another, then the Meshtastic node needs also layers four and five (L4-L5), to be able to accomplish the routing task.

## Layered operations

**WHAT**

plain data (L7) -> encrypted data (L6) -> assigned encrypted data (L5) -> structured segments/datagrams (L4) -> packets (L3) -> frames (L2) --> raw data (L1)

**HOW**

entrying of plain text/data (L7) -> encrypting data (L6) -> determining transport layer (L5) -> structuring to segmants/datagrams (L4) -> logical routing of packets (L3) -> physical addressing of frames (L2) --> physical transmission (L1)

### L1 Physical/Virtualized layer 

Layer 1: Physical Layer (raw bits, managed by LoRa modems/simulated modems)

* physical or virtualized transmitting and receiving of packets
* physical error correction
* *Network medium* or *Medium*

### L2 Data link layer

Layer 2: Data Link Layer (mesh packet, the means for addressing pysical devices)

* physical addressing (physical node numbers)

### L4 Transport layer 

Layer 4: Transport Layer (mesh datagram structuring, includes logical functions for ACK, NACK, error detection, merging and dividing packets, packet retransmission, packet queuing, packet prioritization, packet transmission limitations, and for duty cycle management)

* logical routing
* logical addressing (user addressing/user numbering)
* ACK, NACK
* error detection
* merging and dividing packets
* packet retransmission
* packet queuing
* packet prioritization
* packet transmission limitations
* and for duty cycle management

### L5 Session layer

Layer 5: Session Layer (decision of transport layer: BLE, LoRa, WLAN..)

### L6 Presentation layer

L6 Presentation layer (encryption, encrypted data)

* encryption
* decryption

### L7 Application layer

L7 Application layer (messages, plain data)

* user interface (UI) inputs and outputs










The OSI network stack consists of seven logical layers:

Media layers:
* Layer 1: Physical Layer (raw bits, managed by LoRa modems/simulated modems)
* Layer 2: Data Link Layer (mesh packet, the means for addressing pysical devices)
* Layer 3: Network Layer (mesh datagram, the protocol for logical routing (routing tables) and addressing (user addresses))
Host layers:
* Layer 4: Transport Layer (mesh datagram structuring, includes logical functions for ACK, NACK, error detection, merging and dividing packets, packet retransmission, packet queuing, packet prioritization, packet transmission limitations, and for duty cycle management)
* Layer 5: Session Layer (decision of transport layer: BLE, LoRa, WLAN..)
* Layer 6: Presentation Layer
* Layer 7: Application Layer (http, app)

TCP/IP

* application (L5-L7, message)
* transport (L4, datagram, UDP, host-to-host, physical adressing)
* network (L3, packet, IP, logical adressing, routing)
* link (L2, packet, frame header & footer)
* physical (L1, raw data, LoRa modem/virtual LoRa modem)



raw data <- packet <- datagram <- message







plain data (L7) -> encrypted data (L6) -> transport layer determination (L5) -> transporting segments (L4) -> logical routing of packets (L3) -> physical frame adressing (L2) --> physical transmission (L1)


## Current algorithm

The routing protocol for Meshtastic is really quite simple (and suboptimal). It is heavily influenced by the mesh routing algorithm used in [Radiohead](https://www.airspayce.com/mikem/arduino/RadioHead/) (which was used in very early versions of this project). It has four conceptual layers.



### A note a
