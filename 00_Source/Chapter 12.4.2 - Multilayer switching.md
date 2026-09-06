---
source: [[Chapter 12.4 - Inter-VLAN Routing]]
chapter: 12
section: 12.4.2
tags: [source-section, acting-ccna, vlan]
---

# Chapter 12.4.2 - Multilayer switching

The third option for inter-VLAN routing, and perhaps the most popular (although ROAS is common as well), is to use a multilayer switch. A multilayer switch (also called a Layer 3 switch) is a switch that is also capable of routing packets; it's a switch with a router built in.

> [!translation] 逐句繁體中文翻譯
> - Inter-VLAN routing 的第三種選項，也許也是最受歡迎的選項，雖然 ROAS 也很常見，是使用 multilayer switch。
> - Multilayer switch，也稱為 Layer 3 switch，是同時能 route packets 的 switch；它可以想成內建 router 的 switch。

NOTE: A standard switch that only forwards frames can be called a Layer 2 switch. However, nowadays, almost all switches have some degree of Layer 3 capabilities, so the difference between a multilayer switch and a Layer 2 switch is often determined by how you use the switch rather than the switch itself.

> [!translation] 逐句繁體中文翻譯
> - NOTE：只 forward frames 的標準 switch 可稱為 Layer 2 switch。
> - 不過現在幾乎所有 switches 都具備某種程度的 Layer 3 capabilities，所以 multilayer switch 與 Layer 2 switch 的差異，常常取決於你如何使用該 switch，而不只是 switch 本身。

## Inter-VLAN routing via SVIs

Multilayer switches perform inter-VLAN routing using virtual interfaces called switch virtual interfaces (SVIs). Each SVI is an interface on the multilayer switch's built-in
router, and hosts in each VLAN use the IP address of their VLAN's SVI as their default gateway.

> [!important] 逐句繁體中文翻譯
> - Multilayer switches 使用稱為 switch virtual interfaces（SVIs）的 virtual interfaces 執行 inter-VLAN routing。
> - 每個 SVI 都是 multilayer switch 內建 router 上的一個 interface，而每個 VLAN 中的 hosts 會使用自己 VLAN 的 SVI IP address 作為 default gateway。

Figure 12.11 shows the internal logic of how SW1 (now a multilayer switch) routes a packet from PC1 to PC5. PC1 sends the packet in a frame addressed to SW1's VLAN 10 SVI-each SVI has a unique MAC address. SW1's internal router routes the packet via the VLAN 30 SVI and forwards it out of the G0/0 trunk port in a frame (tagged in VLAN 30) addressed to PC5's MAC, and SW2 forwards the frame to PC5 (untagged).

> [!translation] 逐句繁體中文翻譯
> - Figure 12.11 顯示 SW1 現在作為 multilayer switch 時，如何從 PC1 route packet 到 PC5 的內部邏輯。
> - PC1 把 packet 放進 frame，目的地是 SW1 的 VLAN 10 SVI；每個 SVI 都有唯一 MAC address。
> - SW1 的內部 router 經由 VLAN 30 SVI route packet，並從 G0/0 trunk port 送出一個 tagged in VLAN 30、destination MAC 為 PC5 MAC 的 frame，接著 SW2 把 frame 以 untagged 方式 forward 給 PC5。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-239_1026_1418_529_190.jpg)
Figure 12.11 SW1, a multilayer switch, routes a packet from PC1 to PC5. SW1 has three SVIs: VLAN 10 (172.16.1.1), VLAN 20 (172.16.1.65), and VLAN 30 (172.16.1.129), allowing SW1 to route packets internally, without relying on an external router.

NOTE R1 is no longer present in the figure 12.11 diagram; if we configure SVIs on SW1, there is no need to rely on an external router for inter-VLAN routing.

> [!translation] 逐句繁體中文翻譯
> - NOTE：Figure 12.11 diagram 中不再出現 R1；如果我們在 SW1 上設定 SVIs，就不需要依賴外部 router 來做 inter-VLAN routing。

The first step to configure SW1, as in figure 12.11, is to enable IP routing with the command ip routing in global configuration mode. Without this command, the switch won't be able to forward packets between subnets/VLANs.

