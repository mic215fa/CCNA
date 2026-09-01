## Routing fundamentals

## This chapter covers

- How end hosts send IP packets to local and remote destinations
- The routing process
- Reading and interpreting a router's routing table
- Configuring static routes on a router
- Using default routes to provide internet connectivity

In this chapter, we will cover routing-the process by which routers forward IP packets between networks. Specifically, we will cover elements of the following CCNA exam topics:

> [!translation] 逐句繁體中文翻譯
> 在本章中，我們會介紹 routing，也就是 routers 在 networks 之間 forward IP packets 的 process。  
> 具體來說，我們會涵蓋下列 CCNA exam topics 的部分內容：


- 3.1 Interpret the components of a routing table
- 3.2 Determine how a router makes a forwarding decision by default
- 3.3 Configure and verify IPv4 and IPv6 static routing

The term routing can actually refer to two different processes: the process by which routers build their routing table (a database of known destinations and how to
forward packets toward them) and the process of actually forwarding packets. In this chapter, we will cover both aspects of routing, and we will build upon this foundation in future chapters of this volume and volume 2.

> [!translation] 逐句繁體中文翻譯
> Routing 這個 term 實際上可以指兩個不同 processes。  
> 第一個是 routers 建立 routing table 的 process；routing table 是 known destinations 以及如何 forward packets toward those destinations 的 database。  
> 第二個是實際 forwarding packets 的 process。  
> 在本章中，我們會涵蓋 routing 的兩個面向，並在本卷與 volume 2 的後續章節建立在這個基礎上。

### 9.1 How end hosts send packets

Before we examine the details of how routers forward IP packets, let's take a look at the end hosts that send those packets to each other. After a host prepares a packet to send to another host, it must encapsulate that packet in a frame; even though we are focusing on routing, a Layer 3 process, do not forget about Layer 2! Packets are never sent over the cable (or radio waves) without being encapsulated in a frame.

> [!translation] 逐句繁體中文翻譯
> 在檢視 routers 如何 forward IP packets 的細節前，先看看彼此傳送 packets 的 end hosts。  
> Host 準備好要傳送給另一個 host 的 packet 後，必須將該 packet encapsulate in a frame。  
> 即使我們聚焦於 routing 這個 Layer 3 process，也不要忘記 Layer 2！  
> Packets 絕不會在沒有 encapsulated in a frame 的情況下，直接透過 cable（或 radio waves）送出。

The destination MAC address of the frame depends on the destination IP address of the packet. If the packet is destined for a host in the same network as the sender, the destination MAC address will be that of the destination host; in this case, there is no need for a router. Figure 9.1 demonstrates this process when PC1 sends a packet to PC2. The destination IP and MAC addresses are both PC2s; there is no need for R1 to route the packet because the source and destination are in the same network (the 192.168.1.0/24 network).

> [!translation] 逐句繁體中文翻譯
> Frame 的 destination MAC address 取決於 packet 的 destination IP address。  
> 如果 packet destined for 與 sender 位於同一 network 的 host，destination MAC address 就會是 destination host 的 MAC address。  
> 在這種情況下，不需要 router。  
> Figure 9.1 示範 PC1 傳送 packet 給 PC2 時的 process。  
> Destination IP 與 MAC addresses 都是 PC2 的；因為 source 與 destination 位於同一 network（192.168.1.0/24 network），所以不需要 R1 route 該 packet。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-161_556_1414_982_192.jpg)
Figure 9.1 PC1 (192.168.1.11) sends a packet to PC2 (192.168.1.12). Because PC1 and PC2 are both in the same network (192.168.1.0/24), PC1 encapsulates the packet in a frame addressed to PC2's MAC address. PC1 does not need to send the packet to R1 for routing. This diagram assumes PC1 already knows PC2's MAC address; if not, PC1 will first send an ARP request to learn PC2's MAC address.

NOTE A cloud icon, as shown in figure 9.1, is often used to represent the internet. However, that is not always its purpose. A cloud icon can be used to summarize elements that are not relevant to the diagram. The cloud in figure 9.1 indicates that R1 connects to another network, the details of which aren't relevant to the diagram. That other network could be the internet, or it could be another part of the same enterprise's network.

> [!translation] 逐句繁體中文翻譯
> 注意：如 figure 9.1 所示，cloud icon 常用來代表 internet。  
> 不過，它不一定總是這個用途。  
> Cloud icon 可用來摘要與 diagram 無關的 elements。  
> Figure 9.1 中的 cloud 表示 R1 連到另一個 network，但該 network 的細節與 diagram 無關。  
> 另一個 network 可能是 internet，也可能是同一 enterprise network 的另一部分。

On the other hand, if an end host like a PC wants to send a packet to a destination outside of its local network, it must send the packet to its default gateway-the router that provides connectivity to other networks. In figure 9.2, R1 is the default gateway of the 192.168.1.0/24 network. For PC1 and PC2 to send packets to destinations outside of 192.168.1.0/24, they must encapsulate the packet in a frame addressed to the MAC address of R1's G0/0 interface. Figure 9.2 demonstrates how PC1 sends a packet to PC3: it encapsulates the packet in a frame, which is addressed to the MAC address of R1's G0/0 interface. R1 then forwards the packet out of its G0/1 interface, encapsulated in a new frame addressed to PC3's MAC address.

> [!translation] 逐句繁體中文翻譯
> 另一方面，如果像 PC 這樣的 end host 想傳送 packet 到 local network 外的 destination，就必須將 packet 送到 default gateway，也就是提供到其他 networks connectivity 的 router。  
> 在 figure 9.2 中，R1 是 192.168.1.0/24 network 的 default gateway。  
> 若 PC1 與 PC2 要將 packets 傳送到 192.168.1.0/24 以外的 destinations，就必須把 packet encapsulate in addressed to R1 G0/0 interface MAC address 的 frame。  
> Figure 9.2 示範 PC1 如何傳送 packet 給 PC3：它將 packet encapsulate in a frame，而該 frame addressed to R1 G0/0 interface 的 MAC address。  
> 接著 R1 從 G0/1 interface forward packet，並將它 encapsulated in addressed to PC3 MAC address 的新 frame。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-162_540_1416_618_223.jpg)
Figure 9.2 PC1 (192.168.1.11) sends a packet to PC3 (192.168.2.11). Because PC1 and PC3 are in separate networks, PC1 sends the packet in a frame addressed to its default gateway's MAC address-that of R1's G0/0 interface. R1 then forwards the packet out of its G0/1 interface, encapsulated in a new frame addressed to PC3's MAC address. This diagram assumes PC1 already knows R1 G0/0's MAC address; if not, PC1 will send an ARP request to learn it. Likewise, R1 must also learn PC3's MAC address.

NOTE The default gateway's IP address is usually the first usable address of the network. For example, in the 192.168.1.0/24 network, it's 192.168.1.1, and in the 192.168.2.0/24 network, it's 192.168.2.1. That doesn't have to be the case, but it's common practice, and I will follow that practice in this book. The IP addresses of the PCs, on the other hand, are arbitrary. In this chapter's examples, the PCs' IP addresses end in . 11 and .12, but there is no particular significance to those addresses.

> [!translation] 逐句繁體中文翻譯
> 注意：default gateway 的 IP address 通常是 network 的 first usable address。  
> 例如，在 192.168.1.0/24 network 中是 192.168.1.1，在 192.168.2.0/24 network 中是 192.168.2.1。  
> 這不一定非得如此，但這是 common practice，本書也會遵循這個 practice。  
> 另一方面，PCs 的 IP addresses 是 arbitrary。  
> 在本章 examples 中，PCs 的 IP addresses 以 .11 與 .12 結尾，但這些 addresses 沒有特別意義。

How does PC1 know what its default gateway is? An end host can learn the IP address of its default gateway in a couple of ways. One way is manual configuration, in which an admin manually specifies the default gateway on each device. However, this is very rare for user devices like PCs; they usually use the second method-Dynamic Host Configuration Protocol (DHCP)-to automatically learn information like their default gateway's IP address, as well as their own IP address (DHCP is covered in chapter 4 of volume 2 of this book). On a Windows device, you can use the ipconfig command
in the Command Prompt application to view information like the device's IP address, netmask (called the subnet mask in the command's output), and default gateway. The following example shows the output of this command on PC1:

> [!translation] 逐句繁體中文翻譯
> PC1 如何知道自己的 default gateway 是什麼？  
> End host 可以透過幾種方式學到 default gateway 的 IP address。  
> 一種方式是 manual configuration，也就是 admin 在每個 device 上 manually specifies default gateway。  
> 不過，這對 PCs 這類 user devices 來說非常罕見；它們通常使用第二種方法 Dynamic Host Configuration Protocol（DHCP），自動學習 default gateway IP address 與自己的 IP address 等資訊。  
> DHCP 會在本書 volume 2 chapter 4 介紹。  
> 在 Windows device 上，可以在 Command Prompt application 中使用 ipconfig command 查看 device IP address、netmask（在 command output 中稱為 subnet mask）與 default gateway 等資訊。  
> 下列範例顯示 PC1 上此 command 的 output：

