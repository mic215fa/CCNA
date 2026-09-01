# Same-layer Interaction

tags: #concept #acting-ccna #network-fundamentals

## Aliases

- 同層互動

## Definition

Same-layer Interaction is the logical communication between the same layer on different computers.

## Why It Exists

Although data physically moves down and up layers, each layer's header/trailer is meant to be interpreted by the corresponding layer on another host or next hop.

## Related Concepts

- [[TCP-IP Model]]
- [[Adjacent-layer Interaction]]
- [[Header and Trailer]]
- [[Protocol Data Unit]]

## Mechanism

Layer 4 headers are inspected by Layer 4 on the destination host; Layer 3 headers by Layer 3 on the destination host; Layer 2 headers/trailer by Layer 2 on the next hop.

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-074_480_927_628_362.jpg)
*Source: [[00_Source/Chapter 4 - TCP IP 網路模型|Chapter 4 - TCP IP 網路模型]], Figure 4.8 — Same-layer interaction occurs between the same layer on different communicating hosts.*

## Appears In

- [[02_Source_Notes/Unit02-TCPIP-網路模型|Unit02：TCP/IP 網路模型]]
