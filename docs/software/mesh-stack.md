# Meshtastic mesh network stack

## OSI model explained

The OSI model (https://en.wikipedia.org/wiki/OSI_model) is used to describe the functions of Meshtastic mesh networking. 

The network stack consists of seven logical layers:
* Layer 1: Physical Layer (
Layer 2: Data Link Layer
Layer 3: Network Layer
Layer 4: Transport Layer
Layer 5: Session Layer
Layer 6: Presentation Layer
Layer 7: Application Layer


## Current algorithm

The routing protocol for Meshtastic is really quite simple (and suboptimal). It is heavily influenced by the mesh routing algorithm used in [Radiohead](https://www.airspayce.com/mikem/arduino/RadioHead/) (which was used in very early versions of this project). It has four conceptual layers.



### A note a
