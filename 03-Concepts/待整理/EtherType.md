# EtherType

tags: #concept #acting-ccna #ethernet #layer2

## Definition

EtherType 是 Ethernet Type/Length field 作為「封裝協定類型」使用時的名稱，用來指出 frame payload 裡是哪種 packet，例如 IPv4 或 IPv6。

## Why It Exists

接收端需要知道 Ethernet frame 裡包的是哪一種 Layer 3 packet，才能交給正確的上層協定處理。

## Related Concepts

- [[Ethernet Frame]]
- [[IPv4 Address]]
- [[IPv4 Header]]
- [[Payload]]

## Mechanism

- IPv4：`0x0800`
- IPv6：`0x86DD`
- 1500 以下通常代表 length；1536 以上通常代表 type。

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

