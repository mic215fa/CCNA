---
source: [[Chapter 14 - STP]]
chapter: 14
section: 14.5
tags: [source-section, acting-ccna, stp]
---

# Chapter 14.5 - PortFast and BPDU Guard

Cisco switches include a suite of optional STP features (sometimes called the STP toolkit) that can speed up STP's convergence and improve stability. For the CCNA exam, you need to know a few of these optional STP features. In this section, we'll cover two: PortFast and BPDU Guard.

> [!translation] 逐句繁體中文翻譯
> - Cisco switch包含一套可選的 STP 功能（有時稱為 STP 工具包），可加速 STP 的convergence並提高穩定性。
> - 對於 CCNA 考試，您需要了解其中一些可選的 STP 功能。
> - 在本節中，我們將介紹兩個：PortFast 和 BPDU Guard。

So far, we have focused on connections between switches, but STP is active on all switch ports-not just those connected to other switches. Switch ports connected to devices that do not use STP (such as PCs) will always be designated ports; there is no risk of a Layer 2 loop. However, due to STP's timer-based operation, it will take 30 seconds after connecting a device before the device can actually access the network-before the switch port enters the forwarding state. This can be frustrating for users who aren't aware of STP, and it is an inconvenience in any case.

> [!translation] 逐句繁體中文翻譯
> - 到目前為止，我們關注的是switch之間的連接，但 STP 在所有switchport 上都處於活動狀態，而不僅僅是那些連接到其他switch的port。
> - 連接到不使用 STP 的設備（例如 PC）的switchport將始終是指定port；不存在Layer 2迴路的風險。
> - 但是，由於 STP 基於定時器的操作，連接設備後需要 30 秒才能真正存取網絡，然後switchport才會進入轉送狀態。
> - 這對於不了解 STP 的使用者來說可能會令人沮喪，並且在任何情況下都會帶來不便。

### 14.5.1 PortFast

PortFast is an optional STP feature that allows a switch port to move immediately to the forwarding state, bypassing the listening and learning states. Figure 14.18 shows how PortFast allows a connected device to access the network immediately.

> [!translation] 逐句繁體中文翻譯
> - PortFast 是一項可選的 STP 功能，允許switchport立即進入轉送狀態，繞過偵聽和學習狀態。
> - 圖 14.18 顯示了 PortFast 如何允許連接的設備立即存取network。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-289_514_1410_411_192.jpg)
Figure 14.18 PortFast allows a switch port to immediately move to the forwarding state. Without PortFast, when an end host is connected to a switch port, it must wait 30 seconds before it can access the network. With PortFast, the end host can access the network immediately, bypassing the listening and learning states.

To enable PortFast on a specific port, use the spanning-tree portfast command in interface config mode. Another option is to use the spanning-tree portfast default command in global config mode to enable PortFast on all access ports (not trunk ports). As the following example shows, the switch displays a lengthy warning after configuring PortFast:

> [!translation] 逐句繁體中文翻譯
> - 若要在特定port 上啟用 PortFast，請在interface設定模式下使用 spanning-tree portfast 指令。
> - 另一個選項是在全域設定模式下使用 spanning-tree portfast default 指令在所有存取port（不是trunk port）上啟用 PortFast。
> - 如以下範例所示，設定 PortFast 後交換機會顯示冗長的警告：

```
SW1(config)# interface g1/0
SW1(config-if) # spanning-tree portfast
%Warning: portfast should only be enabled on ports connected to a single
    host. Connecting hubs, concentrators, switches, bridges, etc... to this
    interface when portfast is enabled, can cause temporary bridging loops.
    Use with CAUTION
%Portfast has been configured on GigabitEthernet1/0 but will only
    have effect when the interface is in a non-trunking mode.
```

A warning message is displayed.

> [!translation] 逐句繁體中文翻譯
> - 系統會顯示警告訊息。

Because PortFast puts switch ports in the forwarding state immediately, bypassing the listening and learning states, it is important that you enable it only on ports intended for end hosts. Do not connect switches to PortFast-enabled ports; otherwise, Layer 2 loops can occur, as stated in the warning message in the previous example.

> [!translation] 逐句繁體中文翻譯
> - 由於 PortFast 會立即將switchport置於轉送狀態，從而繞過偵聽和學習狀態，因此僅在用於最終host的port 上啟用它非常重要。
> - 請勿將switch連接到啟用 PortFast 的port；否則，可能會出現Layer 2環路，如上例中的警告訊息中所述。

### 14.5.2 BPDU Guard

BPDU Guard is another optional STP feature that disables a switch port if it receives a BPDU; it should be enabled on all PortFast-enabled ports. Remember, PortFast-enabled
ports should only connect to end hosts, which do not send BPDUs. If a user carelessly connects another switch to a port meant for end hosts, BPDU Guard disables the port and prevents the newly connected switch from affecting the STP topology (e.g., by becoming the new root bridge).

