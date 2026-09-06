---
source: [[Chapter 15 - RSTP]]
chapter: 15
section: 15.2
tags: [source-section, acting-ccna, rstp, stp]
---

# Chapter 15.2 - STP and RSTP comparison

As the word "rapid" in the name suggests, the fundamental difference between STP and RSTP is speed. Whereas 802.1D's STP is a timer-based protocol in which a port can take up to 50 seconds to begin forwarding, 802.1w's RSTP uses a synchronization mechanism in which RSTP-enabled switches communicate with each other to bring ports immediately to the forwarding state, without waiting for timers to count down.

> [!translation] 逐句繁體中文翻譯
> - 顧名思義，STP 和 RSTP 的根本區別在於速度。
> - 802.1D 的 STP 是基於計時器的協議，其中port最多需要 50 秒才能開始轉發，而 802.1w 的 RSTP 使用同步機制，支援 RSTP 的switch相互通信，使port立即進入轉送狀態，而無需立即等待計時器計時。

EXAM TIP The details of RSTP's sync mechanism are beyond the scope of the CCNA exam. Instead, focus on learning the RSTP port costs, states, roles, and link types covered in this chapter.

> [!translation] 逐句繁體中文翻譯
> - 考試提示 RSTP 同步機制的詳細資訊超出了 CCNA 考試的範圍。
> - 相反，應重點了解本章中介紹的 RSTP port成本、狀態、角色和連結類型。

Although we will cover the various differences between STP and RSTP, keep in mind that the algorithm for calculating the topology is identical:

> [!translation] 逐句繁體中文翻譯
> - 儘管我們將介紹 STP 和 RSTP 之間的各種差異，但請記住，計算topology的演算法是相同的：

- Root bridge election (one per LAN)
    - Lowest bridge ID (BID)
- Root port selection (one per switch, excluding root bridge)
    - Lowest root cost
    - Lowest neighbor BID
    - Lowest neighbor port ID
- Designated port selection (one per segment)
    - Port on switch with lowest root cost
    - Port on switch with lowest BID

In STP, the remaining ports will all be nondesignated. In RSTP, they will be one of two roles: alternate or backup (we will cover them in section 15.2.3). Figure 15.2 shows the same LAN we looked at in chapter 14, this time using RSTP. The only difference is that the nondesignated ports are now alternate ports.

> [!translation] 逐句繁體中文翻譯
> - 在STP中，其餘port都將是非指定的。
> - 在 RSTP 中，它們將是兩個角色之一：備用或備份（我們將在第 15.2.3 節中介紹它們）。
> - 图 15.2 显示了我们在第 14 章中看到的同一 LAN，这次使用 RSTP。
> - 唯一的區別是非指定port現在是備用port。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-298_645_1060_179_348.jpg)
Figure 15.2 A LAN after RSTP has created a loop-free topology. The only difference in the topology between STP and RSTP is that the nondesignated ports are now called alternate ports.

NOTE A technical detail that could come up on the exam is how STP and RSTP handle BPDUs. In STP, the root bridge sends BPDUs every 2 seconds, and the other switches forward them out of their designated ports. In RSTP, all switches send BPDUs out of their designated ports every two seconds, whether they received a BPDU from the root bridge or not.

> [!translation] 逐句繁體中文翻譯
> - 注意 考試中可能出現的一個技術細節是 STP 和 RSTP 如何處理 BPDU。
> - 在 STP 中，root bridge每 2 秒發送一次 BPDU，其他switch將它們從其指定port轉送出去。
> - 在 RSTP 中，所有switch每兩秒鐘從其指定port發送 BPDU，無論它們是否從root bridge接收到 BPDU。

### 15.2.1 Port costs

STP defines port costs for speeds of up to 10 Gbps (with a cost of 2). RSTP, on the other hand, introduces a new set of port costs to accommodate ports of greater speeds-up to 10 Tbps. The original STP port costs are now called the short costs, and the new RSTP port costs are called the long costs. Table 15.2 lists the short and long costs of ports of various speeds.

