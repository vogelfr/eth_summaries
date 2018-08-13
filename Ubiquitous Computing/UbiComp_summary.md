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
- Fiber optics: from gbps to tbps
- Wireless
    - GSM, UTMS, 4G, 5G
    - LPWAN, LoRa
    - Wireless LAN, WiFi (>> 10mbps)
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

# Communication
UbiComp promises lots of sensing in the field and lots of processing power in the cloud.
How to connect UbiComp functionality?

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

## Wireless Communication
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

### Long Distance
#### 5G
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

#### LPWAN
- Long range, low number of base stations, low base price
- Low bandwidth (< 10kbps)
- Ideally low latency
- Low power (batter lifetime > 10 years on 5Wh battery)

#### SigFox
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
- if subscription model is acceptable

#### LoRa
- Public ISM band
- Range of several km, up to 10s of km
- Chirp Spread Spectrum
- 3 Device classes: A (basic), B (beacon with scheduled receive slots), C (continuous)
- Data rate of 0.3 - 50kbps, larger messages than SigFox
    - allowed duty cycle for LoRa is 1% (in EU)
    - The Things Network Fair Access Policy: 30s airtime per device per day
- Device names based on IEEE EUI64 (MAC++)

$\to$ better for true bidirectionality, requires denser infrastructure
- more open, more expensive hardware