> [!translation] 逐句繁體中文翻譯
> - BPDU Guard 是另一個可選的 STP 功能，如果switchport收到 BPDU，則會停用該port；應在所有啟用 PortFast 的port 上啟用它。
> - 請記住，啟用 PortFast 的port只能連接到不傳送 BPDU 的終端host。
> - 如果使用者不小心將另一台switch連接到用於最終host的端口，BPDU Guard 會停用該port並防止新連接的switch影響 STP topology（例如，成為新的root bridge）。

To enable BPDU Guard on a port, use the spanning-tree bpduguard enable command in interface config mode. Another option is to use the spanning-tree portfast bpduguard default command in global config mode; this automatically enables BPDU Guard on all PortFast-enabled ports. In the following example, I enable PortFast and BPDU Guard on a switch port:

> [!translation] 逐句繁體中文翻譯
> - 若要在port 上啟用 BPDU 防護，請在interface設定模式下使用 spanning-tree bpduguard enable 指令。
> - 另一個選項是在全域設定模式下使用spanning tree portfast bpduguard 預設指令；這會自動在所有啟用 PortFast 的port 上啟用 BPDU 防護。
> - 在以下範例中，我在switchport 上啟用 PortFast 和 BPDU Guard：

```
SW4(config)# interface g0/0
SW4(config-if) # spanning-tree portfast
SW4(config-if) # spanning-tree bpduguard enable
```

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-290_152_462_603_1149.jpg)
The port I enabled PortFast and BPDU Guard on in the example is connected to another switch, so we can see BPDU Guard in action-don't do this in a real network! If a switch port with BPDU Guard enabled receives a BPDU from another switch, it enters an error-disabled state. The following example shows the error messages displayed when BPDU Guard disables a port:

> [!translation] 逐句繁體中文翻譯
> - 在範例中，啟用 PortFast 與 BPDU Guard 的 port 連接到另一台 switch，因此可以看到 BPDU Guard 如何動作；請勿在實際 network 中這樣連接。
> - 啟用 BPDU Guard 的 switch port 如果收到另一台 switch 傳來的 BPDU，就會進入 error-disabled state。
> - 以下範例顯示 BPDU Guard 停用 port 時產生的錯誤訊息：

```
%SPANTREE-2-BLOCK_BPDUGUARD: Received BPDU on port GiO/O
-with BPDU Guard enabled. Disabling port.
%PM-4-ERR_DISABLE: bpduguard error detected on Gi0/0,
-putting Gi0/0 in err-disable state
```

An error-disabled port is nonoperational; its status will be down/down in the output of show ip interface brief. This is an example of the STP disabled state I mentioned in section 14.4. To reenable an error-disabled port, first solve the problem that caused the error (disconnect the switch from the PortFast/BPDU Guard-enabled port), and then use the shutdown and no shutdown commands on the port to reset it.

> [!translation] 逐句繁體中文翻譯
> - 錯誤停用的port無法運作；其狀態將在 show ip interface Brief 的輸出中為 down/down。
> - 這是我在 14.4 節中提到的 STP 停用狀態的範例。
> - 若要重新啟用因錯誤而停用的port，請先解決導致錯誤的問題（斷開switch與啟用了 PortFast/BPDU Guard 的port的連接），然後在port 上使用 shutdown 和 no shutdown 命令將其重設。

EXAM TIP Remember these best practices: only enable PortFast on ports meant for end hosts, and enable BPDU Guard on all PortFast-enabled ports. It is possible to use only PortFast or only BPDU Guard, but best practice is to use both features together.

> [!translation] 逐句繁體中文翻譯
> - 考試提示 請記住以下最佳實務：僅在用於最終host的port 上啟用 PortFast，並在所有啟用 PortFast 的port 上啟用 BPDU 防護。
> - 可以只使用 PortFast 或僅使用 BPDU Guard，但最佳實踐是同時使用這兩個功能。

## Summary

- The Ethernet header does not have a mechanism to drop looping frames, so they will loop indefinitely.
- If enough looping frames accumulate, a broadcast storm can occur, using up so many network resources that the network becomes unusable.
- Redundancy is the practice of having additional network devices and connections beyond the minimum necessary for communication to eliminate single points of failure.

