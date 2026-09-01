# ICMP

tags: #concept #acting-ccna #network-layer #troubleshooting

## Aliases

- Internet Control Message Protocol

## Definition

ICMP 是輔助 IP 的 protocol；在 Unit03 中最重要的用途是支援 [[Ping]]。

## Why It Exists

IP 本身提供 packet delivery，但需要一些控制、錯誤與診斷訊息；ICMP 扮演這個支援角色。

## Related Concepts

- [[Ping]]
- [[IPv4 Header]]
- [[Time To Live]]

## Mechanism

Ping 使用 ICMP echo request 與 ICMP echo reply，兩者都是 unicast messages。

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

