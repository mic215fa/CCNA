## The life of a packet

## This chapter covers

- A review of the processes involved in delivering a packet from source to destination
- How switches forward frames
- Address Resolution Protocol
- How routers forward packets

The concepts we have covered so far-the TCP/IP model, frame switching, ARP, IPv4 addresses, routing, etc.-are fundamental concepts we will build upon in the rest of this book's two volumes. In this chapter, we will review many of those concepts and see the role each plays in delivering a packet from the sending host to the packet's intended destination.

> [!translation] 逐句繁體中文翻譯
> 到目前為止，我們已涵蓋的 concepts，例如 TCP/IP model、frame switching、ARP、IPv4 addresses、routing 等，都是本書兩冊後續內容會建立在其上的 fundamental concepts。  
> 在本章中，我們會複習其中許多 concepts，並看看每一個 concept 在將 packet 從 sending host 送到 packet intended destination 的過程中扮演什麼角色。

This chapter is unique among the others in this book in that it does not cover new information; everything in this chapter has been covered in previous chapters. Instead of introducing new concepts, the goal of this chapter is to take the most important concepts from previous chapters and tie them all together into one coherent whole.

> [!translation] 逐句繁體中文翻譯
> 本章在本書其他章節中很特別，因為它不涵蓋新資訊；本章所有內容都已在先前章節介紹過。  
> 本章的目標不是介紹新 concepts，而是把前面章節最重要的 concepts 串起來，形成一個 coherent whole。

Figure 10.1 shows the network we will use for this chapter; we used the same when looking at routing in chapter 9. Figure 10.1 also summarizes the different processes involved in delivering a packet from PC1 to PC3: ARP, switching, routing, etc. We will review these processes throughout this chapter.

> [!translation] 逐句繁體中文翻譯
> Figure 10.1 顯示本章會使用的 network；我們在 chapter 9 查看 routing 時也使用同一個 network。  
> Figure 10.1 也摘要了從 PC1 將 packet 傳送到 PC3 所涉及的不同 processes：ARP、switching、routing 等。  
> 我們會在本章中複習這些 processes。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-183_487_1414_390_192.jpg)
Figure 10.1 A summary of actions taken by each device when PC1 sends a packet to PC3. PC1 prepares a packet addressed to PC3, uses ARP to learn the default gateway's MAC address (R1 G0/1), and sends the packet in a frame to that MAC address. The switches learn the MAC addresses of connected devices and forward/flood frames as appropriate. The routers select the best route to forward the packet, use ARP to learn the next hop's MAC address, and forward the packet in a frame addressed to that MAC address.

> [!translation] 逐句繁體中文翻譯
> Figure 10.1：當 PC1 傳送 packet 給 PC3 時，每個 device 採取的 actions 摘要。  
> PC1 準備一個 addressed to PC3 的 packet，使用 ARP 學習 default gateway 的 MAC address（R1 G0/1），並將 packet 放入 addressed to 該 MAC address 的 frame 中送出。  
> Switches 學習 connected devices 的 MAC addresses，並依情況 forward 或 flood frames。  
> Routers 選擇 best route 來 forward packet，使用 ARP 學習 next hop 的 MAC address，並將 packet 放入 addressed to 該 MAC address 的 frame 中 forward。

NOTE The arrows in figure 10.1 are a reminder that, at Layer 3, the packet is addressed to PC3 (IP address 192.168.3.11) throughout the whole journey. However, at Layer 2, the packet is encapsulated in a new frame at each hop, and each frame is addressed to the next hop (until R3 finally addresses its frame to PC3).

> [!translation] 逐句繁體中文翻譯
> 注意：figure 10.1 中的 arrows 提醒我們，在 Layer 3，packet 在整段旅程中都 addressed to PC3（IP address 192.168.3.11）。  
> 然而，在 Layer 2，packet 會在每一 hop 被 encapsulated in 新的 frame，而且每個 frame 都 addressed to next hop（直到 R3 最後將自己的 frame addressed to PC3）。

### 10.1 The life of a packet from PC1 to PC3

Figure 10.1 provides an outline of the different processes involved in delivering a packet from PC1 to PC3. Now let's examine the process step by step to see how the different components we've covered in the book so far come together to enable communications over the network.

> [!translation] 逐句繁體中文翻譯
> Figure 10.1 提供了從 PC1 將 packet 傳送到 PC3 所涉及不同 processes 的 outline。  
> 現在我們逐步檢視這個 process，看看本書目前介紹過的不同 components 如何結合起來，使 network communication 成為可能。

