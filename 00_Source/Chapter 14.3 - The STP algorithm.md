---
source: [[Chapter 14 - STP]]
chapter: 14
section: 14.3
tags: [source-section, acting-ccna, stp]
---

# Chapter 14.3 - The STP algorithm

The process STP uses to create a loop-free topology is called the STP algorithm. There are three main steps in the algorithm:

> [!translation] 逐句繁體中文翻譯
> - STP 用於建立無環路topology的過程稱為 STP 演算法。
> - 此演算法主要分為三個步驟：

1 Root bridge election
2 Root port selection
3 Designated port selection

Figure 14.4 shows an example of a LAN after STP created a loop-free topology. In this section, we will examine this LAN and go through the STP algorithm step by step. Note that I designed this LAN to demonstrate various aspects of the STP algorithm rather than to represent a realistic LAN topology; we will cover LAN architecture best practices in chapter 15 of volume 2 of this book.

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-269_738_1210_1165_320.jpg)
Figure 14.4 A LAN after STP has created a loop-free topology. SW3 is the root bridge, and each other switch has one root port leading to SW3. The remaining ports are either designated or non-designated ports; non-designated ports are blocked, disabling their connections.

### 14.3.1 Root bridge election

The first step in the STP algorithm is to elect a single switch as the root bridge for the LAN. The root bridge is the central point of reference for the STP topology, and in later steps, all other switches ensure that they have exactly one active path to reach the root bridge.

> [!translation] 逐句繁體中文翻譯
> - STP 演算法的第一步是選擇一台switch作為 LAN 的root bridge。
> - root bridge是 STP topology的中心參考點，在後續步驟中，所有其他switch確保它們恰好有一條活動路徑到達root bridge。

NOTE STP was developed for use with Ethernet bridges, which were predecessors to switches. As a result, STP uses the term bridge rather than switch. Although modern networks use switches instead of bridges, the original terminology (such as root bridge) persists. In the context of STP, bridge and switch can be considered synonymous.

> [!translation] 逐句繁體中文翻譯
> - 注意 STP 是為與乙太network橋接器一起使用而開發的，乙太network橋接器是switch的前身。
> - 因此，STP 使用術語「橋」而不是「switch」。
> - 儘管現代network使用switch而不是網橋，但原始術語（例如root bridge）仍然存在。
> - 在 STP 上下文中，橋接器和switch可以被視為同義詞。

The root bridge election is carried out by switches sharing STP Bridge Protocol Data Unit (BPDU) messages with each other. Actually, the information shared in BPDUs is used to make all of the decisions in the STP algorithm, not just the root bridge election. BPDUs are sent every 2 seconds and contain various pieces of STP-related information; the two pieces of information relevant to the root bridge election are the switch's own bridge identifier (BID)-a number that uniquely identifies the switch in the LAN-and the BID of the switch it believes to be the root bridge.

> [!translation] 逐句繁體中文翻譯
> - root bridge選舉是透過switch之間共用 STP BPDU (BPDU) 訊息來執行的。
> - 實際上，BPDU 中共享的資訊用於做出 STP 演算法中的所有決策，而不僅僅是root bridge選舉。
> - BPDU 每 2 秒發送一次，包含各種 STP 相關資訊；與root bridge選舉相關的兩個資訊是switch自己的橋樑識別碼（BID）（在 LAN 中唯一識別switch的數字）和它認為是root bridge的switch的 BID。

When a switch first boots up, it does not yet know the root bridge of the LAN, so it declares itself to be the root bridge. Figure 14.5 demonstrates this: the four switches have all booted up at the same time, and each switch sends BPDUs declaring itself to be the root bridge (the My BID and Root BID fields match).

> [!translation] 逐句繁體中文翻譯
> - 當switch第一次啟動時，它還不知道 LAN 的root bridge，因此它聲明自己是root bridge。
> - 圖 14.5 展示了這一點：四台switch同時啟動，每台switch都發送 BPDU 聲明自己是root bridge（My BID 和 Root BID 字段匹配）。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-270_759_1326_1233_225.jpg)
Figure 14.5 SW1, SW2, SW3, and SW4 boot up simultaneously, each switch declaring itself the root bridge. The switches send BPDUs out of their ports, containing information such as the switch's own BID and the BID of the switch it believes to be the root bridge (itself, in this case).

## The BID

The switch that sends the superior BPDU will be elected the root bridge of the LAN. The superior BPDU is the BPDU that has superior parameters according to the STP algorithm. When it comes to electing the root bridge, that means the BPDU with the numerically lowest My BID field. Before we determine which of the four switches has the lowest BID, let's examine the structure of the BID, as shown in figure 14.6. The BID is a 64-bit number that consists of a 16-bit bridge priority and a 48-bit MAC address.

> [!translation] 逐句繁體中文翻譯
> - 發送上級BPDU的switch將被選舉為LAN的root bridge。
> - 上級 BPDU 是根據 STP 演算法具有更優參數的 BPDU。
> - 當涉及選擇root bridge時，這意味著 My BID 欄位數字最低的 BPDU。
> - 在確定四個switch中哪一個switch的 BID 最低之前，我們先檢查一下 BID 的結構，如圖 14.6 所示。
> - BID 是一個 64 位數字，由 16 位橋接器優先權和 48 位 MAC address組成。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-271_411_1414_533_192.jpg)
Figure 14.6 The contents of the STP BID. It is divided into two parts: a 16-bit bridge priority and a 48-bit MAC address. The bridge priority consists of two further parts: a configurable priority value (default 32768) and the Extended System ID, which is equal to the VLAN ID in Cisco's implementation of STP.

