# Application Layer

tags: #concept #acting-ccna #network-fundamentals #layer7

## Aliases

- Layer 7
- L7

## Definition

Application Layer is the interface between applications running on a computer and the network.

## Why It Exists

User applications need protocols that prepare messages for network communication. Layer 7 protocols provide services for applications so they can communicate with applications on other computers.

## Related Concepts

- [[Transport Layer]]
- [[TCP-IP Model]]
- [[Protocol]]

## Mechanism

An application uses a Layer 7 protocol such as HTTPS to prepare data. Lower layers then deliver that message to the correct application on the destination host.

## Source Figures

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-064_580_1416_592_223.jpg)
*Source: [[00_Source/Chapter 4 - TCP IP 網路模型|Chapter 4 - TCP IP 網路模型]], Figure 4.1 — A web browser on PC1 uses HTTPS, a Layer 7 protocol, to request a web page from SRV1.*

## Appears In

- [[02_Source_Notes/Unit02-TCPIP-網路模型|Unit02：TCP/IP 網路模型]]

## Questions

- 為什麼 HTTPS 是 Layer 7 protocol，但不是 user application 本身？
