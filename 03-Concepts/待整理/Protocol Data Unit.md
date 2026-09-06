# Protocol Data Unit

tags: #concept #acting-ccna #network-fundamentals

## Aliases

- PDU
- L4PDU
- L3PDU
- L2PDU

## Definition

Protocol Data Unit（PDU）is the name given to a message at a specific stage of encapsulation or de-encapsulation.

## Why It Exists

PDU terminology lets us talk precisely about what the message is called at each layer.

## Prerequisites

- [[Encapsulation and De-encapsulation]]
- [[Header and Trailer]]
- [[Payload]]

## Mechanism

- Data + Layer 4 header = segment（L4PDU）
- Segment + Layer 3 header = packet（L3PDU）
- Packet + Layer 2 header/trailer = frame（L2PDU）

## Source Figures

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-073_630_670_190_316.jpg)
*Source: [[00_Source/Chapter 4 - TCP IP 網路模型|Chapter 4 - TCP IP 網路模型]], Figure 4.7 — Application data encapsulated in a Layer 4 header is a segment; a segment encapsulated in a Layer 3 header is a packet; a packet encapsulated in a Layer 2 header/trailer is a frame.*

## Appears In

- [[02_Source_Notes/Unit02-TCPIP-網路模型|Unit02：TCP/IP 網路模型]]
