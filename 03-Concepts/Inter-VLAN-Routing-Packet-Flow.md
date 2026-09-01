最簡單的方法是：把我上一則回答整理成一份 **Markdown 筆記**，然後直接存進你的 Obsidian Vault。

我建議不要原封不動貼進去，而是整理成「可複習、可連結、可考試」的格式。以你目前的 CCNA Obsidian 結構來看，可以把它當成一個獨立 Concept Note，例如：

![[00_Source/images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-234_763_1419_184_220.jpg]]

```text
concepts/
└─ Inter-VLAN-Routing-Packet-Flow.md
```

內容可以整理成這樣：

````markdown
---
title: Inter-VLAN Routing Packet Flow
aliases:
  - VLAN 間路由封包流程
  - Inter-VLAN Routing
tags:
  - CCNA
  - VLAN
  - Routing
  - Ethernet
  - 802.1Q
---

# Inter-VLAN Routing Packet Flow

## 核心觀念

當來源主機與目的主機位於不同 VLAN / Subnet 時：

1. 主機不會直接把 Ethernet Frame 送給目的主機。
2. 主機先把 Frame 送給 Default Gateway。
3. Router 根據 Destination IP 進行路由。
4. Router 會拆掉原本的 Layer 2 Frame。
5. Router 從另一個介面建立新的 Layer 2 Frame。
6. 802.1Q VLAN Tag 通常只出現在 Trunk Link 上。

---

## 範例拓撲

PC3 位於 VLAN 20，要傳送資料給 VLAN 10 的 PC10。

假設：

### VLAN 20

- PC3 IP：`192.168.20.3`
- PC3 MAC：`PC3-MAC`
- R1 G0/1 IP：`192.168.20.1`
- R1 G0/1 MAC：`R1-G01-MAC`

### VLAN 10

- PC10 IP：`192.168.10.10`
- PC10 MAC：`PC10-MAC`
- R1 G0/0 IP：`192.168.10.1`
- R1 G0/0 MAC：`R1-G00-MAC`

---

# Step 1：PC3 判斷目的地是否在相同 Subnet

PC3：

```text
192.168.20.3/24
````

PC10：

```text
192.168.10.10/24
```

兩者不在相同 subnet：

```text
192.168.20.0/24
        ≠
192.168.10.0/24
```

因此 PC3 必須送給：

```text
Default Gateway = R1 G0/1
```

> [!important]  
> 不同 Subnet → 送給 Default Gateway。

---

# Step 2：PC3 建立 Ethernet Frame

IP Packet：

```text
Source IP      = 192.168.20.3
Destination IP = 192.168.10.10
```

Ethernet Frame：

```text
Source MAC      = PC3-MAC
Destination MAC = R1-G01-MAC
```

因此：

```text
Ethernet
├─ Src MAC = PC3
├─ Dst MAC = R1 G0/1
└─ IP
   ├─ Src IP = PC3
   └─ Dst IP = PC10
```

> [!important]  
> Destination IP 是最終目的地。
> 
> Destination MAC 是目前的下一跳。

---

# Step 3：PC3 → SW1

PC3 連接到 SW1 的 Access Port：

```text
SW1 G1/2
switchport mode access
switchport access vlan 20
```

PC3 傳出的 Frame：

```text
Untagged
```

PC 本身通常不知道 VLAN ID。

Switch 根據 ingress access port 判斷：

```text
此 Frame 屬於 VLAN 20
```

---

# Step 4：SW1 → R1 G0/1

SW1 查看：

```text
Destination MAC = R1 G0/1 MAC
```

查 MAC Address Table：

```text
R1-G01-MAC → VLAN 20 → G0/2
```

因此：

```text
PC3
 ↓
SW1
 ↓
R1 G0/1
```

這條連線是 Access Link，因此：

```text
No 802.1Q Tag
```

---

# Step 5：Router 拆掉 Layer 2 Frame

R1 收到：

```text
Dst MAC = R1 G0/1
```

因此 R1 移除 Ethernet Header。

剩下：

```text
IP Packet

Src IP = PC3
Dst IP = PC10
```

Router 接下來看的主要是：

```text
Destination IP
```

---

# Step 6：Router 查 Routing Table

R1 可能有：

```text
192.168.10.0/24 → G0/0
192.168.20.0/24 → G0/1
192.168.30.0/24 → G0/2
```

Destination IP：

```text
192.168.10.10
```

符合：

```text
192.168.10.0/24
```

所以 R1 選擇：

```text
Outgoing Interface = G0/0
```

---

# Step 7：Router 建立新的 Ethernet Frame

舊的 Frame：

```text
Src MAC = PC3
Dst MAC = R1 G0/1
```

已經被丟掉。

R1 建立新的 Frame：

```text
Src MAC = R1 G0/0
Dst MAC = PC10
```

IP Address 基本保持：

```text
Src IP = PC3
Dst IP = PC10
```

可以記成：

> [!important]  
> Router 會改 Layer 2 Address。
> 
> Router 不會改最終 Source / Destination IP。
> 
> TTL 會減 1。

---

# Step 8：R1 → SW1

R1 G0/0 連接 SW1 Access VLAN 10。

因此：

```text
R1 G0/0
 ↓
SW1
```

Frame 仍然：

```text
Untagged
```

SW1 根據 access port 知道：

```text
VLAN 10
```

---

# Step 9：SW1 → SW2 Trunk

PC10 接在 SW2。

因此 SW1 必須透過：

```text
SW1 G0/0
    ↓