### 10.1.1 PC1 to R1

In our scenario, PC1 wants to send a packet to PC3. The type of packet is not significant for this example, so let's assume it's an ICMP echo request message sent by issuing the ping 192.168.3.11 command on PC1.

> [!translation] 逐句繁體中文翻譯
> 在我們的 scenario 中，PC1 想傳送 packet 給 PC3。  
> Packet 的 type 對此例並不重要，所以假設它是在 PC1 上執行 ping 192.168.3.11 command 所送出的 ICMP echo request message。

PC1's IP address is 192.168.1.11, and it has a /24 prefix length (netmask 255.255.255.0), so it knows that its local network includes IP addresses 192.168.1.0 (the network address) through 192.168.1.255 (the broadcast address). Therefore, it knows that PC3 (192.168.3.11) is not in its local network. This means that PC1 must send the
packet to its default gateway in a frame addressed to the default gateway's MAC address (rather than the MAC address of PC3 itself).

> [!translation] 逐句繁體中文翻譯
> PC1 的 IP address 是 192.168.1.11，且具有 /24 prefix length（netmask 255.255.255.0），所以它知道自己的 local network 包含從 192.168.1.0（network address）到 192.168.1.255（broadcast address）的 IP addresses。  
> 因此，它知道 PC3（192.168.3.11）不在自己的 local network 中。  
> 這表示 PC1 必須把 packet 放入 addressed to default gateway MAC address 的 frame 中送給 default gateway，而不是送到 PC3 本身的 MAC address。

PC1 knows that its default gateway's IP address is 192.168.1.1 (most likely learned via DHCP, which we will cover in chapter 4 of volume 2 ), but the information it actually needs is the MAC address of the default gateway; it needs to send the packet (destined for PC3) in a frame addressed to R1 G0/1's MAC address. To learn R1 G0/1's MAC address, it will use ARP. Figure 10.2 outlines the ARP exchange between PC1 and R1.

> [!translation] 逐句繁體中文翻譯
> PC1 知道自己的 default gateway IP address 是 192.168.1.1（最可能透過 DHCP 學到，這會在 volume 2 chapter 4 介紹）。  
> 但它實際需要的資訊是 default gateway 的 MAC address；它需要將 destined for PC3 的 packet 放入 addressed to R1 G0/1 MAC address 的 frame 中。  
> 為了學習 R1 G0/1 的 MAC address，它會使用 ARP。  
> Figure 10.2 描述 PC1 與 R1 之間的 ARP exchange。

NOTE ARP isn't used to learn the MAC address of the packet's destination (PC3), but the MAC address of the default gateway (R1 G0/1). Because PC1 and PC3 are in separate LANs, they do not need to know each other's MAC address.

> [!translation] 逐句繁體中文翻譯
> 注意：ARP 不是用來學習 packet destination（PC3）的 MAC address，而是用來學習 default gateway（R1 G0/1）的 MAC address。  
> 因為 PC1 與 PC3 位於不同 LANs，所以它們不需要知道彼此的 MAC address。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-184_750_1412_719_225.jpg)
Figure 10.2 PC1 uses ARP to learn R1 G0/1's MAC address. (1) PC1 sends an ARP request to 192.168.1.1. SW1 learns PC1's MAC address and floods the frame due to the destination MAC address of ffff.ffff.ffff. (2) After receiving the ARP request, R1 adds an ARP table entry associating IP address 192.168.1.11 with PC1's MAC address. R1 then sends an ARP reply to PC1. SW1 learns R1 G0/1's MAC address and forwards the frame to PC1. (3) After receiving the ARP reply, PC1 adds an ARP table entry associating IP address 192.168.1.1 with R1 G0/1's MAC address.

> [!translation] 逐句繁體中文翻譯
> Figure 10.2：PC1 使用 ARP 學習 R1 G0/1 的 MAC address。  
> （1）PC1 對 192.168.1.1 送出 ARP request。  
> SW1 學到 PC1 的 MAC address，並因 destination MAC address 為 ffff.ffff.ffff 而 flood 該 frame。  
> （2）收到 ARP request 後，R1 新增 ARP table entry，將 IP address 192.168.1.11 與 PC1 的 MAC address 關聯起來。  
> 接著 R1 傳送 ARP reply 給 PC1。  
> SW1 學到 R1 G0/1 的 MAC address，並將 frame forward 給 PC1。  
> （3）收到 ARP reply 後，PC1 新增 ARP table entry，將 IP address 192.168.1.1 與 R1 G0/1 的 MAC address 關聯起來。