```
C:\Users\jmcdo> ipconfig
. . .
Ethernet adapter Local Area Connection:
    Connection-specific DNS Suffix . :
    IPv4 Address. . . . . . . . . . . : 192.168.1.11
    Subnet Mask . . . . . . . . . . . : 255.255.255.0
    Default Gateway . . . . . . . . . : 192.168.1.1
. . .
```

NOTE A host's default gateway is configured as an IP address, not a MAC address. To learn the default gateway's MAC address, the host must send an ARP request to the default gateway's IP address.

> [!translation] 逐句繁體中文翻譯
> 注意：host 的 default gateway 是 configured as IP address，而不是 MAC address。  
> 若要學習 default gateway 的 MAC address，host 必須向 default gateway 的 IP address 傳送 ARP request。

There are a couple of main points to take away from this section: first, to send a packet to a destination in the same network, a host will encapsulate the packet (addressed to the destination host's IP address) in a frame addressed to the destination host's MAC address. The second point is that to send a packet to a destination in a different network, the sending host will encapsulate the packet (addressed to the destination host's IP address) in a frame addressed to the default gateway's MAC address. In either case, ARP must be used to learn the appropriate MAC address (that of the destination host or the default gateway).

> [!translation] 逐句繁體中文翻譯
> 本節有幾個主要重點。  
> 第一，若要傳送 packet 到同一 network 中的 destination，host 會將 addressed to destination host IP address 的 packet encapsulate in addressed to destination host MAC address 的 frame。  
> 第二，若要傳送 packet 到不同 network 中的 destination，sending host 會將 addressed to destination host IP address 的 packet encapsulate in addressed to default gateway MAC address 的 frame。  
> 無論哪種情況，都必須使用 ARP 學習適當的 MAC address，也就是 destination host 或 default gateway 的 MAC address。

### 9.2 The basics of routing

In section 9.1, we saw how a host sends packets to destinations outside of its local network; it sends each packet in a frame addressed to the MAC address of the default gateway. Now we'll examine how the default gateway-which is a router-performs its role of forwarding packets between networks, which is called routing. Figure 9.3 gives a high-level overview of how R1 forwards a packet from PC1 to PC3.

> [!translation] 逐句繁體中文翻譯
> 在 section 9.1，我們看過 host 如何傳送 packets 到 local network 外的 destinations；它會將每個 packet 放入 addressed to default gateway MAC address 的 frame 中。  
> 現在我們會檢視 default gateway，也就是 router，如何執行在 networks 之間 forwarding packets 的角色；這稱為 routing。  
> Figure 9.3 高層次概述 R1 如何將 packet 從 PC1 forward 到 PC3。

NOTE R1's routing table in figure 9.3 is simplified-we will examine R1's complete routing table in section 9.2.1.

> [!translation] 逐句繁體中文翻譯
> 注意：figure 9.3 中 R1 的 routing table 是簡化版本；我們會在 section 9.2.1 檢視 R1 的完整 routing table。

When a router receives a frame destined for its own MAC address, it will de-encapsulate the frame to examine the packet inside (if the destination is not its own MAC address, it will discard the frame). If the destination IP address of the packet is its own IP address, it will continue to de-encapsulate the message-it is a message for the router itself.

> [!translation] 逐句繁體中文翻譯
> 當 router 收到 destined for 自己 MAC address 的 frame 時，它會 de-encapsulate frame 以檢查裡面的 packet。  
> 如果 destination 不是自己的 MAC address，它會 discard 該 frame。  
> 如果 packet 的 destination IP address 是 router 自己的 IP address，它會繼續 de-encapsulate message，因為這是給 router 本身的 message。

However, if the destination IP address of the packet is not its own IP address, the router will attempt to route the packet to forward it toward the packet's destination. It does that by looking up the packet's destination IP address in its routing table to find a suitable route. If a suitable route is found, it will forward the packet according to that route. If not, it will discard the packet.

> [!translation] 逐句繁體中文翻譯
> 然而，如果 packet 的 destination IP address 不是 router 自己的 IP address，router 會嘗試 route 該 packet，將它 forward toward packet destination。  
> 它會在 routing table 中 lookup packet 的 destination IP address，以找到 suitable route。  
> 如果找到 suitable route，它會依該 route forward packet。  
> 如果找不到，它會 discard 該 packet。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-164_769_1250_181_348.jpg)
Figure 9.3 R1 receives a packet from PC1 and forwards it to PC3. (1) R1 receives a frame on its G0/0 interface. The frame is addressed to R1's own MAC address, so it examines the packet inside. (2) R1 looks up the packet's destination IP address in its routing table. 192.168.2.11 is in the 192.168.2.0/24 network, so it selects that route to forward the packet. (3) R1 encapsulates the packet in a new frame destined for PC3's MAC address and forwards it out of the interface specified by the route (G0/1).

### 9.2.1 The routing table

A router's routing table is a database of destinations known by the router. It can be thought of as a set of instructions:

> [!translation] 逐句繁體中文翻譯
> Router 的 routing table 是 router 已知 destinations 的 database。  
> 它可以被視為一組 instructions：

- To send a packet to destination X, forward the packet to next hop Y.
- Or, if the destination is in a directly connected network, forward the packet directly to the destination.
- Or, if the destination is the router's own IP address, continue to de-encapsulate the message (don't forward the packet).

The example we saw in figure 9.3 is an example of the second kind of instruction; the destination of the packet (PC3, 192.168.2.11) is in a network directly connected to R1 (192.168.2.0/24), so R1 forwards the packet directly to the destination (by encapsulating it in a frame addressed to PC3).

> [!translation] 逐句繁體中文翻譯
> Figure 9.3 中看到的例子，就是第二種 instruction 的例子。  
> Packet 的 destination（PC3，192.168.2.11）位於 directly connected to R1 的 network（192.168.2.0/24）中。  
> 因此，R1 會直接將 packet forward 到 destination，方式是將它 encapsulate in addressed to PC3 的 frame。

Unlike switches, which can build their MAC address table automatically without any configuration, a router's routing table will be empty by default-it will not be able to forward packets. The following example shows R1's routing table before any configuration-the command to view the routing table is show ip route:
![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-165_578_1429_181_190.jpg)

> [!translation] 逐句繁體中文翻譯
> Switches 可以在沒有任何 configuration 的情況下自動建立 MAC address table；routers 則不同，routing table 預設會是 empty。  
> 因此，router 將無法 forward packets。  
> 下列範例顯示 R1 在任何 configuration 之前的 routing table；查看 routing table 的 command 是 show ip route：

Without any configuration, the output shows some codes that represent the different route types that could appear in the routing table. Finally, Gateway of last resort is not set indicates that R1 doesn't have a default route, something we'll cover in section 9.4.

> [!translation] 逐句繁體中文翻譯
> 在沒有任何 configuration 的情況下，output 會顯示一些 codes，代表可能出現在 routing table 中的不同 route types。  
> 最後，Gateway of last resort is not set 表示 R1 沒有 default route；這會在 section 9.4 介紹。

Let's configure R1 and see how the output of show ip route changes. First, we won't actually configure any routes; rather, let's configure R1's IP addresses and enable its interfaces, as in the following example:

> [!translation] 逐句繁體中文翻譯
> 接著 configure R1，看看 show ip route 的 output 如何改變。  
> 首先，我們不會實際 configure 任何 routes；而是 configure R1 的 IP addresses 並 enable its interfaces，如下例所示：

```
R1# configure terminal
R1(config) # interface g0/0
R1(config-if)# ip address 192.168.1.1 255.255.255.0
R1(config-if) # no shutdown
R1(config-if) # interface g0/1
R1(config-if) # ip address 192.168.2.1 255.255.255.0
R1(config-if) # no shutdown
```

Configures and enables GO/0

> [!translation] 逐句繁體中文翻譯
> Configure 並 enable G0/0。

Configures and enables G0/1
R1's interfaces are now configured according to the previous diagrams: 192.168.1.1/24 on G0/0 and 192.168.2.1/24 on G0/1. Now let's examine R1's routing table again and see what has changed (omitting some of the codes to save space):

> [!translation] 逐句繁體中文翻譯
> Configure 並 enable G0/1。  
> R1 的 interfaces 現在已依照前面的 diagrams configured：G0/0 上是 192.168.1.1/24，G0/1 上是 192.168.2.1/24。  
> 現在再次檢視 R1 的 routing table，看看有什麼改變；為節省空間，省略部分 codes：

```
R1# show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
. . .
Gateway of last resort is not set
    192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks
C 192.168.1.0/24 is directly connected, GigabitEthernet0/0
L 192.168.1.1/32 is directly connected, GigabitEthernet0/0
    192.168.2.0/24 is variably subnetted, 2 subnets, 2 masks
C 192.168.2.0/24 is directly connected, GigabitEthernet0/1
L 192.168.2.1/32 is directly connected, GigabitEthernet0/1
```