> [!translation] 逐句繁體中文翻譯
> - 依 Figure 12.11 設定 SW1 的第一步，是在 global configuration mode 使用 ip routing command 啟用 IP routing。
> - 沒有這個 command，switch 就無法在 subnets/VLANs 之間 forward packets。

After enabling IP routing, the next step is to configure SW1's SVIs. The command to configure an SVI is interface vlan vlan-id; then, just configure an IP address on the SVI like a router interface. Unlike when configuring a router subinterface (in which the subinterface identifier is not significant), the vlan-idspecified in the interface vlan command is significant; it's what specifies which VLAN the SVI is associated with. In the following example, I enable IP routing and then configure SW1's SVIs for VLAN 10, VLAN 20, and VLAN 30, with the IP addresses configured on R1 in previous examples:
![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-240_314_1402_522_220.jpg)

> [!translation] 逐句繁體中文翻譯
> - 啟用 IP routing 後，下一步是設定 SW1 的 SVIs。
> - 設定 SVI 的 command 是 interface vlan vlan-id；接著像設定 router interface 一樣，在 SVI 上設定 IP address。
> - 不同於設定 router subinterface 時 subinterface identifier 沒有實質意義，interface vlan command 中指定的 vlan-id 是有意義的；它指定該 SVI 關聯到哪個 VLAN。
> - 下面例子中，我啟用 IP routing，然後為 VLAN 10、VLAN 20 與 VLAN 30 設定 SW1 的 SVIs，IP addresses 使用前面例子中設定在 R1 上的位址。

NOTE On some switches, SVIs may be administratively disabled by default. In that case, use no shutdown to enable each SVI.

> [!translation] 逐句繁體中文翻譯
> - NOTE：在某些 switches 上，SVIs 可能預設為 administratively disabled。
> - 這種情況下，使用 no shutdown 啟用每個 SVI。

SW1 is now ready to route packets in the LAN; like a router, SW1 inserts connected and local routes into its routing table for each SVI, so there is no need to configure static routes. In the following example, I check SW1's routing table with show ip route:
![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-240_466_1419_1164_220.jpg)
For an SVI to function, it must be in an up/up state (referring to the Status and Protocol columns in the output of show ip interface brief), just like a physical interface. For an SVI to be in an up/up state, there are four requirements; refer to this list if you need to troubleshoot an SVI that won't reach an up/up state:

> [!translation] 逐句繁體中文翻譯
> - SW1 現在已準備好在 LAN 中 route packets；就像 router 一樣，SW1 會為每個 SVI 把 connected 與 local routes 插入 routing table，因此不需要設定 static routes。
> - 下面例子中，我用 show ip route 檢查 SW1 的 routing table。
> - SVI 要能運作，必須處於 up/up state，也就是 show ip interface brief 輸出中的 Status 與 Protocol 欄位都 up，這點和 physical interface 一樣。
> - SVI 要處於 up/up state 有四個 requirements；如果你需要排查無法到達 up/up state 的 SVI，可參考這份清單。

==1 The VLAN associated with the SVI must exist on the switch (i.e., created with the vlan vlan-idcommand).==

2 The switch must have at least one of the following:
    ==A An access port associated with the VLAN (using the switchport access vlan command) in an up/up state.==

	==B A trunk port that allows the VLAN (using the switchport trunk allowed vlan command) in an up/up state==
==3 The VLAN must be enabled (must not have the shutdown command applied).==

==4 The SVI must be enabled (must not have the shutdown command applied).==

> [!translation] ==逐句繁體中文翻譯==
> ==- B：有一個允許該 VLAN 的 trunk port，使用 switchport trunk allowed vlan command，且處於 up/up state。==
> ==- 第三，該 VLAN 必須 enabled，也就是沒有套用 shutdown command。==
> ==- 第四，該 SVI 必須 enabled，也就是沒有套用 shutdown command。==

EXAM TIP Make sure you understand the difference between a VLAN and an SVI. A VLAN is a Layer 2 concept-a virtual broadcast domain that divides up a switch. An SVI is a virtual Layer 3 interface that is associated with a VLAN. To create a VLAN, use the vlan command. To create an SVI, use the interface vlan command.

> [!important] 逐句繁體中文翻譯
> - EXAM TIP：請務必理解 VLAN 與 SVI 的差異。
> - VLAN 是 Layer 2 concept，也就是分割 switch 的 virtual broadcast domain。
> - SVI 是與 VLAN 關聯的 virtual Layer 3 interface。
> - 要建立 VLAN，使用 vlan command。
> - 要建立 SVI，使用 interface vlan command。

