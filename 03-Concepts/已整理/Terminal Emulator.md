# Terminal Emulator

tags: #concept #acting-ccna #cli

> [!info] Concept role
> Terminal Emulator 是 console access chain 的用戶端軟體；它負責選取連線介面、套用 serial parameters，並收送 CLI 字元。

## Definition

Terminal Emulator 是在電腦上模擬傳統 terminal 的軟體，用來透過 console 連線存取設備 CLI。

## Why It Exists

PC 需要一個軟體來傳送文字命令、接收 CLI 輸出，才能管理 Cisco router 或 switch。

## Related Concepts

- [[Console Port]]
- [[Cisco IOS CLI]]
- [[Rollover Cable]]

## Mechanism

```text
keyboard input → terminal session → console connection → IOS CLI
IOS output → console connection → terminal rendering
```

## Dependencies and Limitations

- 必須選到正確的 serial／USB interface，且連線參數需符合 console port。
- Terminal session 成功不代表資料平面或遠端管理功能正常。

## Troubleshooting

- 沒有輸出：檢查 interface 選擇、session 是否開啟、cable 與設備電源。
- 亂碼：優先檢查 speed 等 serial parameters。
- 無法輸入或 session 中斷：重建 session，並確認其他程式沒有占用同一 serial interface。

## Source Figures

![](../../00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-081_698_830_185_320.jpg)
*Source: [[00_Source/Chapter 5 - Cisco IOS CLI|Chapter 5 - Cisco IOS CLI]], Figure 5.5 — PuTTY serial settings for console access to a Cisco device.*

## Appears In

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]
