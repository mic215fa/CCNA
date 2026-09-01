---
source: [[Chapter 12 - VLAN]]
chapter: 12
section: 12.1
tags: [source-section, acting-ccna, vlan]
---

# Chapter 12.1 - 為什麼需要 VLAN

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
