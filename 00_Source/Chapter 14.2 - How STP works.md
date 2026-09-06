---
source: [[Chapter 14 - STP]]
chapter: 14
section: 14.2
tags: [source-section, acting-ccna, stp]
---

# Chapter 14.2 - How STP works

STP can be summarized in one sentence: it prevents Layer 2 loops by blocking redundant connections, leaving only a single active path between any two nodes in a LAN. Figure 14.2 shows an example: the link between SW2 and SW3 is disabled, preventing a Layer 2 loop from occurring.

> [!translation] 逐句繁體中文翻譯
> - STP可以用一句話來概括：它透過阻塞冗餘連接來防止二層環路，在區域network中的任兩個節點之間只留下一條活動路徑。
> - 圖 14.2 顯示了一個範例：SW2 和 SW3 之間的連結被停用，防止發生Layer 2環路。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-267_748_1275_1203_192.jpg)
Figure 14.2 PC1 sends a broadcast frame, and SW1 floods it. Using STP, SW3 blocks its G0/1 port, effectively disabling the SW2-SW3 connection; this prevents a Layer 2 loop from occurring.

Although the physical topology in figure 14.2 is the same as in figure 14.1, thanks to STP there is no longer a Layer 2 loop. SW3's G0/1 port is now in the blocking state; it does not forward frames and does not process received frames (except for STP-related messages). All other ports are in the forwarding state; they can forward and receive frames as normal. The SW2 G0/1 to SW3 G0/1 link is unused but is available to take over if there is a problem on another link.

> [!translation] 逐句繁體中文翻譯
> - 儘管圖 14.2 中的物理拓樸與圖 14.1 中的相同，但由於 STP，不再存在Layer 2環路。
> - SW3 的 G0/1 port現在處於阻塞狀態；它不轉送frame，也不處理接收到的frame（STP 相關訊息除外）。
> - 所有其他port均處於轉送狀態；他們可以正常轉送和接收frame。
> - SW2 G0/1 到 SW3 G0/1 連結未使用，但如果其他連結出現問題，則可以接手。

NOTE The term topology refers to how devices are arranged and connected in a network. In figure 14.2, SW1, SW2, and SW3 are physically connected in a ring topology, forming a circle. The term STP topology can be used to refer to the logical arrangement of switches and their connections as a result of STP-some actively carrying network traffic, and some blocked by STP to prevent Layer 2 loops.

> [!translation] 逐句繁體中文翻譯
> - 註：術語「topology」是指設備在network中的排列和連接方式。
> - 在圖 14.2 中，SW1、SW2 和 SW3 以環形拓樸物理連接，形成一個圓圈。
> - 術語 STP topology可用於指 STP 導致的switch及其連接的邏輯排列 - 有些主動承載network traffic，有些被 STP 阻止以防止Layer 2環路。

Whereas figures 14.1 and 14.2 only showed three switches, figure 14.3 shows a LAN with many more switches connected in a mesh-a network topology in which each node is connected to each other node (full mesh) or as many other nodes as possible but not all (partial mesh). In a network like this, there are countless Layer 2 loops. However, with STP, the switches will automatically put ports in the blocking state to create a loopfree topology. Although network traffic does not pass over the disabled links, they are available to take over if one of the active links fails.

> [!translation] 逐句繁體中文翻譯
> - 圖 14.1 和 14.2 只顯示了三個交換機，而圖 14.3 顯示了一個具有更多交換機的網狀networktopology，其中每個節點都連接到其他節點（全網狀）或盡可能多的其他節點，但不是全部（部分網狀）。
> - 在這樣的network中，存在著無數的Layer 2環路。
> - 但是，使用 STP，交換機會自動將port置於阻塞狀態以建立無環路topology。
> - 雖然network traffic不會通過禁用的鏈接，但如果活動鏈接之一出現故障，它們可以接管。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-268_609_1305_1081_221.jpg)
Figure 14.3 STP creates a loop-free topology in a meshed LAN. In the physical topology (left), there are countless ways that frames could loop around the network. However, STP creates a logical topology (right) that is loop-free.

## What's a spanning tree?

A spanning tree is a concept in the mathematical field of graph theory. In graph theory, a graph is a structure that models relationships between objects (also called nodes). A tree is a subgraph in which any two nodes are connected by exactly one path, and spanning means that the tree includes all nodes; the tree spans across all nodes.

> [!translation] 逐句繁體中文翻譯
> - spanning tree是圖論數學領域的一個概念。
> - 在圖論中，圖是一種對物件（也稱為節點）之間的關係進行建模的結構。
> - 樹是任兩個節點恰好由一條路徑連接的子圖，產生意味著樹包括所有節點；樹跨越所有節點。

```
(continued)
```

Let's compare that to a network using STP. Each switch running STP is a node in the graph, with various physical connections between them (the physical topology in figure 14.3). STP disables some of the connections, leaving only one active path between any two nodes; this is the subgraph-the spanning tree (the logical topology in figure 14.3).

> [!translation] 逐句繁體中文翻譯
> - 讓我們將其與使用 STP 的network進行比較。
> - 每台運行 STP 的switch都是圖中的一個節點，它們之間有各種物理連接（物理topology如圖 14.3 所示）。
> - STP 停用部分連接，在任兩個節點之間僅留下一條活動路徑；這就是子圖－spanning tree（圖14.3中的邏輯拓樸）。
