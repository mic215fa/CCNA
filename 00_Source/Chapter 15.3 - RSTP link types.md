---
source: [[Chapter 15 - RSTP]]
chapter: 15
section: 15.3
tags: [source-section, acting-ccna, rstp, stp]
---

# Chapter 15.3 - RSTP link types

Another aspect of RSTP is the concept of link types. These types influence how ports transition through the RSTP port states and react to network changes. RSTP defines three link types:

> [!translation] 逐句繁體中文翻譯
> - RSTP 的另一個面向是連結類型的概念。
> - 這些類型會影響port如何透過 RSTP port狀態轉換並對network變更做出反應。
> - RSTP 定義了三種連結類型：

- Point-to-point-Full-duplex ports that can use the RSTP sync mechanism to immediately transition to the forwarding state.
- Shared-Half-duplex ports that cannot use the RSTP sync mechanism. They must transition through the states like standard STP ports.
- Edge-Ports connected to end hosts that can use PortFast to immediately transition to the forwarding state.

Figure 15.7 illustrates these three link types on three switches.

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-305_582_1119_183_316.jpg)
Figure 15.7 RSTP differentiates between three link types: point-to-point, shared, and edge. A full-duplex link is considered a point-to-point link. A half-duplex link is considered a shared link. A link between a switch and an end host that uses PortFast is considered an edge link.

An RSTP point-to-point link is a link that operates in full duplex, meaning that the connected ports (i.e. SW1 G0/3 and SW2 G0/0) operate in full duplex. These are the only links that can take advantage of RSTP's sync mechanism to rapidly transition ports to the forwarding state. Although full-duplex ports will automatically use the pointto-point link type, you can also manually configure it with the spanning-tree link -type point-to-point command in interface config mode.

> [!translation] 逐句繁體中文翻譯
> - RSTP 點對點連結是以全雙工方式運作的連結，這表示連接的port（即
> - SW1 G0/3 和 SW2 G0/0）以全雙工方式運作。
> - 這些是唯一可以利用 RSTP 同步機制將port快速轉換到轉送狀態的連結。
> - 雖然全雙工port將自動使用點對點連結類型，但您也可以在interface組態模式中使用 spanning-tree link -type point-to-point 指令手動設定它。

An RSTP shared link is a link that operates in half duplex. In figure 15.7, SW1 and SW3 are connected via a hub, and devices connected to a hub must operate in halfduplex; therefore, the link is an RSTP shared link. Ports connected to a shared link cannot use RSTP's sync mechanism; in effect, they operate like regular STP links, taking 30 seconds to move to the forwarding state. Half-duplex ports will automatically use the shared link type, but if necessary, the spanning-tree link-type shared command can be used to manually configure it.

> [!translation] 逐句繁體中文翻譯
> - RSTP 共享連結是以半雙工方式運作的連結。
> - 在圖 15.7 中，SW1 和 SW3 透過集線器連接，連接到集線器的設備必須以半雙工方式運作；因此，該連結是RSTP共享連結。
> - 連接到共用連結的port無法使用 RSTP 的同步機制；實際上，它們的運作方式與常規 STP 連結類似，需要 30 秒才能進入轉送狀態。
> - 半雙工port會自動使用共用連結類型，但如有必要，可以使用 spanning-tree link-type shared 指令手動設定。

NOTE Like the backup port role, you probably won't encounter the shared link type in a real network.

> [!translation] 逐句繁體中文翻譯
> - 注意 與備份port角色一樣，您可能不會在實際network中遇到共用連結類型。

Finally, there is the edge link type. These are ports that connect to end hosts: SW1 F1/0 and SW2 F1/0. These ports don't need to use the RSTP sync mechanism; there is no risk of a Layer 2 loop, so they can immediately transition to the forwarding state. Does a port connected to an end host immediately transitioning to the forwarding state sound familiar?

> [!translation] 逐句繁體中文翻譯
> - 最後，還有邊緣連結類型。
> - 這些是連接到終端host的port：SW1 F1/0 和 SW2 F1/0。
> - 這些port不需要使用 RSTP 同步機制；不存在二層迴路的風險，因此可以立即過渡到轉送狀態。
> - 連接到終端host的port立即轉換到轉送狀態聽起來很熟悉嗎？

On Cisco switches, the edge link type must be manually configured by enabling PortFast on the appropriate ports (with the spanning-tree portfast command, as we covered in chapter 14)-not the spanning-tree link-type command like point-topoint and shared links. Note that this is the only link type that can't be automatically
determined by the switch based on the port's duplex mode; you must manually configure it.

> [!translation] 逐句繁體中文翻譯
> - 在 Cisco switch 上，邊緣連結類型必須透過在對應port 上啟用 PortFast 來手動設定（使用 spanning-tree portfast 指令，如我們在第 14 章中所介紹的），而不是像點對點和共用連結那樣使用 spanning-tree link-type 指令。
> - 請注意，這是switch無法根據port的雙工模式自動決定的唯一連結類型；您必須手動設定它。

