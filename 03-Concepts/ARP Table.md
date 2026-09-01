# ARP Table

tags: #concept #acting-ccna #arp

## Definition

ARP Table 是主機用來儲存 IP address 到 MAC address 對應關係的表。

## Why It Exists

設備不需要每次送 packet 前都重新送 ARP request；已學到的 mapping 可以重複使用。

## Related Concepts

- [[Address Resolution Protocol]]
- [[IPv4 Address]]
- [[MAC Address]]

## Mechanism

Host 完成 ARP request/reply 後，會把學到的 IP-to-MAC mapping 存入 ARP table。

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

