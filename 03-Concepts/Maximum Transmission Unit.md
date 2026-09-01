# Maximum Transmission Unit

tags: #concept #acting-ccna #mtu

## Aliases

- MTU

## Definition

Maximum Transmission Unit 是某個 interface 或 link 支援的最大 packet size。

## Why It Exists

網路設備需要知道可承載的最大 packet 大小；超過 MTU 的 IPv4 packet 可能需要 fragmentation。

## Related Concepts

- [[Packet Fragmentation]]
- [[IPv4 Header]]
- [[Ethernet Frame]]

## Mechanism

Chapter 7 提到 typical MTU 為 1500 bytes，現代設備通常都支援；若路徑中某 router 支援較小 MTU，可能會 fragment packet。

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

