---
source: [[Chapter 12 - VLAN]]
chapter: 12
section: 12.4
tags: [source-section, acting-ccna, vlan]
---

# Chapter 12.4 - Inter-VLAN Routing

Even after segmenting a LAN into multiple subnets and VLANs, we usually still want the subnets/VLANs to be able to communicate with each other (and external networks). Although routing is a Layer 3 concept and VLANs are a Layer 2 concept, the term inter-VLAN routing is used to refer to routing between subnets in a LAN that is segmented using VLANs.

> [!translation] 逐句繁體中文翻譯
> - 即使把 LAN 分割成多個 subnets 與 VLANs，我們通常仍希望這些 subnets/VLANs 能彼此通訊，並能連到 external networks。
> - 雖然 routing 是 Layer 3 concept，而 VLANs 是 Layer 2 concept，但 inter-VLAN routing 這個詞用來指在使用 VLANs 分割的 LAN 中，subnets 之間的 routing。

Up to this point, all of the diagrams in this chapter (aside from figure 12.1, which only depicted one subnet) have shown three links between R1 and SW1-one per subnet/VLAN. This is one option for inter-VLAN routing; the router interfaces are configured as normal, and the switch ports are configured as access ports. Figure 12.9 shows how PC3 (in VLAN 20) can communicate with PC10 (in VLAN 10) in this case.

> [!translation] 逐句繁體中文翻譯
> - 到目前為止，本章所有 diagrams，除了只描繪一個 subnet 的 Figure 12.1，都顯示 R1 與 SW1 之間有三條 links，也就是每個 subnet/VLAN 一條。
> - 這是 inter-VLAN routing 的一種選項；router interfaces 以一般方式設定，switch ports 則設為 access ports。
> - Figure 12.9 顯示在此情況下，VLAN 20 中的 PC3 如何與 VLAN 10 中的 PC10 通訊。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-234_763_1419_184_220.jpg)
Figure 12.9 PC3 (in VLAN 20) sends a packet to PC10 (in VLAN 10). (1) PC3 sends the packet in a frame addressed to its default gateway (R1 G0/1). SW2 forwards it out of its G0/2 port (untagged). (2) R1 routes the packet, forwarding it out of G0/0 in a new frame addressed to PC10. The frame is forwarded to PC10 by SW1 and SW2. It is tagged only when crossing the trunk link from SW1 G0/0 to SW2 G0/0.

The following examples show how R1 and SW1 can be configured to enable inter-VLAN routing in this manner:

> [!translation] 逐句繁體中文翻譯
> - 下面例子顯示如何設定 R1 與 SW1，以這種方式啟用 inter-VLAN routing。

```
R1(config)# interface g0/0
R1(config-if)# ip address 172.16.1.1 255.255.255.192
R1(config-if) # no shutdown
R1(config-if) # interface g0/1
R1(config-if) # ip address 172.16.1.65 255.255.255.192
R1(config-if) # no shutdown
R1(config-if) # interface g0/2
R1(config-if) # ip address 172.16.1.129 255.255.255.192
R1(config-if) # no shutdown
SW1(config)# interface g0/1
SW1(config-if)# switchport mode access
SW1(config-if) # switchport access vlan 10
SW1(config-if) # interface g0/2
SW1(config-if)# switchport mode access
SW1(config-if) # switchport access vlan 20
SW1(config-if) # interface g0/3
SW1(config-if)# switchport mode access
SW1(config-if) # switchport access vlan 30
```

```
Configures and enables R1 G0/0
(VLAN 10's default gateway)
Configures and enables R1 G0/1
(VLAN 20’s default gateway)
Configures and enables R1 G0/2
(VLAN 30's default gateway)
Configures and enables SW1
GO/1 (VLAN 10)
Configures and enables SW1
GO/2 (VLAN 20)
Configures and enables SW1
GO/3 (VLAN 30)
```

However, this method of inter-VLAN routing is not common for the same reason it's not common to connect switches using access ports: in a LAN with many VLANs, you'll soon run out of physical ports on your devices. Instead, one of the following options is usually preferred:

> [!translation] 逐句繁體中文翻譯
> - 不過，這種 inter-VLAN routing 方法並不常見，原因與不常用 access ports 連接 switches 一樣：在有許多 VLANs 的 LAN 中，devices 上的 physical ports 很快就會用完。
> - 通常會偏好以下其中一種選項。

