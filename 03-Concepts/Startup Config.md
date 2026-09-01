# Startup Config

tags: #concept #acting-ccna #configuration

## Aliases

- startup-config

## Definition

Startup Config 是 Cisco IOS device 開機時載入的設定檔，儲存在 NVRAM。

## Why It Exists

它讓設備在重開機後能保留設定；若沒有 startup-config，設備會用 factory-default configuration 開機。

## Related Concepts

- [[Running Config]]
- [[IOS Configuration File]]

## Mechanism

常見儲存命令：

- `write`
- `write memory`
- `copy running-config startup-config`

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