Just by configuring IP addresses on and enabling R1's two interfaces, R1 has inserted four routes into its routing table: two connected routes (indicated by code C) and two local routes (indicated by code L).

> [!translation] 逐句繁體中文翻譯
> 只要在 R1 的兩個 interfaces 上 configure IP addresses 並 enable 它們，R1 就已將四條 routes 插入 routing table。  
> 其中兩條是 connected routes（以 code C 表示），兩條是 local routes（以 code L 表示）。

NOTE The line 192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks is not a route. This statement means that in the routing table, there are two routes to subnets that fit within the 192.168.1.0/24 class C network, with two different netmasks (/24 and /32). The same applies for the similar line about 192.168.2.0/24. We will cover subnets in chapter 11.

> [!translation] 逐句繁體中文翻譯
> 注意：192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks 這一行不是 route。  
> 這個 statement 表示 routing table 中有兩條 routes 指向符合 192.168.1.0/24 class C network 的 subnets，且使用兩種不同 netmasks（/24 與 /32）。  
> 關於 192.168.2.0/24 的類似行也同理。  
> 我們會在 chapter 11 介紹 subnets。

## Connected routes

A connected route is a route to the network an interface is connected to. One connected route is automatically added to the routing table for each interface that has an IP address and is in an up/up state (you can check the interface's state with show ip interface brief). For example, R1's G0/0 interface has IP address 192.168.1.1/24, so it automatically adds a route to the 192.168.1.0/24 network into its routing table.

> [!translation] 逐句繁體中文翻譯
> Connected route 是到某個 interface 所連接 network 的 route。  
> 每個具有 IP address 且處於 up/up state 的 interface，都會自動將一條 connected route 加入 routing table。  
> 你可以用 show ip interface brief 檢查 interface state。  
> 例如，R1 的 G0/0 interface 有 IP address 192.168.1.1/24，所以它會自動將到 192.168.1.0/24 network 的 route 加入 routing table。

NOTE 192.168.1.0/24 is the network address, with all bits of the host portion set to 0 . This can be determined by simply changing the final octet from 1 to 0 since the netmask on the interface is /24.

> [!translation] 逐句繁體中文翻譯
> 注意：192.168.1.0/24 是 network address，其 host portion 的所有 bits 都 set to 0。  
> 因為 interface 上的 netmask 是 /24，所以只要將最後一個 octet 從 1 改成 0，就能判斷出來。

A connected route will state that the network is directly connected, and it will also state which interface it is connected to. To view only the connected routes in R1's routing table, in the following example, I filter the output using the pipe (|) followed by include C to display only lines that include C:
![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-166_246_1411_1294_222.jpg)
With these routes in its routing table, R1 knows that to forward a packet to a host with an IP address in the 192.168.1.0/24 or 192.168.2.0/24 networks, it should send the packet out of the interface specified in the route in a frame addressed directly to the destination host. We saw this in figure 9.3; the destination IP address of the packet was 192.168.2.11 (PC3), so R1 forwarded the packet out of the G0/1 interface in a frame addressed to PC3's MAC address.

> [!translation] 逐句繁體中文翻譯
> Connected route 會說明該 network 是 directly connected，也會說明它 connected to 哪個 interface。  
> 若只想查看 R1 routing table 中的 connected routes，在下列範例中，我使用 pipe（|）後接 include C 過濾 output，只顯示包含 C 的 lines。  
> 有了這些 routes 在 routing table 中，R1 知道若要 forward packet 到 IP address 位於 192.168.1.0/24 或 192.168.2.0/24 networks 的 host，應該從 route 指定的 interface 將 packet 送出，並放入 directly addressed to destination host 的 frame。  
> 我們在 figure 9.3 看過這件事；packet 的 destination IP address 是 192.168.2.11（PC3），所以 R1 從 G0/1 interface forward packet，並放入 addressed to PC3 MAC address 的 frame。

Figure 9.4 shows how the route to 192.168.1.0/24 includes all IP addresses from 192.168.1.0 through 192.168.1.255. The network portion of the route's address is fixed, but the host portion can be any 8-bit number.

> [!translation] 逐句繁體中文翻譯
> Figure 9.4 顯示到 192.168.1.0/24 的 route 如何包含從 192.168.1.0 到 192.168.1.255 的所有 IP addresses。  
> Route address 的 network portion 是 fixed，但 host portion 可以是任何 8-bit number。

|  | These bits are fixed (can't change) |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| IP address | 192 |  |  |  |  |  |  |  |  |  |  |  |  | 1 |  |  |  |  |  |  |  |  | 0 |  |  |  |  |  |  |  |
|  | 1 | 1 | 0 | 0 |  | 0 | 1 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |  | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| Netmask | 255 |  |  |  |  |  | 255 |  |  |  |  |  |  | 255 |  |  |  |  |  |  |  |  | 0 |  |  |  |  |  |  |  |
|  | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |  | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |

Figure 9.4 The IP address and prefix length (written as a netmask) of the route to 192.168.1.0/24. Due to the /24 prefix length, the first three octets are fixed (the bits can't change). However, the last octet can be any 8-bit number: .1, .11, .100, .179, etc. This means that any packet with a destination IP address beginning with 192.168.1 can be forwarded using this route.

> [!translation] 逐句繁體中文翻譯
> Figure 9.4：到 192.168.1.0/24 的 route 的 IP address 與 prefix length（以 netmask 表示）。  
> 由於 /24 prefix length，前三個 octets 是 fixed，bits 不能改變。  
> 不過，最後一個 octet 可以是任何 8-bit number，例如 .1、.11、.100、.179 等。  
> 這表示任何 destination IP address 以 192.168.1 開頭的 packet，都可以使用這條 route forward。

EXAM TIP A route to more than one destination IP address is called a network route; it's a route to a network, rather than a route to a single destination IP address. A connected route is an example of a network route. The term network route is explicitly mentioned in exam topic 3.3.b, so remember that definition.

> [!translation] 逐句繁體中文翻譯
> 考試提示：到多個 destination IP addresses 的 route 稱為 network route。  
> 它是到 network 的 route，而不是到單一 destination IP address 的 route。  
> Connected route 是 network route 的例子。  
> Network route 這個 term 在 exam topic 3.3.b 中明確提到，所以請記住這個 definition。

## Local routes

A local route is a route to the exact IP address configured on the router's interface. Like connected routes, one local route is automatically added to the routing table for each interface that has an IP address and is in an up/up state. In the following example, I use show ip route | include L to view only R1's local routes. Note that like connected routes, local routes also state X is directly connected, followed by the interface:

> [!translation] 逐句繁體中文翻譯
> Local route 是到 router interface 上 configured exact IP address 的 route。  
> 和 connected routes 一樣，每個具有 IP address 且處於 up/up state 的 interface，都會自動將一條 local route 加入 routing table。  
> 在下列範例中，我使用 show ip route | include L 只查看 R1 的 local routes。  
> 請注意，和 connected routes 一樣，local routes 也會寫著 X is directly connected，後面接 interface：

```
        A local route to GO/0’s IP address
R1# show ip route | include L
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
. . .
L 192.168.1.1/32 is directly connected, GigabitEthernet0/0
L 192.168.2.1/32 is directly connected, GigabitEthernet0/1
A local route to GO/1’s IP address
```

To specify the exact IP address of the interface, a local route uses a /32 prefix length; all bits of the netmask are set to 1. This is the case regardless of the netmask configured on the interface. For example, even though R1's interfaces both have /24 prefix lengths, their local routes are /32. The reason for this is that a local route specifies only a single IP address. As stated previously, a route to 192.168.1.0/24 includes all IP addresses from 192.168.1.0 through 192.168.1.255; because the prefix length is $/ 24$, the final octet can be any number from 0 to 255 . On the other hand, a route to
192.168.1.1/32 includes only 192.168.1.1, due to its /32 prefix length; all bits are considered part of the network portion and cannot be changed. Figure 9.5 shows the IP address of R1 G0/0 with a /32 prefix length (written as a netmask).

> [!translation] 逐句繁體中文翻譯
> 為了指定 interface 的 exact IP address，local route 使用 /32 prefix length；netmask 的所有 bits 都 set to 1。  
> 不論 interface 上 configured 的 netmask 是什麼，情況都是如此。  
> 例如，即使 R1 的兩個 interfaces 都有 /24 prefix lengths，它們的 local routes 仍是 /32。  
> 原因是 local route 只指定單一 IP address。  
> 如前所述，到 192.168.1.0/24 的 route 包含從 192.168.1.0 到 192.168.1.255 的所有 IP addresses；因為 prefix length 是 $/24$，最後一個 octet 可以是 0 到 255 的任何 number。  
> 另一方面，到 192.168.1.1/32 的 route 因為 /32 prefix length，只包含 192.168.1.1；所有 bits 都被視為 network portion 的一部分，不能改變。  
> Figure 9.5 顯示 R1 G0/0 的 IP address 搭配 /32 prefix length（以 netmask 表示）。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-168_337_1199_350_350.jpg)
Figure 9.5 The IP address of R1's G0/0 interface and a /32 prefix length written as a netmask in dotted decimal and binary. With a prefix length of /32, the route only includes a single IP address (192.168.1.1, in this case). A packet destined for 192.168.1.2, for example, cannot use this route.

> [!translation] 逐句繁體中文翻譯
> Figure 9.5：R1 G0/0 interface 的 IP address，以及以 dotted decimal 與 binary 寫成 netmask 的 /32 prefix length。  
> 使用 /32 prefix length 時，route 只包含單一 IP address（本例為 192.168.1.1）。  
> 例如，destined for 192.168.1.2 的 packet 不能使用這條 route。

A local route tells the router that packets destined for the IP address specified in the route are for the router itself; it should continue to de-encapsulate the message and examine its contents. In this case, the router does not forward the packet; it just receives the packet for itself. The local route is necessary to distinguish the router's own IP address from other IP addresses in the connected network. If R1 only had a connected route to 192.168.1.0/24 but no local route, it would forward packets destined for 192.168.1.1 out of its G0/0 interface, rather than receiving the packets for itself.

> [!translation] 逐句繁體中文翻譯
> Local route 告訴 router，destined for route 中指定 IP address 的 packets 是給 router 本身的。  
> 因此，router 應繼續 de-encapsulate message 並檢查其 contents。  
> 在這種情況下，router 不會 forward packet；它只是為自己 receive packet。  
> Local route 是必要的，因為它可將 router 自己的 IP address 與 connected network 中其他 IP addresses 區分開來。  
> 如果 R1 只有到 192.168.1.0/24 的 connected route，而沒有 local route，它會將 destined for 192.168.1.1 的 packets 從 G0/0 interface forward 出去，而不是為自己 receive packets。

EXAM TIP A route to a single destination IP address (with a /32 prefix length) is called a host route; it's a route to a single host. A local route is an example of a host route. This is in contrast to a network route, which we covered earlier; a network route is any route with a prefix length shorter than / 32. The term host route is explicitly mentioned in exam topic 3.3.c, so remember that definition.

> [!translation] 逐句繁體中文翻譯
> 考試提示：到單一 destination IP address 的 route（具有 /32 prefix length）稱為 host route。  
> 它是到 single host 的 route。  
> Local route 是 host route 的例子。  
> 這與前面介紹的 network route 相反；network route 是任何 prefix length 短於 /32 的 route。  
> Host route 這個 term 在 exam topic 3.3.c 中明確提到，所以請記住這個 definition。

### 9.2.2 Route selection

When a router forwards a packet, it has to decide which route in its routing table it will use to forward the packet, and this is called route selection. To determine how to forward a particular packet, the router will select the most specific matching route. Let's define that term:

> [!translation] 逐句繁體中文翻譯
> 當 router forward packet 時，它必須決定要使用 routing table 中哪條 route 來 forward packet，這稱為 route selection。  
> 為了決定如何 forward 某個特定 packet，router 會選擇 most specific matching route。  
> 我們來定義這個 term：

- Matching route-The packet's destination IP address is part of the network specified in the route. If not, the packet can't be forwarded using this route.
- Most specific-The route with the longest prefix length.

Let's use an example to clarify that concept. Figure 9.6 shows the route selection process when R1 receives a packet addressed to 192.168.1.1. The packet's destination IP address matches two routes in R1's routing table, so it selects the more specific of the two.

> [!translation] 逐句繁體中文翻譯
> 我們用一個例子來釐清這個 concept。  
> Figure 9.6 顯示 R1 收到 addressed to 192.168.1.1 的 packet 時的 route selection process。  
> Packet 的 destination IP address match R1 routing table 中的兩條 routes，所以它會選擇其中較 specific 的 route。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-169_818_1227_392_318.jpg)
Figure 9.6 R1 receives a packet and selects the best route for that packet. (1) R1 receives a frame on its GO/O interface. The destination MAC is its own, so it de-encapsulates it and examines the packet inside. (2) The packet's destination IP address is 192.168.1.1. R1 performs a routing table lookup and finds that two routes match the packet's destination IP address: the connected route to 192.168.1.0/24 and the local route to 192.168.1.1/32. R1 selects the most specific route: 192.168.1.1/32. (3) Because R1 selects a local route, it receives the packet for itself; it does not forward the packet.

A route with a /24 prefix length includes 256 different IP addresses. For example, 192.168.1.0/24 includes 192.168.1.0 through 192.168.1.255. On the other hand, a route with a /32 prefix length includes only a single IP address, so a /32 route is more specific than a /24 route. In fact, a /32 route is the most specific route possible; if a packet's destination IP address matches a /32 route, that route will always be selected for that packet, regardless of how many other matching routes there are.

> [!translation] 逐句繁體中文翻譯
> 具有 /24 prefix length 的 route 包含 256 個不同 IP addresses。  
> 例如，192.168.1.0/24 包含 192.168.1.0 到 192.168.1.255。  
> 另一方面，具有 /32 prefix length 的 route 只包含單一 IP address，所以 /32 route 比 /24 route 更 specific。  
> 事實上，/32 route 是可能的 most specific route。  
> 如果 packet destination IP address match /32 route，不論還有多少其他 matching routes，該 route 都一定會被選中。

EXAM TIP Be aware of this major difference between Layer 3 forwarding done by routers and Layer 2 forwarding done by switches: when a router looks up a packet's destination IP address in its routing table, it looks for the most specific
matching route. On the other hand, when a switch looks up a frame's destination MAC address in its MAC address table, it looks for an exact match; partial matches don't count.

> [!translation] 逐句繁體中文翻譯
> 考試提示：請留意 routers 執行的 Layer 3 forwarding 與 switches 執行的 Layer 2 forwarding 之間的重大差異。  
> 當 router 在 routing table 中 lookup packet destination IP address 時，它尋找 most specific matching route。  
> 另一方面，當 switch 在 MAC address table 中 lookup frame destination MAC address 時，它尋找 exact match；partial matches 不算。

What happens if there aren't any routes in the routing table that match a packet's destination IP address? In that case, the router will drop the packet; it won't flood it out of all ports like switches do with unknown unicast frames. A switch sometimes floods frames, but a router never floods packets; it forwards the packet, receives the packet for itself, or drops the packet. Table 9.1 summarizes the actions a router can take on a packet.

> [!translation] 逐句繁體中文翻譯
> 如果 routing table 中沒有任何 routes match packet destination IP address，會發生什麼事？  
> 在這種情況下，router 會 drop packet；它不會像 switches 對 unknown unicast frames 那樣從所有 ports flood 出去。  
> Switch 有時會 flood frames，但 router 絕不會 flood packets。  
> Router 只會 forward packet、為自己 receive packet，或 drop packet。  
> Table 9.1 摘要 router 可對 packet 採取的 actions。

Table 9.1 Actions a router can take on a packet
| Matching conditions | Router's action |
| :--- | :--- |
| The packet's destination IP address matches one or more nonlocal routes. | Forward the packet according to the most specific matching route |
| The packet's destination IP address matches a local route. | Receive the packet for itself |
| The packet's destination IP address does not match any routes. | Drop the packet |

> [!translation] 逐句繁體中文翻譯
> Table 9.1：Router 可對 packet 採取的 actions。  
> 如果 packet destination IP address match 一條或多條 nonlocal routes，router 會依 most specific matching route forward packet。  
> 如果 packet destination IP address match local route，router 會為自己 receive packet。  
> 如果 packet destination IP address 不 match 任何 routes，router 會 drop packet。


### 9.3 Static routing

The packet forwarding process outlined in section 9.2 is called routing, but the term routing is also used to refer to the processes routers use to learn routes. In addition to the connected and local routes a router automatically inserts into its routing table, there are two main methods by which routers can learn routes:

> [!translation] 逐句繁體中文翻譯
> Section 9.2 概述的 packet forwarding process 稱為 routing。  
> 但 routing 這個 term 也用來指 routers 學習 routes 的 processes。  
> 除了 router 自動插入 routing table 的 connected 與 local routes 之外，routers 主要有兩種學習 routes 的方法：

- Dynamic routing-Routers use dynamic routing protocols (i.e., OSPF) to share information with each other and build their routing tables.
- Static routing-An engineer/admin manually configures routes on the router.

Connected routes allow the router to forward packets to destinations in networks directly connected to the router, and local routes allow the router to receive packets destined for its own IP addresses. However, to forward packets toward destinations that are not in directly connected networks, the router must learn of those destinations using one of the aforementioned methods (we will cover dynamic routing in chapters 17 and 18).

> [!translation] 逐句繁體中文翻譯
> Connected routes 允許 router 將 packets forward 到 directly connected to router 的 networks 中的 destinations。  
> Local routes 允許 router receive destined for 自己 IP addresses 的 packets。  
> 然而，若要將 packets forward toward 不在 directly connected networks 中的 destinations，router 必須使用前述其中一種方法學習那些 destinations。  
> Dynamic routing 會在 chapters 17 與 18 介紹。

When forwarding a packet toward a destination that is not directly connected to the router, it must encapsulate the packet in a frame addressed to the MAC address of the next hop, which is the next router in the path to the destination. Figure 9.7 demonstrates this process.

> [!translation] 逐句繁體中文翻譯
> 當 router 將 packet forward toward 不直接 connected to router 的 destination 時，它必須將 packet encapsulate in addressed to next hop MAC address 的 frame。  
> Next hop 是 path to destination 中的下一台 router。  
> Figure 9.7 示範這個 process。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-171_588_1414_181_192.jpg)
Figure 9.7 PC1 sends a packet to PC3 via R1, R2, and R3. (1) PC1 sends the packet in a frame to R1 G0/1. R1 receives it and performs a routing table lookup. (2) R1 forwards the packet in a frame to R2 G0/0. R2 receives it and performs a routing table lookup. (3) R2 forwards the packet in a frame to R3 G0/0's MAC. R3 receives the packet and performs a routing table lookup. (4) R3 forwards the packet in a frame to PC3, which receives and processes it.