> [!translation] 逐句繁體中文翻譯
> - STP 定義了速度高達 10 Gbps 的port成本（成本為 2）。
> - 另一方面，RSTP 引入了一組新的port成本，以適應速度高達 10 Tbps 的port。
> - 原始 STP port成本現在稱為短成本，新的 RSTP port成本稱為長成本。
> - 表 15.2 列出了各種速度port的短成本和長成本。

Table 15.2 Short and long port costs
| Speed | Short cost | Long cost |
| :--- | :--- | :--- |
| 10 Mbps | 100 | 2,000,000 |
| 100 Mbps | 19 | 200,000 |
| 1 Gbps | 4 | 20,000 |
| 10 Gbps | 2 | 2,000 |
| 100 Gbps | - | 200 |
| 1 Tbps | - | 20 |
| 10 Tbps | - | 2 |


EXAM TIP To remember the long costs, pick one as a point of reference (such as $10 \mathrm{Gbps}=2,000$ ). Then you can easily calculate the long costs of ports of other speeds: increasing the speed by a factor of 10 reduces the cost by a factor of 10 and vice versa.

> [!translation] 逐句繁體中文翻譯
> - 考試提示 若要記住長期成本，請選擇一個作為參考點（例如 $10 \mathrm{Gbps}=2,000$ ）。
> - 然後您可以輕鬆計算其他速度的port的長期成本：將速度提高 10 倍，成本就會降低 10 倍，反之亦然。

Although the long method of calculating port costs was introduced in RSTP, keep in mind that switches don't necessarily use it by default, even when running RSTP-it depends on the switch model and software version. To view which method (short or long) a switch is using, use the show spanning-tree pathcost method command, and to modify which method the switch uses to calculate port costs, use the spanning -tree pathcost method \{short | long\} command in global config mode. As you can see in the following example, my switch uses the short costs by default:

> [!translation] 逐句繁體中文翻譯
> - 雖然 RSTP 中引入了計算port成本的長方法，但請記住，switch不一定預設使用它，即使在運行 RSTP 時也是如此 - 它取決於switch型號和軟體版本。
> - 若要查看 switch 使用 short 或 long method，請使用 `show spanning-tree pathcost method`；若要修改 port cost 的計算方法，請在 global configuration mode 使用 `spanning-tree pathcost method {short | long}`。
> - 正如您在以下範例中看到的，我的switch預設使用短成本：

```
SW1# show spanning-tree pathcost method
Spanning tree default pathcost method used is short
```

The default method is short.

> [!translation] 逐句繁體中文翻譯
> - 預設方法很短。

### 15.2.2 Port states

Whereas STP has four main states (blocking, listening, learning, forwarding), RSTP combines the first two states into a single state called discarding. Table 15.3 compares the STP and RSTP port states.

> [!translation] 逐句繁體中文翻譯
> - STP 有四個主要狀態（阻斷、偵聽、學習、轉送），而 RSTP 將前兩個狀態組合成一個稱為丟棄的狀態。
> - 表 15.3 比較了 STP 和 RSTP port狀態。

Table 15.3 STP and RSTP port states
| STP port state | RSTP port state |
| :--- | :--- |
| Blocking | Discarding |
| Listening |  |
| Learning | Learning |
| Forwarding | Forwarding |


When an RSTP port is first enabled, it will enter the discarding state. If it is decided that the port will be an alternate or backup port (more on that in section 15.2.3), it will remain in the discarding state, blocking traffic to prevent Layer 2 loops. However, if the port becomes a root or designated port, it will proceed to the forwarding state in one of two ways, as shown in figure 15.3.