As the following example shows, SW1's SVIs are currently in an up/up state:

> [!translation] 逐句繁體中文翻譯
> - 如下例所示，SW1 的 SVIs 目前處於 up/up state。

```
SW1# show ip interface brief | include Vlan
Vlan10 172.16.1.1 YES manual up up
Vlan20 172.16.1.65 YES manual up up
Vlan30 172.16.1.129 YES manual up
```

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-241_166_295_784_1297.jpg)

To demonstrate the requirements, in the following example, I violated one require-
To demonstrate the requirements, in the following example, I violated one requirement for each of the VLAN 10, VLAN 20, and VLAN 30 SVIs: ment for each of the VLAN 10, VLAN 20, and VLAN 30 SVIs:

> [!translation] 逐句繁體中文翻譯
> - 為了示範這些 requirements，下面例子中，我對 VLAN 10、VLAN 20 與 VLAN 30 的 SVIs 各違反一項 requirement。

- I deleted VLAN 10 from SW1 (requirement 1).
- I disabled SW1's G1/3 port (an access port in VLAN 30) and removed VLAN 30
- I disabled SW1's G1/3 port (an access port in VLAN 30) and removed VLAN 30 from G0/0's list of allowed VLANs (requirement 2). from G0/0's list of allowed VLANs (requirement 2).

- I disabled VLAN 20 with shutdown (requirement 3).

- I disabled VLAN 20 with shutdown (requirement 3).

As a result, all three SVIs move to an up/down state; they will no longer be able to
As a result, all three SVIs move to an up/down state; they will no longer be able to route packets: route packets:
SW1(config)\# no vlan 10
SW1(config)\# interface g1/3
SW1(config-if)\# shutdown
SW1(config-if)\# interface g0/0
SW1(config-if)\# switchport trunk allowed vlan remove 30
SW1(config-if)\# vlan 20
SW1(config-vlan)\# shutdown
SW1(config-vlan)\# do show ip interface brief | include Vlan
Vlan10
Vlan20
Vlan30

> [!translation] 逐句繁體中文翻譯
> - 結果，三個 SVIs 都移到 up/down state；它們將不再能 route packets。
> - 接著範例透過刪除 VLAN 10、關閉 G1/3、從 G0/0 allowed VLANs 中移除 VLAN 30、關閉 VLAN 20，並用 show ip interface brief 顯示 Vlan interfaces 狀態。

```
172.16.1.1 YES manual up
172.16.1.65 YES manual up
172.16.1.129 YES manual up
```

Removes VLAN 30 from G0/0's allowed VLANs

> [!translation] 逐句繁體中文翻譯
> - 從 G0/0 的 allowed VLANs 中移除 VLAN 30。

Disables VLAN 20
![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-241_170_327_1740_1274.jpg)

> [!translation] 逐句繁體中文翻譯
> - 停用 VLAN 20。

Using routed ports for external connectivity
Routing within a LAN is important, but it's also essential for hosts in the LAN to be able to reach external networks, such as the internet or another LAN in the corporate network. To provide external connectivity, it's common to use a routed port on a
multilayer switch. A routed port is a physical port on a multilayer switch that has been configured to function like a router's interface. Figure 12.12 shows how SW1's G0/1 port can be used as a routed port, providing connectivity to external networks via R1.

> [!translation] 逐句繁體中文翻譯
> - 使用 routed ports 提供 external connectivity。
> - 在 LAN 內 routing 很重要，但 LAN 中的 hosts 也必須能到達 external networks，例如 Internet 或 corporate network 中的另一個 LAN。
> - 為了提供 external connectivity，常見做法是在 multilayer switch 上使用 routed port。
> - Routed port 是 multilayer switch 上被設定成像 router interface 一樣運作的 physical port。
> - Figure 12.12 顯示 SW1 的 G0/1 port 如何作為 routed port，透過 R1 提供到 external networks 的 connectivity。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-242_772_1417_349_220.jpg)
Figure 12.12 SW1, a multilayer switch, uses a routed port (G0/1) to provide connectivity to external networks (via R1, in this case). Like a router interface, SW1 G0/1 is configured with an IP address: 172.16.1.193.