- Router on a stick (a trunk link between the switch and router)
- Multilayer switch (a switch that can also route packets)

### 12.4.1 Router on a stick

Router on a stick (ROAS) is a method of inter-VLAN routing that involves creating a trunk link between a switch and a router; a single physical router interface can be divided into multiple virtual subinterfaces, each with its own IP address. These subinterfaces send and receive tagged frames, like a trunk port on a switch. Figure 12.10 shows how the same packet from PC3 to PC10 can be routed using ROAS.

> [!translation] 逐句繁體中文翻譯
> - Router on a stick（ROAS）是一種 inter-VLAN routing 方法，它在 switch 與 router 之間建立 trunk link；單一 router physical interface 可以被分成多個 virtual subinterfaces，每個 subinterface 都有自己的 IP address。
> - 這些 subinterfaces 會像 switch 上的 trunk port 一樣收送 tagged frames。
> - Figure 12.10 顯示 PC3 到 PC10 的同一個 packet 如何使用 ROAS 被 routed。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-235_839_1420_860_190.jpg)
Figure 12.10 PC3 (in VLAN 20) sends a packet to PC10 (in VLAN 10), and the packet is routed using the router on a stick method. R1's G0/0 interface has three subinterfaces: G0/0.10 (VLAN 10, 172.16.1.1), G0/0.20 (VLAN 20, 172.16.1.65), and G0/0.30 (VLAN 30, 172.16.1.129). PC3's frame to R1 is tagged in VLAN 20 over the trunk link from SW1 G0/1 and R1 G0/0. R1's frame to PC10 is tagged in VLAN 10 over the trunk link from R1 G0/0 to SW1 G0/1, and the trunk link from SW1 G0/0 to SW2 G0/0.

NOTE A router's physical interface and virtual subinterfaces all share the same MAC address. When a frame arrives on the physical interface, the router knows
which subinterface the frame is destined for based on the frame's VLAN tag rather than based on the frame's destination MAC address.

> [!translation] 逐句繁體中文翻譯
> - NOTE：Router 的 physical interface 與 virtual subinterfaces 都共用相同 MAC address。
> - 當 frame 到達 physical interface 時，router 會根據 frame 的 VLAN tag，而不是 destination MAC address，判斷 frame 目標是哪個 subinterface。

## Configuring ROAS

Let's see how to configure ROAS as shown in figure 12.10. SW1's side of the connection is a trunk port, just like we configured in section 12.3. In the following example, I configure SW1 G0/1 as a trunk port, allow only the necessary VLANs, and change the native VLAN to an unused VLAN:
![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-236_276_1406_584_223.jpg)

> [!translation] 逐句繁體中文翻譯
> - 讓我們看看如何設定 Figure 12.10 所示的 ROAS。
> - SW1 端的連線是 trunk port，就像 12.3 節所設定的一樣。
> - 下面例子中，我把 SW1 G0/1 設為 trunk port，只允許必要 VLANs，並把 native VLAN 改為未使用的 VLAN。

NOTE As mentioned previously, switches that only support 802.1Q (and not ISL) don't require the switchport trunk encapsulation command before the switchport mode trunk command.

> [!translation] 逐句繁體中文翻譯
> - NOTE：如前所述，只支援 802.1Q 而不支援 ISL 的 switches，在 switchport mode trunk command 之前不需要 switchport trunk encapsulation command。

Next up is R1's configuration; here we'll use some new commands. To configure a subinterface, use the interface command and follow the interface name with a period and a number that identifies the subinterface, such as interface $\mathrm{g} 0 / 0.10$; this will bring you to subinterface configuration mode. In the following example, I enable R1's G0/0 interface and then enter subinterface configuration mode for the G0/0.10 subinterface. Notice that the prompt changes to R1 (config-subif) \#:
![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-236_204_1050_1345_346.jpg)

> [!translation] 逐句繁體中文翻譯
> - 接下來是 R1 的設定；這裡會使用一些新 commands。
> - 要設定 subinterface，使用 interface command，並在 interface name 後面加上一個句點與識別 subinterface 的數字，例如 interface g0/0.10；這會帶你進入 subinterface configuration mode。
> - 下面例子中，我啟用 R1 的 G0/0 interface，然後進入 G0/0.10 subinterface 的 subinterface configuration mode。
> - 請注意 prompt 變成 R1(config-subif)#。

NOTE The G0/0 interface itself does not need any additional configurations; just make sure you enable it with no shutdown.

