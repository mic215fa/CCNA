# Enable Password

tags: #concept #acting-ccna #security #cli

## Definition

Enable Password 是保護 [[Privileged EXEC Mode]] 存取權的舊式密碼設定。

## Why It Exists

Privileged EXEC 能進入設定模式與執行高權限命令，因此需要密碼避免未授權使用。

## Related Concepts

- [[Enable Secret]]
- [[Privileged EXEC Mode]]
- [[Running Config]]

## Contrast With

- [[Enable Secret]] 較安全，會以 hash 儲存，而且若兩者同時設定，enable secret 優先。
- Enable password 若未加密會以 cleartext 出現在 configuration file；`service password-encryption` 也只是弱加密。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-091_226_1267_480_316.jpg)
*Source: [[00_Source/Chapter 5 - Cisco IOS CLI|Chapter 5 - Cisco IOS CLI]], example image — `enable password` shown in cleartext in running-config.*

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

