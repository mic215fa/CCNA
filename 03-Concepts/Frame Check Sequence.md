# Frame Check Sequence

tags: #concept #acting-ccna #ethernet #error-detection

## Aliases

- FCS
- Cyclic Redundancy Check
- CRC

## Definition

Frame Check Sequence 是 Ethernet trailer 的欄位，用 CRC 檢查 frame 在傳輸過程中是否毀損。

## Why It Exists

實體媒介可能因 EMI 等因素造成資料錯誤；FCS 讓接收端能偵測錯誤並丟棄損壞的 frame。

## Related Concepts

- [[Ethernet Frame]]
- [[Frame Forwarding]]
- [[Header and Trailer]]

## Contrast With

- Ethernet FCS 檢查整個 frame。
- IPv4 Header Checksum 只檢查 IPv4 header。

## Source Figures

![](../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-097_384_1031_891_320.jpg)
*Source: [[00_Source/Chapter 6 - Ethernet LAN 交換|Chapter 6 - Ethernet LAN 交換]], Figure 6.2 — FCS appears as the single field in the Ethernet trailer.*

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]

