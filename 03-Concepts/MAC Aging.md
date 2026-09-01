# MAC Aging

tags: #concept #acting-ccna #switching #layer2

## Definition

MAC Aging 是 switch 在一段時間未再收到某 dynamic MAC address 的 frame 後，自動移除該 MAC address table entry 的機制。

## Why It Exists

Host 可能移動、斷線或改接到其他 port；aging 可避免 MAC address table 保留過時資訊。

## Related Concepts

- [[MAC Address Table]]
- [[MAC Address Learning]]
- [[Frame Forwarding]]

## Mechanism

Chapter 6 提到 dynamic MAC address 若 5 分鐘沒有 activity，switch 會從 MAC address table 移除該 entry。

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

