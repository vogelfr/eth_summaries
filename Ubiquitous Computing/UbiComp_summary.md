---
title: Ubiquitous Computing
desription: Summary of 252-0312-00L Ubiquitous Computing at ETH Zürich FS18
tags: ETH, Zusammenfassung
---
UbiComp Summary
===
Frédéric Vogel `vogelfr@ethz.ch`, ETH Zürich, FS18

[TOC]

<p style="page-break-after:always;"></p>

# Selected UbiComp drivers
## Moore's Law
<img src="https://i.imgur.com/02lbqlD.png" style="float:right;height:200px;"/>

- Most important technology parameters double every 1-3 years
- But: doesn't apply to battery capacity, and some others

## New Materials
Materials determine technology and shape society
Examples:
- flexible displays
- electronic ink
- organic semiconductors
- graphene

## Progress in Communication Technologies
- Fiber optics: from Gbps to Tbps
- Wireless
    - GSM, UTMS, 4G, 5G
    - LPWAN, LoRa
    - Wireless LAN, WiFi (>> 10Mbps)
    - Bluetooth, Zigbee
    - NFC

### Communication by Touching (NFC)
- Almost no energy
- Very small transmitters
- Cheap
- No addressing
- No routing

### Intrabody Communication
- Low power electrical signals sent through human body
- Allows wearable devices to communicate
- Enables touch-selective communication

## Better Sensors
- Miniaturized cameras, microphones, ...
- Biometric sensors
- Sensors for temperature, humidity, acceleration, ...
- Location sensors

- Autonomous perception of the user's environment
    - Context awareness
    - Situation awareness