Figure 9.7 shows how routers forward packets toward remote (not directly connected) destinations. However, routers have no such routes in their routing tables by default; those routes must be manually configured. Without configuring any static routes, if R1 receives a packet from PC1 destined for 192.168.3.11, R1 won't find any matching routes when it performs the routing table lookup; it will have no choice but to drop the packet. Table 9.2 lists the networks that each router in figure 9.7 is aware of without configuring static routes.

> [!translation] 逐句繁體中文翻譯
> Figure 9.7 顯示 routers 如何將 packets forward toward remote（not directly connected）destinations。  
> 不過，routers 的 routing tables 預設沒有這類 routes；這些 routes 必須 manually configured。  
> 如果沒有 configure 任何 static routes，當 R1 收到 PC1 傳來 destined for 192.168.3.11 的 packet 時，R1 在執行 routing table lookup 時找不到任何 matching routes。  
> 它別無選擇，只能 drop packet。  
> Table 9.2 列出 figure 9.7 中每台 router 在未 configure static routes 時知道的 networks。

Table 9.2 Each router's known and unknown networks
| Router | Known networks | Unknown networks |
| :--- | :--- | :--- |
| R1 | 192.168.1.0/24 192.168.12.0/24 | 192.168.3.0/24 192.168.23.0/24 |
| R2 | 192.168.12.0/24 192.168.23.0/24 | 192.168.1.0/24 192.168.3.0/24 |
| R3 | 192.168.3.0/24 192.168.23.0/24 | 192.168.1.0/24 192.168.12.0/24 |