NOTE To reduce clutter, figure 10.2 only briefly mentions PC2. Upon receiving the ARP request from PC1 (which was flooded by SW1), PC2 simply drops the message-the ARP request is not addressed to PC2's own IP address, so PC2 ignores it.

> [!translation] 逐句繁體中文翻譯
> 注意：為了減少雜訊，figure 10.2 只簡短提到 PC2。  
> PC2 收到來自 PC1 的 ARP request（由 SW1 flood）後，只會直接 drop 該 message；ARP request 不是 addressed to PC2 自己的 IP address，所以 PC2 會忽略它。

After the ARP exchange, PC1 now knows the MAC address of its default gateway (in the process, R1 also learns PC1's MAC address and creates an ARP entry). PC1 can
now encapsulate the packet to PC3 in a frame addressed to R1's G0/1 interface. In the following section, we'll see what actions R1 takes upon receiving the frame (and the packet inside the frame) from PC1.

> [!translation] 逐句繁體中文翻譯
> ARP exchange 之後，PC1 現在知道自己的 default gateway MAC address。  
> 在此過程中，R1 也學到 PC1 的 MAC address，並建立 ARP entry。  
> PC1 現在可以把送往 PC3 的 packet encapsulate in addressed to R1 G0/1 interface 的 frame。  
> 在下一節中，我們會看到 R1 收到來自 PC1 的 frame（以及 frame 內的 packet）後會採取哪些 actions。

SW1's role is to learn the MAC addresses of connected devices and then forward or flood frames as necessary. It will flood broadcast frames (i.e., PC1's ARP request) and unknown unicast frames. It will forward known unicast frames (i.e., R1's ARP reply).

> [!translation] 逐句繁體中文翻譯
> SW1 的角色是學習 connected devices 的 MAC addresses，然後視需要 forward 或 flood frames。  
> 它會 flood broadcast frames（例如 PC1 的 ARP request）與 unknown unicast frames。  
> 它會 forward known unicast frames（例如 R1 的 ARP reply）。

EXAM TIP Know the difference between a switch's MAC address table and an end host or router's ARP table. A MAC address table maps MAC addresses to switch ports and is used to allow a switch to forward frames out of the correct port. An ARP table maps IP addresses to MAC addresses and is used to allow a router or end host to encapsulate packets in frames with the proper destination MAC address.

> [!translation] 逐句繁體中文翻譯
> 考試提示：要知道 switch 的 MAC address table 與 end host 或 router 的 ARP table 之間的差異。  
> MAC address table 將 MAC addresses map 到 switch ports，用來讓 switch 從正確 port forward frames。  
> ARP table 將 IP addresses map 到 MAC addresses，用來讓 router 或 end host 把 packets encapsulate in 具有正確 destination MAC address 的 frames。

### 10.1.2 R1 to R2

When R1 receives the frame from PC1, it de-encapsulates it and examines the packet inside. As covered in chapter 9, it then performs a routing table lookup-it looks for the most specific matching route (the matching route with the longest prefix length). The following example shows R1's routing table:

> [!translation] 逐句繁體中文翻譯
> 當 R1 收到來自 PC1 的 frame 時，它會 de-encapsulate 該 frame，並檢查裡面的 packet。  
> 如 chapter 9 所述，它接著會執行 routing table lookup，也就是尋找 most specific matching route（具有 longest prefix length 的 matching route）。  
> 下列範例顯示 R1 的 routing table：

```
R1# show ip route
. . .
    192.168.1.0/24 is variably subnetted, 2 subnets, 2 masks
C 192.168.1.0/24 is directly connected, GigabitEthernet0/1
L 192.168.1.1/32 is directly connected, GigabitEthernet0/1
S 192.168.3.0/24 [1/0] via 192.168.12.2, GigabitEthernet0/0
    192.168.12.0/24 is variably subnetted, 2 subnets, 2 masks
C 192.168.12.0/24 is directly connected, GigabitEthernet0/0
L 192.168.12.1/32 is directly connected, GigabitEthernet0/0
```

The most specific matching route

> [!translation] 逐句繁體中文翻譯
> Most specific matching route。

The most specific matching route is the static route to 192.168.3.0/24 via next hop 192.168.12.2 (actually, it's the only matching route). However, just like how PC1 knew the IP address of its default gateway but not the MAC address (and therefore had to use ARP to learn the MAC address), R1 knows the IP address of the next hop but not its MAC address (and therefore has to use ARP).

> [!translation] 逐句繁體中文翻譯
> Most specific matching route 是透過 next hop 192.168.12.2 到 192.168.3.0/24 的 static route（實際上，它是唯一的 matching route）。  
> 不過，就像 PC1 知道 default gateway 的 IP address 但不知道 MAC address，因此必須使用 ARP 學習 MAC address 一樣，R1 知道 next hop 的 IP address，但不知道它的 MAC address，因此也必須使用 ARP。

R1 sends an ARP request to learn the MAC address of 192.168.12.2 (R2 G0/0), and R2 sends an ARP reply. In the process, they both learn each other's MAC addresses and create entries in their ARP tables. R1 is now ready to encapsulate the packet in a frame addressed to R2 G0/0's MAC address and forward it out of the G0/0 interface. Figure 10.3 outlines the process up to this point.

> [!translation] 逐句繁體中文翻譯
> R1 傳送 ARP request 來學習 192.168.12.2（R2 G0/0）的 MAC address，R2 則傳送 ARP reply。  
> 在此過程中，它們都學到彼此的 MAC addresses，並在各自 ARP tables 中建立 entries。  
> R1 現在已準備好把 packet encapsulate in addressed to R2 G0/0 MAC address 的 frame，並從 G0/0 interface forward 出去。  
> Figure 10.3 描述到目前為止的 process。

NOTE The ARP request sent from R1 to R2 is addressed to the broadcast MAC address (ffff.ffff.ffff). However, R2 is the only device that receives the message- there is no switch to flood the frame in the LAN between R1 and R2.

> [!translation] 逐句繁體中文翻譯
> 注意：R1 送給 R2 的 ARP request 是 addressed to broadcast MAC address（ffff.ffff.ffff）。  
> 然而，R2 是唯一收到該 message 的 device；在 R1 與 R2 之間的 LAN 中沒有 switch 來 flood frame。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-186_687_1415_181_222.jpg)
Figure 10.3 R1 receives PC1's message, performs a routing table lookup, and uses ARP to learn the MAC address of the next hop. (1) R1 receives the frame/packet and performs a routing table lookup. The most specific matching route is to 192.168.3.0/24, next hop 192.168.12.2. (2) R1 uses ARP to learn the MAC address of 192.168.12.2 (R2 G0/0). R1 and R2 both add entries to their ARP tables. R1 is now ready to forward the packet to the next hop.

NOTE I have simplified the ARP exchange in figure 10.3, but remember that it consists of an ARP request from R1 to R2 and then an ARP reply from R2 to R1.

> [!translation] 逐句繁體中文翻譯
> 注意：我在 figure 10.3 中簡化了 ARP exchange，但請記得它包含 R1 到 R2 的 ARP request，接著是 R2 到 R1 的 ARP reply。

### 10.1.3 R2 to R3

When R2 receives the frame from R1, it de-encapsulates it and examines the packet inside. The process it then goes through is identical to the process R1 went through previously. First, it performs a routing table lookup to find the most specific matching route. The following example shows R2's routing table:

> [!translation] 逐句繁體中文翻譯
> 當 R2 收到來自 R1 的 frame 時，它會 de-encapsulate 該 frame，並檢查裡面的 packet。  
> 接著它經歷的 process 與先前 R1 經歷的 process 相同。  
> 首先，它執行 routing table lookup，以找出 most specific matching route。  
> 下列範例顯示 R2 的 routing table：

```
R2# show ip route
. . .
S 192.168.1.0/24 [1/0] via 192.168.12.1, GigabitEthernet0/0
S 192.168.3.0/24 [1/0] via 192.168.23.2, GigabitEthernet0/1
    192.168.12.0/24 is variably subnetted, 2 subnets, 2 masks
C 192.168.12.0/24 is directly connected, GigabitEthernet0/0
L 192.168.12.2/32 is directly connected, GigabitEthernet0/0
    192.168.23.0/24 is variably subnetted, 2 subnets, 2 masks
C 192.168.23.0/24 is directly connected, GigabitEthernet0/1
L 192.168.23.1/32 is directly connected, GigabitEthernet0/1
```

The only route that matches destination 192.168.3.11 is the static route to 192.168.3.0/24, via next hop 192.168.23.2 (R3's G0/0 interface). To learn the MAC address of the next hop, R2 sends an ARP request, and R3 sends an ARP reply. In the
process, R2 and R3 create entries in their ARP tables, and R2 is now ready to forward the packet in a frame addressed to R3 G0/0's MAC address. Figure 10.4 outlines this process.

> [!translation] 逐句繁體中文翻譯
> 唯一 match destination 192.168.3.11 的 route，是經由 next hop 192.168.23.2（R3 的 G0/0 interface）到 192.168.3.0/24 的 static route。  
> 為了學習 next hop 的 MAC address，R2 傳送 ARP request，R3 則傳送 ARP reply。  
> 在此過程中，R2 與 R3 會在各自 ARP tables 中建立 entries，R2 現在已準備好把 packet 放入 addressed to R3 G0/0 MAC address 的 frame 中 forward。  
> Figure 10.4 描述這個 process。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-187_687_1413_347_192.jpg)
Figure 10.4 R2 receives the frame from R1, performs a routing table lookup and uses ARP to learn the MAC address of the next hop. (1) R2 receives the frame/packet and performs a routing table lookup. The most specific matching route is to 192.168.3.0/24, next hop 192.168.23.2. (2) R2 uses ARP to learn the MAC address of 192.168.23.2 (R3 G0/0). R2 and R3 both add entries to their ARP tables. R2 is now ready to forward the packet to the next hop.

### 10.1.4 R3 to PC3

After R2 forwards the message and it reaches R3, the packet is now at the final router before the destination. R3 goes through the same process as R1 and R2 did previously; it performs a routing table lookup to find the most specific matching route. The following example shows R3's routing table:

> [!translation] 逐句繁體中文翻譯
> R2 forward message 並抵達 R3 後，packet 現在位於 destination 前的最後一台 router。  
> R3 會經歷與先前 R1 和 R2 相同的 process；它執行 routing table lookup 來找出 most specific matching route。  
> 下列範例顯示 R3 的 routing table：

```
R3# show ip route The most specific matching route
. . .
S 192.168.1.0/24 [1/0] via 192.168.23.1, GigabitEthernet0/0
    192.168.3.0/24 is variably subnetted, 2 subnets, 2 masks
C 192.168.3.0/24 is directly connected, GigabitEthernet0/1
L 192.168.3.1/32 is directly connected, GigabitEthernet0/1
    192.168.23.0/24 is variably subnetted, 2 subnets, 2 masks
C 192.168.23.0/24 is directly connected, GigabitEthernet0/0
L 192.168.23.2/32 is directly connected, GigabitEthernet0/0
```

The only matching route is the route to 192.168.3.0/24, which is a connected route. Because the packet's destination is in a directly connected network, R3 will encapsulate the packet in a frame addressed to the destination host's MAC address-the MAC address of PC3. To do that, it must use ARP.

> [!translation] 逐句繁體中文翻譯
> 唯一的 matching route 是到 192.168.3.0/24 的 route，也就是 connected route。  
> 因為 packet 的 destination 位於 directly connected network 中，R3 會將 packet encapsulate in addressed to destination host MAC address 的 frame，也就是 PC3 的 MAC address。  
> 若要做到這件事，它必須使用 ARP。

SW2 floods the ARP request message to both PC3 and PC4; PC4 ignores it, but PC3 sends an ARP reply back to R3. In that process, SW2 learns the MAC addresses of R3 G0/1 and PC3. R3 and PC3 also learn each other's MAC addresses and add entries to their ARP tables. Figure 10.5 demonstrates this process.

> [!translation] 逐句繁體中文翻譯
> SW2 會將 ARP request message flood 給 PC3 與 PC4。  
> PC4 會忽略它，但 PC3 會傳送 ARP reply 回 R3。  
> 在此過程中，SW2 學到 R3 G0/1 與 PC3 的 MAC addresses。  
> R3 與 PC3 也會學到彼此的 MAC addresses，並將 entries 加入各自 ARP tables。  
> Figure 10.5 示範這個 process。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-188_830_1412_392_225.jpg)
Figure 10.5 R3 receives the frame from R2, performs a routing table lookup and uses ARP to learn the MAC address of the next hop. (1) R3 receives the frame/packet and performs a routing table lookup. The most specific matching route is the connected route to 192.168.3.0/24. (2) R3 uses ARP to learn the MAC address of 192.168.3.11 (PC3). SW2 learns the MAC addresses of R3 G0/1 and PC3. R3 and PC3 both add entries to their ARP tables. R3 is now ready to forward the packet to the destination.

