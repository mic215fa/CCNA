# Unit06：DTP、VTP、STP 與 RSTP Questions

tags: #questions #acting-ccna #vlan #dtp #vtp #stp #rstp

## Sources

- [[02_Source_Notes/Unit06-DTPVTPSTPRSTP|Unit06 Source Note]]
- [[00_Source/Chapter 13 - DTP 與 VTP]]
- [[00_Source/Chapter 14 - STP]]
- [[00_Source/Chapter 15 - RSTP]]

## DTP／VTP

1. 為什麼 `switchport mode trunk` 已固定 operational mode，仍可能需要 `switchport nonegotiate`？
2. 明確設定 access／trunk 如何同時改善可預測性、multi-vendor compatibility 與 security？
3. 為什麼較高 VTP revision number 代表較新，卻不代表內容正確？
4. VTPv3 primary server 如何縮小 VTPv1/v2 的 overwrite risk？
5. DTP 與 VTP 都是 automation；它們分別控制哪個 state，又各自不控制什麼？

## Layer 2 Loop

1. Redundant links 為什麼是 desirable design，同時又是 Layer 2 loop 的必要條件？
2. Ethernet frame 沒有 TTL，如何改變 loop 的 failure impact？
3. Broadcast storm 與 MAC address flapping 如何由同一個 loop 產生？
4. 若某 switch 的同一 MAC entry 在兩個 trunk ports 間快速移動，應如何驗證是否為 loop，而不是正常 host move？

## STP Algorithm

1. Root bridge 為什麼是共同參考點，而不是所有 traffic 必須實際經過的固定 central switch？
2. 為什麼 root placement 不應交給最低 MAC address 偶然決定？
3. Root port selection 中，為什麼比較的是 accumulated root cost？
4. Root cost 相同時，neighbor BID 與 neighbor port ID 分別解決什麼 tie？
5. Root port 是每台 non-root switch 一個，而 designated port 是每個 segment 一個；這兩種選擇單位為什麼不同？
6. 為什麼「所有 root bridge ports 都 designated」在 hub/shared segment 上需要更精確的表述？
7. Port role 與 port state 有何不同？一個 alternate port 的 role 與 state 通常各是什麼？

## Convergence

1. Classic STP 的 20 + 15 + 15 秒分別防範什麼風險？
2. 實體 port down 與單純停止收到 BPDU，為什麼會有不同 recovery time？
3. RSTP 沿用同一 topology algorithm，為什麼仍能比 STP 快？
4. 為什麼 RSTP port 接到 classic STP neighbor 時不能享有 rapid sync？
5. Point-to-point、shared、edge link types 分別如何影響 transition？
6. 為什麼「接到 end host」不會自動使 Cisco port 成為 RSTP edge port？

## Protection Features

1. PortFast 改善的是什麼問題？為什麼它本身不是 loop protection？
2. BPDU Guard 如何把「此 port 只會接 host」從假設變成 enforced policy？
3. Root Guard 收到 superior BPDU，與 Loop Guard 收不到預期 BPDU，代表哪些不同 failure models？
4. Root Guard 與 Loop Guard 為什麼不能同時用在同一 port？
5. Interface-level 與 global PortFast-level BPDU Filter 收到 BPDU 時有何不同？
6. 為什麼 BPDU Filter 名稱看似安全功能，實際上可能提高 loop risk？

## Cross-Chapter／Cross-Unit

1. DTP 形成 unintended trunk 後，可能如何擴大 VLAN 與 STP trust boundary？
2. VTP 錯誤刪除 VLAN 與 STP 阻擋 link 都可能造成不通；應用哪些 evidence 區分？
3. PVST+ 的 per-VLAN topology 如何利用 blocked links 做 load distribution？代價是什麼？
4. 為什麼 VLAN 很多時，MSTP grouping 可能比 one-instance-per-VLAN 更合理？
5. 如果未來加入 EtherChannel，它會如何改變 STP 看待多條平行 links 的方式？

## REVIEW

- [[06_review/Unit06-DTPVTPSTPRSTP REVIEW|Unit06 REVIEW]]
