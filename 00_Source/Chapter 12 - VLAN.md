## VLANs

> [!map] Chapter 12 Section Index
> - [[Chapter 12.1 - 為什麼需要 VLAN|12.1 為什麼需要 VLAN]]
> - [[Chapter 12.2 - 設定 VLAN 與 Access Ports|12.2 設定 VLAN 與 Access Ports]]
> - [[Chapter 12.3 - 使用 Trunk Ports 連接 Switches|12.3 使用 Trunk Ports 連接 Switches]]
> - [[Chapter 12.4 - Inter-VLAN Routing|12.4 Inter-VLAN Routing]]


## This chapter covers

- How to divide a switch into multiple virtual switches with VLANs
- How to configure trunk ports to carry traffic in multiple VLANs
- Routing between VLANs with a router or multilayer switch

In chapter 11, we covered subnetting, which allows us to divide a network into smaller subnets. This is an example of network segmentation-the division of a network into smaller parts. Virtual LANs (VLANs, pronounced "V-LANs"), the topic of this chapter, can be likened to subnets in that they also allow us to divide up a network into smaller parts. With VLANs, we can divide a LAN (a broadcast domain) into smaller LANs, called VLANs. Whereas subnets allow us to segment the network at Layer 3, VLANs allow us to segment the network at Layer 2. In this chapter, we will cover three CCNA exam topics, all related to the topics of switches and VLANs:

> [!translation] 逐句繁體中文翻譯
> - 在第 11 章，我們介紹了 subnetting，它讓我們能把一個 network 分成較小的 subnets。
> - 這是 network segmentation 的例子，也就是把 network 分成較小部分。
> - 本章主題 Virtual LANs（VLANs，發音為 V-LANs）也可類比為 subnets，因為它們同樣讓我們把 network 分成較小部分。
> - 透過 VLANs，我們可以把一個 LAN，也就是 broadcast domain，分成較小的 LANs，稱為 VLANs。
> - Subnets 讓我們在 Layer 3 分割 network；VLANs 則讓我們在 Layer 2 分割 network。
> - 本章會涵蓋三個 CCNA 考試主題，全部都與 switches 和 VLANs 有關。


- 1.1.b Layer 2 and Layer 3 switches
- 2.1 Configure and verify VLANs (normal range) spanning multiple switches
- 2.2 Configure and verify interswitch connectivity

### 12.1 Why we need VLANs

To understand a technology, it's important to understand why that technology existsto understand the problem it solves. To demonstrate the role VLANs play in segmenting networks, let's examine a network without segmentation, a network with Layer 3 segmentation, and a network with both Layer 3 and Layer 2 segmentation.

> [!translation] 逐句繁體中文翻譯
> - 要理解一項技術，重要的是理解它為什麼存在，也就是它解決了什麼問題。
> - 為了示範 VLANs 在 network segmentation 中扮演的角色，我們會檢視三種 network：沒有 segmentation 的 network、只有 Layer 3 segmentation 的 network，以及同時具備 Layer 3 與 Layer 2 segmentation 的 network。

### 12.1.1 Layer 3 segmentation with subnets

Figure 12.1 depicts an office LAN consisting of three different departments: engineering, HR, and sales. All hosts belong to the 172.16.1.0/24 network, enabling them to communicate directly without using the router as an intermediary.

> [!translation] 逐句繁體中文翻譯
> - Figure 12.1 描繪一個辦公室 LAN，包含 engineering、HR 與 sales 三個部門。
> - 所有 hosts 都屬於 172.16.1.0/24 network，因此它們可以不透過 router 作為中介而直接通訊。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-217_799_1136_683_318.jpg)
Figure 12.1 An unsegmented LAN. All hosts are in the 172.16.1.0/24 network, and VLANs are not used to segment the LAN at Layer 2. Hosts belonging to different departments can communicate with each other directly (by sending their packets in frames addressed directly to each other).

From an information security standpoint, this is not suitable for modern networks. Instead of having all hosts within a single large network, we should use subnetting to segment the network at Layer 3, with each department assigned its own subnet. Figure 12.2 demonstrates how hosts in different departments communicate after the network has been divided into separate subnets.

> [!translation] 逐句繁體中文翻譯
> - 從資訊安全角度來看，這不適合現代 networks。
> - 與其把所有 hosts 都放在單一大型 network 中，我們應該用 subnetting 在 Layer 3 分割 network，並讓每個部門擁有自己的 subnet。
> - Figure 12.2 示範 network 被切成不同 subnets 之後，不同部門 hosts 如何通訊。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-218_969_1264_181_350.jpg)
Figure 12.2 A LAN segmented into three subnets. The engineering department uses subnet 172.16.1.0/26, the HR department uses subnet 172.16.1.64/26, and the sales department uses subnet 172.16.1.128/26. R1 has one interface in each subnet. Communication between hosts in different departments must go through R1.

NOTE In an ideal network, hosts in each subnet would have their own switch to connect to. However, in reality, switches are usually shared, as in figure 12.2; hosts in 172.16.1.0/26, 172.16.1.64/26, and 172.16.1.128/26 all connect to SW1. Network infrastructure is a cost, so reducing the necessary amount of hardware is desirable.

> [!translation] 逐句繁體中文翻譯
> - NOTE：在理想 network 中，每個 subnet 的 hosts 都會有自己的 switch 可連接。
> - 不過現實中 switches 通常會共用，如 Figure 12.2 所示；172.16.1.0/26、172.16.1.64/26 與 172.16.1.128/26 的 hosts 都連到 SW1。
> - Network infrastructure 是成本，所以減少必要硬體數量是理想的。

You might be wondering how segmenting the LAN into separate subnets enhances security. By requiring traffic between departments to pass through the router, you can control which traffic is permitted and which is not; security policies can be implemented on the router to control traffic. Figure 12.2 depicts a PC in the engineering department accessing a server used by the HR department; this is an example of traffic you might want to restrict. You could choose to block all hosts outside the HR department from accessing the server or only allow specific types of communication with the server.

> [!translation] 逐句繁體中文翻譯
> - 你可能會想知道，把 LAN 切成不同 subnets 如何提升安全性。
> - 透過要求部門間 traffic 經過 router，你可以控制哪些 traffic 被允許、哪些不被允許；security policies 可以實作在 router 上來控制 traffic。
> - Figure 12.2 描繪 engineering 部門的一台 PC 存取 HR 部門使用的 server；這是你可能想限制的 traffic 例子。
> - 你可以選擇阻擋 HR 部門以外所有 hosts 存取該 server，或只允許特定類型的通訊連到 server。

NOTE In this chapter, we will not cover how to use a router to control which traffic is permitted and which is denied. For now, we will segment the network but will not specify which traffic to permit or deny. We will cover access control lists (one method to control traffic) in part 6 of this book.

> [!translation] 逐句繁體中文翻譯
> - NOTE：本章不會介紹如何用 router 控制哪些 traffic 被允許或拒絕。
> - 現在我們只會分割 network，但不指定允許或拒絕哪些 traffic。
> - 我們會在本書第 6 部分介紹 access control lists，這是控制 traffic 的一種方法。

### 12.1.2 Layer 2 segmentation with VLANs

Using subnetting, we have segmented the LAN at Layer 3. However, switches aren't Layer 3 aware. From SW1's perspective, all hosts are still part of the same LAN; they are in the same broadcast domain. A broadcast frame sent from any host connected to SW1 will be received by all other connected hosts (the same applies to unknown unicast frames). Figure 12.3 demonstrates this: when a host in the engineering department sends a broadcast frame, SW1 floods it to all other connected hosts, regardless of the subnet.

> [!translation] 逐句繁體中文翻譯
> - 透過 subnetting，我們已經在 Layer 3 分割 LAN。
> - 不過 switches 並不具備 Layer 3 awareness。
> - 從 SW1 的角度看，所有 hosts 仍然是同一個 LAN 的一部分；它們在同一個 broadcast domain。
> - 任何連到 SW1 的 host 送出 broadcast frame，都會被其他所有 connected hosts 收到；unknown unicast frames 也是如此。
> - Figure 12.3 示範這點：engineering 部門的一台 host 送出 broadcast frame 時，SW1 不管 subnet 為何，都會把它 flood 給所有其他 connected hosts。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-219_974_1197_826_318.jpg)
Figure 12.3 Although hosts are divided into three subnets, at Layer 2, they are still part of the same broadcast domain (LAN). (1) SW1 receives a broadcast frame from a host in the engineering department. (2) SW1 floods the frame out of all ports, except the one it was received on. The LAN has been segmented at Layer 3, but not at Layer 2.