> [!translation] 逐句繁體中文翻譯
> Table 9.2：每台 router 已知與未知的 networks。  
> R1 已知 192.168.1.0/24 與 192.168.12.0/24，未知 192.168.3.0/24 與 192.168.23.0/24。  
> R2 已知 192.168.12.0/24 與 192.168.23.0/24，未知 192.168.1.0/24 與 192.168.3.0/24。  
> R3 已知 192.168.3.0/24 與 192.168.23.0/24，未知 192.168.1.0/24 與 192.168.12.0/24。


Given the goal of enabling two-way communication between PC1 and PC3, which routes do we have to configure? Looking at table 9.2, you might assume that we have to configure six routes, so that each router knows about all networks within the greater network.

> [!translation] 逐句繁體中文翻譯
> 如果目標是 enable PC1 與 PC3 之間的 two-way communication，我們必須 configure 哪些 routes？  
> 看著 table 9.2，你可能會假設必須 configure 六條 routes，讓每台 router 都知道整個 larger network 中的所有 networks。

However, to forward packets between two hosts, each router only needs routes to the networks of the communicating hosts (PC1 and PC3). R1, for example, doesn't need to know about the network between R2 and R3 (192.168.23.0/24); R1 only needs
to know that to forward a packet toward a destination in 192.168.3.0/24, it should forward the packet to R2. Figure 9.8 demonstrates this concept; if we configure a route to 192.168.3.0/24 on R1, with R2's IP address specified as the next hop, R1 will be able to forward the packet to R2. It doesn't need to know the details of the path the packet will take after R2; it just needs to know that R2 is the next hop.

> [!translation] 逐句繁體中文翻譯
> 然而，若要在兩台 hosts 之間 forward packets，每台 router 只需要到 communicating hosts（PC1 與 PC3）所在 networks 的 routes。  
> 例如，R1 不需要知道 R2 與 R3 之間的 network（192.168.23.0/24）。  
> R1 只需要知道若要 forward packet toward 192.168.3.0/24 中的 destination，應該將 packet forward 給 R2。  
> Figure 9.8 示範這個 concept；如果我們在 R1 上 configure 到 192.168.3.0/24 的 route，並將 R2 的 IP address 指定為 next hop，R1 就能將 packet forward 給 R2。  
> 它不需要知道 packet 在 R2 之後會走的 path details；它只需要知道 R2 是 next hop。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-172_643_1412_438_225.jpg)
Figure 9.8 R1 forwards a packet destined for 192.168.3.11 (PC3). R1 has a static route to 192.168.3.0/24 via 192.168.12.2 (R2). R1 knows that to forward a packet toward the 192.168.3.0/24 network, it should forward the packet to R2. R1 doesn't know the details of the path the packet will take after R2, and it doesn't have to.

To summarize, each router needs a route to 192.168.3.0/24 so that it can forward packets from PC1 to PC3 and a route to 192.168.1.0/24 so that it can forward packets from PC3 to PC1. R1 already has a connected route to 192.168.1.0/24, and R3 already has a connected route to 192.168.3.0/24. Table 9.3 lists the static routes that we must configure to enable two-way communication between PC1 and PC3.

> [!translation] 逐句繁體中文翻譯
> 總結來說，每台 router 需要一條到 192.168.3.0/24 的 route，以便 forward packets from PC1 to PC3。  
> 也需要一條到 192.168.1.0/24 的 route，以便 forward packets from PC3 to PC1。  
> R1 已經有到 192.168.1.0/24 的 connected route，而 R3 已經有到 192.168.3.0/24 的 connected route。  
> Table 9.3 列出我們必須 configure 的 static routes，以 enable PC1 與 PC3 之間的 two-way communication。

Table 9.3 Routes required to enable communication between PC1 and PC3
| Router | Required routes | Next hop |
| :--- | :--- | :--- |
| R1 | 192.168.3.0/24 | 192.168.12.2 (R2 GO/O) |
| R2 | 192.168.1.0/24 | 192.168.12.1 (R1 GO/0) |
|  | 192.168.3.0/24 | 192.168.23.2 (R3 GO/0) |
| R3 | 192.168.1.0/24 | 192.168.23.1 (R2 G0/1) |

> [!translation] 逐句繁體中文翻譯
> Table 9.3：Enable PC1 與 PC3 communication 所需的 routes。  
> R1 需要到 192.168.3.0/24 的 route，next hop 是 192.168.12.2（R2 G0/0）。  
> R2 需要到 192.168.1.0/24 的 route，next hop 是 192.168.12.1（R1 G0/0）。  
> R2 也需要到 192.168.3.0/24 的 route，next hop 是 192.168.23.2（R3 G0/0）。  
> R3 需要到 192.168.1.0/24 的 route，next hop 是 192.168.23.1（R2 G0/1）。


NOTE In this example, we are talking about the routes required to enable twoway communication between PC1 and PC3. Although not necessary for that purpose, there is nothing wrong with configuring a route to 192.168.23.0/24 on R1 and a route to 192.168.12.0/24 on R3.

> [!translation] 逐句繁體中文翻譯
> 注意：在這個 example 中，我們討論的是 enable PC1 與 PC3 two-way communication 所需的 routes。  
> 雖然對此目的並非必要，但在 R1 上 configure 到 192.168.23.0/24 的 route，以及在 R3 上 configure 到 192.168.12.0/24 的 route，也沒有問題。

### 9.3.1 Configuring static routes

The command to configure a static route is, from global configuration mode, ip route. However, there are a few different options regarding the arguments you provide with the command:

> [!translation] 逐句繁體中文翻譯
> Configure static route 的 command 是在 global configuration mode 中使用 ip route。  
> 不過，這個 command 所提供的 arguments 有幾種不同 options：

- ip route destination-network netmask next-hop
- ip route destination-network netmask exit-interface
- ip route destination-network netmask exit-interface next-hop

Static routes specifying the next hop
A static route can be configured by specifying the destination network address, the netmask, and the IP address of the next hop. Figure 9.9 shows the commands to configure each of the necessary routes on R1, R2, and R3.

