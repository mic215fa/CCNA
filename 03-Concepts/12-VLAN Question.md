---
title: VLAN Trunking Troubleshooting
aliases:
  - VLAN Access 與 Trunk 排錯
  - VLAN Trunk 排錯
tags:
  - concept
  - acting-ccna
  - vlan
  - troubleshooting
---

# VLAN Trunking Troubleshooting

## Purpose

這份筆記整合 [[Default VLAN]]、[[Native VLAN]]、[[Native VLAN Mismatch]]、[[Access Port]]、[[Trunk Port]] 與 [[Allowed VLAN List]]，用來判斷 untagged frame 的 VLAN 歸屬及常見 trunk 設定錯誤。

## Sources

- [[00_Source/Chapter 12.2 - 設定 VLAN 與 Access Ports|Chapter 12.2 - 設定 VLAN 與 Access Ports]]
- [[00_Source/Chapter 12.3 - 使用 Trunk Ports 連接 Switches|Chapter 12.3 - 使用 Trunk Ports 連接 Switches]]

## Appears In

- [[02_Source_Notes/Unit05-SubVLAN|Unit05：Subnetting 與 VLAN]]

## Related Concepts

- [[IEEE 802.1Q Tag]]
- [[VLAN ID]]
- [[VLAN]]

## Core Distinction

這段重點是在分清楚三個東西：

- `Default VLAN`
- `Native VLAN`
- `Native VLAN mismatch`

整理如下。

## VLAN 重點整理

### 1. Default VLAN

Default VLAN 是「access port 預設所在的 VLAN」。

在 Cisco switch 上，預設情況：

```
所有 access ports → VLAN 1
```

也就是說，如果你沒有特別設定：

```
switchport access vlan 10
```

那這個 port 預設就在 VLAN 1。

重點：

- Default VLAN 通常是 `VLAN 1`
- 它是 access port 的預設 VLAN
- 一般不能改成「另一個 default VLAN」
- 但你可以把 port 手動指定到其他 VLAN

例如：

```
interface g0/1
 switchport mode access
 switchport access vlan 10
```

這樣 G0/1 就不再待在 default VLAN 1，而是被放到 VLAN 10。

---

### 2. Native VLAN

Native VLAN 是「trunk port 收到 untagged frame 時，會把它歸類到哪個 VLAN」。

Trunk port 通常會傳送多個 VLAN 的 traffic，所以需要 802.1Q tag：

```
VLAN 10 frame → tagged VLAN 10
VLAN 20 frame → tagged VLAN 20
VLAN 30 frame → tagged VLAN 30
```

但 native VLAN 是例外：

```
Native VLAN 的 frame → 通常不加 tag
```

預設情況：

```
Native VLAN = VLAN 1
```

但 native VLAN 可以 per port 修改：

```
interface g0/0
 switchport mode trunk
 switchport trunk native vlan 999
```

這表示：

```
G0/0 trunk port 上收到 untagged frame → 歸到 VLAN 999
G0/0 trunk port 送出 VLAN 999 frame → 不加 tag
```

---

## Default VLAN vs Native VLAN

|項目|Default VLAN|Native VLAN|
|---|---|---|
|作用對象|Access port|Trunk port|
|預設值|VLAN 1|VLAN 1|
|用途|未設定 access VLAN 時，port 預設所在 VLAN|Trunk 收到 untagged frame 時歸屬的 VLAN|
|是否可改|Default VLAN 本身通常不可改|可以 per trunk port 修改|
|常見誤解|以為所有 VLAN 1 都是 native VLAN|以為 native VLAN 只跟 access port 有關|

一句話記：

> Default VLAN 是 access port 的預設歸屬；Native VLAN 是 trunk port 處理 untagged frame 的規則。

---

## 應用方式

### 情境 1：設定 Access Port

PC 接到 switch port，通常設定為 access port：

```
interface g0/1
 switchport mode access
 switchport access vlan 10
```