The bridge priority itself consists of two parts, the first being a configurable priority value. By default, the most significant bit is set to 1, which is equivalent to 0d32768. The second part is called the Extended System ID and is equal to the VLAN ID; these two numbers are added together to create the bridge priority (e.g., $32,768+1=32,769$ ). Before we continue with the root bridge election, let's dig deeper into the Extended System ID.

> [!translation] 逐句繁體中文翻譯
> - 橋接器優先權本身由兩部分組成，第一部分是可設定的優先權值。
> - 預設情況下，最高有效位元設定為 1，相當於 0d32768。
> - 第二部分稱為擴充系統 ID，等於 VLAN ID；這兩個數位相加即可建立橋接優先權（例如 $32,768+1=32,769$ ）。
> - 在繼續進行root bridge選舉之前，讓我們更深入地了解擴展系統 ID。

Cisco switches run a proprietary version of STP called Per-VLAN Spanning Tree Plus (PVST+). In PVST+, switches run a separate STP instance for each VLAN; they create a separate spanning tree for each VLAN. The benefit is that different links can be disabled in different VLANs, resulting in balanced traffic over all links.

> [!translation] 逐句繁體中文翻譯
> - Cisco switch運行 STP 的專有版本，稱為 Per-VLAN Spanning Tree Plus (PVST+)。
> - 在 PVST+ 中，switch為每個 VLAN 執行單獨的 STP 實例；他們為每個 VLAN 建立一個單獨的spanning tree。
> - 好處是可以在不同的 VLAN 中停用不同的link，從而實現所有link上的traffic平衡。

NOTE Before PVST+, there was PVST, which only supported ISL encapsulation over trunk links. PVST+ supports both ISL and 802.1Q, and modern Cisco switches all run PVST+, not PVST.

> [!translation] 逐句繁體中文翻譯
> - 說明 在PVST+之前，有PVST，僅支援中繼link上的ISL 封裝。
> - PVST+ 支援 ISL 和 802.1Q，現代 Cisco switch都運行 PVST+，而不是 PVST。

If all VLANs share the same STP instance, blocked links go completely unused until an active link fails, which can lead to congestion on the active links. Figure 14.7 shows how a separate spanning tree can be made for each VLAN. The LAN has two VLANs (VLAN 1 and VLAN 2), and the switches have disabled different links in each VLAN.

> [!translation] 逐句繁體中文翻譯
> - 如果所有 VLAN 共用相同的 STP 實例，則被封鎖的連結將完全不被使用，直到活動連結發生故障，這可能會導致活動連結發生擁塞。
> - 圖 14.7 顯示如何為每個 VLAN 建立單獨的spanning tree。
> - LAN 有兩個 VLAN（VLAN 1 和 VLAN 2），且switch已停用每個 VLAN 中的不同連結。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-272_449_891_183_369.jpg)
Figure 14.7 Switches in a LAN create separate spanning trees for VLANs 1 and 2 by disabling different links in each VLAN (as indicated by the dotted lines). Traffic in VLAN 1 will use different links than traffic in VLAN 2, avoiding network congestion.

Because the VLAN ID becomes part of the BID, the switch will have a unique bridge priority for each STP instance (for each VLAN running STP). For example, with the default priority of 32768, the total bridge priority will be 32769 ( 32,768 + 1) in VLAN 1 and 32770 (32,768 + 2) in VLAN 2.

> [!translation] 逐句繁體中文翻譯
> - 由於 VLAN ID 成為 BID 的一部分，因此switch將為每個 STP 執行個體（對於每個執行 STP 的 VLAN）擁有唯一的網橋優先權。
> - 例如，預設優先權為 32768，則 VLAN 1 中的總網橋優先權將為 32769 (32,768 + 1)，VLAN 2 中的總網橋優先權將為 32770 (32,768 + 2)。

NOTE For the rest of this chapter, we will focus on a single-VLAN topology. I would not expect any questions about creating a unique spanning tree for each VLAN on the CCNA exam.

> [!translation] 逐句繁體中文翻譯
> - 注意 在本章的其餘部分中，我們將重點討論單一 VLAN topology。
> - 我不希望在 CCNA 考試中出現任何有關為每個 VLAN 建立唯一spanning tree的問題。

## Why include the VLAN ID in the bridge priority?

The STP standard (IEEE 802.1D) specifies that each switch must have a unique BID. This is achieved by combining the bridge priority with the switch's MAC address. Even if all switches in the LAN have the same bridge priority, MAC addresses are unique, so the result is a unique BID for each switch.

> [!translation] 逐句繁體中文翻譯
> - STP 標準 (IEEE 802.1D) 指定每個switch必須有唯一的 BID。
> - 這是透過將網橋優先權與switch的 MAC address結合來實現的。
> - 即使 LAN 中的所有switch都具有相同的橋接器優先權，MAC address也是唯一的，因此結果是每個switch的唯一 BID。

However, Cisco switches running PVST+ run a separate STP instance for each VLAN. As we covered in chapter 12, each VLAN is like a separate virtual switch, so to comply with the standard, each STP instance running on the switch must have a unique BID. That's the role of the Extended System ID, which is set to the VLAN ID of the STP instance. By adding the VLAN ID to the priority value, each STP instance will have a unique bridge priority and, therefore, a unique BID.

> [!translation] 逐句繁體中文翻譯
> - 但是，執行 PVST+ 的 Cisco switch為每個 VLAN 執行單獨的 STP 實例。
> - 正如我們在第 12 章中介紹的，每個 VLAN 就像一個單獨的虛擬交換機，因此為了符合標準，switch 上執行的每個 STP 實例必須有一個唯一的 BID。
> - 這就是擴充系統 ID 的作用，它設定為 STP 實例的 VLAN ID。
> - 透過將 VLAN ID 新增至優先權值，每個 STP 實例將具有唯一的橋接器優先權，因此也具有唯一的 BID。