NOTE One definition of a LAN is "a group of interconnected devices in a limited area," but as covered in chapter 6, a more nuanced definition considers how the devices are connected and how network traffic is forwarded between them, rather than just their physical location. For this chapter, a LAN is the same thing as a broadcast domain-the group of devices that will receive a broadcast frame sent by any other member of the group.

> [!translation] 逐句繁體中文翻譯
> - NOTE：LAN 的一種定義是「有限區域內互相連接的一組設備」；但如第 6 章所說，更細緻的定義會考慮設備如何連接，以及 network traffic 如何在它們之間 forwarding，而不只是 physical location。
> - 本章中，LAN 等同於 broadcast domain，也就是任何成員送出 broadcast frame 時會收到該 frame 的設備群組。

From a security perspective, this is still not suitable-traffic from hosts in one subnet can reach hosts in other subnets. Furthermore, all hosts being in the same broadcast domain can have negative effects on network performance; the unnecessary flooding of frames out of all ports can cause or worsen network congestion. To solve these issues, we should segment the network at Layer 2, and we can use VLANs to do so.

> [!translation] 逐句繁體中文翻譯
> - 從安全角度來看，這仍然不合適；一個 subnet 中 hosts 的 traffic 可以到達其他 subnets 中的 hosts。
> - 此外，所有 hosts 都在同一 broadcast domain，也可能對 network performance 造成負面影響；不必要地把 frames flood 到所有 ports，可能造成或加劇 network congestion。
> - 要解決這些問題，我們應該在 Layer 2 分割 network，而 VLANs 可以做到這件事。

VLANs allow us to divide a single physical switch into multiple virtual switches, thereby dividing the broadcast domain into multiple broadcast domains. Figure 12.4 demonstrates this concept, illustrating how SW1 is divided into multiple virtual switches. By assigning each of SW1's ports to a specific VLAN, SW1 is divided into three virtual switches: one for VLAN 10, one for VLAN 20, and one for VLAN 30. These VLAN numbers are arbitrary; I selected VLANs 10, 20, and 30 for this example, but any numbers within the valid range can be used (more on that in section 12.2.1).

> [!translation] 逐句繁體中文翻譯
> - VLANs 讓我們能把單一 physical switch 分成多個 virtual switches，進而把 broadcast domain 分成多個 broadcast domains。
> - Figure 12.4 示範這個概念，說明 SW1 如何被分成多個 virtual switches。
> - 透過把 SW1 的每個 port 指派到特定 VLAN，SW1 被分成三個 virtual switches：VLAN 10、VLAN 20 與 VLAN 30。
> - 這些 VLAN numbers 是任意的；我在此例選擇 10、20、30，但有效範圍內任何 numbers 都可使用，12.2.1 節會進一步說明。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-220_873_1321_1052_223.jpg)
Figure 12.4 By assigning SW1's interfaces to three separate VLANs, SW1 is divided into three virtual switches, each a separate broadcast domain. G0/0, G0/1, G0/2, and G0/3 are part of VLAN 10. G1/0, G1/1, G1/2, and G1/3 are part of VLAN 20. G2/0, G2/1, G2/2, and G2/3 are part of VLAN 30. SW1 will not forward or flood a frame out of ports in a different VLAN than the port the frame was received on.

NOTE The physical network in figure 12.4 is the same as we saw in figure 12.3; the only difference is that SW1's ports are now in three separate VLANs. I have shown SW1 as three separate virtual switches to illustrate how VLANs work. Network diagrams are usually not represented in this manner; in a typical network diagram, VLANs are labeled, but only the physical switch is shown.

> [!translation] 逐句繁體中文翻譯
> - NOTE：Figure 12.4 的 physical network 與 Figure 12.3 相同；唯一差別是 SW1 的 ports 現在位於三個不同 VLAN。
> - 我把 SW1 畫成三個 virtual switches，是為了說明 VLANs 如何運作。
> - 一般 network diagrams 通常不會這樣表示；典型圖中會標示 VLANs，但只顯示 physical switch。

We have now successfully segmented the LAN at both Layer 3 (with subnets) and Layer 2 (with VLANs). SW1 will not forward or flood frames between VLANs-hosts in separate VLANs can only communicate with each other through R1. As a general rule, there should be a one-to-one relationship between subnets and VLANs, as shown in figure 12.4-one subnet per VLAN. If you continue your studies beyond the CCNA, you will find cases where there are multiple subnets associated with a single VLAN, but for the CCNA, you can assume that they are one-to-one.

> [!translation] 逐句繁體中文翻譯
> - 現在我們已經成功在 Layer 3（用 subnets）與 Layer 2（用 VLANs）分割 LAN。
> - SW1 不會在 VLANs 之間 forward 或 flood frames；位於不同 VLAN 的 hosts 只能透過 R1 彼此通訊。
> - 一般規則是 subnets 與 VLANs 應該一對一，如 Figure 12.4 所示，也就是一個 subnet 對一個 VLAN。
> - 如果你在 CCNA 之後繼續深入學習，會遇到單一 VLAN 關聯多個 subnets 的案例；但在 CCNA 中，可以假設它們是一對一。

### 12.2 Configuring VLANs and access ports

Up to this point in the book, we haven't done much configuration of switches. That's because a switch can fulfill its basic role of forwarding frames without any particular configuration; it builds its MAC address table automatically by examining the source MAC address of frames it receives and then can forward frames between hosts in a LAN. However, to use VLANs, we must configure them on the switch's ports.

> [!translation] 逐句繁體中文翻譯
> - 到目前為止，本書還沒有做太多 switch configuration。
> - 這是因為 switch 不需要特別設定就能完成基本的 frame forwarding 角色；它會檢查收到 frames 的 source MAC address，自動建立 MAC address table，然後在 LAN hosts 之間 forward frames。
> - 不過，要使用 VLANs，就必須在 switch ports 上設定它們。

### 12.2.1 Creating and naming VLANs

First, let's examine the default status of VLANs on SW1. The following example shows the output of the show vlan brief command before configuring any VLANs. This command shows the list of VLANs that exist on the switch (the VLAN database), as well as which ports are in each VLAN:
![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-221_421_1433_1370_190.jpg)

> [!translation] 逐句繁體中文翻譯
> - 首先，讓我們檢查 SW1 上 VLANs 的預設狀態。
> - 下面例子顯示設定任何 VLANs 之前，show vlan brief command 的輸出。
> - 這個 command 顯示 switch 上存在的 VLANs 清單，也就是 VLAN database，以及每個 VLAN 中有哪些 ports。

There are two main takeaways from that output. First, without any configuration, all of SW1's ports are in VLAN 1. VLAN 1 is the default VLAN-the VLAN that all ports are in by default. We can also confirm this by looking at the switch's MAC address table; all MAC addresses are learned in VLAN 1, as shown in the leftmost column of the following example:

> [!translation] 逐句繁體中文翻譯
> - 這個輸出有兩個重點。
> - 第一，在沒有任何設定時，SW1 的所有 ports 都在 VLAN 1。
> - VLAN 1 是 default VLAN，也就是所有 ports 預設所在的 VLAN。
> - 我們也可以查看 switch 的 MAC address table 來確認；所有 MAC addresses 都是在 VLAN 1 中學到的，如下方例子最左欄所示。

```
SW1# show mac address-table
    Mac Address Table
```

SW1 learned all MAC
addresses in VLAN 1.

> [!translation] 逐句繁體中文翻譯
> - SW1 在 VLAN 1 中學到所有 MAC addresses。

The second takeaway from the output of show vlan brief is that VLANs 1002, 1003, 1004, and 1005 also exist on the switch by default. These VLANs are reserved for use by FDDI and Token Ring-two legacy Data Link Layer technologies. FDDI and Token Ring are no longer used in modern networks, but even in modern versions of Cisco IOS, these four VLANs are reserved for backward compatibility-they cannot be deleted or used for Ethernet VLANs.

> [!translation] 逐句繁體中文翻譯
> - show vlan brief 輸出的第二個重點是，VLANs 1002、1003、1004 與 1005 也預設存在於 switch 上。
> - 這些 VLANs 保留給 FDDI 與 Token Ring 使用，兩者都是 legacy Data Link Layer technologies。
> - FDDI 與 Token Ring 已不再用於現代 networks，但即使在現代 Cisco IOS 版本中，這四個 VLANs 仍為 backward compatibility 保留，不能刪除，也不能用作 Ethernet VLANs。

NOTE There are 4096 VLANs in total (from 0 through 4095), but VLANs 0 and 4095 are reserved for special purposes beyond the scope of the CCNA exam. With VLANs 1002-1005 being reserved for FDDI and Token Ring, the range of usable VLANs is 1 to 1001 and 1006 to 4094 (4,090 VLANs in total). That means that a single LAN (broadcast domain) can be divided into a maximum of 4,090 VLANs-far more VLANs than most LANs will ever need.

