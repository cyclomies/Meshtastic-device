# Meshtastic mesh protocol stack (version: pre-alpha)

**TODO** 
* Merge available information of current status
* Merge notes for future development
* Merge information of important settings, features, parameters and configurations (what are the effects)
* Merge information of important restrictions and regulations (what are the effects)
* Split the protocol stack (fundamentals), and Meshtastic mesh routing protocol (L3, logical 	implementation), to two different files?
* Categorize the terms in Terminology
* Split Terminology to two files, and leave only the most inmportant ones here?
* Fix FIXME

## Protocol stack explained

The protocol stack (https://en.wikipedia.org/wiki/Protocol_stack) is used to ease the designing of protocol designing, and to simplify the functions of the protocols.

## OSI model explained

The OSI model (https://en.wikipedia.org/wiki/OSI_model) is used here to describe the functions of Meshtastic mesh networking. 

## Terminology - FIXME

To avoid some inconsistency, concerning the usage of terms like flooding, routing, unicast, multicast, and broadcasting, the terms are defined beneath:

**Addressing**

* **Unicast** From one source to one destination i.e. One-to-One messaging (to one specified address)
* **Broadcast** From one source to all possible destinations i.e. One-to-All (to a broadcast address)
* **Multicast** From one source to multiple specific destinations i.e. One-to-Many (to several specified addresses)

*“The original multicast design i.e. RFC 1112, supports both the ASM (any-source-multicast) based on many-to-many service model and the SSM (source specific multicast) based on a one-to-many model.”*

**Logical networking functions**

* **Flooding** In flooding, the node sends the packet to all nodes, because it doesn’t know how to reach the defined (unicast or multicast, or it might be a broadcast address) destination.
* **Routing** In routing, the node sends the packet only to a specific node, according to the routing table, to be retransmitted (unicast or multicast, or it might be a specific broadcast address), for avoiding overusing airtime
* **Repeating** Indicating forwarding a packet according to the routing logic, and the pre-set parameters
* **Re-broadcasting** Indicating repeating a packet addresserd with a broadcast address

**Acknowledgements**

* **Explicit ACK** Sending a ACK message back to the orginal sender, from the intended recipient, whenever a ACK is required
* **Implicit ACK** A node considers a packet is received by the intended node, if the sender node overhears the packet is retransmitted by the intended node
* **NACK** Negative-acknowledgement (NAK or NACK) indicates a failed transfer.

**To simplify the practice:**

* Addressing: Unicast if one-to-one, Multicast if one-to-many, and Broadcast if one-to-all
* Routing: Use flooding, if no routes are present, the recipient is unknown/unseen, and for broadcasting/re-broadcasting. Execute routing according to route tables only (with distributed algorithm), if the node knows the route to the destination.
* ACK’s: use implicit ACK’s whenever possible (i.e. while routing) to avoid overusing airtime, and send explicit ACK’s only from intended recipients (if ACK’s are required). ACK's should always be registered, and clearly presented, individually for every intended recipient. ACK's loose their credibility, if the user can not be sure of received acknowledgements.

**Physical networking** - FIXME
* **Airtime** Airtime indicates the time it takes for a transceiver to transmit a single packet (usually less is better). An example of LoRa Airtime calculation can be found here: https://github.com/sudomesh/disaster-radio/wiki/Protocol#lora-airtime 
* **Hops** Describes the amount of retransmissions between devices.
* **RF Air Quality** Radio Frequency Air Quality describes physical circumstances for radio waves.
* **RX** is the abbreviation for Transmit
* **RS** is the abbreviation for Receive
* **LoRa SF** is the spreading factor (between 6-12). Each step up in spreading factor doubles the time on air to transmit a symbol, and each unit increase in SF correlates to about 2.5dB extra link budget. Higher spreading factors are more resistant to local noise effects, and will be read more reliably at the cost of higher battery drain, lower data rate and more congestion. *The measurement shows that it takes approximately 25 times longer and 25 times more energy to transmit in SF12 compared to SF7.* (TABLE 1, https://www.thethingsnetwork.org/article/how-spreading-factor-affects-lorawan-device-battery-life)
* **LoRa BW** is the Bandwidth. Higher bandwidth has higher data rates and is more power-efficient, but has more congestion and less range.
* **LoRa HD** is a header bit, 0 for implicit header, 1 for explicit header - FIXME
* **LoRa DR** is a bit for low data optimization: 1 when enabled, 0 when disabled - FIXME
* **LoRa CR** is the coding rate (1 corresponding to 4/5 and 4 to 4/8)
* **LoRa Link Budget** This is how many decibels (dBs) can be lost, between the transmit PA and the receiver interface (IF). Higher link budgets translate to longer distances: 6dB = twice distance.

**Other** - FIXME

* **Duty Cycle** 
* **Payload** 
* **Header** 
* **Frame** 
* **Datagram** 
* **TTL** The "time to live", i.e. the number of hops/time allowed before a packet is discarded

## Goals - FIXME

* Self healing and automatically forming multi-hop capable mesh network
* FIXME

## Restrictions - FIXME

* FIXME 

## Assumptions - FIXME

* FIXME

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

* L7: allowing entry of plain text/data <-> 
* L6: encrypting data <-> 
* L5: determining transport layer <-> 
* L4: structuring encrypted data to segmanted datagrams <-> 
* L3: determining logical routing of packets <-> 
* L2: physical addressing of transporting frames <-> 
* L1: physical transmission

**Outcome**

* L7: plain text/*data* <-> 
* L6: *encrypted data* <-> 
* L5: assigned encrypted *datapackages* <-> 
* L4: structured and segmented *datagrams* <-> 
* L3: *packets* <-> 
* L2: *frames* <-> 
* L1 *raw data bits*

### L1 Physical/Virtualized layer (raw data bits)

Layer 1 is the physical layer, implying for the *Networkin Interface* (later *interface*). The interface can also be engineered as a virtualized interface, used in virtualized testing environments. The interface transports transmissions trough different mediums (wired, wireless, and virtualized).

Main functions of L1:

* executes physical or virtualized transmissions trough mediums
* handles transmitting and receiving of raw data bits by physical modulation and encoding of radio waves, or virtualized
* manage channel hopping to increase security (under development)
* manage channel blacklisting to increase reliability (under development)
* transform the structure between raw data bits (L1) and frames (L2)

Currently, Meshtastic can control folowing interfaces for networking:

* LoRa transceivers
* Bluetooth LE
* WLAN (client and software access point)
* Serial over USB OTG
* ANT and ANT+ (under development)
* virtualized LoRa transceiver

Parameters for LoRa transceivers on Meshtastic mesh:
* 32 bit LORA preamble (to allow receiving radios to synchronize clocks and start framing). We use a longer than minimum (8 bit) preamble to maximize the amount of time the LORA receivers can stay asleep, which dramatically lowers power consumption.

### L2 Data link layer (frames)

Layer 2, data link layer, is responsible for controling addressing of the interfaces. Every interface has to be assigned with a unique address. Currently, Meshtastic uses 4 byte (32bit) NodeID adressing. NodeID's are constructed from the bottom four bytes of the macaddr of the bluetooth address. Because the OUI is assigned by the IEEE and we currently only support a few CPU manufacturers, the upper byte is defacto guaranteed unique for each vendor. The bottom 3 bytes are guaranteed unique by that vendor.

Main functions of L2:
* control addressing of physical interfaces
* execute collision avoidance
* execute error correction (under developement) 
* transform the structure between frames (L2) and packets (L3)

Parameters for data link layer:
* 4 byte (32bit) addresses (bottom four bytes of the macaddr)
* NodeID of 0xffffffff (NodeNum_BROADCAST) is used for broadcasting.
* To prevent collisions, all transmitters will listen before attempting to send. If they hear some other node transmitting, they will reattempt transmission in x milliseconds. This retransmission delay is random between FIXME and FIXME (these two numbers are currently hardwired, but really should be scaled based on expected packet transmission time at current channel settings).

### L3 Networking layer (packets) - FIXME

Layer 3, networking layer, does route packets according to logical addressing, and a pre-defined routing logic. Routing allow transmissions between different physical, and virtualized, network structures: between devices wich can't reach to each other directly (within L1 and L2).

As L3 is responsible for managing of the logical network topology. Logical topology refers to how the data moves through the network: *Who is communicating with between each other*, not *How the communication is made*. Logical topology refers to the pattern of the data flow within a network.

Main functions of L3:

* routing packets between logical connections
* using logical addressing (user addressing/user numbering)
* building and maintaining routing tables
* transform the structure between packets (L3) and datagrams (L4)

Meshtastic can route packets trough folowing networks:

* LoRa <-> LoRa
* LoRa <-> Bluetooth LE (between a node and one Android App / Python client)
* LoRa <-> Serial over USB OTG (between a node and one Android App / Python client)
* LoRa <-> WLAN client (under developement)
* LoRa <-> WLAN AP (under developement)
* Bluetooth <-> Bluetooth (under developement)

#### Meshtastic Mesh Protocol

The *Meshtastic Mesh Protocol* (later *protocol* or *L3*) is currently developed mainly to be used only with LoRa. However, in theory, the protocol can later be universalized. This would allow Meshtastic to generate mesh networks trough almost any interface (L1) and physical medium.

Current algorithm: The routing protocol for Meshtastic is really quite simple (and suboptimal). It is heavily influenced by the mesh routing algorithm used in [Radiohead](https://www.airspayce.com/mikem/arduino/RadioHead/) (which was used in very early versions of this project).

The protocol implementation is currently utilizing naive flooding, with some optimizations. 

**Flooding protocol**

* Supports unicast and broadcast addressing for transmit
* Includes multiple hop forwarding logic
* Current imlementation loosly resembles of UDP traffic
* In future, flooding should be used mainly for transmitting broadcast addressed packets, and if there are no known routes to be used with the routing protocol
* FIXME

**Routing protocol**

* Under developement
* Should minimize transport times (i. e. airtime) by: avoiding flooding, avoidinkg unnecessary transmits and re-transmits
* Should support unicast and multicast addressing
* Should be able to automatically learn and maintain the logical mesh networking layout/structure
* Should be able to automatically heal the logical mesh network
* Could support routing trough different L1 layer types (universalized mesh routing protocol)
* Could support routing trough different logical node naming schemes
* 
* FIXME

### L4 Transport layer (datagrams) - FIXME

Layer 4: Transport Layer (mesh datagram structuring, includes logical functions for ACK, NACK, error detection, merging and dividing datagrams, datagram retransmission, datagram queuing, datagram prioritization, datagram transmission limitations, and for duty cycle management)

* ACK and NACK features
* error detection
* merging and dividing datapackages to and from datagrams
* datagram retransmission
* datagram queuing
* datagram prioritization
* datagram transmission limitations
* and for duty cycle management

### L5 Session layer (datapackages) - FIXME

Layer 5, Session layer, determines wether the datapackage is intended for local transport (i. e. parameters for the node trough Bluetooth), or for external transport (i. e. LoRa mesh, or WLAN). Tihs is done by determining the type of the datapackege.

Main functions of L5:

* FIXME

### L6 Presentation layer (encrypted data)

Layer 6 is the presentation layer. In Meshtastic, the layer is utilized for data processing (i. e. protobuf encoding) and for encrypt and decrypt plain data. 

Main functions of L6:

* encryption and decryption of data
* data pricessing (i. e. Protocol Buffers)
* transform the structure between encrypted data (L6) and data (L7)

### L7 Application layer (data) - FIXME

L7 Application layer (messages, plain data)

* user interface (UI) inputs and outputs


-------- NOTES --------


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

superframes (10ms): https://www.awiatech.com/an-overview-of-wirelesshart%E2%80%99s-osi-layers/

LoRa settings: https://medium.com/home-wireless/testing-lora-radios-with-the-limesdr-mini-part-2-37fa481217ff

## Current algorithm

The routing protocol for Meshtastic is really quite simple (and suboptimal). It is heavily influenced by the mesh routing algorithm used in [Radiohead](https://www.airspayce.com/mikem/arduino/RadioHead/) (which was used in very early versions of this project). It has four conceptual layers.



### A note a