For example, if a switch running two STP instances (VLAN 1 and VLAN 2) has the default priority value of 32768 and a MAC address 5254.000f.adab, the resulting BID would be 32769:5254.000f.adab for VLAN 1 and 32770:5254.000f.adab for VLAN 2. Note that the bridge priority is written in decimal, whereas the MAC address is written in hexadecimal (as usual), and the two are often separated by a colon, as in 32769:5254.000f. adab.

> [!translation] 逐句繁體中文翻譯
> - 例如，某 switch 執行 VLAN 1 與 VLAN 2 兩個 STP instances，預設 priority value 為 32768，MAC address 為 5254.000f.adab；其 BID 在 VLAN 1 為 32769:5254.000f.adab，在 VLAN 2 為 32770:5254.000f.adab。
> - Bridge priority 使用十進位表示，MAC address 則照常使用十六進位表示，兩者通常以冒號分隔，例如 32769:5254.000f.adab。

## Comparing BIDs

Now that we've covered the bridge priority (priority + VLAN ID), what is the MAC address that forms the second part of the BID? It's not the MAC of any of the switch's ports; rather, it's a separate MAC address that identifies the switch as a whole. In this section, we'll compare BIDs and see how the MAC address is used as a tiebreaker. The following are the BIDs of the four switches we saw in figure 14.5:

> [!translation] 逐句繁體中文翻譯
> - 現在我們已經介紹了網橋優先權（優先權 + VLAN ID），那麼構成 BID 第二部分的 MAC address是什麼？
> - 它不是任何switchport的 MAC；相反，它是一個單獨的 MAC address，用於識別整個switch。
> - 在本節中，我們將比較 BID 並了解如何將 MAC address用作決勝局。
> - 以下是我們在圖 14.5 中看到的四個switch的 BID：

- SW1: 32769:5254.000f.adab
- SW2: 32769:5254.0013.cf9a
- SW3: 32769:5254.0016.5d5e
- SW4: 32769:5254.001d.d23a

Which of these BIDs is numerically lower and, therefore, superior? To compare them, first compare the bridge priorities. In this case, all four switches have the same bridge priority of 32769, so we must compare the MAC addresses to break the tie.

> [!translation] 逐句繁體中文翻譯
> - 這些 BID 中哪一個在數值上更低，因此更優越？
> - 要比較它們，首先比較網橋優先權。
> - 在這種情況下，所有四台switch都具有相同的網橋優先權 32769，因此我們必須比較 MAC address以打破平局。

NOTE Although we divide the BID into multiple parts, remember that it is just a 64-bit number. The bits written on the left (those that make up the bridge priority) are the most significant, which is why you should compare them first when determining which BID is numerically lower.

> [!translation] 逐句繁體中文翻譯
> - 注意 雖然我們將 BID 分為多個部分，但請記住它只是一個 64 位數。
> - 寫在左側的位元（構成網橋優先權的位元）是最重要的，這就是為什麼在確定哪個 BID 數值較低時應先比較它們。

When comparing MAC addresses, remember that they are just numbers written in a hexadecimal format, and comparing them is the same process you go through when comparing decimal numbers. For example, when comparing the decimal numbers 1999 and 9111, how do you know the second number is greater when it has only one 9, whereas the first number has three? The reason is the 9 in 9111 is the most significant digit; the single 9 in 9111 has a greater value $(9,000)$ than all of the other digits in 1999 combined. Just by seeing that the most significant digit of 9111 is greater than that of 1999, you can declare that 9111 is the greater of the two-no need to compare the other three digits.

> [!translation] 逐句繁體中文翻譯
> - 在比較 MAC address時，請記住它們只是以十六進位格式編寫的數字，比較它們的過程與比較十進位數字時經歷的過程相同。
> - 例如，當比較十進位數 1999 和 9111 時，如何知道第二個數字只有一個 9，而第一個數字有 3 個？
> - 原因是 9111 中的 9 是最高有效數字； 9111 中的單一 9 的價值 $(9,000)$ 比 1999 年所有其他數字的總和還要大。
> - 只要看到 9111 的最高有效數字大於 1999，您就可以聲明 9111 是兩者中較大的一個 - 無需比較其他三位數字。

The same applies when finding the greater (or lesser, in this case) of two or more MAC addresses: compare the most significant digits first. The first six digits of all four MAC addresses (the OUI) are the same: 5254.00. Then, the following digit is 1 for SW2, SW3, and SW4, but 0 for SW1, and therefore SW1 has the lowest BID of the four-it is the root bridge! We can confirm this with the show spanning-tree command on SW1, as in the following example:

> [!translation] 逐句繁體中文翻譯
> - 當尋找兩個或多個 MAC address中較大（或較小，在本例中）的 MAC address時，同樣適用：首先比較最高有效數字。
> - 所有四個 MAC address (OUI) 的前六位數相同：5254.00。
> - 那麼，接下來的數字對於 SW2、SW3 和 SW4 來說是 1，但是對於 SW1 來說是 0，因此 SW1 具有四個中最低的 BID - 它是root bridge！
> - 我們可以使用 SW1 上的 show spanning-tree 指令來確認這一點，如下例所示：
![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-273_297_1210_1785_316.jpg)

```
This switch’s priority (same as
previously because it is the root)
Bridge ID Priority 32769 (priority 32768 sys-id-ext 1)
Address 5254.000f.adab
```

This switch's MAC address (same as previously because it is the root)

## Configuring the bridge priority

As we just confirmed, SW1 is the root bridge for the LAN because it has the lowest MAC address. However, it is possible to configure the bridge priority to change which switch becomes the root bridge. This is often desirable because of the role of the root bridge; it serves as the central reference point for the spanning tree, and the other switches will ensure that their most efficient path to reach the root bridge is enabled. If a switch is connected to the router that end hosts use to access external networks, it's a good choice to be the root bridge; there should be an efficient path to reach the router without frames having to pass through too many switches.

