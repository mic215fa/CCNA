# IPv4 Address Class

tags: #concept #acting-ccna #ipv4 #classful

> [!info] Concept role
> [[IPv4 Addressing]] 的歷史子概念：說明早期如何由 first-octet pattern 推導固定 address class。

## Definition

IPv4 Address Class 是早期 IPv4 classful addressing 中依 first octet bit pattern 劃分的 address 類別：A、B、C、D、E。

## Why It Exists

早期 IPv4 需要用固定類別提供不同大小的 networks；雖然現代已由 classless networking 取代，但 CCNA 仍需要理解。

## Related Concepts

- [[Classful Network]]
- [[Prefix Length]]
- [[IPv4 Address]]

## Mechanism

- Class A：0–127，預設 /8。
- Class B：128–191，預設 /16。
- Class C：192–223，預設 /24。
- Class D：224–239，保留給 multicast。
- Class E：240–255，experimental。

## Source Figures

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-136_295_780_617_348.jpg)
*Source: [[00_Source/Chapter 7 - IPv4 位址|Chapter 7 - IPv4 位址]], Figure 7.16 — Class A, B, and C network/host portion sizes.*

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]