> [!translation] 逐句繁體中文翻譯
> 指定 next hop 的 static routes。  
> Static route 可以透過指定 destination network address、netmask 與 next hop IP address 來 configure。  
> Figure 9.9 顯示在 R1、R2 與 R3 上 configure 每條必要 route 的 commands。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-173_488_1400_828_201.jpg)
Figure 9.9 Configuring static routes on R1, R2, and R3 to enable two-way communication between PC1 and PC3. The routes specify the next-hop IP address. R1 requires a route to 192.168.3.0/24, R2 requires routes to 192.168.1.0/24 and 192.168.3.0/24, and R3 requires a route to 192.168.1.0/24.

A static route that specifies only the next-hop IP address is called a recursive static route. The reason for the name recursive is that the route necessitates multiple lookups in the routing table to forward a packet:

> [!translation] 逐句繁體中文翻譯
> 只指定 next-hop IP address 的 static route 稱為 recursive static route。  
> 之所以稱為 recursive，是因為該 route 需要在 routing table 中進行 multiple lookups 才能 forward packet：

- A lookup to find the IP address of the next hop
- A lookup to find which interface the next hop is connected to

To demonstrate that, let's look at R1's routing table in the following example. When R1 receives a packet destined for 192.168.3.11, it finds that the most specific matching route (actually, the only matching route) is the static route:

> [!translation] 逐句繁體中文翻譯
> 為了示範這點，我們看下列範例中的 R1 routing table。  
> 當 R1 收到 destined for 192.168.3.11 的 packet 時，它發現 most specific matching route（其實也是唯一 matching route）是 static route：

```
R1# show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
    192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks
```

Matches the packet's destination (192.168.3.11)

> [!translation] 逐句繁體中文翻譯
> Match packet 的 destination（192.168.3.11）。

```
C 192.168.1.0/24 is directly connected, GigabitEthernet0/1
L 192.168.1.1/32 is directly connected, GigabitEthernet0/1
S 192.168.3.0/24 [1/0] via 192.168.12.2
    192.168.12.0/24 is variably subnetted, 2 subnets, 2 masks
C 192.168.12.0/24 is directly connected, GigabitEthernet0/0
L 192.168.12.1/32 is directly connected, GigabitEthernet0/0
```

Matches the next-hop IP address (192.168.12.2)
NOTE The [1/0] in the static route indicates the administrative distance (AD) and metric of the route, respectively. AD and metric will be covered in chapter 17; they are not relevant to this chapter.

> [!translation] 逐句繁體中文翻譯
> Match next-hop IP address（192.168.12.2）。  
> 注意：static route 中的 [1/0] 分別表示 route 的 administrative distance（AD）與 metric。  
> AD 與 metric 會在 chapter 17 介紹；它們與本章無關。

The static route states S 192.168.3.0/24 [1/0] via 192.168.12.2 (note the code S for static), but that information alone doesn't tell R1 which interface to forward the packet out of. To learn that, it then performs a second lookup for the next-hop IP address: 192.168.12.2. The most specific (and only) matching route for 192.168.12.2 is the connected route to 192.168.12.0/24, which specifies the G0/0 interface. Now, after two lookups, R1 knows the next-hop IP address and the interface to forward the packet out of.

> [!translation] 逐句繁體中文翻譯
> Static route 寫著 S 192.168.3.0/24 [1/0] via 192.168.12.2，請注意 code S 代表 static。  
> 但光是這些資訊並不會告訴 R1 應該從哪個 interface forward packet。  
> 為了得知這點，它接著會對 next-hop IP address 192.168.12.2 執行第二次 lookup。  
> 對 192.168.12.2 來說，most specific 且唯一 matching route 是到 192.168.12.0/24 的 connected route，而這條 route 指定 G0/0 interface。  
> 現在，經過兩次 lookups 後，R1 知道 next-hop IP address，也知道要從哪個 interface forward packet。

NOTE R1 knows the next-hop IP address, but the information R1 really needs is the next-hop MAC address. To learn that, it must send an ARP request to the next-hop IP address.

> [!translation] 逐句繁體中文翻譯
> 注意：R1 知道 next-hop IP address，但 R1 真正需要的資訊是 next-hop MAC address。  
> 若要學習該 MAC address，它必須向 next-hop IP address 傳送 ARP request。

## Static routes specifying the exit interface

Rather than specifying the next-hop IP address of the route, you can specify the exit interface-the interface the router should forward packets out of. The following example shows the same static routes as we saw in figure 9.9 but configured using the exit interface:

> [!translation] 逐句繁體中文翻譯
> 除了指定 route 的 next-hop IP address，也可以指定 exit interface，也就是 router 應從哪個 interface forward packets。  
> 下列範例顯示與 figure 9.9 相同的 static routes，但改用 exit interface configure：

- R1(config)\# ip route 192.168.3.0 255.255.255.0 g0/0
- R2(config) \# ip route 192.168.1.0 255.255.255.0 g0/0
- R2(config) \# ip route 192.168.3.0 255.255.255.0 g0/1
- R3(config)\# ip route 192.168.1.0 255.255.255.0 g0/0

A static route that specifies only the exit interface is called a directly connected static route. The reason for this is that the route will appear as a directly connected network in the routing table. The following example shows the route to 192.168.3.0/24 in R1's routing table:

> [!translation] 逐句繁體中文翻譯
> 只指定 exit interface 的 static route 稱為 directly connected static route。  
> 原因是該 route 在 routing table 中會顯示為 directly connected network。  
> 下列範例顯示 R1 routing table 中到 192.168.3.0/24 的 route：

```
R1# show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
. . .
S 192.168.3.0/24 is directly connected, GigabitEthernet0/0
. . .
```

The static route appears as "directly connected."

> [!translation] 逐句繁體中文翻譯
> Static route 顯示為「directly connected」。

There is a downside to this kind of static route: because R1 thinks that the 192.168.3.0/24 network is directly connected to its G0/0 interface, it will try to send packets in frames addressed directly to PC3, rather than in frames addressed to the next hop.

> [!translation] 逐句繁體中文翻譯
> 這種 static route 有一個缺點。  
> 因為 R1 認為 192.168.3.0/24 network directly connected to its G0/0 interface，所以它會嘗試將 packets 放入 directly addressed to PC3 的 frames 中送出，而不是放入 addressed to next hop 的 frames。

This means that R1 won't send an ARP request to learn the next-hop MAC address; instead, it will send an ARP request to learn PC3's MAC address. The problem is that this ARP request won't reach PC3-it will only reach R2 (broadcast messages don't go beyond their local network). However, R2 can use a feature called proxy ARP to reply on behalf of PC3, telling R1 to send the packet to R2 G0/0's MAC address. This is demonstrated in figure 9.10.

> [!translation] 逐句繁體中文翻譯
> 這表示 R1 不會傳送 ARP request 來學習 next-hop MAC address。  
> 相反地，它會傳送 ARP request 來學習 PC3 的 MAC address。  
> 問題是這個 ARP request 不會到達 PC3；它只會到達 R2，因為 broadcast messages 不會超出 local network。  
> 不過，R2 可以使用稱為 proxy ARP 的 feature 代表 PC3 回覆，告訴 R1 將 packet 傳送到 R2 G0/0 的 MAC address。  
> Figure 9.10 示範這件事。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-175_356_1411_663_193.jpg)
Figure 9.10 A proxy ARP exchange between R1 and R2. (1) R1 sends an ARP request to learn PC3's MAC address. (2) 192.168.3.11 isn't R2's IP address, but R2 has a route to 192.168.3.0/24 in its routing table, so R2 uses proxy ARP to reply on behalf of PC3.

NOTE A router will only use proxy ARP to reply to an ARP request if it has a route to the destination in its routing table. Otherwise, it will ignore the request.

> [!translation] 逐句繁體中文翻譯
> 注意：Router 只有在 routing table 中有到 destination 的 route 時，才會使用 proxy ARP 回覆 ARP request。  
> 否則，它會 ignore 該 request。

The following example shows R1's ARP table (you can view it with the show arp command). Notice that the same MAC address (Hardware Addr) is listed for both 192.168.12.2 and 192.168.3.11-the MAC address of R2's G0/0 interface:

> [!translation] 逐句繁體中文翻譯
> 下列範例顯示 R1 的 ARP table，你可以用 show arp command 查看。  
> 請注意，192.168.12.2 與 192.168.3.11 兩者都列出相同 MAC address（Hardware Addr），也就是 R2 G0/0 interface 的 MAC address：

```
R1# show arp
Protocol Address Age (min) Hardware Addr Type Interface
. . .
Internet 192.168.3.11 0 5254.0003.e684 ARPA GigabitEthernet0/0 <
. . .
Internet 192.168.12.2 0 5254.0003.e684 ARPA GigabitEthernet0/0 <
```

R2 GO/0's MAC address is listed for both IP addresses.

> [!translation] 逐句繁體中文翻譯
> R2 G0/0 的 MAC address 被列在兩個 IP addresses 上。