> [!translation] 逐句繁體中文翻譯
> - 正如我們剛剛確認的，SW1 是 LAN 的root bridge，因為它具有最低的 MAC address。
> - 但是，可以設定網橋優先權以變更哪個switch成為根網橋。
> - 由於root bridge的作用，這通常是可取的；它充當spanning tree的中心參考點，其他交換機將確保啟用到達root bridge的最有效路徑。
> - 如果switch連接到終端host用來存取外部network的router，那麼作為root bridge是一個不錯的選擇；應該有一條有效的路徑到達router，而frame不必經過太多switch。

Following the example we saw in figure 14.4, let's configure SW3 as the root bridge and lower SW4's priority so it functions as a secondary root bridge-the bridge that will take over as the root if the root bridge malfunctions (because it has the lowest BID of the remaining switches). The command to configure a switch's root priority is spanning -tree vlan vlan-id priority priority-value. In the following example, I attempt to set SW3's priority to 20000, but an error message is displayed:

> [!translation] 逐句繁體中文翻譯
> - 按照我們在圖 14.4 中看到的範例，讓我們將 SW3 配置為root bridge並降低 SW4 的優先級，以便它充當輔助root bridge - 如果root bridge發生故障，該橋將作為root bridge接替（因為它具有其餘交換機中最低的 BID）。
> - 設定switch根優先權的指令是 spanning -tree vlan vlan-idprioritypriority-value。
> - 在以下範例中，我嘗試將 SW3 的優先權設為 20000，但顯示錯誤訊息：
![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-274_242_1205_1229_347.jpg)
As the error message states, the priority can only be configured in increments of 4096. The reason is that although the bridge priority field as a whole is 16 bits in length, only the four most significant bits make up the configurable priority value: the bits with values of 0d32768, 0d16384, 0d8192, and 0d4096. The lesser 12 bits are fixed as the VLAN ID (VLAN 1 in our examples here). That's why the bridge priority must be configured in increments of 4096: it's the value of the least significant bit that we can change. In the following example, I configure SW3's priority as 24576 and SW4's priority as 28672 and then confirm with show spanning-tree on SW4:

> [!translation] 逐句繁體中文翻譯
> - 如錯誤訊息所示，priority 只能以 4096 為增量進行設定。
> - 原因是 bridge priority 欄位雖然總長 16 bits，但只有最高的 4 bits 構成可設定的 priority value，也就是值為 0d32768、0d16384、0d8192 與 0d4096 的 bits。
> - 較低的 12 bits 固定用作 VLAN ID；此處範例為 VLAN 1。
> - 因此 bridge priority 必須以 4096 為增量設定，因為 4096 是可變更之最低有效 bit 的值。
> - 在以下範例中，我將 SW3 的 priority 設為 24576、SW4 的 priority 設為 28672，然後在 SW4 使用 `show spanning-tree` 確認：

```
SW3(config)# spanning-tree vlan 1 priority 24576
SW4(config)# spanning-tree vlan 1 priority 28672
SW4(config)# do show spanning-tree
VLAN0001
    Spanning tree enabled protocol ieee The priority configured on
    Root ID Priority 24577 ← \
```

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-275_266_1054_179_533.jpg)
Figure 14.8 After configuring SW3's priority to 24576 (+1 for VLAN 1), all four switches agree that SW3 is the root bridge because it has the lowest BID. All ports on the root bridge are designated ports (indicated by D). If SW3 malfunctions and a new election is held, SW4 will become the new root bridge because it has the second-lowest BID.

Figure 14.8 shows the result after configuring the bridge priorities of SW3 and SW4. All four switches agree that SW3 is the root bridge. SW4 has a lower BID than SW1 and SW2, but it is not the root bridge yet; that would only happen if SW3 malfunctions and a new root bridge election is held.

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-275_818_1414_712_192.jpg)
Figure 14.8 After configuring SW3's priority to 24576 (+1 for VLAN 1), all four switches agree that SW3 is the root bridge because it has the lowest BID. All ports on the root bridge are designated ports (indicated by D). If SW3 malfunctions and a new election is held, SW4 will become the new root bridge because it has the second-lowest BID.

NOTE All ports on the root bridge are designated ports, meaning they are in the forwarding state (not the blocking state). We will examine designated ports further in section 14.3.3.

> [!translation] 逐句繁體中文翻譯
> - 說明 root bridge上的所有端口都是designated port，即處於轉發狀態（而不是阻塞狀態）。
> - 我們將在第 14.3.3 節中進一步檢查指定port。

There is one more method to configure the bridge priority that you should know for the CCNA exam: the spanning-tree vlan vlan-id root \{primary | secondary\} command. The secondary keyword is simple: it sets the priority to 28672 (one
increment of 4096 under the default of 32768). The primary keyword, on the other hand, works like this:

> [!translation] 逐句繁體中文翻譯
> - 還有一種在 CCNA 考試中您應該了解的設定橋接器優先權的方法：spanning tree vlan vlan-id root \{primary | }輔助\}指令。
> - 第二個關鍵字很簡單：它將優先權設為 28672（在預設值 32768 下增加 4096）。另一方面，
> - 主關鍵字的工作方式如下：

- Set the priority to 24576 (two increments of 4096 under the default).
- Or, if 24576 isn't sufficient to make the switch become the root bridge (i.e., the current root bridge's priority is 24576), set the priority to the highest multiple of 4096 that will make the switch the root bridge.

These commands will serve their purpose if all other switches in the LAN have the default priority of 32768; the switch configured with the primary keyword will be the root bridge, and the switch configured with the secondary keyword will be next in line if the root bridge fails.