### Surface Acoustic Wave Sensors
![](https://i.imgur.com/B3P30Rw.png)
- RF pulse $\to$ SAW
- SAW reflected by Piezzo reflector
- Reflected SAW $to$ RF pulse sequence as answer


**Leads to Cyberphysical Systems (CPS)**

# Wireless Communication
UbiComp promises lots of sensing in the field and lots of processing power in the cloud.
How to connect UbiComp functionality?

## Basics
### Radio duty cycling
Core Idea
: Exploit energy assymmetry in sensor networks

Duty Cycle
: Fraction of a period in which a given system is active

Low-power Listening
: Receiver periodically turns on radio and polls medium

Low-power Probing
: Receiver periodically send probes, sender listens for probes and sends payload immediately after receiving probe


### Energy harvesting
- Photovoltaic
- Piezoelectricity
- Pyroelectricity
- Atmospheric pressure changes
- Ambient radiation

### Passive WiFi
- RF components offloaded to single plugged-in device in network
- Device creates single-frequency tone
- Passive Wi-Fi devices communicate by reflecting this tone via digital switch $\to$ "real" 802.11b transmissions

### Multiplexing
Multiplexing
: Multiple use of shared medium

Multiple options:
- Space (build three motorways)
- Frequency (build multi-lane motorways)
- Time (Have cars use lanes one after the other)
- Code (Put people into a bus)

DS-CDMA
: Senders use unique pseudorandom binary chipping sequences, XOR their signal with sequence
Example: 802.11b WiFi

FH-CDMA
: Discrete changes of carrier frequency determined via pseudorandom number sequence
Example: Bluetooth

## Long Distance
### 5G
Goal: improve 4G services by combination of upgrades, new technologies
- Wider bands
- More antennas
- Softwareization and adaptivity (e.g. SDN / logically isolated network partitions)

Milimeter wave bands
26 GHz band opened on EU level
- Pseudo-optical propagation characteristics
    - Blocked by walls, significant attenuation by plants
    - High atmospheric attenuation and rain fade
    - Doppler shift problematic even at low speeds

Massive MIMO
- Spacial multiplexing
    - Transmit separately encoded data signals from each of the antennas
    - Increases channel capacity + possiblity to transmit to multiple users (SDMA)
- Precoding / Multi-stream beamforming
    - Emit same signal from all antennas
    - Set phase and gain to maximize singla power at receiver
- Diversity coding
    - If transmitter has no challenge knowledge
    - Send identical copies of the data stream from different antennas with different encoding

### LPWAN
- Long range, low number of base stations, low base price
- Low bandwidth (< 10kbps)
- Ideally low latency
- Low power (batter lifetime > 10 years on 5Wh battery)

### SigFox
<img src="https://i.imgur.com/klyjZjA.png" style="float:right;width:200px;"/>

- Public ISM band (868MHz in Europe, 902MHz in US/FCC)
- Range of several km, up to 1000km
- Device transmission strictly regulated
    - max 30s per hour
    - max data range 100bps, max message size 96bits
- Unique Sigfox ID per device
- Fire-and-forget transmission mode (no ACKs)
- Bidirectional communication
    - device is informed of "demand for control" at next uplink $\to$ switches to receive mode

$\to$ better for unidirectional extreme long range links
if subscription model is acceptable

### LoRa
- Public ISM band
- Range of several km, up to 10s of km
- Chirp Spread Spectrum
- 3 Device classes: A (basic), B (beacon with scheduled receive slots), C (continuous)
- Data rate of 0.3 - 50kbps, larger messages than SigFox
    - allowed duty cycle for LoRa is 1% (in EU)
    - The Things Network Fair Access Policy: 30s airtime per device per day
- Device names based on IEEE EUI64 (MAC++)

$\to$ better for true bidirectionality, requires denser infrastructure
more open, more expensive hardware



## Short Distance
### Bluetooth

<img src="https://i.imgur.com/hqqelqF.png" style="float:right;width:250px;"/>

- Original goal: much smaller, cheaper, less power than WLAN
- spontaneous, infrastructure-less networks
- Now competes in many respects with 802.11
- BLE marketed as *the* short range IoT-technology
- complex API
    - complete communication stack (802.11 defines only 2 layers)

Ad-hoc networks with no a-priori central coordinator
- ISM band (2.4GHz)
- 79 channels à 1MHz width ($2402\textrm{MHz} + k \textrm{MHz}, k = {0, \ldots, 78}$)
- Frequency hopping: 1600 hops/s (625µs)
- 1mW transmission power

#### Frequency hopping
- Interference protection, enhanced robustness
- Enhances security (hopping pattern not known to outsiders)
- Collisions with adjacent Bluetooth links possible
- Many other devices use same frequency band
- Collision? Retransmit packet on different channel

#### Baseband layer
Specifies:
- Hopping sequence
- Searching and connecting to other devices
- Packet formats
- Error handling, retransmissions
- Master/slave roles

#### Addressing
- each device has 48-bit device ID (compatible with IEEE 802 MAC)
- 3 bit active member address (AMA) when active in piconet $\to$ max 7 slaves, address 0 for broadcast to all slaves
- 8 bit parked member address (PMA) $\to$ max 255 parked slaves in piconet

#### Piconet
- Bluetooth units sharing single (frequency-hopping) channel
- Up to 2.1Mbps
- Time multiplex and duplex within piconet
- Master sets clock for all slaves
- All devices hop together (determined by master device ID)
- Point-to-point master-slave or multicast from master to all slaves

#### Inquiry
Source ("Inquiry"):
- Inquiry packet transmitted on new frequency every 312.5μs (3200 hops/s, listen after each packet)

Destination ("Inquiry Scan"):
- Changes listening frequency all 1.28s ("Set device to discoverable")

To find each other devices must be in complimentary states (inquiry / inquiry scans) on same channel

Paging
: Invitation to known device to join, paging device becomes master

#### Communication
Data packets
: Filled in dedicated slots of 0.625ms, stricly assigned to master or specific slave

Control packets
: Usually on next slot, ACKs + Flow control
`NULL` when slave has nothing to send, `POLL` when master has nothing to send

#### Packet format

```
+-----------+--------+-----------------------------------+
|access code| header |              payload              |
+-----------+--------+-----------------------------------+
   72 bits   54 bits              0-2745 bits
```
Access code identifies all packets of piconet

Header (repeated 3 times), contains:
- 3 bits AMA
- 4 bits payload type indication
- 1 bit stop/go flow control switch set by receiver
- 1 bit ACK/NAK flag
- 1 bit odd/even seq. number
- 8 bit header checksum

#### Error handling
Automatic Repeat Request (ARQ)
: Determined from 1 bit ACK/NAK, 1 bit seq. number

Forward Error Correction (FEC)
: Reduces number of repeat requests, but induces general overhead

#### Low-power modes
Saving power by
- Adapting tx power
- Adapting hopping sequence
- Compressing data
- Low power modes

Sniff state (keep AMA)
: Slave wakes up every x ms to check if packet is available, master knows when slaves listens

Hold state (keep AMA)
: Master puts device on hold temporarily, slave returns automatically

Park state (release AMA)
: Stays virtually connected to piconet, but no participation anymore. Listens only to synchronisation beacons.

BLE: 15mA peak, 1μA average

#### Security
- Fast frequency hopping
- Low tx power
- Link layer encryption (128bit AES)
- Pairing
    - PIN
    - Number comparison
    - NFC
#### L2CAP
- Adapts upper layer protocols to baseband
- Segmentation and reassembly of upper layer protocol packets
- Protocol multiplexing over single air interface

#### RFCOMM
Emulates serial port, similar to TCP

#### Bluetooth Profiles
Vertical slices through protocol stack, specifications for interoperable applications

#### BLE
- Diminshed power, up to 260kbps
- Fewer but broader channels
- Modulation scheme optimised for low power usage
- Faster connection setup

Generic Attribute Profile (GATT)
: BLE application layer protocol, enables reading and writing of remote variables ("characteristics")
Related characteristics grouped to *services*

### IEEE 802.15.4
<img src="https://i.imgur.com/oWf7IA1.png" style="float:right;width=250px;"/>

#### IEEE 802.15.4
- Defines PHY and MAC layer
- uses CSMA and ΤDMA
- provides frame security with AES

#### Low-power wireless personal area network (LoPWAN)
- 20kbps - 250kbps, 127Bytes per frame
- 16 channels in 2.4GHz band, 10 channels in 915MHz band, 1 channel in 868MHz band
- Master-slave or P2P operation

#### 6LoWPAN
- Adaptation of IEEE 802.15.4 lower layers to IP
- Goal: direct end-to-end internet integration of resource-constrained embedded devices
    - Use IP as narrow waist
    - Simple transformation between compressed IPv6 and uncompressed format
- IPv6 packets over IEEE 802.15.4 networks
    - 6LoPWAN provides standard socket API (RFC 4944)
    - Header compression given dedicated 802.15.4 context
    - Packet fragmentation
- Two routing strategies
    - Mesh-Under: emulate local link over multiple hopsin link layer
    - Route-Over: perform routing at IP level
- Pure adaptation protocol

#### ZigBee
Based on 802.15.4 lower layers
- Defines network and application layers
- Implements routing and multi-hop topology
- Provides useful general services (discovery, security, etc.)
- Used in home application, smart energy, hospital care, toys, ...


# RFID
3 Categories
1. Low-end features
    - read only
    - tag repeatedly sends serial number (while powered)
    - no collision detection
2. Medium-range features
    - collision detection (30items/s)
    - read-write memory (EEPROM/SRAM)
3. High-end features
    - complex functions such as crypto $\to$ contactless smartcards

## Power supply
Inductive coupling (magnetic field)
EEPROM need significantly more energy than ROM
Need multiplexed antennas with different orientations (magnetic induction $u = u_0 \cos \delta$)

## Operation frequency
<img src="https://i.imgur.com/r9wiqd0.png" style="float:right;width:200px;"/>
ISM bands
- 134kHz (LF), 5kbps
- 13.56MHz (HF), 5kbps
- 868/915MHz (UHF), 50kbps
- 2.4GHz (Microwave)

## Communication, coding, modulation
### Reader -> Tag
<img src="https://i.imgur.com/CfCj4Yx.png" style="float:right;"/>
- Pulse pause coding (PCP)
    - 1 = short period to next pulse, 0 = long period
    - almost continuous energy flow in inductive systems
- Amplitude shift keying (ASK)
        - switching on/off the antenna driver of the reader

### Tag -> Reader
<img src="https://i.imgur.com/6rFpchk.png" style="float:right;"/>
- Manchester coding
- Load modulation (short range, magnetic field)
    - Transponder absorbs more or less energy from magnetic field by turning resistor on/off with 200kHz
    - Yields small voltage change at antenna of reader
    - Use of sub-carrier, receiver can separate signal by band-pass-filter
- Modulated backscatter (long range, electromagnetic field)
    - Same principle with resistor
    - Change reflection properties
- Data rate up to 100kbps

## Anti-collision protocols
- Transponders don't hear signals from other transponders, only from reader
- FDMA?
    - Limited number of frequencies / different channels
    - More expensive readers
    - Only suitable for particular applications

### Deterministic protocols
- All present tags detected in read cycle
- Induces high overhead in reader -> tag communication

Tree-walking anti-collision algorithm
: - With Manchester encoding it is possible to locate bits where two signals differ
1. Reader broadcasts `sync` command to all transponders
2. Reader requests ID number of all transponders
3. Reader determines leftmost bit $b$ that yields collision. Collision?
    5. Reader broadcasts `mute for value 0 at position b`
    6. Only transponders with value 1 at $b$ move to next round (repeat from 2) all others remain mute
7. Reader requests data from unique transponder $x$
8. Reader sends `halt` command to $x$
9. Repeat from 2



### Stochastic protocols
ALOHA
: Stochastic TDMA, transponders repeatedly send out data with random long quiet periods in between
But: higher load? more collisions

Slotted ALOHA
: - Reader periodically sends `sync` command
- All transponders randomly select one of following $n$ time slots to send serial number
- If reader gets unique serial number $s$ in a time slot (no collision) it sends `select(s)` command
- Selected transponder reacts by sending pay-load data (exclusive access), then remains quiet

### Adaptive round algorithms
Transponder replies are in slots, number of slots is dynamically altered