NOTE SW1's icon in figure 12.12 is a new one. Network diagrams typically use an icon like this to represent multilayer switches, differentiating them from Layer 2 switches.

> [!translation] 逐句繁體中文翻譯
> - NOTE：Figure 12.12 中 SW1 的 icon 是新的。
> - Network diagrams 通常會用這類 icon 表示 multilayer switches，以區分它們和 Layer 2 switches。

To configure a routed port, use the no switchport command in interface configuration mode; then, you can configure an IP address just like on a router's interface. In the following example, I configure SW1 G0/1 as a routed port with an IP address and then check SW1's routing table:

> [!translation] 逐句繁體中文翻譯
> - 要設定 routed port，在 interface configuration mode 使用 no switchport command；接著就能像 router interface 一樣設定 IP address。
> - 下面例子中，我把 SW1 G0/1 設為 routed port 並設定 IP address，然後檢查 SW1 的 routing table。

```
SW1(config)# interface g0/1
SW1(config-if)# no switchport
SW1(config-if)# ip address 172.16.1.193 255.255.255.252
SW1(config-if)# do show ip route
. . .
    172.16.0.0/16 is variably subnetted, 8 subnets, 3 masks
C 172.16.1.0/26 is directly connected, Vlan10
L 172.16.1.1/32 is directly connected, Vlan10
C 172.16.1.64/26 is directly connected, Vlan20
L 172.16.1.65/32 is directly connected, Vlan20
C 172.16.1.128/26 is directly connected, Vlan30
L 172.16.1.129/32 is directly connected, Vlan30
```

```
C 172.16.1.192/30 is directly connected, GigabitEthernet0/1
L 172.16.1.193/32 is directly connected, GigabitEthernet0/1
```

Connected and local routes for G0/1

> [!translation] 逐句繁體中文翻譯
> - G0/1 的 connected 與 local routes。

SW1 G0/1 is now a routed port with an IP address, and SW1 has added connected and local routes for it, but SW1 still isn't able to forward packets outside of the LAN; it needs a route (or routes) to external destinations. Just like on a router, you can configure static routes on a multilayer switch or use a dynamic routing protocol (the topic of part 4 of this volume). In the following example, I configure a static default route on SW1, using R1's IP address as the next hop:

> [!translation] 逐句繁體中文翻譯
> - SW1 G0/1 現在是有 IP address 的 routed port，SW1 也已為它加入 connected 與 local routes，但 SW1 仍無法把 packets forward 到 LAN 外部；它需要一條或多條通往 external destinations 的 route。
> - 就像 router 一樣，你可以在 multilayer switch 上設定 static routes，或使用 dynamic routing protocol，這是本 volume 第 4 部分的主題。
> - 下面例子中，我在 SW1 上設定 static default route，使用 R1 的 IP address 作為 next hop。

```
SW1(config)# ip route 0.0.0.0 0.0.0.0 172.16.1.194
```

SW1's default route (using R1 as the next hop)

> [!translation] 逐句繁體中文翻譯
> - SW1 的 default route，使用 R1 作為 next hop。

Now that SW1 has a route to external networks, it can provide connectivity between the LAN and external networks, as well as between the subnets/VLANs in the LAN. Figure 12.13 shows the internal logic of how SW1 can forward a packet from a host in the LAN toward an external destination; SW1's routed port (G0/1) provides connectivity from the internal router to R1.

> [!translation] 逐句繁體中文翻譯
> - 現在 SW1 有了通往 external networks 的 route，它就能提供 LAN 與 external networks 之間的 connectivity，也能提供 LAN 內 subnets/VLANs 之間的 connectivity。
> - Figure 12.13 顯示 SW1 如何把 LAN 中 host 的 packet forward 到 external destination 的內部邏輯；SW1 的 routed port G0/1 提供從 internal router 到 R1 的 connectivity。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-243_987_1241_1023_316.jpg)
Figure 12.13 A host in the LAN sends a packet to an external destination, routed by SW1. G0/1 is a routed port, connecting SW1's internal router to R1.

Exam scenarios
VLANs are one of the major topics of the CCNA exam, so you can expect at least a few VLAN-related questions on the CCNA exam. The following are a few questions demonstrating how your understanding of VLANs might be tested on the CCNA exam:

