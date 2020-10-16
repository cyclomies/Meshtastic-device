# Meshtastic mesh network stack

## OSI model explained

The OSI model (https://en.wikipedia.org/wiki/OSI_model) is used to describe the functions of Meshtastic mesh networking. 

The network stack consists of seven logical layers:
Media layers:
* Layer 1: Physical Layer (raw bits, managed by LoRa modems/simulated modems)
* Layer 2: Data Link Layer (mesh packet, the means for addressing pysical devices)
* Layer 3: Network Layer (mesh datagram, the protocol for logical routing (routing tables) and addressing (user addresses))
Host layers:
* Layer 4: Transport Layer (mesh datagram structuring, includes logical functions for ACK, NACK, error detection, merging and dividing packets, packet retransmission, packet queuing, packet prioritization, packet transmission limitations, and for duty cycle management)
* Layer 5: Session Layer 
* Layer 6: Presentation Layer
* Layer 7: Application Layer



application (L5-L7, message)
transport (L4, datagram, host-to-host, physical adressing)
network (L3, packet, logical adressing), 
link (L2, raw data)
physical (L1, raw data)

raw data <- packet <- datagram <- message



## Current algorithm

The routing protocol for Meshtastic is really quite simple (and suboptimal). It is heavily influenced by the mesh routing algorithm used in [Radiohead](https://www.airspayce.com/mikem/arduino/RadioHead/) (which was used in very early versions of this project). It has four conceptual layers.



### A note a