> [!translation] 逐句繁體中文翻譯
> - 如果 LAN 中的所有其他switch都具有預設優先級 32768，則這些命令將發揮作用；配置 Primary 關鍵字的switch將成為root bridge，如果root bridge發生故障，則配置有 secondary 關鍵字的switch將成為下一個。

However, using these commands is not recommended. There are a couple of reasons: first, there's no guarantee that configuring this command with the secondary keyword will make the switch the next in line to become the root bridge if the current root bridge fails; another non-root switch could have a priority value lower than 28672. Likewise, there are situations where this command with the primary keyword will fail: it cannot set the switch's priority to 0 . The following example shows what happens when the current root bridge (SW1) has a priority of 4096, and you use this command with the primary keyword on another switch (SW2):

> [!translation] 逐句繁體中文翻譯
> - 但是，不建議使用這些命令。
> - 有幾個原因：首先，如果目前root bridge發生故障，不能保證使用 secondary 關鍵字配置此指令將使switch成為下一個佇列中的root bridge；另一個非根switch的優先權值可能低於 28672。
> - 同樣，在某些情況下，帶有 Primary 關鍵字的命令會失敗：它無法將switch的優先權設為 0。
> - 以下範例顯示噹噹前根網橋 (SW1) 的優先權為 4096，並且您在另一個switch (SW2) 上將此指令與 Primary 關鍵字一起使用時會發生什麼情況：

```
Sets SW1's
Sets SW1's
priority to 4096
priority to 4096
The command
The command
fails on SW2.
fails on SW2.
% Failed to make the bridge root tor van 1
% It may be possible to make the bridge root by setting the priority
% for some (or all) of these instances to zero.
```

EXAM TIP The best way to ensure that a switch will be the root bridge is to use the spanning-tree vlan vlan-id priority 0 command. Then, the only way to usurp the root is to use the same command on another switch that has a lower MAC address (and, therefore, a lower BID). Remember this point for the exam!

> [!translation] 逐句繁體中文翻譯
> - 檢查提示 確保switch成為root bridge的最佳方法是使用 spanning-tree vlan vlan-idpriority 0 指令。
> - 然後，篡奪根的唯一方法是在另一台具有較低 MAC address（因此 BID 較低）的switch 上使用相同的命令。
> - 考試時記住這一點！

### 14.3.2 Root port selection

After electing the root bridge, each non-root switch will select one of its ports as its root port-the port with the best path to the root bridge. The switch calculates this based on information in the BPDUs it receives from its neighbors. The root port is selected using a few parameters: the root cost (which measures the port's proximity to the root bridge), the neighbor's BID, and the neighbor's port ID, in that order of priority:

> [!translation] 逐句繁體中文翻譯
> - 選出root bridge後，每台非根switch都會選擇自己的一個端口作為自己的root port——到達root bridge的最佳路徑的端口。
> - switch根據從鄰居接收到的 BPDU 中的資訊來計算此值。
> - 使用幾個參數來選擇根port：根成本（衡量port與根網橋的接近程度）、鄰居的 BID 和鄰居的port ID，按優先順序排列：

1 Lowest root cost
2 Lowest neighbor BID
3 Lowest neighbor port ID

A port's root cost is a value that indicates how efficient the path to the root bridge is via that port; a lower value is better. Each port has a given cost value associated with it, as shown in table 14.1.

> [!translation] 逐句繁體中文翻譯
> - port的根成本是一個值，表示通過該port到root bridge的路徑的效率；值越低越好。
> - 每個port都有一個與其關聯的給定成本值，如表 14.1 所示。

Table 14.1 STP port cost values
| Speed | Cost |
| :--- | :--- |
| 10 Mbps | 100 |
| 100 Mbps | 19 |
| 1 Gbps | 4 |
| 10 Gbps | 2 |


A port's root cost is the total cost of the ports leading toward the root bridge (not just the cost of the individual port), and the port with the lowest root cost will become the root port. If there are multiple ports on the switch with the same root cost, the port connected to the neighbor with the lowest BID will become the root port. If two or more ports have the same root cost and are connected to the same neighbor, the port connected to the port on the neighbor switch with the lowest port ID will become the root port. Figure 14.9 shows which port each non-root switch selects as its root port and how it comes to that decision. In the rest of this section, we will go through each switch's decision step by step.

> [!translation] 逐句繁體中文翻譯
> - port的根成本是通往root bridge的port的總成本（而不僅僅是單一port的成本），根成本最低的port將成為根port。
> - 如果switch 上有多個具有相同根成本的端口，則連接到具有最低 BID 的鄰居的端口將成為root port。
> - 如果兩個或多個port具有相同的根成本並且連接到同一鄰居，則連接到鄰居switch 上具有最低port ID 的port的port將成為根port。
> - 圖 14.9 顯示了每個非根switch選擇哪個port作為其根port以及如何做出該決定。
> - 在本節的其餘部分中，我們將逐步了解每個switch的決定。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-277_776_1414_1153_192.jpg)
Figure 14.9 Non-root switches each select one root port. SW4 selects G0/0 because it has the lowest root cost of its ports. SW2 GO/0 and G0/1 have the same root cost, so SW2 selects G0/1 because it has the lowest neighbor BID. SW1 G0/0 and G0/1 have the same root cost and neighbor BID, so SW1 selects G0/1 because it has the lowest neighbor port ID.

## Lowest ROOT COST

As I've mentioned a couple of times already, the decisions that make up the STP algorithm are all based on information in the BPDUs passed among the switches. Once the root bridge has been decided, it is the only switch that generates new BPDUs; other switches receive those BPDUs and forward them to their neighbors, updating some information in the BPDUs. One of the pieces of information in a BPDU is the root cost. The BPDUs sent by the root bridge all have a cost of 0 (the root bridge's cost to reach itself is 0). When non-root switches forward those BPDUs, they add the cost of the port they received the BPDUs on.

