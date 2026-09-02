---
source: [[Chapter 12.4 - Inter-VLAN Routing]]
chapter: 12
section: 12.4.1
tags: [source-section, acting-ccna, vlan]
---

# Chapter 12.4.1 - Router on a stick

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