> [!translation] 逐句繁體中文翻譯
> - NOTE：VLAN 總共有 4096 個，從 0 到 4095；但 VLANs 0 與 4095 保留給 CCNA 範圍外的特殊用途。
> - 由於 VLANs 1002 到 1005 保留給 FDDI 與 Token Ring，可用 VLAN 範圍是 1 到 1001，以及 1006 到 4094，總共 4,090 個 VLANs。
> - 這表示單一 LAN，也就是 broadcast domain，最多可分成 4,090 個 VLANs；這遠多於大多數 LANs 實際需要的數量。

To configure a VLAN, use the vlan vlan-id command from global configuration mode (vlan-id is a number). That will take you to VLAN configuration mode, from which you can also configure the VLAN's name with the name vlan-name command. In the following example, I create and name VLANs 10, 20, and 30 on SW1 and then confirm with show vlan brief (leaving VLANs 1002-1005 out of the output to save space):

> [!translation] 逐句繁體中文翻譯
> - 要設定 VLAN，從 global configuration mode 使用 vlan vlan-id command，其中 vlan-id 是數字。
> - 這會帶你進入 VLAN configuration mode，並可在其中使用 name vlan-name command 設定 VLAN 名稱。
> - 下面例子中，我在 SW1 上建立並命名 VLANs 10、20、30，接著用 show vlan brief 確認；為了節省空間，輸出省略 VLANs 1002 到 1005。

```
SW1(config)# vlan 10
SW1(config-vlan) # name Engineering
SW1(config-vlan)# vlan 20
SW1(config-vlan) # name HR
SW1(config-vlan) # vlan 30
SW1(config-vlan) # name Sales
SW1(config-vlan) # end
SW1# show vlan brief
VLAN Name
----
1 default
10 Engineering
20 HR
    Sales
. . .
```

```
Creates and names VLAN 10
Creates and names VLAN 20
Creates and names VLAN 30
```

```
Ports
GiO/O, GiO/1, GiO/2, GiO/3
Gil/O, Gil/1, Gil/2, Gil/3
Gi2/O, Gi2/1, Gi2/2, Gi2/3
```

VLANs 10, 20, and 30 are in SW1's VLAN database.

> [!translation] 逐句繁體中文翻譯
> - VLANs 10、20 與 30 位於 SW1 的 VLAN database 中。

NOTE Naming a VLAN is optional. If you don't configure a name, the default name is $V L A N x x x x$, where $x x x x$ is the VLAN ID in four digits (i.e., VLAN0010 for VLAN 10).

> [!translation] 逐句繁體中文翻譯
> - NOTE：命名 VLAN 是 optional。
> - 如果你沒有設定名稱，預設名稱是 $VLANxxxx$，其中 xxxx 是四位數 VLAN ID，例如 VLAN 10 的預設名稱是 VLAN0010。

In the previous example, the status of each VLAN is active. However, you can temporarily disable a VLAN by using the shutdown command in VLAN configuration mode. In the following example, I disable VLAN 10 on SW1 and confirm with show vlan brief. Notice that the status changes to act/lshut (active/locally shutdown):

> [!translation] 逐句繁體中文翻譯
> - 在前一個例子中，每個 VLAN 的 status 都是 active。
> - 不過，你可以在 VLAN configuration mode 使用 shutdown command 暫時停用 VLAN。
> - 下面例子中，我停用 SW1 上的 VLAN 10，並用 show vlan brief 確認。
> - 請注意 status 變成 act/lshut，也就是 active/locally shutdown。

```
SW1(config)# vlan 10
SW1(config-vlan) # shutdown
SW1(config-vlan)# end
SW1# show vlan brief
VLAN Name Status Ports
---- ------------------------------- --------- -------------------------------
. . .
10 Engineering act/lshut
```

VLAN 10 is active in the LAN
but locally shutdown (on SW1).

> [!translation] 逐句繁體中文翻譯
> - VLAN 10 在 LAN 中是 active，但在 SW1 上 locally shutdown。

NOTE If you want to delete a VLAN entirely, you can negate the command you used to create it by adding no in front of it, such as no vlan 10.

> [!translation] 逐句繁體中文翻譯
> - NOTE：如果你想完全刪除 VLAN，可以否定建立它時使用的 command，也就是在前面加 no，例如 no vlan 10。

### 12.2.2 Assigning ports to VLANs

Now that we have created VLANs 10, 20, and 30 on SW1, let's assign SW1's ports to the appropriate VLANs. There are two steps to do so:

> [!translation] 逐句繁體中文翻譯
> - 現在我們已經在 SW1 上建立 VLANs 10、20 與 30，接著把 SW1 的 ports 指派到適當 VLAN。
> - 這有兩個步驟。

1 Configure SW1's ports in access mode.
2 Configure the access mode VLAN of the ports.

An access port is a switch port that belongs to a single VLAN, as opposed to a trunk port, which carries traffic in multiple VLANs (we will cover trunk ports in section 12.3). By default, Cisco switch ports use a protocol called Dynamic Trunking Protocol (DTP) to automatically determine whether each port should operate in access mode or trunk mode. We will cover DTP in chapter 13, but for now, just know that it is best practice to manually configure access or trunk mode, rather than letting DTP automatically determine interfaces' status.

> [!translation] 逐句繁體中文翻譯
> - Access port 是屬於單一 VLAN 的 switch port；相對地，trunk port 可承載多個 VLANs 的 traffic，我們會在 12.3 節介紹 trunk ports。
> - Cisco switch ports 預設使用 Dynamic Trunking Protocol（DTP）自動判斷每個 port 應該以 access mode 或 trunk mode 運作。
> - 我們會在第 13 章介紹 DTP；現在只要知道，最佳實務是手動設定 access 或 trunk mode，而不是讓 DTP 自動判斷 interfaces 的狀態。

You can manually configure a switch port to operate in access mode with the switchport mode access command in interface configuration mode. Then, use the switchport access vlan vlan-id command to configure which VLAN the port belongs to. In the following example, I configure SW1's G0/0, G0/1, G0/2, and G0/3
interfaces as access ports in VLAN 10, its G1/0, G1/1, G1/2, and G1/3 interfaces as access ports in VLAN 20, and its G2/0, G2/1, G2/2, and G2/3 ports as access ports in VLAN 30:

> [!translation] 逐句繁體中文翻譯
> - 你可以在 interface configuration mode 使用 switchport mode access command，手動把 switch port 設為 access mode。
> - 接著使用 switchport access vlan vlan-id command 設定該 port 屬於哪個 VLAN。
> - 下面例子中，我把 SW1 的 G0/0、G0/1、G0/2、G0/3 interfaces 設為 VLAN 10 的 access ports；G1/0、G1/1、G1/2、G1/3 設為 VLAN 20 的 access ports；G2/0、G2/1、G2/2、G2/3 ports 設為 VLAN 30 的 access ports。

```
SW1(config)# interface range g0/0-3
SW1(config-if-range) # switchport mode access
SW1(config-if-range) # switchport access vlan 10
SW1(config-if-range) # interface range g1/0-3
SW1(config-if-range) # switchport mode access
SW1(config-if-range) # switchport access vlan 20
SW1(config-if-range) # interface range g2/0-3
SW1(config-if-range) # switchport mode access
SW1(config-if-range) # switchport access vlan 30
```

