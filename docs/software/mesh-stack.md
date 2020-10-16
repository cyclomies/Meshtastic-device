# Meshtastic mesh protocol stack

## Protocol stack explained

The protocol stack (https://en.wikipedia.org/wiki/Protocol_stack) is used to ease the designing of protocol designing, and to simplify the functions of the protocols.

## OSI model explained

The OSI model (https://en.wikipedia.org/wiki/OSI_model) is used here to describe the functions of Meshtastic mesh networking. 

## The layers of Meshtastic mesh network

Utilizing the OSI model, the Meshtastic mesh operations are divided to seven different layers (L1-L7). These layers should be held separated within the codebase, to be able to modify/update functions on one layer, without bricking functions of the other layers.

* L1 Physical/Virtualized layer (raw data bits)
* L2 Data link layer (frames)
* L3 Networking layer (packets)
* L4 Transport layer (segmented datagrams)
* L5 Session layer (interhost cummunication: determining of transport layer, forwarded encrypted data)
* L6 Presentation layer (encryption of data)
* L7 Application layer (plain messages and data)

Layers L1-L3 are called *Media Layers*, and they are responsible of transfering the data to the intended destination. Layers L4-L7 are called *Host Layers*, and they are responsible for accurate data delivery between end users.

## The footprint of resources regarding the layers

**Repeating:** A networking device, set to retransmit transmissions (i. e. *Repeater Mode*), does only need media layers (L1-L3), to be able to *Repeat* packets. This applies only, if the packet is repeated within the same physical layer (i. e. LoRa --> LoRa). Therefore the resource footprint, for repeater only devices, can be held tiny.

**Routing:** When the forwarding process do need the a physical layer to be changed from one type to another (i. e. LoRa --> TCP/IP), then the networking device do also need layers four and five (L4-L5), to be able to accomplish the forwarding task. This type of a task, is called *Routing*. Layers L4 and L4 are needed due to a) the need for refactoring the datagram for a different media layer (L4), and b) for eshtablishing internal connection between the different media layers (L5).

**Interacting:** A device requires all seven layers (L1-L7), to be able to show a message on the screen. Decryption (L6), and user interface (L7) do need resources, and therefore the footprint of the software is greater, than with the two excamples above.

## Functions of the layers

**Function**

* L7: allowing entry of plain text/data -> 
* L6: encrypting data -> 
* L5: determining transport layer -> 
* L4: structuring encrypted data to segmanted datagrams -> 
* L3: determining logical routing of packets -> 
* L2: physical addressing of transporting frames --> 
* L1: physical transmission

**Outcome**

* L7: plain text/*data* -> 
* L6: *encrypted data* -> 
* L5: assigned encrypted *datapackages* -> 
* L4: structured and segmented *datagrams* -> 
* L3: *packets* -> 
* L2: *frames* --> 
* L1 *raw data bits*

### L1 Physical/Virtualized layer 

Layer 1 is the physical layer, implying for the *Networking Medium* (later *medium*). It is usually refered as a networking device or as a *Networkin Interface*. The medium can also be engineered as a virtualized interface, used in virtualized testing environments.

Main functions of L1:

* executes physical or virtualized transmissions
* handles transmitting and receiving of raw data bits by physical modulation and encoding of radio waves, or virtualized
* transform the structure between raw data bits (L1) and frames (L2)

Currently, Meshtastic can control folowing mediums for networking:

* LoRa transceivers
* Bluetooth LE
* WLAN (client and software access point)
* Serial over USB OTG
* ANT and ANT+ (under development)
* virtualized LoRa transceiver

Parameters for LoRa transceivers on Meshtastic mesh:
* 32 bit LORA preamble (to allow receiving radios to synchronize clocks and start framing). We use a longer than minimum (8 bit) preamble to maximize the amount of time the LORA receivers can stay asleep, which dramatically lowers power consumption.

### L2 Data link layer

Layer 2, data link layer, is responsible for controling addressing of the physical mediums. Every medium has to be assigned with a unique address. Currently, Meshtastic uses 4 byte (32bit) NodeID adressing. NodeIds are constructed from the bottom four bytes of the macaddr of the bluetooth address. Because the OUI is assigned by the IEEE and we currently only support a few CPU manufacturers, the upper byte is defacto guaranteed unique for each vendor. The bottom 3 bytes are guaranteed unique by that vendor.

Main functions of L2:
* control addressing of physical mediums
* naive collision avoidance
* naive error correction (under developement) 
* transform the structure between frames (L2) and packets (L3)

Parameters for data link layer:
* 4 byte (32bit) addresses (bottom four bytes of the macaddr)
* NodeID of 0xffffffff (NodeNum_BROADCAST) is used for broadcasting.
* To prevent collisions, all transmitters will listen before attempting to send. If they hear some other node transmitting, they will reattempt transmission in x milliseconds. This retransmission delay is random between FIXME and FIXME (these two numbers are currently hardwired, but really should be scaled based on expected packet transmission time at current channel settings).

### L3 Networking layer (packets)

Layer 3: Network Layer (mesh datagram, the protocol for logical routing (routing tables) and addressing (user addresses))
Host layers:

* logical routing
* logical addressing (user addressing/user numbering)
* routing tables

### L4 Transport layer 

Layer 4: Transport Layer (mesh datagram structuring, includes logical functions for ACK, NACK, error detection, merging and dividing datagrams, datagram retransmission, datagram queuing, datagram prioritization, datagram transmission limitations, and for duty cycle management)

* ACK and NACK features
* error detection
* merging and dividing packets
* datagram retransmission
* datagram queuing
* datagram prioritization
* datagram transmission limitations
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