> [!translation] 逐句繁體中文翻譯
> - Exam scenarios。
> - VLANs 是 CCNA 考試的主要主題之一，所以你可以預期考試中至少會有幾題 VLAN-related questions。
> - 以下幾題示範 CCNA 考試可能如何測驗你對 VLANs 的理解。

1(multiple choice, multiple answers)
Examine the following configuration of SW1's GO/0 interface:

> [!translation] 逐句繁體中文翻譯
> - 第一題是 multiple choice、multiple answers。
> - 請檢查 SW1 G0/0 interface 的以下 configuration。

```
interface GigabitEthernet0/0
    switchport access vlan 5
    switchport trunk native vlan 10
    switchport mode trunk
```

Which of the following statements are true? (select two)

> [!translation] 逐句繁體中文翻譯
> - 下列哪些 statements 正確？請選兩個。


    A SW1 G0/0 is a trunk port.
    B SW1 GO/0 is an access port.
    c SW1 will assign untagged frames received on GO/0 to VLAN 5.
    D SW1 will assign untagged frames received on GO/0 to VLAN 10.

> [!translation] 逐句繁體中文翻譯
> - A：SW1 G0/0 是 trunk port。
> - B：SW1 G0/0 是 access port。
> - C：SW1 會把在 G0/0 收到的 untagged frames 指派到 VLAN 5。
> - D：SW1 會把在 G0/0 收到的 untagged frames 指派到 VLAN 10。

The challenging part of this question is that GO/O has configurations related both to access ports and trunk ports. The switchport access vlan 5 command implies that SW1 will assign untagged frames received on GO/0 to VLAN 5. However, the switchport trunk native vlan 10 command implies that SW1 will assign them to VLAN 10. The key to this question is that the switchport mode command specifies trunk, so GO/0 is operating as a trunk port (and A is one of the correct answers). Therefore, the switchport access vlan 5 command will not affect GO/0; it is only significant if GO/O is operating as an access port. So, the second correct answer is D; SW1 will assign untagged frames received on GO/0 to VLAN 10 (the native VLAN).

> [!translation] 逐句繁體中文翻譯
> - 這題的難點在於，G0/0 同時有 access ports 與 trunk ports 相關設定。
> - switchport access vlan 5 command 暗示 SW1 會把在 G0/0 收到的 untagged frames 指派到 VLAN 5。
> - 不過，switchport trunk native vlan 10 command 暗示 SW1 會把它們指派到 VLAN 10。
> - 這題的關鍵是 switchport mode command 指定 trunk，因此 G0/0 以 trunk port 運作，而 A 是正確答案之一。
> - 因此，switchport access vlan 5 command 不會影響 G0/0；它只有在 G0/0 以 access port 運作時才有意義。
> - 所以第二個正確答案是 D；SW1 會把在 G0/0 收到的 untagged frames 指派到 VLAN 10，也就是 native VLAN。

2 (drag and drop)
On the left are four statements about the native VLAN and the default VLAN. Drag the statements to the default VLAN or native VLAN on the right. Each statement can only be used once.

| (A) Related to access ports | Default VLAN |
| :--- | :--- |
| (B) Related to trunk ports |  |
| (C) VLAN 1 by default, and can be changed | Native VLAN |
| (D) VLAN 1 by default, and cannot be changed |  |

The correct answers are A/D for the default VLAN and B/C for the native VLAN. As mentioned in the note in section 12.3.2, the default VLAN and native VLAN are often confused, so make sure you can differentiate between the two for the exam.

> [!translation] 逐句繁體中文翻譯
> - Default VLAN 的正確答案是 A/D，native VLAN 的正確答案是 B/C。
> - 如 12.3.2 節 NOTE 所述，default VLAN 與 native VLAN 常被混淆，所以請確保你能在考試中分辨兩者。

3 (lab simulation)

A lab simulation might provide you with a network diagram and ask you to configure access ports and trunk ports as appropriate. Remember the basic configurations of each:

> [!translation] 逐句繁體中文翻譯
> - Lab simulation 可能會提供 network diagram，並要求你視情況設定 access ports 與 trunk ports。
> - 請記住各自的基本設定。

```
(continued)
```


- Access ports-switchport mode access, switchport access vlan vlan-id
- Trunk ports-switchport trunk encapsulation dot1q (if needed), switchport mode trunk, switchport trunk allowed vlan vlans