NOTE If you use the switchport access vlan command to assign a port to a VLAN that doesn't exist yet on the switch, the switch will automatically create the VLAN. This means that it's not necessary to create VLANs with the vlan command before assigning ports to VLANs (although it's necessary if you want to use the name command to name the VLANs).

> [!translation] 逐句繁體中文翻譯
> - NOTE：如果你使用 switchport access vlan command，把 port 指派到 switch 上尚不存在的 VLAN，switch 會自動建立該 VLAN。
> - 這表示在把 ports 指派給 VLANs 之前，不一定要先用 vlan command 建立 VLANs；但如果你想用 name command 命名 VLANs，就必須先建立。

We have now finished configuring SW1! It will forward and flood frames between hosts in each VLAN but not between VLANs-each VLAN is a separate broadcast domain. Keep in mind that VLANs are configured on the switch ports; although it's common to say that an end host is in VLAN X, that host is not actually aware of what VLAN it is in-VLANs are a concept used by switches, not typical end hosts like PCs.

> [!translation] 逐句繁體中文翻譯
> - 現在我們已經完成 SW1 的設定！它會在每個 VLAN 內的 hosts 之間 forward 與 flood frames，但不會在 VLANs 之間轉送；每個 VLAN 都是獨立 broadcast domain。
> - 請記住，VLANs 是設定在 switch ports 上；雖然常說某個 end host 在 VLAN X 中，但 host 本身其實不知道自己在哪個 VLAN，VLANs 是 switches 使用的概念，不是一般 PCs 這類 typical end hosts 使用的概念。

NOTE There are exceptions where end hosts are VLAN aware; we will look at an example when we cover virtual machines in chapter 17 of volume 2.

> [!translation] 逐句繁體中文翻譯
> - NOTE：有些例外情況中，end hosts 會具備 VLAN awareness；我們會在 volume 2 第 17 章介紹 virtual machines 時看到例子。

### 12.3 Connecting switches with trunk ports

Access ports are assigned to a single VLAN, and only forward and flood traffic between ports in the same VLAN. Trunk ports, on the other hand, are not assigned to a single VLAN; rather, they can forward traffic in multiple VLANs. Figure 12.5 shows a situation in which a trunk port should be used: the LAN consists of two switches (SW1 and SW2), and hosts in VLANs 10, 20, and 30 are connected to each switch. For hosts in each VLAN to be able to communicate with each other, the link between SW1 and SW2 must be able to carry traffic in multiple VLANs; to enable that, frames sent between SW1 and SW2 are tagged to indicate which VLAN each frame belongs to.

> [!translation] 逐句繁體中文翻譯
> - Access ports 被指派給單一 VLAN，而且只在同一 VLAN 的 ports 之間 forward 與 flood traffic。
> - 另一方面，trunk ports 不屬於單一 VLAN；它們可以 forward 多個 VLANs 的 traffic。
> - Figure 12.5 顯示應該使用 trunk port 的情況：LAN 由兩台 switches，SW1 與 SW2 組成，而 VLANs 10、20、30 的 hosts 分別連到每台 switch。
> - 為了讓每個 VLAN 中的 hosts 能彼此通訊，SW1 與 SW2 之間的 link 必須能承載多個 VLANs 的 traffic；為了做到這點，SW1 與 SW2 之間送出的 frames 會被 tagged，以指出每個 frame 屬於哪個 VLAN。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-225_744_1366_183_190.jpg)
Figure 12.5 SW1 and SW2 are connected by a trunk link, which can carry traffic in multiple VLANs. SW1 and SW2 are two physical switches, each consisting of three virtual switches-one for each VLAN. (1) PC1 (connected to SW1) sends a frame addressed to PC10's MAC. (2) SW1 forwards the frame out of its G0/0 port, which is in trunk mode. It adds a tag to the frame, indicating that the frame is in VLAN 10. (3) SW2 forwards the frame out of its G0/1 port (untagged).

NOTE Instead of connecting SW1 and SW2 with a single trunk link, another option is to use a separate access link between the switches for each VLAN (one for VLAN 10, one for VLAN 20, and one for VLAN 30). Although this could work in small networks with few VLANs, this does not scale to networks with many VLANs; a trunk link is a better option.

> [!translation] 逐句繁體中文翻譯
> - NOTE：除了用單一 trunk link 連接 SW1 與 SW2，另一個選項是為每個 VLAN 使用一條獨立 access link 連接 switches，例如 VLAN 10 一條、VLAN 20 一條、VLAN 30 一條。
> - 雖然這在 VLAN 數很少的小型 networks 中可行，但在有很多 VLANs 的 networks 中無法擴展；trunk link 是更好的選擇。

That's how trunk ports work: the switch forwarding a frame adds a tag before sending it out of the trunk port. For that reason, another name for a trunk port is a tagged port. The switch receiving the frame then checks the tag and assigns the frame to the VLAN specified by the tag. If the frame's destination is in a different VLAN than the one specified by the tag, the switch won't be able to forward the frame to its proper destination; remember, hosts in different VLANs can't communicate directly with each other.

> [!translation] 逐句繁體中文翻譯
> - Trunk ports 的運作方式就是這樣：forwarding frame 的 switch 會在 frame 從 trunk port 送出之前加上 tag。
> - 因此 trunk port 的另一個名稱是 tagged port。
> - 接收 frame 的 switch 接著檢查 tag，並把 frame 指派到 tag 指定的 VLAN。
> - 如果 frame 的 destination 位於不同於 tag 所指定的 VLAN，switch 就無法把 frame forward 到正確目的地；請記得，不同 VLAN 的 hosts 不能直接彼此通訊。

Likewise, another name for an access port is an untagged port; frames forwarded by an access port are not tagged to indicate the VLAN, and frames received by an access port are assigned to the VLAN specified in the switchport access vlan command. Because access ports are associated with only one VLAN, a tag is not necessary to identify which VLAN frames that are sent and received by the port belong to.

> [!translation] 逐句繁體中文翻譯
> - 同樣地，access port 的另一個名稱是 untagged port；access port 送出的 frames 不會加上 VLAN tag，而 access port 收到的 frames 會被指派到 switchport access vlan command 指定的 VLAN。
> - 因為 access ports 只關聯一個 VLAN，所以不需要 tag 來識別該 port 收送的 frames 屬於哪個 VLAN。

NOTE Access ports are typically used to connect to end hosts, such as PCs. Trunk ports are typically used to connect to other switches (and sometimes routers, as we'll see in section 12.4).

> [!translation] 逐句繁體中文翻譯
> - NOTE：Access ports 通常用來連接 end hosts，例如 PCs。
> - Trunk ports 通常用來連接其他 switches，有時也會連接 routers，如 12.4 節會看到。

### 12.3.1 The IEEE 802.1Q tag

The protocol used to tag frames forwarded out of trunk ports is IEEE 802.1Q (typically pronounced "dot one Q"). The 802.1Q tag is 4 bytes in length and is added in between the Source and EtherType fields of the Ethernet header. Figure 12.6 shows the position of the 802.1Q tag within a frame, as well as the fields of the tag.

> [!translation] 逐句繁體中文翻譯
> - 用於標記 trunk ports 送出 frames 的 protocol 是 IEEE 802.1Q，通常發音為 dot one Q。
> - 802.1Q tag 長度為 4 bytes，插入 Ethernet header 的 Source 與 EtherType fields 之間。
> - Figure 12.6 顯示 802.1Q tag 在 frame 中的位置，以及 tag 的 fields。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-226_314_1298_457_225.jpg)
Figure 12.6 The 802.1Q tag's position in an Ethernet frame and the field of the tag. The fields are TPID (Tag Protocol Identifier) and TCI (Tag Control Information). TCI contains three subfields: PCP (Priority Code Point), DEI (Drop Eligible Indicator), and VID (VLAN Identifier).

The Tag Protocol Identifier (TPID) field is 16 bits in length and always contains the value 0x8100. When a frame is 802.1Q tagged, the TPID field is in the position the EtherType field would normally be. When the switch sees the value $0 \times 8100$ here, it knows the frame is tagged using 802.1Q; that's the purpose of the TPID field.

> [!translation] 逐句繁體中文翻譯
> - Tag Protocol Identifier（TPID）field 長度為 16 bits，且永遠包含 0x8100 這個值。
> - 當 frame 被 802.1Q tagged 時，TPID field 位於 EtherType field 通常所在的位置。
> - 當 switch 在此處看到 $0\times8100$ 這個值時，就知道 frame 使用 802.1Q tagged；這就是 TPID field 的用途。

The second half of the 802.1Q is the Tag Control Information (TCI), which contains three subfields: PCP, DEI, and VID. The Priority Code Point (PCP) field is 3 bits in length and can be used to mark frames as higher or lower in priority; this is used for Quality of Service (QoS), a topic we will cover in chapter 10 of volume 2. The Drop Eligible Indicator (DEI) field is a single bit in length and is also used for QoS; it can be used to indicate frames that can be dropped if the network is congested.

> [!translation] 逐句繁體中文翻譯
> - 802.1Q 的後半部是 Tag Control Information（TCI），其中包含三個 subfields：PCP、DEI 與 VID。
> - Priority Code Point（PCP）field 長度為 3 bits，可用來把 frames 標記為較高或較低 priority；這會用於 Quality of Service（QoS），我們會在 volume 2 第 10 章介紹。
> - Drop Eligible Indicator（DEI）field 長度為 1 bit，也用於 QoS；它可用來指出 network congested 時可被 drop 的 frames。

The VLAN Identifier (VID) field is perhaps the most important; it's the field that indicates which VLAN the frame is in. It is 12 bits in length, and that's why there are 4,096 VLANs in total ( $2^{12}=4096$ ).

> [!translation] 逐句繁體中文翻譯
> - VLAN Identifier（VID）field 也許是最重要的 field；它指出 frame 位於哪個 VLAN。
> - 它長度為 12 bits，這也是為什麼 VLAN 總共有 4,096 個（$2^{12}=4096$）。

