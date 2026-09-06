# Unit06：DTP、VTP、STP 與 RSTP REVIEW

tags: #review #acting-ccna #vlan #stp #rstp

## Sources

- [[01_Units/Unit06-DTPVTPSTPRSTP|Unit06 Definition]]
- [[02_Source_Notes/Unit06-DTPVTPSTPRSTP|Unit06 Source Note]]
- [[04_Maps/Unit06-DTPVTP-STP-RSTP Knowledge Map|Unit06 Knowledge Map]]
- [[05_Questions/Unit06-DTPVTPSTPRSTP Questions|Unit06 Questions]]

## Resolved During Processing

### Root bridge designated-port wording

- Chapter 14 uses the practical shortcut「all ports on the root bridge are designated」。
- Chapter 15 adds the shared-segment exception：若 root bridge 的多個 ports 接到同一 segment，只能有一個 designated port。
- Resolution：Concept 採用更精確表述「root bridge is the designated bridge for every attached segment」，並保留常見簡化說法作為提醒。
- Status：RESOLVED from cross-document evidence。

### STP and RSTP algorithm boundary

- STP 與 RSTP 不應拆出兩套 root／port selection Concepts，因 source 明確指出 topology calculation algorithm 相同。
- Resolution：共同 algorithm 集中於 [[Spanning Tree Protocol]]；[[Rapid Spanning Tree Protocol]] 只承載不同的 BPDU behavior、states、roles、link types 與 convergence。
- Status：RESOLVED，避免重複筆記。

### Protection-feature fragmentation

- PortFast、BPDU Guard、Root Guard、Loop Guard、BPDU Filter 各有獨立 command，但都依賴 BPDU／topology assumptions。
- Resolution：整合為 [[STP Protection Features]] protection matrix，不為每個 command 建立薄 Concept。
- Status：RESOLVED；若後續 security Units 大量引用其中單一 feature，再重新評估。

## REVIEW Items

### MSTP concept boundary

- Chapter 15 只要求理解「multiple VLANs grouped into one instance」與 RSTP mechanics，未涵蓋 MST region、revision、mapping digest 或 inter-region behavior。
- Current decision：保留在 [[Rapid Spanning Tree Protocol#Versions|RSTP Versions]]，不建立獨立 MSTP Concept。
- Status：REVIEW when a later source provides operational MSTP configuration or troubleshooting。

### EtherChannel relationship

- Strong inference：EtherChannel 可讓 parallel physical links 對 STP 呈現為一條 logical link，延伸本 Unit 的 blocked-capacity 問題。
- Evidence boundary：Chapter 13–15 在目前 Unit 尚未完整教授 EtherChannel mechanism。
- Status：REVIEW when EtherChannel Unit is processed；再確認並建立正式 Concept／Map relationship。

### Platform-dependent defaults

- Source 指出 default PVST+／Rapid PVST+ mode 與 short／long path-cost method 會依 switch model 和 IOS version 不同。
- Current decision：Concept 不宣稱單一 universal default，要求用 `show spanning-tree` 與 `show spanning-tree pathcost method` 驗證。
- Status：REVIEW only when documenting a specific platform image or lab environment。

### Protection deployment scope

- Source 提供 feature mechanics 與典型用途，但沒有定義完整 enterprise placement policy。
- Current decision：只記錄 source-supported assumptions，不把所有 access/trunk ports 一律套用 Root Guard 或 Loop Guard。
- Status：REVIEW when campus-design or security-hardening sources are added。
