# Encapsulation and De-encapsulation

tags: #concept #acting-ccna #network-fundamentals

## Aliases

- Encapsulation
- De-encapsulation
- 封裝與解封裝

## Definition

Encapsulation is the process of adding headers and trailers to data before sending it over a network. De-encapsulation is the reverse process performed by the receiving host.

## Why It Exists

Each layer needs to add the information required for its own delivery role, such as port number, IP address, MAC address, and error-checking data.

## Prerequisites

- [[TCP-IP Model]]
- [[Header and Trailer]]
- [[Protocol Data Unit]]

## Mechanism

Sending host:

```text
Application data
  ↓ L4 header
Segment
  ↓ L3 header
Packet
  ↓ L2 header + trailer
Frame
  ↓ transmitted as bits
```

Receiving host reverses the process by inspecting and removing Layer 2, Layer 3, and Layer 4 headers/trailer until application data remains.

## Source Figures

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-071_462_1389_470_206.jpg)
*Source: [[00_Source/Chapter 4 - TCP IP 網路模型|Chapter 4 - TCP IP 網路模型]], Figure 4.5 — The five-step process of encapsulating and transmitting data.*

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-072_462_1391_181_230.jpg)
*Source: [[00_Source/Chapter 4 - TCP IP 網路模型|Chapter 4 - TCP IP 網路模型]], Figure 4.6 — The five-step process of receiving and de-encapsulating data.*

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-073_630_670_190_316.jpg)
*Source: [[00_Source/Chapter 4 - TCP IP 網路模型|Chapter 4 - TCP IP 網路模型]], Figure 4.7 — PDUs and their payloads during encapsulation.*

## Appears In

- [[02_Source_Notes/Unit02-TCPIP-網路模型|Unit02：TCP/IP 網路模型]]
