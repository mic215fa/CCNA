# Adjacent-layer Interaction

tags: #concept #acting-ccna #network-fundamentals

## Aliases

- 相鄰層互動

## Definition

Adjacent-layer Interaction is when each layer within a host provides services for the layer above it.

## Why It Exists

Layering works because each layer can rely on the layer below it to provide a specific service, while providing a service to the layer above it.

## Related Concepts

- [[TCP-IP Model]]
- [[Same-layer Interaction]]
- [[Encapsulation and De-encapsulation]]

## Mechanism

- Layer 4 serves Layer 7 by delivering data to the correct application.
- Layer 3 serves Layer 4 by delivering segments to the correct destination host.
- Layer 2 serves Layer 3 by delivering packets to the next hop.
- Layer 1 serves Layer 2 by providing a physical medium.

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-074_480_927_628_362.jpg)
*Source: [[00_Source/Chapter 4 - TCP IP 網路模型|Chapter 4 - TCP IP 網路模型]], Figure 4.8 — Each layer on a host provides services for the layer above it; this is adjacent-layer interaction.*

## Appears In

- [[02_Source_Notes/Unit02-TCPIP-網路模型|Unit02：TCP/IP 網路模型]]