Trunk
    ↓
SW2 G0/0
```

把 Frame 傳到 SW2。

因為 Trunk 可以同時傳送：

- VLAN 10
    
- VLAN 20
    
- VLAN 30
    

所以 SW1 必須加入：

```text
802.1Q Tag
VLAN ID = 10
```

傳送中的 Frame：

```text
Ethernet
├─ 802.1Q Tag = VLAN 10
├─ Src MAC = R1 G0/0
├─ Dst MAC = PC10
└─ IP
   ├─ Src IP = PC3
   └─ Dst IP = PC10
```

---

# Step 10：SW2 → PC10

SW2 從 Trunk 收到：

```text
VLAN ID = 10
```

查 MAC Table：

```text
PC10-MAC → VLAN 10 → G0/1
```

G0/1 是 Access Port。

因此 SW2 在送給 PC10 前移除 VLAN Tag：

```text
Tagged
   ↓
SW2
   ↓
Untagged
   ↓
PC10
```

---

# 完整封包流程

```text
PC3
VLAN 20
 │
 │ Untagged
 ▼
SW1 G1/2
 │
 │ VLAN 20
 ▼
SW1 G0/2
 │
 │ Untagged
 ▼
R1 G0/1
 │
 │ Routing
 ▼
R1 G0/0
 │
 │ Untagged
 ▼
SW1 G0/1
 │
 ▼
SW1 G0/0
 │
 │ 802.1Q Tag = VLAN 10
 │
 │ ===== TRUNK =====
 ▼
SW2 G0/0
 │
 ▼
SW2 G0/1
 │
 │ Remove VLAN Tag
 ▼
PC10
```

---

# Layer 2 與 Layer 3 變化

|階段|Source MAC|Destination MAC|Source IP|Destination IP|
|---|---|---|---|---|
|PC3 → R1|PC3|R1 G0/1|PC3|PC10|
|R1 → PC10|R1 G0/0|PC10|PC3|PC10|

最重要的規則：

```text
MAC Address
→ 每經過 Router 都可能改變

IP Address
→ 端到端基本維持不變
```

---

# Access vs Trunk

|Link|Port Type|VLAN Tag|
|---|---|---|
|PC3 → SW1|Access|No|
|SW1 → R1 G0/1|Access|No|
|R1 G0/0 → SW1|Access|No|
|SW1 → SW2|Trunk|Yes, VLAN 10|
|SW2 → PC10|Access|No|

> [!note]  
> Access Port 通常傳送 Untagged Frame。
> 
> Trunk Port 使用 802.1Q Tag 區別不同 VLAN。
> 
> Native VLAN 是例外，通常不加 Tag。

---

# Router 與 Trunk 的差異

## Router

負責：

```text
VLAN 20
   ↓
Routing
   ↓
VLAN 10
```

也就是：

**不同 IP Subnet / VLAN 之間的 Layer 3 forwarding。**

## Trunk

負責：

```text
SW1 ===== SW2

VLAN 10
VLAN 20
VLAN 30
```

也就是：

**讓多個 VLAN 共用一條實體 Link。**

簡單記：

```text
Router = 跨 VLAN

Trunk = 攜帶 VLAN
```

---

# CCNA 考試重點

> [!tip] Rule 1  
> Different Subnet → Send to Default Gateway

> [!tip] Rule 2  
> Router 根據 Destination IP 做 Routing。

> [!tip] Rule 3  
> Router 會移除舊 Layer 2 Frame，再建立新的 Layer 2 Frame。

> [!tip] Rule 4  
> Destination MAC 通常是 Next Hop 的 MAC。

> [!tip] Rule 5  
> 802.1Q Tag 主要存在於 Trunk Link。

---

# 一句話理解

```text
PC3
先把 Packet 交給 Default Gateway
        ↓
Router 根據 Destination IP 換到 VLAN 10
        ↓
Switch 透過 Trunk 把 VLAN 10 Frame 傳到 SW2
        ↓
SW2 再交給 PC10
```

---

## Related Concepts

- [[VLAN]]
    
- [[Access Port]]
    
- [[Trunk Port]]
    
- [[802.1Q]]
    
- [[Default Gateway]]
    
- [[ARP]]
    
- [[MAC Address Table]]
    
- [[Routing Table]]
    
- [[Inter-VLAN Routing]]
    
- [[Router-on-a-Stick]]
    

````

### 我比較建議你怎麼放

依你現在的 CCNA Obsidian 架構，我會把它放在：

```text
concepts/Inter-VLAN-Routing-Packet-Flow.md
````

而不是放進 Chapter Note，因為這個知識點未來在 **VLAN、Trunk、ARP、Routing、Router-on-a-Stick** 等章節都會重複引用。Chapter Note 裡只需要寫：

```markdown
詳細封包流程請參考：
[[Inter-VLAN-Routing-Packet-Flow]]
```

這樣你的 Obsidian 就會逐漸形成真正的「知識網路」，而不是每一章各自複製一份內容。

另外，**你原本那張 Figure 12.9 很值得一起放進這個 Note**，放在「範例拓撲」下面，然後用 Obsidian 圖片連結：

```markdown
![[figure-12-9-inter-vlan-routing.jpg]]
```

這樣閱讀時會比只有文字清楚很多。