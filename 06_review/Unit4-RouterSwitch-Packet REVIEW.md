# Unit04：Router/Switch/Packet REVIEW

tags: #review #acting-ccna #unit-review #routing #interface

## Sources

- [[01_Units/Unit4-RouterSwitch-Packet|Unit4-RouterSwitch-Packet]]
- [[02_Source_Notes/Unit4-RouterSwitch-Packet|Unit04：Router / Switch 介面、路由與封包生命週期]]
- [[04_Maps/Unit04-RouterSwitch-Packet 知識地圖|Unit04：Router/Switch/Packet 知識地圖]]
- [[05_Questions/Unit4-RouterSwitch-Packet Questions|Unit04：Router/Switch/Packet Questions]]

## REVIEW Items

### Concept boundary: Network Interface vs physical port

- Unit04 建立 [[Network Interface]] 作為泛用概念，涵蓋 router/switch interface。
- Reason：後續可能出現 switchport、routed port、SVI、subinterface、loopback interface；屆時可能需要拆出更精確概念。
- Status：REVIEW when VLAN / trunk / SVI / routing protocol interface topics appear。

### Concept boundary: CSMA-CD relevance

- [[CSMA-CD]] 在現代 switched full-duplex Ethernet 中較少實際使用，但 Chapter 8 用它解釋 hub、collision domain 與 duplex 的歷史背景。
- Reason：保留為考試與概念脈絡，但不要過度延伸成現代 switchport 常態行為。
- Status：OK as historical/prerequisite concept；REVIEW if later source discusses modern Ethernet behavior。

### Concept boundary: Proxy ARP

- [[Proxy ARP]] 已建立為 Unit04 concept，因 Chapter 9 用它說明只指定 exit interface 的 static route 情境。
- Reason：實務與考試上可能會建議避免依賴 Proxy ARP，改用 next-hop 或 fully specified static route；目前來源只建立基本機制。
- Status：REVIEW after more advanced routing/static route materials。

### Concept boundary: Static route forms

- [[Static Route#Recursive Static Route|Recursive Static Route]] 與 [[Static Route#Fully Specified Static Route|Fully Specified Static Route]] 已整合回 [[Static Route]]，並加入 exit-interface-only form 作完整比較。
- Reason：它們是同一 route source 的不同 forwarding-instruction forms，不需要形成獨立薄 Concept Notes。
- Status：Concept boundary 已解決；Proxy ARP 與 multiaccess behavior 仍保留 REVIEW。

### Concept boundary: Route selection depth

- [[Route Selection]] 目前以 Chapter 9 的 longest prefix match 為主。
- Reason：後續 dynamic routing 會加入 administrative distance、metric、protocol preference 等更完整的 route selection rules。
- Status：REVIEW after OSPF / dynamic routing Units。

### Relationship confidence: interface state → connected route

- Relationship：[[Network Interface]] health is prerequisite to useful [[Connected Route]] and real forwarding。
- Reason：Chapter 9 connected routes depend on configured interfaces; Chapter 8 explains interface operational state. The combined relationship is strongly inferred across chapters rather than quoted as one source sentence。
- Status：Strongly Inferred；manual validation useful。

### Duplicate risk: Default Gateway vs Next Hop

- [[Default Gateway]] and [[Next Hop]] are related but not identical。
- Reason：Default gateway is usually a host's configured next-hop router; next hop is a broader routing concept used by routers as well。
- Status：Keep separate for now；REVIEW if notes become redundant。

### Periodic knowledge review trigger

- Unit01–Unit04 have now been processed into the knowledge network.
- Reason：AGENTS.md recommends higher-level review after 3–5 Units to identify hubs, duplicate concepts, orphan concepts, missing maps, and major prerequisite chains。
- Status：REVIEW recommended before or after Unit05, especially around [[Router]], [[Switch]], [[IPv4 Address]], [[Address Resolution Protocol]], [[Routing Table]], and [[Packet Life Cycle]]。