> [!translation] 逐句繁體中文翻譯
> - 當RSTPport首次啟用時，它將進入丟棄狀態。
> - 如果確定該port將是備用或備份port（更多資訊請參閱第 15.2.3 節），它將保持在丟棄狀態，阻止traffic以防止Layer 2環路。
> - 但是，如果port成為root port或designated port，它將通過兩種方式之一進入轉發狀態，如圖 15.3 所示。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-299_344_746_1758_320.jpg)
Figure 15.3 An RSTP port can transition to the forwarding state in one of two ways. If the RSTP sync mechanism succeeds, the port immediately transitions to the forwarding state. If the RSTP sync mechanism fails, the port transitions from discarding to learning to forwarding, like a regular STP port.

If the RSTP sync mechanism succeeds, the port immediately transitions to the forwarding state-no need to wait for any timers. This is expected if the port is connected to another RSTP-enabled switch. However, the sync mechanism will not work in all situations-for example, if the port is connected to a device that doesn't use RSTP, such as a router, a PC, or even a switch that is using STP instead of RSTP. In that case, the port will remain in the discarding state for 15 seconds (the duration of the forward delay timer), then transition to the learning state for another 15 seconds, and then finally transition to the forwarding state-this is like a regular STP port transitioning through the listening and learning states before forwarding.

> [!translation] 逐句繁體中文翻譯
> - 如果RSTP同步機製成功，port立即轉換到轉送狀態－無需等待任何定時器。
> - 如果port連接到另一個啟用 RSTP 的交換機，則這是預期的情況。
> - 但是，同步機制並非在所有情況下都有效 - 例如，如果port連接到不使用 RSTP 的設備，例如router、PC，甚至是使用 STP 而不是 RSTP 的switch。
> - 在這種情況下，port將保持丟棄狀態 15 秒（轉送延遲計時器的持續時間），然後再轉換到學習狀態 15 秒，最後轉換到轉送狀態 - 這就像常規 STP port在轉送之前經歷偵聽和學習狀態的轉換一樣。

NOTE RSTP and STP are compatible. However, a port on an RSTP-enabled switch that is connected to a port on an STP-enabled switch will not be able to take advantage of RSTP's sync mechanism. The port on the RSTP-enabled switch will operate like a regular STP port.

> [!translation] 逐句繁體中文翻譯
> - 說明 RSTP 和STP 是相容的。
> - 但是，啟用 RSTP 的switch 上的port如果連接到啟用 STP 的switch 上的port，將無法利用 RSTP 的同步機制。
> - 啟用 RSTP 的switch 上的port將像常規 STP port一樣運作。

These days, you can safely expect that all switches will support RSTP, so you should only expect the timer-based transition to the forwarding state on ports connected to a host like a PC. However, as we covered in chapter 14, PortFast can be configured on such ports to allow them to start forwarding frames immediately-we will cover PortFast again in section 15.3.

> [!translation] 逐句繁體中文翻譯
> - 如今，您可以放心地期望所有switch都將支援 RSTP，因此您應該只期望在連接到 PC 等host的port 上基於計時器的轉換到轉送狀態。
> - 然而，正如我們在第 14 章中介紹的那樣，可以在此類port 上配置 PortFast，以允許它們立即開始轉送frame - 我們將在 15.3 節中再次介紹 PortFast。

### 15.2.3 Port roles

The three port roles of STP are root, designated, and nondesignated; we covered these in chapter 14. In RSTP, the root and designated roles remain unchanged. A root port is a forwarding port that points toward the root bridge; it provides the switch's only active path to reach the root bridge. A designated port is a forwarding port that points away from the root bridge, and all segments must have exactly one designated port. However, the nondesignated port role has been replaced by two distinct roles: alternate and backup. Like nondesignated ports, alternate and backup ports block traffic to prevent Layer 2 loops.

> [!translation] 逐句繁體中文翻譯
> - STP的三種port角色是根、指定和非指定；我們在第 14 章中介紹了這些內容。
> - 在 RSTP 中，根角色和指定角色保持不變。
> - 根port是指向root bridge的轉送埠；它提供switch到達root bridge的唯一活動路徑。
> - designated port是指遠離root bridge的轉送端口，並且所有網段必須恰好有一個designated port。
> - 但是，非指定port角色已被兩個不同的角色所取代：備用和備份。
> - 與非指定port一樣，備用port和備份port會阻止traffic以防止Layer 2迴路。