> [!translation] 逐句繁體中文翻譯
> - 正如我已經多次提到的，構成 STP 演算法的決策全部基於switch之間傳遞的 BPDU 中的資訊。
> - 一旦確定了root bridge，它就是唯一產生新 BPDU 的switch；其他switch接收這些 BPDU 並將其轉送給其鄰居，更新 BPDU 中的一些資訊。
> - BPDU 中的資訊之一是根成本。
> - root bridge發送的 BPDU 的成本均為 0（root bridge到達自身的成本為 0）。
> - 當非根switch轉送這些 BPDU 時，它們會增加接收 BPDU 的port的成本。

Figure 14.10 demonstrates how switches advertise their root cost to each other and each switch's logic in selecting its root port. Only SW4 is able to do so at this step. SW3 (the root bridge) sends BPDUs with a root cost of 0 . When SW1 and SW4 forward those BPDUs, they add the cost of the ports on which they received those BPDUs; in this case, all ports are GigabitEthernet ports, so they have a cost of 4 . When SW2 forwards the BPDUs it receives from SW1 and SW4, it adds its own ports' cost (4) to the cost of the BPDUs it received (4); it advertises a cost of 8.

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-278_847_1416_938_223.jpg)
Figure 14.10 Switches advertise their root cost to each other in BPDUs. SW3 (the root bridge) advertises a root cost of 0, SW1 and SW4 advertise a root cost of 4, and SW2 advertises a root cost of 8. SW4 selects G0/0 as its root port because it has the lowest root cost of its three ports. SW1 and SW2 are unable to select a root port based on root cost alone; a tiebreaker is needed.

Although SW4 is able to determine its root port based only on root cost, SW1 and SW2 cannot; SW1 has a root cost of 4 via both its G0/0 and G0/1 ports, and SW2 has a root
cost of 8 via both its G0/0 and G0/1 ports. For SW1 and SW2 to select their root ports, they must proceed to the next step in the selection process: the lowest neighbor BID.

> [!translation] 逐句繁體中文翻譯
> - 雖然SW4能夠僅根據根成本確定其root port，但是SW1和SW2不能； SW1 透過其 G0/0 和 G0/1 端口的根成本均為 4，SW2 透過其 G0/0 和 G0/1 端口的根成本均為 8。
> - 為了讓 SW1 和 SW2 選擇其root port，它們必須繼續進行選擇過程的下一步：最低的鄰居 BID。

## Lowest neighbor bridge ID

When a switch sends BPDUs, one of the pieces of information it includes is its own BID. This can then be used by the receiving switch as a tiebreaker when deciding its root port. The port connected to the neighbor with the lowest BID will become the switch's root port. Figure 14.11 shows how SW1 and SW2 compare their neighbors' BIDs to decide their root ports; SW2 is able to select G0/1, but SW1 is not yet able to select a root port.

> [!translation] 逐句繁體中文翻譯
> - 當switch發送 BPDU 時，它包含的資訊之一是它自己的 BID。
> - 接收switch在決定其根埠時可以將其用作決勝局。
> - 連接到具有最低 BID 的鄰居的port將成為switch的根port。
> - 圖 14.11 顯示了 SW1 和 SW2 如何比較其鄰居的 BID 來決定其root port； SW2 能夠選擇 G0/1，但 SW1 尚無法選擇root port。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-279_784_1355_639_194.jpg)
Figure 14.11 SW1 compares the neighbor BID of its G0/0 and G0/1 ports, and SW2 compares the neighbor BID of its G0/0 and G0/1 ports. SW2 G0/1's neighbor (SW4) has a lower BID than SW2 G0/0's neighbor (SW1), so SW2 selects G0/1 as its root port. SW1's G0/0 and G0/1 ports are both connected to SW3, so they both have the same neighbor BID; SW1 is unable to select a root port at this point.

NOTE The port connected to another switch's root port must be a designated port (forwarding). The root port provides the switch's single path to the root bridge, so its neighbor must not block the link.

> [!translation] 逐句繁體中文翻譯
> - 說明 連接到另一台switch根port的port必須是指定port（轉送）。
> - 根port提供switch到根網橋的單一路徑，因此其鄰居不得阻斷此連結。

## Lowest neighbor port ID

Another piece of information included in an STP BPDU is the port ID of the port that sent the BPDU. This is used as the final tiebreaker when selecting the root port. It's worth emphasizing that when using the port ID as a tiebreaker, it's the neighbor's port IDs that count-not the local switch's port ID. When deciding SW1's root port, we have to compare the port IDs of SW3's ports that are connected to SW1.

> [!translation] 逐句繁體中文翻譯
> - STP BPDU 中包含的另一個資訊是傳送 BPDU 的port的port ID。
> - 這在選擇根port時用作最終的決勝局。
> - 值得強調的是，當使用port ID 作為決勝局時，計算的是鄰居的port ID，而不是本地switch的port ID。
> - 在決定 SW1 的根port時，我們必須比較與 SW1 連接的 SW3 port的port ID。

The port ID is a unique identifier for each port of the switch; like the BID, it consists of a configurable priority value (128 by default) and a sequential number (1 for the first port, 2 for the second port, etc). In the following example, I use show spanning-tree on SW3 to check the IDs of its G0/0 and G0/1 ports (in the Prio.Nbr column):

> [!translation] 逐句繁體中文翻譯
> - 端口ID是switch每個端口的唯一識別碼；與 BID 一樣，它由可配置的優先權值（預設為 128）和序號（1 表示第一個端口，2 表示第二個端口，等等）組成。
> - 在以下範例中，我在 SW3 上使用 show spanning-tree 檢查其 G0/0 和 G0/1 port的 ID（在 Prio.Nbr 列中）：