R3 is now able to forward the packet in a frame addressed to PC3's MAC address. The packet has reached its final destination! Upon receipt of the packet, PC3 will process it as appropriate. Earlier I stated that PC1's message was an ICMP echo request message. In that case, PC3 will send an ICMP echo reply message back to PC1.

> [!translation] 逐句繁體中文翻譯
> R3 現在能夠將 packet 放入 addressed to PC3 MAC address 的 frame 中 forward。  
> Packet 已經抵達 final destination！  
> 收到 packet 後，PC3 會依適當方式處理它。  
> 先前我說 PC1 的 message 是 ICMP echo request message。  
> 在那種情況下，PC3 會傳送 ICMP echo reply message 回 PC1。

### 10.2 The life of a packet from PC3 to PC1

The processes involved in delivering PC3's response to PC1 are similar, but there are two major differences: the switches have already learned the necessary MAC addresses, and the PCs and routers already have the necessary ARP table entries. This simplifies the process a bit-because the devices already have the necessary information in their tables, there is no need for the switches to learn MAC addresses or the PCs and routers to use ARP. Figure 10.6 outlines how PC3's packet is delivered to PC1, addressed to and from different MAC addresses at each hop.

> [!translation] 逐句繁體中文翻譯
> 將 PC3 的 response 傳送給 PC1 所涉及的 processes 類似，但有兩個主要差異。  
> Switches 已經學到必要的 MAC addresses，而且 PCs 與 routers 已經有必要的 ARP table entries。  
> 這讓 process 稍微簡化，因為 devices 的 tables 中已經有必要資訊，所以 switches 不需要再學習 MAC addresses，PCs 與 routers 也不需要再使用 ARP。  
> Figure 10.6 描述 PC3 的 packet 如何被傳送到 PC1，並且在每個 hop 使用不同 source/destination MAC addresses。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-189_487_1414_181_192.jpg)
Figure 10.6 PC3 sends a reply to PC1. PC3 sends the packet in a frame addressed to the default gateway (R3 G0/1), and SW2 forwards the frame to R3. R3 forwards the packet in a frame to R2 G0/1, and R2 forwards the packet in a frame to R1 G0/0. Finally, R1 forwards the packet in a frame to the destination (PC1), and SW1 forwards the frame to PC1.