> [!translation] 逐句繁體中文翻譯
> - NOTE：G0/0 interface 本身不需要額外設定；只要確定用 no shutdown 啟用它即可。

Once in subinterface configuration mode, there are two things to configure on the subinterface: the VLAN associated with the subinterface and the IP address. To configure the VLAN ID, use the encapsulation dot1q vlan-id command. In the following example, I configure the VLAN ID and IP address of R1's G0/0.10 subinterface:
![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-236_190_1237_1918_346.jpg)

> [!translation] 逐句繁體中文翻譯
> - 進入 subinterface configuration mode 後，subinterface 上要設定兩件事：與該 subinterface 關聯的 VLAN，以及 IP address。
> - 要設定 VLAN ID，使用 encapsulation dot1q vlan-id command。
> - 下面例子中，我設定 R1 G0/0.10 subinterface 的 VLAN ID 與 IP address。

After these configurations, any frames R1 receives on its G0/0 interface that are tagged with VLAN 10 will be sent to the G0/0.10 subinterface, and any frames sent by the G0/0.10 subinterface will be tagged with VLAN 10.

> [!translation] 逐句繁體中文翻譯
> - 完成這些設定後，R1 在 G0/0 interface 收到任何 tagged with VLAN 10 的 frames，都會送到 G0/0.10 subinterface；而 G0/0.10 subinterface 送出的任何 frames，也會 tagged with VLAN 10。

NOTE The number used to identify the subinterface (the . 10 in G0/0.10) does not have to match the VLAN ID; the number has no significance beyond identifying the subinterface. It's the encapsulation dot1q command that tells the router which VLAN to associate with this subinterface. However, I recommend you match these two numbers; there's no reason not to.

> [!translation] 逐句繁體中文翻譯
> - NOTE：用來識別 subinterface 的數字，也就是 G0/0.10 中的 .10，不一定要等於 VLAN ID；這個數字除了識別 subinterface 以外沒有其他意義。
> - 真正告訴 router 要把哪個 VLAN 關聯到此 subinterface 的，是 encapsulation dot1q command。
> - 不過，我建議你讓兩個數字一致，因為沒有理由不這樣做。

In the following example, I configure two more subinterfaces: one for VLAN 20 and one for VLAN 30. I then confirm with the show ip interface brief command. Notice that the G0/0 interface itself does not have an IP address; rather, the three virtual subinterfaces have IP addresses, and they send and receive traffic through the physical G0/0 interface:

> [!translation] 逐句繁體中文翻譯
> - 下面例子中，我再設定兩個 subinterfaces：一個給 VLAN 20，一個給 VLAN 30。
> - 接著我用 show ip interface brief command 確認。
> - 請注意 G0/0 interface 本身沒有 IP address；三個 virtual subinterfaces 才有 IP addresses，並透過 physical G0/0 interface 收送 traffic。

```
R1(config-subif) # interface g0/0.20
R1(config-subif) # encapsulation dot1q 20
R1(config-subif) # ip address 172.16.1.65 255.255.255.192
R1(config-subif) # interface g0/0.30
R1(config-subif) # encapsulation dot1q 30
R1(config-subif) # ip address 172.16.1.129 255.255.255.192
R1(config-subif) # do show ip interface brief
```

| Interface | IP-Address | OK? Method Status |
| :--- | :--- | :--- |
| GigabitEthernet0/0 | unassigned | YES manual up |
| GigabitEthernet0/0.10 | 172.16.1.1 | YES manual up |
| GigabitEthernet0/0.20 | 172.16.1.65 | YES manual up |
| GigabitEthernet0/0.30 | 172.16.1.129 | YES manual up |

```
Configures the GO/0.20
subinterface
Configures the GO/0.30
subinterface
Protocol
up
up
up
up
GO/0’s three subinterfaces
The physical GO/0 interface
```

The ROAS configuration is now complete; R1 can route traffic between the three subnets/VLANs in the LAN, using the single physical trunk connection with SW1. Note that I didn't do any configurations related to the native VLAN on R1's side of the connection; if not using the native VLAN, there is no need to do any particular configurations on the router.

> [!translation] 逐句繁體中文翻譯
> - ROAS configuration 現在完成了；R1 可透過與 SW1 的單一 physical trunk connection，在 LAN 中三個 subnets/VLANs 之間 route traffic。
> - 請注意，我沒有在 R1 端做任何 native VLAN 相關設定；如果不使用 native VLAN，router 上不需要任何特定設定。

