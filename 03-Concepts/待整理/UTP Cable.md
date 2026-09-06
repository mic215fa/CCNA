# UTP Cable

tags: #concept #acting-ccna #network-fundamentals #cabling

## Aliases

- Unshielded Twisted Pair
- 非遮蔽雙絞線
- Copper UTP

## Definition

UTP Cable 是常見的銅製網路線，內含 8 條線，扭成 4 組線對，且沒有金屬遮蔽層。

## Why It Exists

UTP 提供便宜、普遍、端點設備廣泛支援的 Ethernet LAN 實體連線方式。

## Prerequisites

- [[Ethernet]]
- [[8P8C Connector]]

## Related Concepts

- [[Switch]]
- [[End Host]]
- [[Straight-through and Crossover Cable]]
- [[Auto MDI-X]]
- [[Fiber Optic Cable]]
- [[Physical Layer]]

## Mechanism

- 8 條線形成 4 組 twisted pairs。
- Twisting 可降低每組線對之間的 EMI。
- 10BASE-T / 100BASE-T 使用 2 組線對。
- 1000BASE-T / 10GBASE-T 使用 4 組線對。

## Contrast With

- [[Fiber Optic Cable]]：Fiber 距離更遠、成本更高；UTP 更常用於 Switch 到 End Host。

## Limitations

- 常見 Ethernet UTP 標準最大線材長度為 100 m。
- 易受 EMI 影響。
- 訊號可能洩漏到線材外部，形成潛在安全風險。

## Source Figures

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-049_339_464_1713_320.jpg)
*Source: [[00_Source/Chapter 3 - 線材、接頭與連接埠|Chapter 3 - 線材、接頭與連接埠]], Figure 3.2 — Two 8P8C ports on a Cisco switch and an 8P8C connector on a copper UTP network cable.*

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-055_369_1416_310_192.jpg)
*Source: [[00_Source/Chapter 3 - 線材、接頭與連接埠|Chapter 3 - 線材、接頭與連接埠]], Figure 3.7 — Pin and wire pairs used on 1000BASE-T and 10GBASE-T connections; all eight wires are used.*

## Appears In

- [[02_Source_Notes/Unit01-設備-線材|Unit01：設備與線材]]
- [[02_Source_Notes/Unit02-TCPIP-網路模型|Unit02：TCP/IP 網路模型]]
