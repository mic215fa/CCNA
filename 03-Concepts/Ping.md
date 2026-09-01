# Ping

tags: #concept #acting-ccna #troubleshooting

## Definition

Ping 是測試兩個 hosts 是否能透過網路到達彼此的軟體工具。

## Why It Exists

它提供簡單、快速的 connectivity test，是網路 troubleshooting 中最常用的基本工具之一。

## Related Concepts

- [[ICMP]]
- [[IPv4 Address]]
- [[Address Resolution Protocol]]
- [[Forwarding]]

## Mechanism

Ping 使用 [[ICMP]] echo request 與 echo reply。Cisco IOS 中一次 `ping <ip-address>` 預設送出 5 個 echo requests。

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

## Questions

- Ping 成功代表哪些 layer 或機制至少大致正常？Ping 失敗又不能直接證明什麼？