## Configuring the native VLAN with ROAS

If you decide to use the native VLAN over the ROAS trunk, there are two methods to configure the router's side of the connection:

> [!translation] 逐句繁體中文翻譯
> - 如果你決定在 ROAS trunk 上使用 native VLAN，router 端有兩種設定方法。

- Use the encapsulation dot1q vlan-id native command on the appropriate subinterface.
- Configure the IP address for the native VLAN on the physical interface, not a subinterface.

Let's try both. In the following example, I show the ROAS configuration once again, this time configuring VLAN 10 as the native VLAN by adding the native keyword to
the encapsulation dot1q command. Aside from that, the configurations are identical to the previous examples:

> [!translation] 逐句繁體中文翻譯
> - 兩種方法都試看看。
> - 下面例子中，我再次顯示 ROAS configuration，這次透過在 encapsulation dot1q command 加上 native keyword，把 VLAN 10 設為 native VLAN。
> - 除此之外，設定與前面例子相同。

```
R1(config) # interface g0/0
R1(config-if) # no shutdown
R1(config-if) # interface g0/0.10
R1(config-subif) # encapsulation dot1q 10 native
R1(config-subif) # ip address 172.16.1.1 255.255.255.192
R1(config-subif) # interface g0/0.20
R1(config-subif) # encapsulation dot1q 20
R1(config-subif) # ip address 172.16.1.65 255.255.255.192
R1(config-subif) # interface g0/0.30
R1(config-subif) # encapsulation dot1q 30
R1(config-subif) # ip address 172.16.1.129 255.255.255.192
```

In the following example, I use the second method of configuring the native VLAN on the router. I don't configure a subinterface for VLAN 10, but rather configure the native VLAN's IP address on the G0/0 interface itself; the encapsulation dot1q command is not necessary for VLAN 10 in this case, although it's still needed on the subinterfaces of the non-native VLANs (VLANs 20 and 30):

> [!translation] 逐句繁體中文翻譯
> - 下面例子中，我使用第二種方法在 router 上設定 native VLAN。
> - 我不為 VLAN 10 設定 subinterface，而是把 native VLAN 的 IP address 直接設定在 G0/0 interface 本身；在此情況下，VLAN 10 不需要 encapsulation dot1q command，但 non-native VLANs，也就是 VLANs 20 與 30 的 subinterfaces 仍然需要。

```
R1(config) # interface g0/0
R1(config-if) # no shutdown
R1(config-if)# ip address 172.16.1.1 255.255.255.192
R1(config-if) # interface g0/0.20
R1(config-subif) # encapsulation dot1q 20
R1(config-subif) # ip address 172.16.1.65 255.255.255.192
R1(config-subif) # interface g0/0.30
R1(config-subif) # encapsulation dot1q 30
R1(config-subif) # ip address 172.16.1.129 255.255.255.192
```

NOTE Whichever method you use to configure the native VLAN on the router, make sure the native VLAN matches on the switch.

> [!translation] 逐句繁體中文翻譯
> - NOTE：不論你用哪種方法在 router 上設定 native VLAN，都要確保 native VLAN 與 switch 上相符。

### 12.4.2 Multilayer switching

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

> [!translation] 逐句繁體中文翻譯
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

1 The VLAN associated with the SVI must exist on the switch (i.e., created with the vlan vlan-idcommand).
2 The switch must have at least one of the following:
    A An access port associated with the VLAN (using the switchport access vlan command) in an up/up state.

B A trunk port that allows the VLAN (using the switchport trunk allowed vlan command) in an up/up state.
3 The VLAN must be enabled (must not have the shutdown command applied).
4 The SVI must be enabled (must not have the shutdown command applied).

> [!translation] 逐句繁體中文翻譯
> - B：有一個允許該 VLAN 的 trunk port，使用 switchport trunk allowed vlan command，且處於 up/up state。
> - 第三，該 VLAN 必須 enabled，也就是沒有套用 shutdown command。
> - 第四，該 SVI 必須 enabled，也就是沒有套用 shutdown command。

EXAM TIP Make sure you understand the difference between a VLAN and an SVI. A VLAN is a Layer 2 concept-a virtual broadcast domain that divides up a switch. An SVI is a virtual Layer 3 interface that is associated with a VLAN. To create a VLAN, use the vlan command. To create an SVI, use the interface vlan command.

> [!translation] 逐句繁體中文翻譯
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