```
SW3# show spanning-tree
. . .
Interface Role Sts Cost Prio.Nbr Type
Gi0/0
Gi0/1
Gi0/2
```

| Desg FWD 4 | 128.1 | P2p | ![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-280_23_25_521_1217.jpg) |  |
| :--- | :--- | :--- | :--- | :--- |
| Desg FWD 4 | 128.2 | P2p |  |  |
| Desg FWD 4 | 128.3 | P2p |  |  |
| SW3 GO/1's port ID is 128.2. |  |  |  |  |

NOTE To influence root port selection, you can configure a port's priority value (the first part of the port ID) with the spanning-tree vlan vlan-id port-priority priority-value in interface config mode. However, I would not expect any questions about this on the CCNA exam. Generally, you can just compare the ports' names to decide which has a lower port ID. G0/0 is lower than $\mathrm{G} 0 / 1$ ( 0 is lower than 1 ), so it has a lower port ID.

> [!translation] 逐句繁體中文翻譯
> - 注意 若要影響根port選擇，您可以在interface設定模式下使用 spanning-tree vlan vlan-id port-prioritypriority-value 設定port的優先權值（port ID 的第一部分）。
> - 但是，我預期 CCNA 考試中不會出現任何與此相關的問題。
> - 通常，您可以透過比較port名稱來確定哪個port ID 較低。
> - G0/0 低於 $\mathrm{G} 0 / 1$ （ 0 低於 1 ），因此它具有較低的port ID。

Figure 14.12 shows how SW1 selects its root port. SW1 G0/0 is connected to SW3 G0/1 (port ID 128.2), and SW1 G0/1 is connected to SW3 G0/0 (port ID 128.1). Because the neighbor port ID of G0/1 is lower, SW1 selects G0/1 as its root port.

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-280_683_1019_1205_354.jpg)
Figure 14.12 SW1 compares the neighbor port IDs of its GO/0 and G0/1 ports. G0/1 is connected to a lower port ID (SW3 GO/0, port ID 128.1) than G0/0 (SW3 G0/1, port ID 128.2), so G0/1 selects G0/1 as its root port.

EXAM TIP Remember that you're comparing the neighbor's port IDs, not the local switch's port IDs; that's a potential trick question on the exam!

> [!translation] 逐句繁體中文翻譯
> - 檢查提示 請記住，您正在比較鄰居的port ID，而不是本機switch的port ID；這是考試中潛在的技巧問題！

### 14.3.3 Designated port selection

Now that each non-root switch has selected a root port, the final step is to select designated ports. Whereas a root port is a port in the forwarding state that points toward the root bridge, a designated port is a port in the forwarding state that points away from the root bridge. That's why every port on the root bridge is a designated port-they all point away from the root bridge.

> [!translation] 逐句繁體中文翻譯
> - 現在每台非根switch都已經選擇了root port，最後一步是選擇designated port。
> - root port是指指向根網橋的轉送狀態的端口，而designated port是指遠離根網橋的轉送狀態的端口。
> - 這就是為什麼root bridge上的每個port都是指定port－它們都指向遠離root bridge的位置。

There must be exactly one designated port for each segment in the LAN. The exact meaning of the term segment can vary, but in this case, a segment is a link between switches. Designated ports are selected using the following parameters (in order of priority):

> [!translation] 逐句繁體中文翻譯
> - LAN 中的每個網段都必須有一個指定port。
> - 術語「段」的確切含義可能有所不同，但在本例中，「段」是switch之間的連結。
> - 使用下列參數選擇指定port（按優先順序）：

1 The port on the switch with the lowest root cost becomes designated.
2 The port on the switch with the lowest BID becomes designated.

## What is a segment?

A segment is a division of a network, the extent of which depends on the context. A Layer 1 segment can be defined as an electrical connection between devices and is equivalent to a collision domain; this is the meaning of segment as used in this chapter. Two connected switches are another example of a Layer-1 segment. Another example is a group of devices connected to an Ethernet hub; an electrical signal sent by one device is received by all other devices connected to the hub.

> [!translation] 逐句繁體中文翻譯
> - 網段是network的一個部分，其範圍取決於上下文。
> - 第 1 層段可以定義為設備之間的電氣連接，相當於衝突域；這就是本章中使用的段落的含義。
> - 兩個連接的switch是第 1 層網段的另一個範例。
> - 另一個範例是連接到乙太network集線器的一組裝置；一個裝置發送的電訊號被連接到集線器的所有其他裝置接收。

A Layer 2 segment is equivalent to a LAN or broadcast domain-a group of devices that can send frames directly to each other. If a physical LAN is divided into multiple VLANs, each VLAN is its own Layer 2 segment. A Layer 3 segment is equivalent to a subnet. As I mentioned in chapter 12 (VLANs), Layer 2 and Layer 3 segments usually have a oneto-one relationship (one subnet per VLAN), but it is possible for a single VLAN to include multiple subnets.

> [!translation] 逐句繁體中文翻譯
> - Layer 2網段相當於 LAN 或廣播域 - 一組可以直接互相傳送frame的裝置。
> - 如果一個實體 LAN 被分割為多個 VLAN，則每個 VLAN 都是其自己的Layer 2網段。
> - Layer 3網段相當於子network。
> - 正如我在第 12 章（VLAN）中提到的，Layer 2和Layer 3網段通常具有一對一的關係（每個 VLAN 一個子network），但單一 VLAN 可能包含多個子network。

In electing the root bridge and selecting a root port for each switch, we were already able to identify some designated ports in the LAN: all ports on the root bridge are designated, and all ports connected to a root port are designated. For each remaining segment, there must be one designated port, and the other ports must be non-designated. Non-designated ports are in the blocking state; this is how STP prevents loops.

