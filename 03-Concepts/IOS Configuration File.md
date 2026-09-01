# IOS Configuration File

tags: #concept #acting-ccna #cli #configuration

## Definition

IOS Configuration File 是 Cisco IOS device 用來保存設備設定的文字檔，主要包含 [[Running Config]] 與 [[Startup Config]]。

## Why It Exists

設備需要同時管理「目前正在生效的設定」與「下次開機要載入的設定」，因此需要兩種用途不同的 configuration files。

## Related Concepts

- [[Running Config]]
- [[Startup Config]]
- [[Cisco IOS CLI]]

## Mechanism

- `show running-config`：查看目前設定。
- `show startup-config`：查看開機設定。
- `copy running-config startup-config`：把目前設定存成下次開機設定。

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

