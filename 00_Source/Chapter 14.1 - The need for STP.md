---
source: [[Chapter 14 - STP]]
chapter: 14
section: 14.1
tags: [source-section, acting-ccna, stp]
---

# Chapter 14.1 - The need for STP

In chapter 7 (IPv4 addressing), we briefly covered the fields of the IPv4 header; one of those is the Time-to-Live (TTL) field, which is decremented each time a router forwards a packet. When the value in the TTL field reaches 0, the packet is dropped, preventing packets from looping around the network indefinitely as the result of a misconfiguration; this is called a routing loop or Layer 3 loop.

> [!translation] 逐句繁體中文翻譯
> - 在第 7 章（IPv4 尋址）中，我們簡要介紹了 IPv4 標頭的字段；其中之一是生存時間 (TTL) 字段，每當router轉送packet時，該字段就會遞減。
> - 當 TTL 欄位中的值達到 0 時，packet將被丟棄，從而防止packet因配置錯誤而在network中無限循環；這稱為路由迴路或Layer 3環路。

The Ethernet header has no such field; if a loop occurs between switches-a Layer 2 loop-there is no mechanism in place to prevent frames from looping around the LAN indefinitely. If there are too many frames looping around the LAN, the switches can be overwhelmed, resulting in a loss of service for all hosts in the LAN.

> [!translation] 逐句繁體中文翻譯
> - 乙太network標頭沒有這樣的欄位；如果switch之間發生環路（Layer 2環路），則沒有適當的機制可以防止frame在 LAN 中無限期地循環。
> - 如果 LAN 中循環的frame過多，switch可能會不堪重負，從而導致 LAN 中所有host的服務遺失。

So, how do Layer 2 loops occur? Whereas Layer 3 loops are the result of a misconfiguration somewhere in the network, Layer 2 loops are inevitable in a LAN where there are multiple paths between any two nodes in the LAN, as a result of the flooding of BUM traffic (broadcast, unknown unicast, and multicast) frames.

> [!translation] 逐句繁體中文翻譯
> - 那麼，二層環路是如何產生的呢？
> - Layer 3環路是network中某處配置錯誤的結果，而Layer 2環路在 LAN 中是不可避免的，因為 LAN 中的任意兩個節點之間存在多條路徑，這是 BUM traffic（廣播、未知frame單播和多播）的結果。

NOTE I will mention multicast traffic a few times throughout this book's two volumes. For now, just know that multicast frames are flooded by switches by default.

> [!translation] 逐句繁體中文翻譯
> - 注意 在本書的兩卷中，我將多次提到多播traffic。
> - 現在，只需知道預設交換機會泛洪多播frame。

Having multiple paths between hosts is an example of redundancy and is a desirable thing in a network. Redundancy means having additional network devices and connections beyond the minimum necessary for communication. By having redundant devices and connections, network service isn't lost if one device or connection fails- there is no single point of failure.

> [!translation] 逐句繁體中文翻譯
> - host之間擁有多個路徑是冗餘的一個例子，並且是network中理想的事情。
> - 冗餘意味著擁有超出通訊所需最低限度的額外network設備和連線。
> - 透過擁有冗餘設備和連接，如果一台設備或連接發生故障，network服務也不會遺失 - 不存在單點故障。

However, without something like STP to prevent loops, frames will loop indefinitely in a LAN with redundant connections, as demonstrated in figure 14.1. Any one of the connections between switches in the figure could be removed (e.g., the connection between SW2 and SW3), and the PCs would still be able to communicate with each other; this is an example of redundancy.

> [!translation] 逐句繁體中文翻譯
> - 然而，如果沒有像 STP 這樣的東西來防止環路，frame將在具有冗餘連接的 LAN 中無限循環，如圖 14.1 所示。
> - 圖中switch之間的任何一個連接都可以被刪除（例如SW2和SW3之間的連接），並且PC仍然能夠相互通信；這是冗餘的一個例子。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-266_559_1233_1170_225.jpg)
Figure 14.1 PC1 sends a broadcast frame, and SW1 floods it. When SW2 and SW3 receive their copies of the frame, they flood it too, resulting in two loops: counterclockwise (A) and clockwise (B). These frames will loop indefinitely between SW1, SW2, and SW3.

NOTE As the arrows pointing toward the PCs in figure 14.1 indicate, SW1, SW2, and SW3 will also flood the looping frames toward connected end hosts, potentially overwhelming them by requiring the hosts to process the looping frames repeatedly.

> [!translation] 逐句繁體中文翻譯
> - 注意 如圖 14.1 中指向 PC 的箭頭所示，SW1、SW2 和 SW3 也將向連接的終端host洪氾循環frame，可能會要求host重複處理循環frame，從而使它們不堪重負。

There are two main problems caused by Layer 2 loops. First, if enough looping frames accumulate in the network, the result is a broadcast storm, consuming so many network resources (CPU resources on the devices or bandwidth of the links) that the network is rendered unusable. PCs and other end hosts connected to the switches also receive the same frames repeatedly, which could overwhelm their available resources too.

> [!translation] 逐句繁體中文翻譯
> - Layer 2環路會導致兩個主要問題。
> - 首先，如果network中累積了足夠多的循環frame，就會導致broadcast storm，消耗大量network資源（設備上的 CPU 資源或link頻寬），導致network無法使用。
> - 連接到switch的 PC 和其他終端host也會重複接收相同的frame，這也可能會耗盡其可用資源。

The second problem is MAC address flapping-when a switch learns the same MAC address repeatedly on separate ports. Using the example in figure 14.1, when SW1 first receives PC1's broadcast frame, it learns PC1's MAC address on the G0/2 port. However, when looped Frame A arrives back on G0/1, it learns PC1's MAC address on that port, and the same applies when looped Frame B arrives back on G0/0. SW1 will constantly update the entry for PC1's MAC address in its MAC address table between multiple ports, resulting in PC1 being unable to receive frames; SW1 doesn't know which port PC1 is actually connected to.

> [!translation] 逐句繁體中文翻譯
> - 第二個問題是 MAC address flapping - 當switch在不同的port 上重複學習相同的 MAC address。
> - 使用圖 14.1 中的範例，當 SW1 第一次接收到 PC1 的廣播frame時，它會獲知 PC1 在 G0/2 port 上的 MAC address。
> - 但是，當循環frame A 返回 G0/1 時，它會獲悉 PC1 在該port 上的 MAC address，當循環frame B 傳回 G0/0 時，同樣適用。
> - SW1會在多個port之間不斷更新其MAC位址表中PC1的MAC位址條目，導致PC1無法接收frame； SW1 不知道 PC1 實際連接到哪個port。

A Layer 2 loop can bring down a LAN in a matter of seconds (depending on the amount of BUM traffic), so it's absolutely essential to avoid Layer 2 loops. That's the role of STP.

> [!translation] 逐句繁體中文翻譯
> - Layer 2環路可以在幾秒鐘內關閉 LAN（取決於 BUM traffic），因此避免Layer 2環路是絕對必要的。
> - 這就是 STP 的作用。
