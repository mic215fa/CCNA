# Classful Network

tags: #concept #acting-ccna #ipv4 #classful

> [!info] Concept role
> [[IPv4 Addressing]] 的歷史子概念：使用 A/B/C 類別的固定預設 network boundary。

## Definition

Classful Network 是遵循 IPv4 A/B/C 類別固定 prefix length 規則的 network。

## Why It Exists

它是早期 IPv4 address allocation 的方式，幫助理解為何後來需要更彈性的 classless networking 與 subnetting。

## Related Concepts

- [[IPv4 Address Class]]
- [[Prefix Length]]
- [[Netmask]]

## Contrast With

- Classful networking：prefix length 受 address class 限制。
- Classless networking：prefix length 明確隨 route/address 一起表達，不受 class A/B/C 固定 boundary 限制；[[Subnetting]]、VLSM 與現代 routing 都依賴這種彈性。

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

## Limitation

Classful boundary 無法有效符合不同 network sizes，也無法單靠 address value 表達任意 prefix。現代設計應以明確 prefix length 為準；address class 主要保留作歷史背景與舊術語辨識。