## Alternate role

An RSTP alternate port can be thought of as an alternative for the switch's root port; it provides an alternative path toward the root bridge and is ready to take over (by transitioning to the forwarding state) if the root port fails. The rule for becoming an alternate port is as follows: any port that is not a root or designated port is an alternate port if the switch is not the designated bridge for that segment.

> [!translation] 逐句繁體中文翻譯
> - RSTP 備用port可以被視為switch根port的替代port；它提供通往root bridge的替代路徑，並準備在根port發生故障時接管（透過轉換到轉送狀態）。
> - 成為備用port的規則如下：如果switch不是該網段的指定網橋，則任何不是根port或指定port的port都是備用port。

NOTE Designated bridge is a new term; it is the switch that has the designated port for a particular segment. This term applies to both STP and RSTP.

> [!translation] 逐句繁體中文翻譯
> - 注意：指定橋接器是一個新術語；它是具有特定網段的指定port的switch。
> - 此術語適用於 STP 和 RSTP。

In almost all cases, a port that is neither a root port nor a designated port will be an alternate port. Figure 15.4 explains why each of the alternate ports in the LAN is in that state.

> [!translation] 逐句繁體中文翻譯
> - 在幾乎所有情況下，既不是root port也不是designated port的端口將是備用端口。
> - 圖 15.4 解釋了為什麼 LAN 中的每個備用port都處於該狀態。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-301_717_1416_181_190.jpg)
Figure 15.4 A port will become an alternate port if it is neither root nor designated and if the switch is not the designated bridge for the segment. In almost all cases, a port that is neither root nor designated will be an alternate port.

NOTE A simple way to define an alternate port is as a port in the discarding state that is connected to a designated port on another switch.

> [!translation] 逐句繁體中文翻譯
> - 注意 定義備用port的簡單方法是，將其定義為連接到另一台switch 上的指定port的處於丟棄狀態的port。

## Backup role

An RSTP backup port provides a backup path to the same segment as a designated port on the same switch. This will only occur if two ports on the same switch are connected to the same segment (collision domain)-for example, with a hub. You will most likely never use a hub in a modern network, so I wouldn't expect to encounter a backup port "in the wild." However, backup ports are worth covering, even if just for the possibility of a question about them on the exam.

> [!translation] 逐句繁體中文翻譯
> - RSTP備份port提供與相同switch 上的指定port相同網段的備份路徑。
> - 只有當同一switch 上的兩個port連接到同一網段（衝突域）（例如透過集線器）時，才會發生這種情況。
> - 您很可能永遠不會在現代network中使用集線器，因此我不會期望在「野外」遇到備份port。然而，備份port是值得討論的，即使只是為了在考試中可能會出現有關它們的問題。

The rule for becoming a backup port is as follows: any port that is not a root or designated port is a backup port if the switch is the designated bridge for that segment. In figure 15.5, I have added a hub between SW1 and SW3 (and removed SW2 and SW4 from the diagram), connecting their four ports to the same segment. SW1 G0/1 becomes an alternate port (because SW1 isn't the designated bridge for the segment), but SW3 G0/1 becomes a backup port (because SW3 is the designated bridge for the segment).

> [!translation] 逐句繁體中文翻譯
> - 成為備份port的規則如下：如果switch是該網段的指定橋，則任何不是根port或指定port的port都是備份port。
> - 在圖 15.5 中，我在 SW1 和 SW3 之間添加了一個集線器（並從圖中刪除了 SW2 和 SW4），將它們的四個連接到同一網段。
> - SW1 G0/1 成為備用port（因為 SW1 不是該網段的指定橋接器），但 SW3 G0/1 成為備援port（因為 SW3 是該網段的指定網橋）。

NOTE We can simplify this rule too: a backup port is a port in the discarding state that is connected to a designated port on the same switch (via a hub). In figure 15.5, SW3 G0/1 is connected to SW3 G0/0 (a designated port) via a hub.

> [!translation] 逐句繁體中文翻譯
> - 注意我們也可以簡化這個規則：備份port是連接到同一switch 上的指定port（透過集線器）的處於丟棄狀態的port。
> - 在圖 15.5 中，SW3 G0/1 透過集線器連接到 SW3 G0/0（指定port）。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-302_522_1110_181_351.jpg)
Figure 15.5 SW1 and SW3 both have multiple ports connected to the same segment. SW1 G0/1 becomes an alternate port because SW1 is not the designated bridge for the segment. SW3 GO/1 becomes a backup (B) port because SW3 is the designated bridge for the segment.

