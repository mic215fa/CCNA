# Enable Secret

tags: #concept #acting-ccna #security #cli

## Definition

Enable Secret 是較安全的 privileged EXEC password，會以 hash 形式儲存在 configuration 中。

## Why It Exists

它取代較弱的 [[Enable Password]]，避免能看到 running-config 的人直接讀出密碼。

## Related Concepts

- [[Enable Password]]
- [[Privileged EXEC Mode]]
- [[Running Config]]

## Mechanism

使用 `enable secret <password>` 設定。若 enable password 與 enable secret 同時存在，只有 enable secret 可用來進入 privileged EXEC。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-092_266_1431_788_221.jpg)
*Source: [[00_Source/Chapter 5 - Cisco IOS CLI|Chapter 5 - Cisco IOS CLI]], example image — `enable secret` stored as a hash in running-config.*

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