意思是：

```
G0/1 只屬於 VLAN 10
PC 送來的 untagged frame → switch 視為 VLAN 10 traffic
```

應用：

- PC
- Printer
- Server
- 一般 end host

---

### 情境 2：設定 Trunk Port

兩台 switches 之間要傳多個 VLAN：

```
interface g0/0
 switchport mode trunk
 switchport trunk allowed vlan 10,20,30
```

意思是：

```
G0/0 可以承載 VLAN 10、20、30
非 native VLAN traffic 會加上 802.1Q tag
```

應用：

- Switch-to-switch
- Switch-to-router，ROAS
- Switch-to-multilayer switch

---

### 情境 3：設定 Native VLAN

安全實務上，通常不要使用 VLAN 1 當 native VLAN，可以改成未使用 VLAN：

```
vlan 999
 name UNUSED_NATIVE

interface g0/0
 switchport mode trunk
 switchport trunk native vlan 999
```

應用目的：

- 避免 untagged traffic 混入正常 user VLAN
- 降低 VLAN hopping 風險
- 讓 native VLAN 不承載正常使用者流量

---

## 可能錯誤

### 錯誤 1：Default VLAN 和 Native VLAN 混淆

錯誤理解：

```
VLAN 1 是 default VLAN，所以所有 untagged traffic 都一定是 access traffic。
```

正確理解：

```
Access port 收到 untagged frame → 歸到 access VLAN
Trunk port 收到 untagged frame → 歸到 native VLAN
```

重點是：

```
同樣是 untagged frame，要看它進來的是 access port 還是 trunk port。
```

---

### 錯誤 2：Native VLAN mismatch

假設：

```
SW1 G0/0 native VLAN = 10
SW2 G0/0 native VLAN = 30
```

那麼：

```
SW1 送出 VLAN 10 的 untagged frame
↓
SW2 收到 untagged frame
↓
SW2 以為它是 VLAN 30
```

結果：

```
Frame 被放到錯誤 VLAN
可能無法到達目的地
也可能造成安全風險
```

這就是 native VLAN mismatch。

---

### 錯誤 3：忘記 allowed VLAN list

例如 trunk 只允許 VLAN 10、20：

```
switchport trunk allowed vlan 10,20
```

但你以為 VLAN 30 也能過 trunk。

結果：

```
VLAN 30 的 traffic 無法跨 switch
同 VLAN 的 PC 在不同 switch 上可能無法通訊
```

---

### 錯誤 4：修改 allowed VLAN 時忘記 add

你原本允許：

```
switchport trunk allowed vlan 10,20,30
```

想新增 VLAN 40，錯誤下法：

```
switchport trunk allowed vlan 40
```

這不是「新增 VLAN 40」。

這會變成：

```
只允許 VLAN 40
```

正確應該是：

```
switchport trunk allowed vlan add 40
```

---

### 錯誤 5：把 access port 當 trunk port 排錯

如果 port 實際是 access port：

```
switchport mode access
switchport access vlan 10
```

那麼這些 trunk 設定不會生效：

```
switchport trunk native vlan 20
switchport trunk allowed vlan 10,20,30
```

因為 port 不是 trunk mode。

所以排錯時要先確認：

```
show interfaces trunk
show interfaces switchport
show vlan brief
```

## 最重要考點

你可以這樣記：

```
Default VLAN
= access port 沒設定時的預設 VLAN
= VLAN 1

Native VLAN
= trunk port 收到 untagged frame 時歸到哪個 VLAN
= 預設 VLAN 1，但可以改

Native VLAN mismatch
= trunk 兩端 native VLAN 不一致
= untagged frame 被放到錯誤 VLAN
```

最實務的安全建議：

```
不要讓 native VLAN 使用 VLAN 1
不要讓 native VLAN 承載正常使用者流量
trunk 兩端 native VLAN 必須一致
allowed VLAN list 只允許必要 VLAN
```