- Layer 2 loops occur as a result of the flooding of BUM traffic-broadcast, unknown unicast, and multicast frames-in a LAN with redundant connections.
- Spanning Tree Protocol (STP) prevents Layer 2 loops in a LAN by blocking redundant connections, leaving only one active path to each destination in the LAN.
- The process STP uses to create a loop-free topology is called the STP algorithm. It consists of three main steps: (1) root bridge election, (2) root port selection, and (3) designated port selection.
- All of the decisions in the algorithm are made by switches sharing STP Bridge Protocol Data Unit (BPDU) messages, which are sent every 2 seconds.
- The root bridge is the central point of reference for the STP topology. All switches in the LAN will ensure they have exactly one active path to the root bridge.
- The switch with the lowest bridge ID (BID) becomes the root bridge. The BID is a 64-bit number that uniquely identifies the switch. It consists of a 16-bit bridge priority and a 48-bit MAC address.
- The bridge priority consists of a configurable priority value (default 32768) and the Extended System ID, which is the VLAN ID.
- Cisco's implementation of STP is called Per-VLAN Spanning Tree Plus (PVST+), which creates a separate spanning tree for each VLAN.
- When a switch boots up, it announces itself as the root bridge and sends BPDUs out of all ports. If it receives BPDUs from a switch with a lower BID, it will accept that switch as the root bridge.
- Use the show spanning-tree command to view information about the root bridge's BID, the local switch's BID, and the local switch's ports.
- Use the spanning-tree vlan vlan-id priority priority-value command to configure the switch's priority for the specified VLAN (in increments of 4096).
- You can also use spanning-tree vlan vlan-id root \{primary | secondary\} to configure the priority. The secondary keyword sets the priority to 28672, and the primary keyword sets the priority to 24576, or the highest multiple of 4096 that will make the switch the root bridge (but it won't set the priority to 0).
- After electing the root bridge, all non-root switches will select exactly one root port, which provides the switch's single active path to the root bridge.
- The root port is selected using the following parameters in order of priority: (1) lowest root cost, (2) lowest neighbor BID, and (3) lowest neighbor port ID.
- A port's root cost indicates how efficient the path to the root bridge is via that port.
- When the root bridge sends BPDUs, they have a root cost of 0. When a non-root switch forwards BPDUs, it adds the cost of the port it received the BPDU on.
- The STP port cost values are $10 \mathrm{Mbps}=100,100 \mathrm{Mbps}=19,1 \mathrm{Gbps}=4$, and $10 \mathrm{Gbps}=2$.
- If a switch has the same root cost via two or more ports, the port connected to the neighbor with the lowest BID becomes the root port. If two or more of those

ports are connected to the same neighbor, the port connected to the neighbor's port with the lowest port ID becomes the root port.

> [!translation] 逐句繁體中文翻譯
> - 如果port連接到同一鄰居，則與鄰居port連接的port ID 最小的port成為根port。
- The port ID is a unique identifier for each port of the switch. It consists of a priority value (128 by default) and a sequential number.
- Each segment (link) must have exactly one designated port. All ports on the root bridge are designated, and the port connected to a root port must be designated.
- The remaining links then select one designated port, and the rest of the ports will be non-designated (blocking).
- Designated ports are selected with the following parameters in order of priority: (1) the port on the switch with the lowest root cost and (2) the port on the switch with the lowest BID.
- The four STP port states are blocking, listening, learning, and forwarding. There is also the disabled state, which refers to a nonoperational port.
- A newly enabled port will enter the listening state, where the switch decides its role.
- If the port becomes non-designated, it will immediately move to the blocking state, in which it is effectively disabled (this is how STP prevents loops).
- If the port becomes root or designated, it will move to the learning state, in which it starts to learn MAC addresses to build the MAC address table. Then, it will move to the forwarding state, where it can finally forward frames.
- The hello timer determines how often BPDUs are sent. It is 2 seconds by default.
- The forward delay timer determines the length of the listening and learning states. It is 15 seconds (per state) by default.
- The max age timer determines how long a switch will wait to change the STP topology after ceasing to receive BPDUs on a port.
- The timers on the root bridge dictate the timers that will be used on all switches in the LAN.
- It can take up to 50 seconds (max age timer + listening state + learning state) for a port to start forwarding after a change in the network.
- It can take 30 seconds for a newly enabled port to start forwarding frames.
- PortFast is an optional feature that can be configured on ports connected to end hosts to allow them to move directly to the forwarding state (no listening/ learning).
- Use the spanning-tree portfast command in interface config mode to enable PortFast on a specific port or the spanning-tree portfast default command in global config mode to enable PortFast on all access ports.
- BPDU Guard should be configured on PortFast-enabled ports to disable them in case another switch is connected to the port.
- Use the spanning-tree bpduguard enable command in interface config mode to enable BPDU Guard on a specific port or the spanning-tree portfast

bpduguard default command in global config mode to enable it on all Port-Fast-enabled ports.

> [!translation] 逐句繁體中文翻譯
> - 在全域設定模式下使用 bpduguard default 指令可在所有啟用 Port-Fast 的port 上啟用它。
- If a BPDU Guard-enabled port receives a BPDU, the port will enter an errordisabled state, rendering it nonoperational. To reenable the port, disconnect the switch that caused the error and use shutdown and no shutdown on the port.
