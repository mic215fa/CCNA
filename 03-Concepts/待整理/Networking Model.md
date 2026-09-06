# Networking Model

tags: #concept #acting-ccna #network-fundamentals

## Aliases

- 網路模型

## Definition

Networking Model 是一種 framework，用來定義資料如何從 source 透過網路到達 destination 所需的各種功能。

## Why It Exists

網路通訊很複雜，單一技術如 [[Ethernet]] 不足以完成完整通訊。Networking Model 將功能分層，讓不同 [[Protocol]] 能各自填補某一層所需的角色。

## Prerequisites

- [[Protocol]]
- [[Network Standard]]

## Related Concepts

- [[OSI Model]]
- [[TCP-IP Model]]
- [[Adjacent-layer Interaction]]
- [[Same-layer Interaction]]

## Mechanism

```text
Network communication requirements
  ↓ divided into
Layers
  ↓ implemented by
Protocols
```

## Source Figures

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-064_580_1416_592_223.jpg)
*Source: [[00_Source/Chapter 4 - TCP IP 網路模型|Chapter 4 - TCP IP 網路模型]], Figure 4.1 — A web browser on PC1 uses HTTPS to request a web page from SRV1; Layers 1, 2, 3, 4, and 7 cooperate to enable communication.*

## Appears In

- [[02_Source_Notes/Unit02-TCPIP-網路模型|Unit02：TCP/IP 網路模型]]

## Questions

- 為什麼分層模型比把所有通訊功能放在單一巨大 protocol 更容易理解與維護？