The reliance on proxy ARP is a downside to directly connected static routes for two reasons. First, although proxy ARP is enabled on Cisco routers by default, in some cases, it might be disabled (e.g., if R2 is not a Cisco router). If proxy ARP is disabled on R2, it won't reply to R1's ARP request, and R1 won't be able to forward the packet to PC3.

> [!translation] 逐句繁體中文翻譯
> 依賴 proxy ARP 是 directly connected static routes 的缺點，原因有兩個。  
> 第一，雖然 Cisco routers 預設 enabled proxy ARP，但某些情況下它可能被 disabled，例如 R2 不是 Cisco router。  
> 如果 R2 上 proxy ARP disabled，它就不會回覆 R1 的 ARP request，R1 也無法將 packet forward 給 PC3。

The second downside is that R1 will need to make a separate ARP entry for every host in 192.168.3.0/24. It thinks each host in 192.168.3.0/24 is directly connected, so it will try to learn each host's MAC address; that could waste memory on R1 if there are a lot of hosts in the network. On the other hand, if the next-hop IP address is specified instead of the exit interface, R1 will only need one ARP entry to forward packets to 192.168.3.0/24: an ARP entry for the next hop.

> [!translation] 逐句繁體中文翻譯
> 第二個缺點是，R1 需要為 192.168.3.0/24 中的每個 host 建立 separate ARP entry。  
> 它認為 192.168.3.0/24 中每個 host 都是 directly connected，所以會嘗試學習每個 host 的 MAC address。  
> 如果 network 中有很多 hosts，這可能浪費 R1 的 memory。  
> 另一方面，如果指定 next-hop IP address 而不是 exit interface，R1 只需要一個 ARP entry 就能 forward packets 到 192.168.3.0/24，也就是 next hop 的 ARP entry。

NOTE Although we are focusing on R1 as an example, the same applies for R2, which will think that the 192.168.1.0/24 and 192.168.3.0/24 networks are directly connected, as well as for R3, which will think that the 192.168.1.0/24 network is directly connected.

> [!translation] 逐句繁體中文翻譯
> 注意：雖然我們聚焦於 R1 作為 example，但同樣邏輯也適用於 R2。  
> R2 會認為 192.168.1.0/24 與 192.168.3.0/24 networks 是 directly connected。  
> 同樣地，R3 也會認為 192.168.1.0/24 network 是 directly connected。

You should know the definition of and be able to configure directly connected static routes, but because of the downsides of relying on proxy ARP, I recommend that you do not use them in a real network. Rather, use recursive static routes or the next option: fully specified static routes.

> [!translation] 逐句繁體中文翻譯
> 你應該知道 directly connected static routes 的 definition，並能 configure 它們。  
> 但因為依賴 proxy ARP 有缺點，我建議你不要在 real network 中使用它們。  
> 請改用 recursive static routes，或下一個選項：fully specified static routes。

## Static routes specifying both the exit interface and next hop

The third option when configuring a static route is to specify both the exit interface and the next hop, which is called a fully specified static route. The following are the same four static routes, this time configured as fully specified static routes:

> [!translation] 逐句繁體中文翻譯
> Configure static route 的第三個選項，是同時指定 exit interface 與 next hop，這稱為 fully specified static route。  
> 以下是相同四條 static routes，這次 configured as fully specified static routes：

- R1(config)\# ip route 192.168.3.0 255.255.255.0 g0/0 192.168.12.2
- R2(config) \# ip route 192.168.1.0 255.255.255.0 g0/0 192.168.12.1
- R2(config)\# ip route 192.168.3.0 255.255.255.0 g0/1 192.168.23.2
- R3(config)\# ip route 192.168.1.0 255.255.255.0 g0/0 192.168.23.1

The benefit of this kind of static route is that the router knows both the next-hop IP address and which interface to forward the packet out of; there is no need to do a recursive lookup or to rely on proxy ARP. The following example shows how a fully specified route appears in the routing table:

> [!translation] 逐句繁體中文翻譯
> 這種 static route 的好處是 router 同時知道 next-hop IP address，以及要從哪個 interface forward packet。  
> 因此，不需要做 recursive lookup，也不需要依賴 proxy ARP。  
> 下列範例顯示 fully specified route 在 routing table 中如何呈現：

```
R1# show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
. . .
S 192.168.3.0/24 [1/0] via 192.168.12.2, GigabitEthernet0/0
. . .
```

The route indicates both the next hop and the exit interface.

> [!translation] 逐句繁體中文翻譯
> 該 route 同時指出 next hop 與 exit interface。

This type of static route may seem the best of the three, but in reality, you can use either recursive or fully specified static routes without a noticeable difference in performance. Directly connected static routes, however, should generally be avoided.

> [!translation] 逐句繁體中文翻譯
> 這種類型的 static route 可能看起來是三者中最好的一種。  
> 但實際上，你可以使用 recursive 或 fully specified static routes，performance 不會有明顯差異。  
> 然而，directly connected static routes 通常應避免使用。

### 9.3.2 Configuring a default route

A default route is a route to the least-specific destination possible: 0.0.0.0/0. This route matches all possible IP addresses, from 0.0.0.0 through 255.255.255.255. Because it is the least-specific route possible, the default route will only be selected if there aren't any more specific routes in the router's routing table.

> [!translation] 逐句繁體中文翻譯
> Default route 是到 least-specific destination possible 的 route：0.0.0.0/0。  
> 這條 route match 所有 possible IP addresses，從 0.0.0.0 到 255.255.255.255。  
> 因為它是 possible least-specific route，所以 default route 只有在 router routing table 中沒有任何 more specific routes 時才會被選中。

A default route is often used to provide a route to the internet. There are over 1 million routes in the global internet routing table, which is far more than most routers can handle. Fortunately, there's no need for a router to know about any specific destination networks over the internet; if the router has a default route to the internet, it can use that route to forward packets toward the internet, and then the Internet Service Provider (ISP) infrastructure will take care of forwarding the packets to the proper destination.

> [!translation] 逐句繁體中文翻譯
> Default route 常用來提供到 internet 的 route。  
> Global internet routing table 中有超過 100 萬條 routes，遠超過大多數 routers 能處理的量。  
> 幸好，router 不需要知道 internet 上任何 specific destination networks。  
> 如果 router 有到 internet 的 default route，它就能使用該 route 將 packets forward toward internet，接著由 Internet Service Provider（ISP）infrastructure 負責將 packets forward 到正確 destination。