## Cisco Inter-Switch Link

Before IEEE 802.1Q, Cisco developed a protocol called Inter-Switch Link (ISL) to tag frames over trunk links. As a Cisco-proprietary protocol, ISL can only be used by Cisco switches. Whereas 802.1Q adds a 4-byte tag to the Ethernet header, ISL encapsulates the Ethernet frame with a 26-byte header and 4-byte trailer, containing an FCS (separate from the Ethernet trailer's FCS).

> [!translation] 逐句繁體中文翻譯
> - 在 IEEE 802.1Q 之前，Cisco 開發了一個稱為 Inter-Switch Link（ISL）的 protocol，用於在 trunk links 上 tag frames。
> - 作為 Cisco-proprietary protocol，ISL 只能由 Cisco switches 使用。
> - 802.1Q 是在 Ethernet header 中加入 4-byte tag；ISL 則用 26-byte header 與 4-byte trailer encapsulate Ethernet frame，其中包含一個與 Ethernet trailer FCS 分開的 FCS。

ISL is now considered deprecated and is not supported on new Cisco switches. However, you may still encounter Cisco switches that support both 802.1Q and ISL; in such cases, an extra command is required when configuring trunk ports, as we will cover in section 12.3.2. Although you don't have to know ISL itself for the CCNA exam, you should understand how it affects trunk configuration on switches that support it (by requiring an extra command).

> [!translation] 逐句繁體中文翻譯
> - ISL 現在被視為 deprecated，新的 Cisco switches 不再支援。
> - 不過，你仍可能遇到同時支援 802.1Q 與 ISL 的 Cisco switches；在這些情況下，設定 trunk ports 時需要額外 command，我們會在 12.3.2 節介紹。
> - 雖然 CCNA 考試不要求你知道 ISL 本身，但你應該理解它如何影響支援 ISL 的 switches 上的 trunk configuration，也就是會需要額外 command。

### 12.3.2 Configuring trunk ports

To demonstrate the configuration of trunk ports, let's configure SW1's G0/0 port as we saw in figure 12.5-a trunk link capable of carrying traffic in VLANs 10, 20, and 30. Although I will only demonstrate SW1's side of the link, if you're trying this out yourself, make sure that SW2's G0/0 port is configured to match (with the same commands as on SW1). In the following example, I attempt to configure SW1 G0/0 as a trunk, but the command is rejected.

> [!translation] 逐句繁體中文翻譯
> - 為了示範 trunk ports 的設定，我們把 SW1 的 G0/0 port 設成 Figure 12.5 所示的 trunk link，可承載 VLANs 10、20、30 的 traffic。
> - 雖然我只示範 SW1 端的 link，但如果你自己實作，請確保 SW2 的 G0/0 port 也設定成相同狀態，也就是使用與 SW1 相同的 commands。
> - 下面例子中，我嘗試把 SW1 G0/0 設為 trunk，但 command 被拒絕。

```
SW1(config)# interface g0/0
SW1(config-if) # switchport mode trunk
Command rejected: An interface whose trunk encapsulation
-is "Auto" can not be configured to "trunk" mode.
```

The command is rejected.

> [!translation] 逐句繁體中文翻譯
> - Command 被拒絕。

The reason the command is rejected is that SW1 supports both 802.1Q and ISL. By default, ports on a switch that supports both 802.1Q and ISL will use DTP (mentioned earlier in section 12.2.2) to automatically determine which of the two protocols to use for the trunk. However, to manually configure the port in trunk mode, you must also manually configure the encapsulation protocol (802.1Q or ISL); you can't manually configure trunk mode, but you can allow DTP to automatically determine whether to use 802.1Q or ISL. The command to configure which protocol to use is switchport trunk encapsulation \{dot1q | isl\}. In the following example, I configure G0/0 to use 802.1Q, and then I can successfully configure G0/0 as a trunk port:

> [!translation] 逐句繁體中文翻譯
> - Command 被拒絕的原因是 SW1 同時支援 802.1Q 與 ISL。
> - 預設情況下，同時支援 802.1Q 與 ISL 的 switch ports 會使用前面 12.2.2 節提過的 DTP，自動判斷 trunk 要使用哪個 protocol。
> - 不過，若要手動把 port 設為 trunk mode，也必須手動設定 encapsulation protocol，也就是 802.1Q 或 ISL；你不能手動設定 trunk mode，卻讓 DTP 自動判斷使用 802.1Q 或 ISL。
> - 設定使用哪個 protocol 的 command 是 switchport trunk encapsulation {dot1q | isl}。
> - 下面例子中，我把 G0/0 設為使用 802.1Q，然後就能成功把 G0/0 設為 trunk port。

```
SW1(config-if)# switchport trunk encapsulation dot1q
SW1(config-if) # switchport mode trunk
```

Manually configures

> [!translation] 逐句繁體中文翻譯
> - 手動設定。

```
SW1(config-if) #
```

802.1Q encapsulation

> [!translation] 逐句繁體中文翻譯
> - 802.1Q encapsulation。

NOTE If a switch only supports 802.1Q (not ISL), it is not necessary to use the switchport trunk encapsulation command before switchport mode trunk; in fact, the switch won't even support the switchport trunk encapsulation command.

> [!translation] 逐句繁體中文翻譯
> - NOTE：如果 switch 只支援 802.1Q 而不支援 ISL，就不需要在 switchport mode trunk 之前使用 switchport trunk encapsulation command；事實上，該 switch 甚至不會支援 switchport trunk encapsulation command。

After configuring a port as a trunk, it will no longer appear in the output of show vlan brief. The example below demonstrates this; G0/0 is not present in the output. Note that I configured SW1's access ports in their appropriate VLANs, according to figure 12.5:

> [!translation] 逐句繁體中文翻譯
> - 把 port 設為 trunk 後，它就不會再出現在 show vlan brief 的輸出中。
> - 下面例子示範這點；G0/0 沒有出現在輸出中。
> - 請注意，我已經依照 Figure 12.5，把 SW1 的 access ports 設定到適當 VLAN。

```
        GO/1, G1/0, and G1/1 are access ports in VLAN 10.
            GO/1, G1/0, and G1/1 are access ports in VLAN 10.
        Status Ports
    --------- -------------------------------
    active Gi2/0, Gi2/1, Gi2/2, Gi2/3
    active GiO/1, Gil/O, Gil/1
    active Gi0/2, Gil/2
    active Gi0/3, Gil/3
            GO/2 and G1/2
            are access ports
GO/3 and G1/3 are access ports in VLAN 30.
            in VLAN 20.
```

To verify trunk ports, you can use the command show interfaces trunk, as shown in the following example. The output is divided into four parts, but we will focus on the first three:
![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-228_445_1300_339_346.jpg)

> [!translation] 逐句繁體中文翻譯
> - 要驗證 trunk ports，可以使用 show interfaces trunk command，如下面例子所示。
> - 輸出分成四個部分，但我們會聚焦在前三個部分。

The first section (the top two lines) lists each trunk port and some basic information. The value of on in the Mode column means that G0/0 is manually configured as a trunk (with the switchport mode trunk command). The encapsulation column is self-explanatory; the value is 802.1q because I configured the switchport trunk encapsulation dot1q command earlier. The Status column says trunking; this is expected because I manually configured G0/0 in trunk mode. The final column is Native vlan; the native VLAN is an important topic to understand for the CCNA, and we will cover it in this section.

> [!translation] 逐句繁體中文翻譯
> - 第一個 section，也就是最上面兩行，列出每個 trunk port 與一些基本資訊。
> - Mode 欄位中的 on 表示 G0/0 被手動設定為 trunk，也就是使用 switchport mode trunk command。
> - Encapsulation 欄位本身很直覺；值是 802.1q，因為我前面設定了 switchport trunk encapsulation dot1q command。
> - Status 欄位顯示 trunking；這是預期結果，因為我手動把 G0/0 設成 trunk mode。
> - 最後一欄是 Native vlan；native VLAN 是 CCNA 必須理解的重要主題，本節會介紹。

The second part of the output lists the VLANs allowed on each trunk port (vlans allowed on trunk). As indicated by 1-4094, all VLANs are allowed on a trunk port by default; this means that traffic in all VLANs can be forwarded and received by the port.

> [!translation] 逐句繁體中文翻譯
> - 輸出的第二部分列出每個 trunk port 允許的 VLANs，也就是 vlans allowed on trunk。
> - 由 1-4094 可知，trunk port 預設允許所有 VLANs；這表示所有 VLANs 的 traffic 都可以被該 port forward 與 receive。

However, the following part lists the VLANs that are allowed and exist on the switch (Vlans allowed and active in management domain). VLAN 1 exists by default, and I created VLANs 10, 20, and 30, so those four are listed here. If a VLAN does not exist on a switch, it cannot forward traffic in that VLAN; therefore, although all VLANs are allowed on the trunk, SW1 can only forward traffic in VLANs 1, 10, 20, and 30.

> [!translation] 逐句繁體中文翻譯
> - 不過，下一部分列出的是 switch 上 allowed 且 actually exist 的 VLANs，也就是 Vlans allowed and active in management domain。
> - VLAN 1 預設存在，而我建立了 VLANs 10、20、30，所以這四個 VLAN 會列在這裡。
> - 如果某個 VLAN 不存在於 switch 上，該 switch 就不能 forward 該 VLAN 的 traffic；因此，雖然 trunk 允許所有 VLANs，SW1 實際只能 forward VLANs 1、10、20、30 的 traffic。

NOTE The management domain referred to in the line vlans allowed and active in management domain is a reference to the VLAN Trunking Protocol (VTP) domain. VTP is one of the topics of chapter 13, so I won't mention it any further in this chapter.

> [!translation] 逐句繁體中文翻譯
> - NOTE：vlans allowed and active in management domain 這行中的 management domain，是指 VLAN Trunking Protocol（VTP）domain。
> - VTP 是第 13 章主題之一，所以本章不再進一步說明。

## Modifying the list of allowed VLANs

Although all VLANs are allowed on a trunk port by default, it is considered best practice to allow only the necessary VLANs. This can help to limit the size of broadcast domains; if a VLAN isn't allowed on a trunk, broadcast (and unknown unicast) frames in that VLAN won't be flooded out of the interface. The command to configure the list of VLANs allowed on the trunk is switchport trunk allowed vlan, and then there are several possible keywords and arguments, as shown in the following example:

> [!translation] 逐句繁體中文翻譯
> - 雖然 trunk port 預設允許所有 VLANs，但最佳實務是只允許必要 VLANs。
> - 這可幫助限制 broadcast domains 的大小；如果某 VLAN 不被允許通過 trunk，該 VLAN 中的 broadcast 與 unknown unicast frames 就不會從此 interface 被 flood 出去。
> - 設定 trunk 上 allowed VLANs 清單的 command 是 switchport trunk allowed vlan，後面可接數個 keywords 與 arguments，如下面例子所示。

Configures the VLANs allowed on the trunk

> [!translation] 逐句繁體中文翻譯
> - 設定 trunk 上允許的 VLANs。

```
SW1(config-if)# switchport trunk allowed vlan
    WORD VLAN IDs of the allowed VLANs when this port is
    - in trunking mode
    add add VLANs to the current list
    all all VLANs
```

The available keywords

> [!translation] 逐句繁體中文翻譯
> - 可用的 keywords。

```
    except all VLANs except the following
```

and arguments

> [!translation] 逐句繁體中文翻譯
> - 以及 arguments。

```
    none no VLANs
    remove remove VLANs from the current list
```

WORD allows you to specify the list of VLANs allowed on the trunk as an argument, such as switchport trunk allowed vlan 10,20,30; this will allow only VLANs 10, 20, and 30 on the trunk. This is the desired state for the network we saw in figure 12.5, which uses only VLANs 10, 20, and 30. I demonstrate this configuration in the following example:

> [!translation] 逐句繁體中文翻譯
> - WORD 讓你能以 argument 指定 trunk 上允許的 VLANs 清單，例如 switchport trunk allowed vlan 10,20,30；這會只允許 VLANs 10、20、30 通過 trunk。
> - 這正是 Figure 12.5 network 的 desired state，因為該 network 只使用 VLANs 10、20、30。
> - 下面例子示範此設定。

Allows VLANs 10, 20, and 30

> [!translation] 逐句繁體中文翻譯
> - 允許 VLANs 10、20、30。

```
SW1(config-if)# switchport trunk allowed vlan 10,20,30
SW1(config-if) # do show interfaces trunk
. . .
Port Vlans allowed on trunk
```

Only VLANs 10, 20, and 30

> [!translation] 逐句繁體中文翻譯
> - 只有 VLANs 10、20、30。

```
Gi0/0 10,20,30
```

are allowed on G0/0.

> [!translation] 逐句繁體中文翻譯
> - 被允許在 G0/0 上通過。

The other options are keywords, and for the CCNA exam, it's important to understand how each keyword functions. add and remove are used to modify the current list of allowed VLANs. In the following example, I add VLAN 1 and remove VLAN 30 from the list of allowed VLANs; the list of allowed VLANs then changes to 1, 10, and 20:

> [!translation] 逐句繁體中文翻譯
> - 其他 options 是 keywords；對 CCNA 考試而言，理解每個 keyword 的功能很重要。
> - add 與 remove 用來修改目前 allowed VLANs 清單。
> - 下面例子中，我把 VLAN 1 加入 allowed VLANs 清單，並把 VLAN 30 從清單移除；allowed VLANs 清單因此變成 1、10、20。

```
Adds VLAN 1 to GO/O’s list of allowed VLANs
```

Removes VLAN 30 from GO/0's list of
SW1(config-if)\# switchport trunk allowed vlan add 1 allowed VLANs SW1(config-if)\# switchport trunk allowed vlan remove 30 SW1(config-if)\# do show interfaces trunk

> [!translation] 逐句繁體中文翻譯
> - 從 G0/0 的 allowed VLANs 清單移除 VLAN 30，並使用相關 commands 顯示 interfaces trunk。

```
Port Vlans allowed on trunk
Gi0/0 1,10,20
```

VLANs 1, 10, and 20 are allowed on G0/0.

> [!translation] 逐句繁體中文翻譯
> - VLANs 1、10、20 被允許在 G0/0 上通過。

The all and none keywords are self-explanatory; all allows all VLANs (the default setting), and none allows no VLANs, preventing the port from forwarding or receiving any traffic. In the following example, I demonstrate both keywords:

> [!translation] 逐句繁體中文翻譯
> - all 與 none keywords 的意思很直覺；all 允許所有 VLANs，這是預設設定；none 不允許任何 VLANs，會阻止該 port forward 或 receive 任何 traffic。
> - 下面例子示範這兩個 keywords。

```
SW1(config-if)# switchport trunk allowed vlan all
SW1(config-if) # do show interfaces trunk
```

```
Allows all VLANs
```

on GO/0

> [!translation] 逐句繁體中文翻譯
> - 在 G0/0 上。

All VLANs are allowed on G0/0.

> [!translation] 逐句繁體中文翻譯
> - 所有 VLANs 都被允許在 G0/0 上通過。

Allows no VLANs
Allows no VLANs
Allows no VLANs on GO/0 on GO/0 on GO/0
. . .

> [!translation] 逐句繁體中文翻譯
> - 不允許任何 VLANs 在 G0/0 上通過。

```
Port Vlans allowed on trunk
Gi0/0 none
```

No VLANs are allowed on G0/0.

> [!translation] 逐句繁體中文翻譯
> - G0/0 上不允許任何 VLANs。

The final keyword is except, which allows all VLANs except the VLAN(s) you specify as an argument. In the following example, I return the list of allowed VLANs to the desired state (allowing only VLANs 10, 20, and 30) by using the except keyword and specifying all VLANs except 10, 20, and 30 (a bit unconventional, but this is just a demonstration!):

> [!translation] 逐句繁體中文翻譯
> - 最後一個 keyword 是 except，它允許除了你指定為 argument 的 VLAN 或 VLANs 以外的所有 VLANs。
> - 下面例子中，我使用 except keyword，並指定除了 10、20、30 以外的所有 VLANs，將 allowed VLANs 清單恢復到 desired state，也就是只允許 VLANs 10、20、30；這有點不尋常，但只是示範。

```
Allows all VLANs on GO/0
except 1-9, 11-19,
SW1(config-if)# switchport trunk allowed vlan except
21-29, and 31-4094
= 1-9,11-19,21-29,31-4094
SW1(config-if)# do show interfaces trunk
. . .
Port Vlans allowed on trunk
```

Only VLANs 10, 20, and 30

> [!translation] 逐句繁體中文翻譯
> - 只有 VLANs 10、20、30。

```
Gi0/0 10,20,30
```

are allowed on G0/0.

> [!translation] 逐句繁體中文翻譯
> - 被允許在 G0/0 上通過。

```
. . .
```


## Don't forget add!

A common rookie mistake (and the subject of many networking memes-yes, such a thing exists!) is to forget the add keyword when modifying the list of allowed VLANs on a trunk. For example, if you want to add VLAN 40 to the list of allowed VLANs, but you use the command switchport trunk allowed vlan 40, you haven't added VLAN 40 to the list of allowed VLANs; you have replaced the list of allowed VLANs with only VLAN 40!

> [!translation] 逐句繁體中文翻譯
> - 常見的新手錯誤，也是許多 network memes 的題材，是修改 trunk 的 allowed VLANs 清單時忘記 add keyword。
> - 例如，如果你想把 VLAN 40 加入 allowed VLANs 清單，但使用 switchport trunk allowed vlan 40，你並沒有把 VLAN 40 加入清單；你是把 allowed VLANs 清單替換成只有 VLAN 40。

It's a simple mistake, but the results can be disastrous: blocking all communications over the trunk except for hosts in a single VLAN. This is a potential "trick question" on the exam, so make sure you are aware of the difference between specifying the list of allowed VLANs (switchport trunk allowed vlan vlans) and adding to the list of allowed VLANs (switchport trunk allowed vlan add vlans).

> [!translation] 逐句繁體中文翻譯
> - 這是簡單錯誤，但結果可能很嚴重：除了單一 VLAN 中的 hosts 以外，trunk 上所有通訊都被阻擋。
> - 這可能是考試中的 trick question，所以務必理解指定 allowed VLANs 清單（switchport trunk allowed vlan vlans）與加入 allowed VLANs 清單（switchport trunk allowed vlan add vlans）之間的差異。

## The native VLAN

As mentioned in section 12.2, access ports (untagged ports) send and receive frames without 802.1Q tags. Trunk ports (tagged ports), on the other hand, send and receive frames with 802.1Q tags to indicate which VLAN each frame belongs to, but what happens if a switch receives an untagged frame on a trunk port? The native VLAN is the answer to that question.

> [!translation] 逐句繁體中文翻譯
> - 如 12.2 節所述，access ports（untagged ports）收送 frames 時不帶 802.1Q tags。
> - 另一方面，trunk ports（tagged ports）收送 frames 時會使用 802.1Q tags 來指出每個 frame 屬於哪個 VLAN；但如果 switch 在 trunk port 上收到 untagged frame，會發生什麼事？Native VLAN 就是這個問題的答案。

The native VLAN is the VLAN that untagged traffic received on a trunk port is assigned to. Furthermore, any traffic in the native VLAN forwarded by a trunk port is forwarded without a tag. By default, the native VLAN is VLAN 1, as shown in the output of show interfaces trunk:

> [!translation] 逐句繁體中文翻譯
> - Native VLAN 是 trunk port 收到 untagged traffic 時會被指派到的 VLAN。
> - 此外，trunk port forward native VLAN 中的 traffic 時，也會不加 tag。
> - 預設 native VLAN 是 VLAN 1，如 show interfaces trunk 輸出所示。

```
SW1# show interfaces trunk
Port Mode
Gi0/0 on
. . .
```

```
Encapsulation Status
Native vlan
802.1q
1
```

VLAN 1 is the native VLAN by default.

> [!translation] 逐句繁體中文翻譯
> - VLAN 1 預設是 native VLAN。

EXAM TIP The default VLAN and the native VLAN are often confused. The default VLAN is the VLAN that access ports are assigned to by default: VLAN 1 (this cannot be changed). The native VLAN is the VLAN that untagged frames are assigned to when received on a trunk port, and frames in the native VLAN are forwarded untagged over that port. The native VLAN is also VLAN 1 by default, but this can be changed per port.

> [!translation] 逐句繁體中文翻譯
> - EXAM TIP：Default VLAN 與 native VLAN 常被混淆。
> - Default VLAN 是 access ports 預設被指派到的 VLAN：VLAN 1，且這不能改變。
> - Native VLAN 則是在 trunk port 收到 untagged frames 時指派給它們的 VLAN，而且 native VLAN 中的 frames 會在該 port 上以 untagged 方式 forward。
> - Native VLAN 預設也是 VLAN 1，但可以 per port 修改。

To configure the native VLAN of a trunk port, use the switchport trunk native vlan vlan-id command. In the following example, I configure VLAN 30 as the native VLAN on SW1's G0/0 interface and then confirm with show interfaces trunk:

> [!translation] 逐句繁體中文翻譯
> - 要設定 trunk port 的 native VLAN，使用 switchport trunk native vlan vlan-id command。
> - 下面例子中，我在 SW1 的 G0/0 interface 上把 VLAN 30 設為 native VLAN，然後用 show interfaces trunk 確認。

```
Changes GO/0’s native
VLAN to VLAN 30
SW1(config-if)# switchport trunk native vlan 30
SW1(config-if)# show interfaces trunk
Port Mode Encapsulation Status
Gi0/0 on 802.1q trunking 30 native VLAN.
```

Figure 12.7 shows how traffic in the native VLAN is forwarded over a trunk link. The frame from PC1 to PC10 (both in VLAN 10) is tagged when SW1 forwards it to SW2. The frame from PC4 to PC5 (both in VLAN 30), however, is not tagged when SW1 forwards it to SW2; VLAN 30 is the native VLAN on SW1's G0/0 port. Likewise, VLAN 30 is the native VLAN on SW2's G0/0 port, so when the frame is received by SW2, SW2 assigns the frame to VLAN 30 and forwards it to the destination (which is also in VLAN 30).

> [!translation] 逐句繁體中文翻譯
> - Figure 12.7 顯示 native VLAN 的 traffic 如何透過 trunk link forward。
> - PC1 到 PC10 的 frame 兩者都在 VLAN 10，SW1 forward 到 SW2 時會加 tag。
> - PC4 到 PC5 的 frame 兩者都在 VLAN 30，但 SW1 forward 到 SW2 時不會加 tag；VLAN 30 是 SW1 G0/0 port 的 native VLAN。
> - 同樣地，VLAN 30 也是 SW2 G0/0 port 的 native VLAN，所以 SW2 收到 frame 時，會把該 frame 指派到 VLAN 30，並 forward 到同樣位於 VLAN 30 的 destination。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-231_763_1420_1210_190.jpg)
Figure 12.7 Frames forwarded over a trunk link in the native VLAN and a non-native VLAN. (1) PC1's frame to PC10 is tagged over the trunk link because VLAN 10 is not the native VLAN. (2) PC4's frame to PC5 is untagged over the trunk link because VLAN 30 is the native VLAN.