In the following example, I use the show spanning-tree command on SW1 and SW3 to confirm their port roles; Altn stands for alternate, and Back stands for backup. Note that the Sts column states BLK in each case; even though the blocking state was renamed to discarding in RSTP, this command retains the "blocking" terminology:

> [!translation] 逐句繁體中文翻譯
> - 在以下範例中，我在 SW1 和 SW3 上使用 show spanning-tree 命令來確認它們的port角色； Altn 代表備用，Back 代表備份。
> - 請注意，Sts 列在每種情況下都狀態為 BLK；儘管阻塞狀態在 RSTP 中被重新命名為丟棄，但此命令保留了「阻塞」術語：

```
SW1# show spanning-tree
. . .
Interface Role Sts Cost Prio.Nbr Type
------------------- ---- --- --------- -------- -------------------------------
Gi0/0 Root FWD 4 128.1 Shr
Gio/1 Altn BLK 4 128.2 Shr
SW3# show spanning-tree
. . .
        Prio.Nbr Type
Interface Role Sts Cost Prio.Nbr Type
Gio/0
Gio/0
    Desg FWD 4
        128.1 Shr
Gi0/1 Back BLK 4
Gi0/1
    Back BLK 4
        128.2 Shr
                SW3 GO/0 is a
                designated port.
```

Why does SW1 G0/0 become a root port instead of SW1 G0/1, and why does SW3 G0/0 become a designated port instead of SW3 G0/1? When multiple ports on the same switch are connected to the same segment, there is an additional tiebreaker for the root/designated port selection steps of the STP algorithm: the port with the lowest port ID on the local switch becomes root/designated. SW1 G0/0 has a lower port ID than SW1 G0/1, so G0/0 becomes a root port, and G0/1 becomes an alternate port. Likewise, SW3 G0/0 has a lower port ID than SW3 G0/1, so G0/0 becomes a designated port, and G0/1 becomes a backup port. With these tiebreakers added, the STP algorithm is as follows:

> [!translation] 逐句繁體中文翻譯
> - 為什麼SW1 G0/0而不是SW1 G0/1成為root port，為什麼SW3 G0/0而不是SW3 G0/1成為designated port？
> - 當同一switch 上的多個port連接到同一網段時，STP 演算法的根/指定port選擇步驟還有一個額外的決定因素：本機switch 上port ID 最小的port成為根/指定port。
> - SW1 G0/0 的port ID 低於 SW1 G0/1，因此 G0/0 成為root port，G0/1 成為備用端口。
> - 同樣，SW3 G0/0 的端口 ID 低於 SW3 G0/1，因此 G0/0 成為designated port，G0/1 成為備份端口。
> - 在加入這些決勝局後，STP 演算法如下：

- Root bridge election (one per LAN)
    - Lowest BID
- Root port selection (one per switch, excluding root bridge)
    - Lowest root cost
    - Lowest neighbor BID
    - Lowest neighbor port ID
    - Lowest local port ID
- Designated port selection (one per segment)
    - Port on switch with lowest root cost
    - Port on switch with lowest BID
    - Lowest local port ID

NOTE These tiebreakers apply to STP too. If SW1 and SW3 were running STP rather than RSTP, their alternate/backup ports would both be nondesignated ports; there is no such distinction of roles in STP.

