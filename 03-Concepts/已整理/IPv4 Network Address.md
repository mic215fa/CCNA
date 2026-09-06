# IPv4 Network Address

tags: #concept #acting-ccna #ipv4

> [!info] Concept role
> [[IPv4 Addressing]] 的 address-role 子概念：以 host bits 全 0 識別整個 IPv4 prefix。

## Definition

IPv4 Network Address 是某個 IPv4 network 的第一個 address，用來識別整個 network，不能指派給 host。

## Why It Exists

Network address 讓路由與網路描述可以指向一整段 addresses，而不是單一 host。

## Related Concepts

- [[IPv4 Address]]
- [[Network Portion and Host Portion]]
- [[Prefix Length]]
- [[Usable IPv4 Address Range]]

## Mechanism

當 IPv4 address 的 host portion 全部為 0，該 address 就是 network address。

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]
