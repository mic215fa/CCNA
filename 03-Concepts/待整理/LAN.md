# LAN

tags: #concept #acting-ccna #network-fundamentals

## Aliases

- Local Area Network
- 區域網路

## Definition

LAN 是有限區域內互相連接的一組設備，例如辦公室內的 PC、Server、Printer、Switch 等。

## Why It Exists

LAN 讓同一個局部範圍內的設備能有效互通與共享資源。

## Related Concepts

- [[Computer Network]]
- [[Switch]]
- [[Router]]
- [[WAN]]
- [[Ethernet]]
- [[Layer 2 Domain]]
- [[Broadcast Domain]]
- [[Frame Forwarding]]

## Contrast With

- [[WAN]]：WAN 跨越較大的地理範圍；LAN 通常侷限在辦公室、樓層或類似有限區域。

## Mechanism

在本 Unit 中，LAN 內設備通常透過 [[Switch]] 連接；LAN 要連到外部網路時，通常需要 [[Router]]。

Unit03 / Chapter 6 進一步補充：LAN 可被視為 [[Layer 2 Domain]]，也就是 hosts 不需 router 即可透過 switching 溝通的範圍；router 通常分隔 LAN / broadcast domain。

## Source Figures

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-039_403_1418_185_190.jpg)
*Source: [[00_Source/Chapter 2 - 網路設備|Chapter 2 - 網路設備]], Figure 2.1 — An enterprise network connecting multiple offices over the internet; each office is presented as a LAN.*

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-043_424_1418_185_190.jpg)
*Source: [[00_Source/Chapter 2 - 網路設備|Chapter 2 - 網路設備]], Figure 2.5 — Two LANs connected to the internet via a router at the edge of each LAN.*

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-096_447_1414_1125_225.jpg)
*Source: [[00_Source/Chapter 6 - Ethernet LAN 交換|Chapter 6 - Ethernet LAN 交換]], Figure 6.1 — Switches directly connected are in one LAN; switches separated by a router are separate LANs.*

## Appears In

- [[02_Source_Notes/Unit01-設備-線材|Unit01：設備與線材]]
- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

## Questions

- 為什麼 Switch 的角色主要在 LAN 內，而不是拿來連接 Internet？
