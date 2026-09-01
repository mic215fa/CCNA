# Unit01：設備與線材 Questions

tags: #questions #acting-ccna #network-fundamentals

## Sources

- [[02_Source_Notes/Unit01-設備-線材|Unit01：設備與線材]]

## Why

- 為什麼 [[Computer Network]] 的定義要包含 [[Node]] 與 [[Resource]]？
- 為什麼 [[Client-Server Model]] 應該被視為角色關係，而不是硬體分類？
- 為什麼 [[Switch]] 適合 LAN 內連接設備，但不適合直接拿來連接 Internet？
- 為什麼 [[Router]] 通常被放在 LAN edge？
- 為什麼連到 Internet 會讓 [[Firewall]] 成為必要或至少重要的設備？
- 為什麼 [[Network Standard]] 對不同廠商設備互通如此重要？
- 為什麼 [[Ethernet]] 不是「一條網路線」，而是一組標準家族？

## What If

- 如果兩台使用相同 Tx pin pair 的設備用 straight-through cable 直連，會發生什麼事？為什麼？
- 如果 UTP 線材長度超過標準支援距離，可能造成什麼結果？
- 如果兩台 fiber 設備的 transmitter 沒有接到對方 receiver，會發生什麼事？
- 如果 client device 沒有 SFP port，為什麼 Fiber 可能不是可行選擇？

## Relationship

- [[LAN]]、[[Switch]]、[[End Host]] 三者如何互相定義彼此的角色？
- [[Router]] 與 [[WAN]] 的關係是什麼？Chapter 2 有哪些內容尚未展開？
- [[UTP Cable]]、[[8P8C Connector]]、[[Straight-through and Crossover Cable]] 之間的依賴關係是什麼？
- [[Auto MDI-X]] 如何改變使用者對 straight-through / crossover cable 的操作負擔？
- [[Fiber Optic Cable]]、[[SFP Transceiver]]、[[MMF and SMF]] 之間如何形成一個選型問題？

## Contrast

- [[Switch]] 與 [[Router]] 的角色差異是什麼？
- [[UTP Cable]] 與 [[Fiber Optic Cable]] 的主要取捨是什麼？
- MMF 與 SMF 在 core、transmitter、距離、成本上的差異是什麼？
- Host-based firewall 與 network firewall 的差異是什麼？
- bit 與 byte 的差異為什麼會影響網路速度與檔案大小的理解？

## Cause and Effect

- 共同標準不足會如何影響設備互通？
- 端點數量增加如何導致對 Switch 的需求？
- Internet 連線如何導致安全風險，並引出 Firewall？
- UTP 的距離限制如何引出 Fiber 的使用場景？
- EMI 如何影響 UTP 訊號？

## Troubleshooting

- 若兩台相同類型設備以 UTP 直連無法通訊，應先檢查 cable type、pin pair 還是 IP 設定？本 Unit 支援哪一個優先假設？
- 若跨樓層設備連線品質差，如何從 UTP 長度限制推論可能原因？
- 若 Fiber 連線不通，為什麼應檢查 transmitter/receiver 是否交叉正確？

## Cross-Document / Cross-Unit

- Chapter 2 中的 Switch 概念，如何在 Chapter 3 中被 port、connector、UTP cable 具體化？
- Chapter 2 中的 Router 概念，如何在 Chapter 3 的 straight-through/crossover 範例中再次出現？
- Chapter 3 的 Ethernet 標準，如何為後續 Chapter 6 Ethernet LAN switching 鋪路？
- Chapter 3 的 bit/binary，如何為後續 IPv4 addressing 與 subnetting 鋪路？

## REVIEW

- 集中 REVIEW 紀錄：[[06_review/Unit01-設備-線材 REVIEW|Unit01-設備-線材 REVIEW]]
