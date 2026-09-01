# Transport Layer

tags: #concept #acting-ccna #network-fundamentals #layer4

## Aliases

- Layer 4
- L4

## Definition

Transport Layer provides application-to-application delivery by addressing data to a specific application process on the destination host.

## Why It Exists

資料到達正確 host 還不夠；目的主機同時執行多個 applications，因此需要 [[Port Number]] 指定正確 application process。

## Prerequisites

- [[Network Layer]]
- [[Port Number]]

## Related Concepts

- [[Application Layer]]
- [[Protocol Data Unit]]
- TCP
- UDP

## Mechanism

Layer 4 protocols such as TCP and UDP add a header containing port number information. The resulting PDU is a segment.

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-068_402_1414_782_223.jpg)
*Source: [[00_Source/Chapter 4 - TCP IP 網路模型|Chapter 4 - TCP IP 網路模型]], Figure 4.4 — Layer 4 addresses PC1's message to port 443, which is used by HTTPS on SRV1.*

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-073_630_670_190_316.jpg)
*Source: [[00_Source/Chapter 4 - TCP IP 網路模型|Chapter 4 - TCP IP 網路模型]], Figure 4.7 — Application data encapsulated in a Layer 4 header is a segment (L4PDU).*

## Appears In

- [[02_Source_Notes/Unit02-TCPIP-網路模型|Unit02：TCP/IP 網路模型]]
