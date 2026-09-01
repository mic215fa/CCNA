# Unit04：Router/Switch/Packet Questions

tags: #questions #acting-ccna #unit-questions #routing #interface

## Source

- [[02_Source_Notes/Unit4-RouterSwitch-Packet|Unit04：Router / Switch 介面、路由與封包生命週期]]
- [[00_Source/Chapter 8 - Router 與 Switch 介面|Chapter 8 - Router 與 Switch 介面]]
- [[00_Source/Chapter 9 - 路由基礎|Chapter 9 - 路由基礎]]
- [[00_Source/Chapter 10 - 封包的一生|Chapter 10 - 封包的一生]]

## Interface Reasoning

1. 為什麼 [[Interface Description]] 對 forwarding 沒有直接影響，卻仍然是好的實務？
2. 如果兩端 [[Interface Speed]] 不一致，為什麼可能直接造成 link down？
3. 為什麼 [[Duplex Mismatch]] 可能比 [[Speed Mismatch]] 更難察覺？
4. 為什麼一端手動設定 speed/duplex、另一端使用 [[Autonegotiation]] 時容易出問題？
5. Hub 與 switch 對 [[Collision Domain]] 的影響有什麼不同？這如何影響 [[Duplex]]？
6. `show interfaces` 中的 [[Interface Error]] 為什麼只能當作證據，不能單獨當作最終結論？

## Routing Reasoning

1. Router 為什麼需要 [[Routing Table]]，而不是只靠 ARP？
2. [[Connected Route]] 與 [[Local Route]] 分別回答什麼問題？
3. 為什麼 [[Route Selection]] 要偏好 longest prefix match？
4. 如果 router 收到一個 destination IP 沒有任何 route 的 packet，會發生什麼事？
5. 為什麼只設定 PC1 → PC3 方向的 [[Static Route]] 不足以保證 ping 成功？
6. [[Default Route]] 為什麼是 fallback，而不是永遠優先的 route？
7. [[Next Hop]] 與 [[Exit Interface]] 的差異是什麼？什麼情境下同時指定兩者更清楚？
8. [[Proxy ARP]] 為什麼需要 router routing table 中有通往目的地的 route 才能回覆？

## Packet Life-cycle Reasoning

1. PC1 送 packet 給遠端 PC3 時，為什麼 Ethernet destination MAC 是 [[Default Gateway]] 的 MAC，而不是 PC3 的 MAC？
2. 在 [[Packet Life Cycle]] 中，哪些資訊每一 hop 都改變？哪些資訊保持 end-to-end？
3. Router 收到 frame 後，為什麼要先 de-encapsulate 再重新 encapsulate？
4. 為什麼 switch 在 Chapter 10 中負責轉送 frames，但不算 IP routing hop？
5. [[Address Resolution Protocol]] 在 host-to-router、router-to-router、router-to-host 三種情境中扮演什麼共同角色？
6. 如果 R2 缺少往 PC3 network 的 route，PC1 到 PC3 的 packet life-cycle 會卡在哪裡？

## Cross-unit

1. Unit02 的 [[Encapsulation and De-encapsulation]] 如何在 Unit04 的 [[Packet Life Cycle]] 中被具體化？
2. Unit03 的 [[IPv4 Address]] 與 [[Prefix Length]] 如何支援 Unit04 的 [[Route Selection]]？
3. Unit03 的 [[Address Resolution Protocol]] 如何從 same-LAN lookup 延伸成 next-hop MAC resolution？
4. Unit01 的 [[Router]] / [[Switch]] 在 Unit04 中新增了哪些 interface 與 forwarding 細節？
5. Unit04 的 routing table concepts 會如何銜接後續 dynamic routing / OSPF？

## REVIEW

- 集中 REVIEW 紀錄：[[06_review/Unit4-RouterSwitch-Packet REVIEW|Unit4-RouterSwitch-Packet REVIEW]]