More specific routes can be used for destinations in the internal corporate network, and then all other traffic (that doesn't match any other routes) will be routed using the default route. Figure 9.11 shows an example of this: R1 has specific routes to 192.168.2.0/24 and 192.168.3.0/24 and then a default route to the internet; packets with destinations that don't match 192.168.2.0/24 or 192.168.3.0/24 (or R1's connected and local routes) will be forwarded using the default route.

> [!translation] 逐句繁體中文翻譯
> More specific routes 可用於 internal corporate network 中的 destinations。  
> 接著，所有其他 traffic，也就是不 match 任何其他 routes 的 traffic，會使用 default route 被 routed。  
> Figure 9.11 顯示這個例子：R1 有到 192.168.2.0/24 與 192.168.3.0/24 的 specific routes，接著有一條到 internet 的 default route。  
> Destination 不 match 192.168.2.0/24 或 192.168.3.0/24（或 R1 的 connected 與 local routes）的 packets，會使用 default route forward。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-177_668_1414_1043_188.jpg)
Figure 9.11 R1 has two routes to specific destination networks and one default route to the internet. (1) A route to 192.168.2.0/24, with R2 G0/0 as the next hop. (2) A route to 192.168.3.0/24, with R3 G0/0 as the next hop. (3) A default route, with the ISP's IP address (203.0.113.2) as the next hop.

To configure a default route, specify a destination network of 0.0.0.0 and a netmask of 0.0.0.0 in the ip route command; that results in 0.0.0.0/0, which includes all possible IP addresses. If a router does not have a default route configured, you will see the statement Gateway of last resort is not set above the routes in the
routing table, as shown in the following example. This means the router does not have a default route:

> [!translation] 逐句繁體中文翻譯
> 若要 configure default route，請在 ip route command 中指定 destination network 為 0.0.0.0，netmask 為 0.0.0.0。  
> 這會產生 0.0.0.0/0，包含所有 possible IP addresses。  
> 如果 router 沒有 configured default route，你會在 routing table 的 routes 上方看到 Gateway of last resort is not set 這個 statement，如下例所示。  
> 這表示 router 沒有 default route：

```
R1# show ip route
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
. . .
Gateway of last resort is not set
. . .
```

NOTE Gateway of last resort is another term for the default gateway. The default route on a router is like a PC's default gateway; it's used to forward traffic to destinations outside of the router's known networks.

> [!translation] 逐句繁體中文翻譯
> 注意：Gateway of last resort 是 default gateway 的另一個 term。  
> Router 上的 default route 就像 PC 的 default gateway；它用來將 traffic forward 到 router known networks 之外的 destinations。

After configuring the static routes shown in figure 9.11, the output changes. In the following example, I use the show ip route static command to view only R1's static routes. Note that the ISP's IP address (203.0.113.2) is now listed as the gateway of last resort:

> [!translation] 逐句繁體中文翻譯
> Configure figure 9.11 所示的 static routes 後，output 會改變。  
> 在下列範例中，我使用 show ip route static command 只查看 R1 的 static routes。  
> 請注意，ISP 的 IP address（203.0.113.2）現在被列為 gateway of last resort：

```
Views static routes in the routing table
R1# show ip route static
Codes: L - local, C - connected, S - static, R - RIP, M - mobile, B - BGP
. . .
    ia - IS-IS inter area, * - candidate default, U - per-user static route
. . .
Gateway of last resort is 203.0.113.2 to network 0.0.0.0
S* 0.0.0.0/0 [1/0] via 203.0.113.2
S 192.168.2.0/24 [1/0] via 192.168.12.2
S 192.168.3.0/24 [1/0] via 192.168.13.2
```

The default route we configured

> [!translation] 逐句繁體中文翻譯
> 我們 configured 的 default route。

NOTE Earlier in this chapter, I stated that a router will drop packets that don't match any routes in its routing table. However, if the router has a default route, that situation won't occur; the default route matches all IP addresses. If a packet doesn't match a more specific route, the router will forward it via the default route, rather than dropping the packet.

> [!translation] 逐句繁體中文翻譯
> 注意：本章稍早我說過，router 會 drop 不 match routing table 中任何 routes 的 packets。  
> 然而，如果 router 有 default route，這種情況不會發生，因為 default route match 所有 IP addresses。  
> 如果 packet 不 match more specific route，router 會透過 default route forward 它，而不是 drop packet。

Exam scenarios
Routing is a key topic of the CCNA exam. Here are a few examples of how your knowledge of routing fundamentals might be tested on the CCNA exam:

> [!translation] 逐句繁體中文翻譯
> 考試情境。  
> Routing 是 CCNA exam 的 key topic。  
> 以下是幾個 examples，說明你的 routing fundamentals 知識可能如何在 CCNA exam 中被測驗：

1 (multiple choice, multiple answers)
You issue the command ip route 10.0.0.0 255.0.0.0 192.0.2.1. Which of the following statements are true about the route created by the command? (select two)

(continued)

> [!translation] 逐句繁體中文翻譯
> （續）

A It is a network route.
B It is a host route.
c It is a recursive route.
D It is a directly connected route.
E It is a fully specified route.

> [!translation] 逐句繁體中文翻譯
> A 它是 network route。  
> B 它是 host route。  
> C 它是 recursive route。  
> D 它是 directly connected route。  
> E 它是 fully specified route。

This question requires you to distinguish between different route types. Network and host routes are classifications based on the route's destination (a network of IP addresses or a single IP address). Recursive, directly connected, and fully specified routes are classifications based on how the route's next hop is specified (next-hop IP address, exit interface, or both). In this case, (A) is correct because the route's destination is 10.0.0.0/8-a network, not a single IP address. And (C) is correct because the route specifies only the next-hop IP address, making it a recursive route.

> [!translation] 逐句繁體中文翻譯
> 這題要求你區分不同 route types。  
> Network 與 host routes 是根據 route destination 進行分類，也就是一個 IP addresses 的 network 或單一 IP address。  
> Recursive、directly connected 與 fully specified routes 則是根據 route next hop 如何被 specified 進行分類，也就是 next-hop IP address、exit interface 或兩者。  
> 在此例中，A 正確，因為 route destination 是 10.0.0.0/8，是 network，不是單一 IP address。  
> C 也正確，因為 route 只指定 next-hop IP address，使它成為 recursive route。

2 (drag and drop)
Your knowledge of route types could also be tested with a drag-and-drop question like this: drag and drop the routes on the left to the correct route types on the right.

| (A) ip route 192.168.1.0 255.255.255.0 GigabitEthernet0/1 | Recursive |
| :--- | :--- |
| (B) ip route 0.0.0.0 0.0.0.0 203.0.113.120 | Directly connected |
| (C) ip route 172.20.0.0 255.255.0.0 GigabitEthernet0/1 192.168.2.1 | Fully specified |
| (D) ip route 192.0.2.0 255.255.255.0 172.16.25.209 |  |

In this case, (A) is a directly connected route because it specifies only the exit interface, (B) and (D) are recursive routes because they specify only the next-hop IP address, and C) is a fully specified route because it specifies both.

> [!translation] 逐句繁體中文翻譯
> 在此例中，A 是 directly connected route，因為它只指定 exit interface。  
> B 與 D 是 recursive routes，因為它們只指定 next-hop IP address。  
> C 是 fully specified route，因為它同時指定兩者。

3 (lab simulation)
A lab simulation on the CCNA exam could provide you with a network diagram and ask you to configure static routes to enable hosts in different networks to communicate with each other. Remember the basic syntax of the ip route command to configure static routes. If the question expects you to configure a certain type of static route (recursive, directly connected, or fully specified), it should state so; Cisco exams can be difficult, but they aren't unfair.

> [!translation] 逐句繁體中文翻譯
> 3（lab simulation）。  
> CCNA exam 中的 lab simulation 可能會提供 network diagram，並要求你 configure static routes，讓不同 networks 中的 hosts 能彼此 communicate。  
> 請記住 configure static routes 的 ip route command basic syntax。  
> 如果題目期待你 configure 某一類 static route（recursive、directly connected 或 fully specified），它應該會明確說明。  
> Cisco exams 可能很難，但並不會不公平。

## Summary

- The term routingcan refer to the process of forwarding packets between networks and the process of building a routing table.
- Hosts in the same network can send packets to each other without the use of a router. However, to send packets to destinations outside of the local network, a router is required.

- The router a host will send packets destined for external networks to is called the default gateway. The host will send the packets in frames addressed to the default gateway's MAC address.
- A host's default gateway can be manually configured or automatically learned via DHCP.
- You can use the ipconfig command in the Windows Command Prompt to see information such as the PC's IP address, netmask, and default gateway.
- The routing table is the router's database of known destinations. It is a set of instructions about what action to take on packets. The routing table can be viewed with show ip route.
- For each interface that has an IP address and is in an up/up state, the router will automatically add two routes to its routing table: a connected route and a local route.
- A connected route is a route to the network that an interface is connected to. Connected routes are indicated by code C in the routing table. If a router receives a packet destined for a host in a directly connected network, it will forward the packet directly to the destination host (in a frame addressed to the host's MAC address).
- A local route is a route to the exact IP address configured on the interface. Local routes use a /32 prefix length to specify a single IP address. If a router receives a packet destined for the IP address of a local route, it means the packet is destined for the router itself; the router will receive the packet for itself-it will not forward it.
- A route to more than one destination IP address (any route with a prefix length shorter than /32) is called a network route. A connected route is an example of a network route.
- A route to a single destination IP address (a route with a /32 prefix length) is called a host route. A local route is an example of a host route.
- The process of deciding which route is appropriate for forwarding a packet is called route selection. To determine how to forward a particular packet, the router will select the most specific matching route-the matching route with the longest prefix length.
- A /32 route is the most specific route possible; it specifies only one IP address. A /0 route (default route) is the least specific route possible; it specifies every possible IP address.
- Whereas Layer 3 forwarding involves looking in the routing table for the most specific matching route, Layer 2 forwarding involves looking for an exact match in the MAC address table; partial matches don't count.
- If there aren't any routes that match a packet's destination IP address, the router will drop the packet.

- To route packets to destinations that aren't directly connected to the router, the router needs to learn routes to those destinations either via dynamic routing (using a protocol such as OSPF) or static routing (in which routes are manually configured on the router).
- To forward a packet toward a remote destination, the router will encapsulate the packet in a frame destined for the MAC address of the next hop-the next router in the path to the destination.
- For a router to forward packets between two hosts, the router needs routes to each host's network; it doesn't need routes to every network in the path between each destination.
- The command to configure a static route is ip route destination-network netmask \{next-hop | exit-interface | exit-interface next-hop\}.
- A static route that specifies only the next hop is called a recursive static route; it requires multiple lookups in the routing table to forward a packet: one to find the next-hop IP address and one to find which interface the next hop is connected to.
- A static route that specifies only the exit interface is called a directly connected static route because it causes the router to treat the network as a directly connected network.
- Directly connected static routes require proxy ARP to function. Proxy ARP allows a router to reply to ARP requests on behalf of other hosts. Proxy ARP is enabled on Cisco routers by default but might not be enabled on other vendors' routers.
- A static route that specifies both the exit interface and the next hop is called a fully specified static route.
- A default route is a route to 0.0.0.0/0. Because it is the least specific route possible, it will only be used to forward packets that don't match any other routes in the routing table.
- If a router has a default route, it won't drop packets that don't match other routes; instead, it will forward those packets using the default route.
- The default route is often used to provide a route to the internet; it is not feasible for a router to learn specific routes to each possible destination over the internet.
