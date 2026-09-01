# Unit03：CLI、Ethernet、IPv4 Questions

tags: #questions #acting-ccna #unit-questions

## Source

- [[02_Source_Notes/Unit03-CLI-Eth-IPV4|Unit03：CLI、Ethernet Switching 與 IPv4]]
- [[00_Source/Chapter 5 - Cisco IOS CLI|Chapter 5 - Cisco IOS CLI]]
- [[00_Source/Chapter 6 - Ethernet LAN 交換|Chapter 6 - Ethernet LAN 交換]]
- [[00_Source/Chapter 7 - IPv4 位址|Chapter 7 - IPv4 位址]]

## CLI Reasoning

1. 為什麼 Cisco IOS 要區分 [[User EXEC Mode]]、[[Privileged EXEC Mode]] 與 [[Global Configuration Mode]]？
2. 如果你設定了 hostname，但 reload 後設定消失，最可能漏掉了哪個概念？為什麼？
3. 為什麼 `enable secret` 比 `enable password` 更適合實務使用？
4. 為什麼在 configuration mode 中需要 `do show ...` 這類用法？

## Ethernet Switching Reasoning

1. Switch 為什麼用 source MAC 來學習，卻用 destination MAC 來決定 forwarding？
2. Known unicast、unknown unicast、broadcast 三者在 switch 上的處理方式有何不同？
3. 為什麼 Chapter 6 說 switch 對 hosts 是 transparent？
4. 為什麼 traffic 經過 switch 不算 Chapter 4 意義下的 hop？
5. 如果 MAC address table 是空的，PC1 第一次送 frame 給 PC3 時，為什麼 PC2 / PC4 也會收到？

## ARP / Ping Reasoning

1. 為什麼 [[Address Resolution Protocol]] 需要先 broadcast，再由目標 host unicast 回覆？
2. ARP table 與 MAC address table 分別存在誰身上？用途差在哪裡？
3. Ping 成功通常代表哪些基礎條件成立？Ping 失敗時，為什麼不能只說「IP 壞了」？
4. ARP 解決的是「知道 IP、不知道 MAC」；那「不知道 IP」可能要靠哪些後續機制？

## IPv4 Reasoning

1. 為什麼 IPv4 address 要分成 network portion 與 host portion？
2. Prefix length 與 netmask 是如何描述同一個 boundary 的？
3. 為什麼 network address 與 broadcast address 不能指派給 host？
4. `/24` network 為什麼通常有 254 個 usable host addresses，而不是 256 個？
5. TTL 為什麼是防止 routing loop 的重要安全閥？
6. IPv4 classes 為什麼是歷史上重要、但現代又不夠彈性的設計？

## Cross-unit

1. Unit02 的 [[Data Link Layer]] 在 Unit03 中被哪些具體機制實作？
2. Unit02 的 [[Network Layer]] 在 Unit03 中被哪些 IPv4 概念具體化？
3. Unit01 的 [[Switch]] 在 Unit03 中新增了哪些行為細節？
4. Unit01 的 [[Bit and Byte]] 為什麼會在 IPv4 addressing 重新變重要？

## REVIEW

- 集中 REVIEW 紀錄：[[06_review/Unit03-CLI-Eth-IPV4 REVIEW|Unit03-CLI-Eth-IPV4 REVIEW]]

