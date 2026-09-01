# Packet Fragmentation

tags: #concept #acting-ccna #ipv4

## Definition

Packet Fragmentation 是 IPv4 packet 超過路徑中某個 interface 的 [[Maximum Transmission Unit]] 時，被拆成多個 smaller fragments 的機制。

## Why It Exists

不同 links 可能支援不同最大 packet size；fragmentation 讓過大的 packet 仍可能通過較小 MTU 的路徑。

## Related Concepts

- [[IPv4 Header]]
- [[Maximum Transmission Unit]]
- [[Payload]]

## Mechanism

IPv4 header 的 Identification、Flags、Fragment Offset fields 共同支援 fragmentation 與目的端 reassembly。

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

## REVIEW

- Path MTU、DF bit 與實務 troubleshooting 後續再補。