> [!translation] 逐句繁體中文翻譯
> - 注意 這些決定因素也適用於 STP。
> - 如果 SW1 和 SW3 運行的是 STP 而不是 RSTP，則它們的備用/備份port都將是非指定port；STP 中沒有這種角色區分。

In chapter 14, I mentioned that every port on the root bridge should be designated. This is almost always true, but as we just saw, if multiple ports on the root bridge are connected to the same segment, only one can be designated; the "one designated port per segment" rule wins out. So rather than "every port on the root bridge should be designated," a more accurate rule is "the root bridge is the designated bridge for every segment." However, because you will rarely (if ever) encounter a LAN using hubs, you'll most often hear that every port on the root bridge should be designated.

> [!translation] 逐句繁體中文翻譯
> - 在第14章中，我提到root bridge上的每個port都應該被指定。
> - 這幾乎總是正確的，但正如我們剛才看到的，如果root bridge上的多個port連接到同一網段，則只能指定一個； “每段一個指定port”規則勝出。
> - 因此，更準確的規則是“root bridge是每個網段的指定橋”，而不是“root bridge上的每個port都應該被指定”。但是，由於您很少（如果有的話）遇到使用集線器的 LAN，因此您最常聽到的是應該指定root bridge上的每個port。

### 15.2.4 RSTP topology changes

In chapter 14, we covered an example in which a switch's nondesignated port took 50 seconds to move to a forwarding state after a change in the topology (due to a hardware failure). Although the details of the topology change processes of STP and RSTP are beyond the scope of the CCNA exam, let's take a brief look at one way that RSTP speeds up the process. Figure 15.6 shows the same example from chapter 14, this time using RSTP.

> [!translation] 逐句繁體中文翻譯
> - 在第14章中，我們介紹了一個範例，其中switch的非指定port在topology發生變化（由於硬體故障）後花了50秒才進入轉送狀態。
> - 雖然STP和RSTP的topology變化過程的細節超出了CCNA考試的範圍，但讓我們簡單看一下RSTP加速該過程的一種方式。
> - 圖 15.6 顯示了第 14 章中的相同範例，這次使用 RSTP。

Whereas an STP-enabled switch waits 20 seconds (the max age timer) after ceasing to receive BPDUs on a port to react and initiate the topology change process, RSTP only waits until a port misses three BPDUs (6 seconds by default) to react. Then, as in figure 15.6, the next-best port can sync with its neighbor and immediately move to the forwarding state; a 50-second process has been shortened to just over 6 seconds.

> [!translation] 逐句繁體中文翻譯
> - 啟用 STP 的switch在port停止接收 BPDU 後會等待 20 秒（最大老化計時器）來做出反應並啟動topology變更過程，而 RSTP 只會等到port錯過 3 個 BPDU（預設為 6 秒）才做出反應。
> - 然後，如圖 15.6 所示，次佳埠可以與其鄰居同步並立即進入轉送狀態； 50秒的過程已縮短至6秒多一點。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-304_613_1418_183_221.jpg)
Figure 15.6 RSTP speeds up convergence after a topology change like this from 50 seconds to just over 6 seconds. (1) Due to a hardware failure, SW1 stops receiving BPDUs on G0/1 (without the port going down). (2) G0/1 remains the root port for 6 seconds (3× the hello timer of 2 seconds). (3) G0/0 becomes the root port and, after performing the RSTP sync process, immediately moves to the forwarding state.

NOTE If the hardware failure caused SW1's G0/1 port to go down (enter a disabled state), the process would be even faster; there would be no need to wait 6 seconds. In that case, SW1 G0/0 will immediately initiate the sync process, bringing the total downtime to less than 1 second.

> [!translation] 逐句繁體中文翻譯
> - 注意 如果硬體故障導致 SW1 的 G0/1 port關閉（進入停用狀態），則該過程會更快；不需要等待6秒。
> - 在這種情況下，SW1 G0/0 將立即啟動同步過程，使總停機時間少於 1 秒。
