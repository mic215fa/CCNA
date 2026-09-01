# Unit05：Subnetting 與 VLAN REVIEW

tags: #review #acting-ccna #unit-review #subnetting #vlan

## Sources

- [[01_Units/Unit05-SubVLAN|Unit05-SubVLAN]]
- [[02_Source_Notes/Unit05-SubVLAN|Unit05：Subnetting 與 VLAN]]
- [[04_Maps/Unit05-SubVLAN 知識地圖|Unit05：Subnetting 與 VLAN 知識地圖]]
- [[05_Questions/Unit05-SubVLAN Questions|Unit05：Subnetting 與 VLAN Questions]]

## REVIEW Items

### Concept boundary: Subnetting calculation methods

- [[Subnetting]]、[[Borrowed Bits]]、[[Subnet Five Attributes]]、[[Magic Number Method]] 已拆成概念。
- Reason：這些是 CCNA 反覆出現的計算與推理骨架，但後續可能需要更多 worked examples，而不是再拆更多微概念。
- Status：REVIEW when adding practice-oriented blue/yellow notes or subnetting drills。

### Concept boundary: FLSM and VLSM

- [[FLSM]] 與 [[VLSM]] 已分開。
- Reason：兩者在設計目的與計算流程上不同，且 VLSM 會在 routing design 中反覆出現。
- Status：OK；後續 route summarization / OSPF 可能補充。

### Concept boundary: VLAN vs subnet

- [[VLAN]] 與 [[Subnetting]] 保持分開，但在 Unit05 map 中建立強關係。
- Reason：Chapter 12 強調 Layer 3 segmentation 與 Layer 2 segmentation 都需要；兩者常一對一規劃，但不是同一種概念。
- Status：重要觀念；人工複習時應特別確認。

### Concept boundary: Access Port / Trunk Port / Network Interface

- [[Access Port]]、[[Trunk Port]] 被建立為 [[Network Interface]] 的更具體 switchport roles。
- Reason：後續 DTP、VTP、STP、EtherChannel、native VLAN troubleshooting 都會重用。
- Status：OK；後續若出現 dynamic desirable/auto 或 trunk negotiation，再追加。

### Concept boundary: ISL

- Chapter 12 briefly mentions Cisco ISL, but this Unit did not create a standalone [[ISL]] concept.
- Reason：Chapter 12 primarily teaches 802.1Q; ISL appears mainly as legacy contrast and may not deserve a standalone long-term Concept yet。
- Status：REVIEW if Chapter 13 or later source gives ISL more substantive treatment。

### Relationship confidence: subnet-to-VLAN one-to-one design

- Relationship：[[Subnetting]] supplies Layer 3 boundaries that [[VLAN]] design should often mirror at Layer 2。
- Reason：Chapter 12 examples align departments, subnets, and VLANs; however, real networks may have exceptions.
- Status：Strongly Inferred；teach as common design pattern, not absolute law。

### SVI operational requirements

- [[Switch Virtual Interface]] requires more than IP configuration to be up/up.
- Reason：Chapter 12 lists VLAN existence and active access/trunk conditions; future labs may reveal additional platform-specific behavior。
- Status：REVIEW during multilayer switching labs。

### Periodic knowledge review

- Unit01–Unit05 have now been processed.
- Reason：AGENTS.md recommends higher-level review after 3–5 Units. This is a good point to inspect hubs, duplicates, orphan concepts, missing prerequisite chains, and map coverage.
- Status：REVIEW recommended before processing later Layer 2 chapters, especially around [[Switch]], [[Router]], [[IPv4 Address]], [[Subnetting]], [[VLAN]], [[Trunk Port]], [[Routing Table]], and [[Packet Life Cycle]]。