NOTE A port that is connected to an end host but hasn't been configured with the spanning-tree portfast command won't immediately transition to the forwarding state-it will have to wait 30 seconds like a regular STP port.

> [!translation] 逐句繁體中文翻譯
> - 注意 連接到終端host但尚未使用 spanning-tree portfast 命令配置的port不會立即轉換到轉送狀態 - 它必須像常規 STP port一樣等待 30 秒。

One more characteristic of edge ports is that RSTP doesn't consider any activity on them (such as moving to the forwarding or discarding states) a topology change and doesn't notify its neighbors of the activity. Because edge ports connect to end hosts (not switches), and therefore pose no risk of causing Layer 2 loops, there's no need to notify other switches of activity on those ports; they don't affect the rest of the RSTP topology.

> [!translation] 逐句繁體中文翻譯
> - 邊緣port的另一個特徵是，RSTP 不會將其上的任何活動（例如轉至轉送或丟棄狀態）視為topology更改，並且不會將該活動通知其鄰居。
> - 由於邊緣port連接到終端host（而不是switch），因此不存在導致Layer 2迴路的風險，因此無需將這些port 上的活動通知其他switch；它們不會影響 RSTP topology的其餘部分。

## Topology changes

STP and RSTP each have defined processes for reacting to changes in the topology, allowing switches to adapt to the new topology with minimal disruptions; the details of these processes are beyond the scope of the CCNA exam. STP triggers its topology change process in the following situations:

> [!translation] 逐句繁體中文翻譯
> - STP 和 RSTP 各自定義了對topology變化做出反應的流程，允許switch以最小的中斷適應新的topology；這些過程的細節超出了 CCNA 考試的範圍。
> - STP 在下列情況下觸發其topology變更過程：

- When any port transitions to the forwarding state
- When a port in the learning state or forwarding state transitions to the blocking state or disabled state

Switches using RSTP, on the other hand, only trigger the topology change process when a port with a non-edge link type (point-to-point or shared) transitions to the forwarding state; they don't trigger the process when an edge port transitions to the forwarding state or when any port transitions to the discarding state.

> [!translation] 逐句繁體中文翻譯
> - 另一方面，使用RSTP的switch僅在非邊緣連結類型（點對點或共用）的port轉變為轉送狀態時觸發topology改變過程；當邊緣port轉換到轉送狀態或任何port轉換到丟棄狀態時，它們不會觸發該過程。

EXAM TIP The details of STP/RSTP topology changes are beyond the scope of the CCNA exam, but remember this characteristic of RSTP edge ports: they don't trigger the topology change process when moving to the forwarding state.

> [!translation] 逐句繁體中文翻譯
> - 考試提示 STP/RSTP topology變更的詳細資訊超出了 CCNA 考試的範圍，但請記住 RSTP 邊緣port的這一特性：它們在進入轉送狀態時不會觸發topology變更過程。

You can view the type of link of each port with the show spanning-tree command. The final column on the right lists the link types: point-to-point (P2p), shared (Shr), and edge (Edge). In the following example, I use the command on SW1:

> [!translation] 逐句繁體中文翻譯
> - 您可以使用show spanning-tree指令查看每個port的連結類型。
> - 右側最後一列列出了連結類型：點對點 (P2p)、共享 (Shr) 和邊緣 (Edge)。
> - 在以下範例中，我在 SW1 上使用該指令：

```
SW1# show spanning-tree
. . .
Interface Role Sts Cost Prio.Nbr Type
Gio/0
    Root FWD 4 128.1 Shr
Gi0/1
    Altn BLK 4
                                Shr indicates a shared link.
Gi0/3
    Desg FWD 4 128.4 P2p
Fal/0
    Desg FWD 19
P2p Edge indicates a point-to-point edge link
            (a full-duplex link using PortFast).
```

NOTE F1/0's type is listed as P2p Edge. In Cisco IOS, an edge port will appear as P2p Edge if it operates in full duplex or Shr Edge if it operates in half duplex.

> [!translation] 逐句繁體中文翻譯
> - 注意 F1/0 的類型被列為 P2p Edge。
> - 在 Cisco IOS 中，如果邊緣port以全雙工方式運行，則邊緣port將顯示為 P2p Edge；如果以半雙工方式運行，則邊緣port將顯示為 Shr Edge。

In a modern network that uses only switches (not hubs), links between two switches should have the point-to-point type, and links between a switch and an end host should have the edge link type (more specifically, the point-to-point edge).

> [!translation] 逐句繁體中文翻譯
> - 在僅使用switch（而不是集線器）的現代network中，兩個switch之間的link應具有點對點類型，switch和終端host之間的link應具有邊緣link類型（更具體地說，點對點邊緣）。