Aside from the lack of MAC address learning and ARP, the process is the same as before. PC3 sends its packet in a frame addressed to the default gateway, which SW2 forwards out of the proper port. The routers in the path perform routing table lookups to forward the packet toward the next hop until R1 forwards it in a frame addressed to PC1 itself, and the frame is forwarded to PC1 by SW1.

> [!translation] 逐句繁體中文翻譯
> 除了沒有 MAC address learning 與 ARP 之外，process 與先前相同。  
> PC3 將自己的 packet 放入 addressed to default gateway 的 frame 中送出，SW2 會從正確 port forward 該 frame。  
> Path 中的 routers 執行 routing table lookups，將 packet forward toward next hop，直到 R1 將它放入 addressed to PC1 本身的 frame 中 forward，最後該 frame 由 SW1 forward 給 PC1。

## Summary

- To send packets to remote destinations, an end host (such as a PC) will send the packet to its default gateway (router). To do so, it will encapsulate the packet in a frame addressed to the default gateway's MAC address. It uses ARP to learn the default gateway's MAC address.
- ARP involves two messages: ARP request (broadcast) and ARP reply (unicast).
- When a device receives an ARP request, it doesn't just send an ARP reply; it also makes an entry in its own ARP table, mapping the IP address of the host that sent the request to that host's MAC address.
- Switches learn MAC addresses and forward or flood frames as appropriate. They do not modify the frames they forward; their operations are transparent to the devices connected to them.
- A switch will flood broadcast and unknown unicast frames. It will forward known unicast frames.
- When a router receives a frame addressed to its own MAC address, it will de-encapsulate it and examine the packet inside. It then performs a routing table lookup to determine how to forward the packet (or drop the packet or receive it for itself).
- A router will forward a packet according to the most specific matching route: the matching route with the longest prefix length.

- To forward a packet to the next hop in the path, a router will forward the packet in a frame addressed to the next hop's MAC address. It uses ARP to learn the next hop's MAC address.
- To forward a packet to the packet's destination host, a router will forward the packet in a frame addressed to the destination host's MAC address, using ARP to learn the MAC address.