NOTE The native VLAN is configured per port. If a switch has multiple trunk ports, it is possible to configure a different native VLAN on each port.

> [!translation] 逐句繁體中文翻譯
> - NOTE：Native VLAN 是 per port 設定的。
> - 如果 switch 有多個 trunk ports，每個 port 都可能設定不同 native VLAN。

## Native VLAN Mismatch

Because the native VLAN is configured on each switch's ports, it is possible to configure a different native VLAN on each end of a link. However, this is a misconfiguration and should not be done. Make sure the native VLAN matches on both ends of the link! Figure 12.8 shows one example of what can happen when there is a native VLAN mismatch.

> [!translation] 逐句繁體中文翻譯
> - 因為 native VLAN 是設定在每台 switch 的 ports 上，所以同一條 link 兩端可以設定不同 native VLAN。
> - 然而，這是錯誤設定，不應該這樣做。
> - 請確保 link 兩端 native VLAN 相同！Figure 12.8 顯示 native VLAN mismatch 時可能發生的一個例子。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-232_763_1418_618_223.jpg)
Figure 12.8 A native VLAN mismatch resulting in frames not reaching their destination. SW1 G0/0's native VLAN is VLAN 10, and SW2 GO/0's is VLAN 30. (1) PC1's frame to PC10 is untagged over the trunk link because VLAN 10 is SW1 G0/0's native VLAN. (2) When SW2 receives the frame, it assigns the frame to VLAN 30 (SW2 GO/0's native VLAN) and therefore cannot forward the frame to its destination (in VLAN 10).

SW1 G0/0's native VLAN is 10, but SW2 G0/0's native VLAN is 30. When PC1 sends a frame to PC10, SW1 forwards the frame untagged to SW2. However, when SW2 receives the untagged frame, it assigns the frame to VLAN 30 (SW2 G0/0's native VLAN). Because the frame's destination is connected to SW2's G0/1 (an access port in VLAN 10), SW2 cannot forward the frame to its proper destination. When traffic crosses from one VLAN to another like this, it is called VLAN hopping.

> [!translation] 逐句繁體中文翻譯
> - SW1 G0/0 的 native VLAN 是 10，但 SW2 G0/0 的 native VLAN 是 30。
> - 當 PC1 送 frame 給 PC10 時，SW1 會把 frame 以 untagged 方式 forward 給 SW2。
> - 不過 SW2 收到 untagged frame 時，會把它指派到 VLAN 30，也就是 SW2 G0/0 的 native VLAN。
> - 因為 frame 的 destination 連到 SW2 G0/1，而該 port 是 VLAN 10 的 access port，SW2 無法把 frame forward 到正確目的地。
> - 當 traffic 以這種方式從一個 VLAN 跨到另一個 VLAN，稱為 VLAN hopping。

NOTE Cisco switches typically run Per-VLAN Spanning Tree Plus (PVST+) or Rapid Per-VLAN Spanning Tree Plus (Rapid-PVST+). If there is a native VLAN mismatch, these protocols will prevent traffic from being forwarded over the trunk in the mismatched VLANs and display a message indicating so. We will cover PVST+ and Rapid-PVST+ in chapters 14 and 15, respectively. Cisco Discovery Protocol (CDP) can also detect native VLAN mismatches but will not block traffic in the mismatched VLANs; it will only display messages indicating the mismatch. We will cover CDP in chapter 1 of volume 2.

> [!translation] 逐句繁體中文翻譯
> - NOTE：Cisco switches 通常會執行 Per-VLAN Spanning Tree Plus（PVST+）或 Rapid Per-VLAN Spanning Tree Plus（Rapid-PVST+）。
> - 如果有 native VLAN mismatch，這些 protocols 會阻止 mismatched VLANs 的 traffic 在 trunk 上 forward，並顯示相關訊息。
> - 我們會分別在第 14 與 15 章介紹 PVST+ 與 Rapid-PVST+。
> - Cisco Discovery Protocol（CDP）也能偵測 native VLAN mismatches，但不會阻擋 mismatched VLANs 的 traffic；它只會顯示 mismatch 訊息。
> - 我們會在 volume 2 第 1 章介紹 CDP。

## Disabling the native VLAN

The native VLAN was developed to accommodate devices that do not support 802.1Q tagging, such as hubs. However, these days, there is usually no need to use the native VLAN, and its use can render the network vulnerable to security exploits. Therefore, it is best practice to disable the native VLAN on trunk ports.

> [!translation] 逐句繁體中文翻譯
> - Native VLAN 最初是為了容納不支援 802.1Q tagging 的設備，例如 hubs。
> - 不過現在通常沒有使用 native VLAN 的必要，而且使用它可能讓 network 暴露在 security exploits 中。
> - 因此，最佳實務是在 trunk ports 上停用 native VLAN。

However, the native VLAN feature can't actually be disabled; rather, an unused VLAN (that is not the default of VLAN 1) should be configured as the native VLAN, which is equivalent to disabling it. The network I have been using for demonstrations in this chapter uses VLANs 10, 20, and 30, so I could configure switchport trunk native vlan 999 on SW1 and SW2's G0/0 ports to configure VLAN 999-an unused VLAN-as the native VLAN.

> [!translation] 逐句繁體中文翻譯
> - 然而，native VLAN feature 其實不能真正停用；比較正確的做法是把一個未使用的 VLAN，而且不是預設 VLAN 1，設定為 native VLAN，這等同於停用它。
> - 我本章示範的 network 使用 VLANs 10、20、30，所以我可以在 SW1 與 SW2 的 G0/0 ports 上設定 switchport trunk native vlan 999，把未使用的 VLAN 999 設為 native VLAN。

EXAM TIP Remember that as a best practice for security: configure an unused VLAN (that isn't the default of VLAN 1) as the native VLAN on your trunk ports.

> [!translation] 逐句繁體中文翻譯
> - EXAM TIP：請記住這項安全最佳實務：在 trunk ports 上把未使用且不是預設 VLAN 1 的 VLAN 設為 native VLAN。

### 12.4 Inter-VLAN routing

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

## Summary

- Network segmentation is the process of dividing a network into smaller parts and provides network security and performance benefits. Subnets can be used to segment a network at Layer 3, and virtual LANs (VLANs) can be used to segment a network at Layer 2.
- VLANs divide a broadcast domain (LAN) into multiple broadcast domains by dividing a physical switch into multiple virtual switches. Frames sent by a host in one VLAN cannot be forwarded/flooded to hosts in another VLAN.
- Although there can be multiple subnets per VLAN, for the CCNA exam, you can assume a one-to-one relationship (one subnet per VLAN).
- Use the show vlan brief command to view the VLANs that exist on the switch and which ports are in each VLAN.
- VLANs 1 and 1002-1005 exist by default and cannot be deleted. VLAN 1 is the default VLAN-the VLAN that all ports are in by default. VLANS 1002-1005 are reserved for use by FDDI and Token Ring-two legacy Data Link Layer technologies.
- Use the vlan vlan-id command to create a VLAN, and then the name vlan -name command to give the VLAN an optional name (the default name is VLANxxxx). You can use the shutdown command to temporarily disable the VLAN or no vlan vlan-id to delete the VLAN.
- An access port is a switch port that sends and receives traffic in a single VLAN. Access ports are also called untagged ports because they send and receive frames without VLAN tags.
- Use the switchport mode access command to configure a port in access mode. Then, use the switchport access vlan vlan-id command to configure which VLAN the port belongs to. If you assign a port to a VLAN that doesn't exist yet on the switch, the switch will automatically create the VLAN.
- A trunk port is a switch port that sends and receives traffic in multiple VLANs. Trunk ports differentiate between VLANs by adding a VLAN tag to each frame using the IEEE 802.1Q protocol.
- The 802.1Q tag is 4 bytes in length and is added between the Source and EtherType fields of the Ethernet header. The main fields of the 802.1Q tag are TPID and TCI.

- The Tag Protocol Identifier (TPID) field always contains the value 0x8100; it is used to identify 802.1Q-tagged frames.
- The Tag Control Information (TCI) field consists of three subfields: Priority Code Point (PCP) and Drop Eligible Indicator (DEI) are used for Quality of Service (QoS). The VLAN Identifier (VID) field is used to indicate which VLAN the frame is in. The VID field is 12 bits in length, and for that reason, there are $4,096\left(2^{12}\right)$ VLANs in total.
- To configure a trunk port, use the switchport mode trunk command. If the switch supports both 802.1Q and ISL, you must use the switchport trunk encapsulation dot1q command first; if the switch only supports 802.1Q, this command is not needed.
- Use the show interfaces trunk command to verify trunk ports, including information such as which VLANs are allowed on each trunk port.
- By default, all VLANs are allowed on a trunk port, meaning it can forward and receive frames in all VLANs.
- Use the switchport trunk allowed vlan command to specify the VLANs allowed on a trunk. You can specify the list of VLANs or use the keywords add, all, except, none, or remove.
- The native VLAN is the VLAN that is untagged on a trunk port. Untagged frames received on a trunk port are assigned to the native VLAN, and frames in the native VLAN are forwarded untagged. The native VLAN of a trunk port is VLAN 1 by default.
- The native VLAN can be configured with the switchport trunk native vlan vlan-id command. The command is configured per port, so each port on a switch can have a different native VLAN, but make sure the native VLAN matches on both sides of a trunk connection.
- It is recommended that you configure an unused VLAN (that is not the default of VLAN 1) as the native VLAN, which is equivalent to disabling it.
- Inter-VLAN routing is the process of routing between subnets in a LAN that is segmented using VLANs. Inter-VLAN routing can be performed by an external router or by a multilayer switch (a switch that has routing capabilities).
- A router can perform inter-VLAN routing by using a separate interface per subnet/VLAN or by router on a stick (ROAS), in which a trunk link connects the router and switch.
- ROAS uses virtual subinterfaces. To configure a subinterface, use the interface command and add a period and a number to identify the subinterface to the end of the interface name (i.e., interface g0/0.10). The subinterface identifier does not have to match the VLAN ID.
- Configure a subinterface's VLAN with the encapsulation dot1q vlan-id command. Then, configure an IP address in the same manner as on a router.

- If using the native VLAN over the ROAS trunk, use the encapsulation dot1q vlan-id native command on the native VLAN's subinterface. Or, configure the native VLAN's IP address on the physical interface (the encapsulation dot1q command is not necessary on the physical interface).
- A multilayer switch can also be called a Layer 3 switch (in contrast to a standard Layer 2 switch). A multilayer switch uses switch virtual interfaces (SVIs) to perform inter-VLAN routing. Each SVI is associated with a VLAN and can be configured with the interface vlan vlan-idcommand.
- Use the ip routing command on a multilayer switch to allow the switch to route packets.
- A physical port on a multilayer switch can be configured as a routed port, which functions like a router interface. Use the no switchport command to convert a switch port to a routed port, and then configure an IP address on it.
- To forward packets to external destinations, multilayer switches need routes, just like routers-either static routes or routes learned via a dynamic routing protocol (such as OSPF).