> [!translation] 逐句繁體中文翻譯
> - 在選舉root bridge和為每台switch選擇根port時，我們已經能夠識別區域network中的一些指定port：root bridge上的所有port都是指定的，並且連接到根port的所有port都是指定的。
> - 對於每個剩餘段，必須有一個designated port，其他端口必須是非designated port。
> - 非指定port處於阻塞狀態；這就是 STP 防止環路的方法。

First, SW1 G0/0 is connected to SW3 G0/1 (a designated port) and is, therefore, a non-designated port; there can only be one designated port per segment. That leaves two segments remaining: the SW1 G0/2 to SW4 G0/1 link and the SW1 G0/3 to SW2

> [!translation] 逐句繁體中文翻譯
> - 首先，SW1 G0/0 連接到 SW3 G0/1（指定port），因此是非指定port；每個網段只能有一個指定port。
> - 剩下兩個段落：SW1 G0/2 到 SW4 G0/1 連結和 SW1 G0/3 到 SW2

G0/0 link. Figure 14.13 shows which ports will be designated and non-designated, and how those decisions were made. In the rest of this section, we will go through the process step by step.

> [!translation] 逐句繁體中文翻譯
> - G0/0 連結。
> - 圖 14.13 顯示了哪些port將被指定和非指定，以及如何做出這些決定。
> - 在本節的其餘部分中，我們將逐步完成該過程。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-282_826_1199_348_346.jpg)
Figure 14.13 Each segment must have exactly one designated port. All ports on the root bridge are designated, and so are all ports connected to a root port. One designated port is selected on each remaining segment, and the remaining ports are non-designated.

## Port on the switch with lowest root cost

The first parameter used to decide which side of the remaining links becomes designated is root cost: the port on the switch with the lowest root cost becomes designated, and the other port becomes non-designated. Pay attention to the wording: it's "the port on the switch with the lowest root cost becomes designated," not "the port with the lowest root cost becomes designated." We are comparing the root cost of each switch via its root port, not the cost of each port whose role is being decided.

> [!translation] 逐句繁體中文翻譯
> - 用於決定剩餘連結的哪一側成為指定的第一個參數是根成本：switch 上具有最低根成本的port成為指定的，而另一個port成為非指定的。
> - 請注意措詞：它是“switch 上具有最低根成本的port被指定”，而不是“具有最低根成本的port被指定”。我們正在透過其根port比較每個switch的根成本，而不是正在決定角色的每個port的成本。

Figure 14.14 shows how the switches compare their root costs to decide which port becomes designated. SW1's root cost (4) is lower than SW2's root cost (8), so SW1's port becomes designated, and SW2's becomes non-designated. SW1 and SW4 have the same root cost (4) and, therefore, will have to use a tiebreaker to decide which port becomes designated.

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-283_847_1406_188_196.jpg)
Figure 14.14 Switches compare their root costs to select designated ports. SW1's root cost (4) is lower than SW2's (8), so SW1 G0/3 becomes designated, and SW2 G0/0 becomes non-designated. SW1 and SW4 have the same root cost (4), so a tiebreaker is needed to decide which port becomes designated.

## Port on switch with lowest bridge id

As a tiebreaker to decide which port becomes designated, the switches will compare their BIDs; the port on the switch with the lowest BID will become designated, and the port on the other switch will become non-designated. Figure 14.15 shows how SW1 and SW4 compare BIDs to decide which switch's port becomes designated; SW4's BID is lower than SW1's, so SW4 G0/1 becomes designated, and SW1 G0/2 becomes non-designated.

> [!translation] 逐句繁體中文翻譯
> - 作為決定哪個port被指定的決定因素，switch將比較它們的 BID； BID 最低的switch 上的port將變為designated port，而另一台switch 上的port將變為非指定port。
> - 圖 14.15 顯示了 SW1 和 SW4 如何比較 BID 來決定哪個switch的port被指定； SW4 的 BID 低於 SW1，因此 SW4 G0/1 變為指定狀態，而 SW1 G0/2 變為非指定狀態。

All port roles have now been decided: root, designated, and non-designated. Note that BPDUs are only sent out of designated ports. When a switch first boots up, it believes it is the root bridge, so all of its ports are designated; the switch sends BPDUs out of all its ports. However, if it then becomes a non-root switch and some of its ports transition to other roles (root or non-designated), the switch does not send BPDUs out of those ports. BPDUs originate from the root bridge and are forwarded throughout the LAN via designated ports only. To summarize this section, here is a summary of the STP algorithm:

> [!translation] 逐句繁體中文翻譯
> - 所有port角色現已確定：根port、指定port和非指定port。
> - 請注意，BPDU 僅從指定port傳送。
> - 當switch第一次啟動時，它認為自己是root bridge，因此它的所有port都被指定；switch從其所有port發送 BPDU。
> - 但是，如果它隨後成為非根switch並且其某些port轉換為其他角色（根或非指定），則switch不會從這些port發送 BPDU。
> - BPDU 源自root bridge，僅透過指定port在整個 LAN 中轉送。
> - 為了總結本節，這裡是 STP 演算法的總結：

1 Root bridge election (one per LAN)
    - Lowest BID
2 Root port selection (one per switch, excluding root bridge)

- Lowest root cost
- Lowest neighbor BID
- Lowest neighbor port ID
3 Designated port selection (one per segment)
    - Port on the switch with the lowest root cost
    - Port on the switch with the lowest BID

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-284_826_1199_550_348.jpg)
Figure 14.15 Switches compare their BIDs as a tiebreaker when selecting designated ports. SW4's BID (28673:5254.001d.d23a) is lower than SW1's BID (32769:5254.000f.adab), so SW4 GO/1 becomes designated, and SW1 G0/2 becomes non-designated.
