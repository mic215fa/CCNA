## Subnetting IPv4 networks

## This chapter covers

- What subnetting is and why it's necessary
- How to borrow bits from the host portion of a network to expand the network portion and create subnets
- How to identify the five attributes of a subnet
- How to divide a network into subnets of equal and variable sizes

In chapter 7, we covered IPv4 address classes, focusing on classes A, B, and C-the three classes of addresses which can be assigned to hosts. Each class is defined by the first bit(s) of the address, and the prefix length of addresses in each range is also defined:

> [!translation] 逐句繁體中文翻譯
> - 在第 7 章，我們介紹了 IPv4 位址類別，重點放在 A、B、C 三類，也就是可以指派給主機的三種位址類別。
> - 每一類都由位址開頭的 bit 定義，而且每個範圍的 prefix length 也已經被定義好。

- Class A addresses begin with 0b0 and use a /8 prefix length.
- Class B addresses begin with 0b10 and use a /16 prefix length.
- Class C addresses begin with 0b110 and use a /24 prefix length.

This addressing architecture, called classful addressing, was defined in the original Internal Protocol standard in 1981 (RFC 791). However, with the rapid growth of the internet, classful addressing soon proved to be too rigid, resulting in inefficient use of addresses; the pool of available IPv4 addresses was drying up. Subnetting, which involves dividing a larger network up into smaller networks, is one answer to this problem and is a fundamental skill for network engineers. Subnetting is the second half of CCNA exam topic 1.6: Configure and verify IPv4 addressing and subnetting.

> [!translation] 逐句繁體中文翻譯
> - 這種位址架構稱為 classful addressing，最早在 1981 年的 Internet Protocol 標準 RFC 791 中定義。
> - 不過，隨著 Internet 快速成長，classful addressing 很快就顯得太僵硬，造成位址使用效率低落；可用的 IPv4 位址池也逐漸枯竭。
> - Subnetting，也就是把較大的 network 切成較小 networks，是解決這個問題的方法之一，也是網路工程師的基礎技能。
> - Subnetting 是 CCNA 考試主題 1.6「Configure and verify IPv4 addressing and subnetting」的後半部分。

Before we get started, I want to emphasize that subnetting is a skill, and it requires practice to become proficient. Just reading this chapter alone won't make you good at subnetting; you need to spend some time actually doing it. However, if you read through this chapter carefully and do the recommended practice, you'll be a confident "subnetter" in no time.

> [!translation] 逐句繁體中文翻譯
> - 開始之前，我想強調 subnetting 是一項技能，需要練習才會熟練。
> - 只讀完這一章並不會讓你擅長 subnetting；你必須實際花時間動手做。
> - 不過，只要你仔細讀完本章並完成建議練習，很快就會成為有信心的 subnetter。

### 11.1 What is subnetting?

The problem with classful addressing is that it doesn't allow us to create networks of appropriate sizes. The smallest network size-a class C network-contains 254 usable addresses ( $2^{8}-2$ for the network and broadcast addresses). That is far more addresses than necessary for a home network or many small offices. Assigning a class C network to a small office with only a few dozen devices would result in over 200 IP addresses left unused.

> [!translation] 逐句繁體中文翻譯
> - Classful addressing 的問題在於，它不允許我們建立大小合適的 networks。
> - 最小的 network size，也就是 class C network，包含 254 個可用位址（$2^{8}-2$，扣掉 network 與 broadcast addresses）。
> - 這對家庭網路或許多小型辦公室來說遠遠太多。
> - 若把一個 class C network 指派給只有數十台設備的小型辦公室，會留下超過 200 個 IP 位址未使用。

However, a class C network is too small for most enterprise networks, meaning that a class B network would be required. A class B network contains $65,534\left(2^{16}-2\right)$ usable addresses, far more than even a very large network requires, resulting in thousands of wasted addresses. And a class A network contains 16,777,214 usable addresses, a ludicrous number of addresses for a single network. These classful rules were designed for simplicity, not efficiency, but with the internet's exploding popularity, a better solution was needed.

> [!translation] 逐句繁體中文翻譯
> - 然而，class C network 對大多數企業網路又太小，於是會需要 class B network。
> - Class B network 包含 $65,534\left(2^{16}-2\right)$ 個可用位址，遠超過即使是大型網路所需，造成成千上萬個位址浪費。
> - 而 class A network 包含 16,777,214 個可用位址，對單一 network 來說更是不合理地龐大。
> - 這些 classful 規則是為了簡單，而不是效率；但隨著 Internet 爆炸性普及，需要更好的解法。

To support the fast-growing internet and use the available IPv4 address space more efficiently, a new system was introduced in 1993: Classless Inter-Domain Routing (CIDR, pronounced like "cider"). CIDR throws out the rules of classful addressing and replaces them with a more flexible system. With CIDR, prefix lengths don't have to be /8, / 16, or /24. Instead, the boundary between the network portion and host portion of an IP address can be in the middle of an octet, resulting in prefix lengths like /23, /26, /28, etc.

> [!translation] 逐句繁體中文翻譯
> - 為了支援快速成長的 Internet，並更有效率地使用可用的 IPv4 位址空間，1993 年導入了一套新系統：Classless Inter-Domain Routing（CIDR，發音像 cider）。
> - CIDR 拋棄 classful addressing 的規則，改用更有彈性的系統。
> - 在 CIDR 中，prefix length 不必只能是 /8、/16 或 /24。
> - 相反地，IP 位址中 network portion 與 host portion 的邊界可以落在 octet 中間，因此會出現 /23、/26、/28 等 prefix lengths。

NOTE The method of notating an address's prefix length with /X is also known as CIDR notation because it was introduced with CIDR. Before CIDR notation, the prefix length was always indicated with a netmask, such as 255.255.255.0. Another term for netmask is subnet mask; I will use the latter term in this chapter because we are focusing on subnetting, but the terms are interchangeable.

> [!translation] 逐句繁體中文翻譯
> - NOTE：用 /X 表示位址 prefix length 的方法也稱為 CIDR notation，因為它是隨 CIDR 被導入的。
> - 在 CIDR notation 之前，prefix length 總是用 netmask 表示，例如 255.255.255.0。
> - Netmask 的另一個名稱是 subnet mask；本章因為聚焦在 subnetting，所以會使用 subnet mask 這個詞，但兩者可以互換。

With CIDR, an enterprise can be assigned an address block that can be divided into networks of appropriate size, called subnets (subdivided networks). The process of dividing an address block into subnets is called subnetting.

> [!translation] 逐句繁體中文翻譯
> - 有了 CIDR，企業可以被分配一段 address block，並把它切成大小合適的 networks，稱為 subnets（subdivided networks）。
> - 把 address block 分割成 subnets 的過程稱為 subnetting。

NOTE An address block is a range of IP addresses. It can be used to refer to a network before it has been subnetted. For example, 192.168.1.0/24 is an address block (including IP addresses 192.168.1.0 through 192.168.1.255), which can be divided into multiple smaller networks (subnets).

> [!translation] 逐句繁體中文翻譯
> - NOTE：Address block 是一段 IP 位址範圍。
> - 它可以用來指尚未被 subnetted 的 network。
> - 例如 192.168.1.0/24 是一個 address block，包含 192.168.1.0 到 192.168.1.255，並且可以被切成多個較小 networks，也就是 subnets。

Figure 11.1 demonstrates how a /24 address block can be divided into subnets. The 192.168.1.0/24 address range allows for a single subnet with a /24 prefix length, including all addresses from 192.168.1.0 through 192.168.1.255. Dividing the /24 address block in half gives two /25 subnets, each containing 128 addresses. Or it can be divided into four /26 subnets, each containing 64 addresses. For each bit by which you extend the prefix length, the number of possible subnets doubles, but the number of addresses in each subnet halves.

> [!translation] 逐句繁體中文翻譯
> - Figure 11.1 示範如何把一個 /24 address block 切成 subnets。
> - 192.168.1.0/24 位址範圍可形成一個 /24 prefix length 的 subnet，包含 192.168.1.0 到 192.168.1.255。
> - 把 /24 address block 對半切開會得到兩個 /25 subnets，每個包含 128 個位址。
> - 也可以切成四個 /26 subnets，每個包含 64 個位址。
> - 每當你把 prefix length 延長 1 個 bit，可能的 subnet 數量就會加倍，但每個 subnet 內的位址數會減半。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-193_807_1391_902_190.jpg)
Figure 11.1 The 192.168.1.0/24 address block (network) divided into smaller subnets. With a /24 prefix length, it is one subnet of 256 addresses. Using a /25 prefix length allows the block to be divided into two subnets of 128 addresses each. /26 allows for 4 subnets of 64 addresses each. /27 allows for 8 subnets of 32 addresses each. /28 allows for 16 subnets of 16 addresses each.

NOTE Figure 11.1 only shows prefix lengths of up to /28, but longer prefix lengths follow the same pattern: increasing the length of the prefix length by 1 bit doubles the number of possible subnets, but halves the number of addresses contained in each subnet.

> [!translation] 逐句繁體中文翻譯
> - NOTE：Figure 11.1 只顯示到 /28 的 prefix length，但更長的 prefix length 也遵循相同模式：prefix length 每增加 1 bit，可能的 subnet 數量就會加倍，而每個 subnet 包含的位址數會減半。

### 11.2 FLSM subnetting

Subnetting is the process of dividing an address block into smaller subnets. That process can be done in a couple of different ways: Fixed-Length Subnet Masking (FLSM) divides the block into subnets of equal sizes. On the other hand, Variable-Length Subnet Masking (VLSM) divides the block into subnets of varying sizes depending on how many addresses are actually needed in the subnet.

> [!translation] 逐句繁體中文翻譯
> - Subnetting 是把 address block 分割成較小 subnets 的過程。
> - 這個過程可以用幾種方式完成：Fixed-Length Subnet Masking（FLSM）會把 block 切成大小相同的 subnets。
> - 另一方面，Variable-Length Subnet Masking（VLSM）會依照 subnet 實際需要的位址數，把 block 切成不同大小的 subnets。

In the real world, VLSM is what you'll be using; it allows you to more efficiently use a block of addresses since you can make each subnet only as large as it needs to be-this wastes fewer addresses. However, FLSM serves as a useful stepping stone when learning how to subnet, and you should know it for the CCNA exam, so in this section, we will focus on FLSM.

> [!translation] 逐句繁體中文翻譯
> - 在真實世界中，你會使用的是 VLSM；它可以讓你更有效率地使用一段 address block，因為每個 subnet 只需要做得剛好足夠大，這會浪費較少位址。
> - 不過，FLSM 是學習 subnetting 時很有用的踏腳石，而且 CCNA 考試也要求你知道它，所以本節會聚焦在 FLSM。

### 11.2.1 Subnetting /24 address blocks

First, we will look at how to subnet address blocks with a prefix length of / 24 or greater. The reason for this is that it allows us to focus only on the final octet of the address, simplifying the process a bit.

> [!translation] 逐句繁體中文翻譯
> - 首先，我們會看如何 subnet prefix length 為 /24 或更長的 address blocks。
> - 原因是這讓我們只需要專注在位址的最後一個 octet，能稍微簡化流程。

The network portion of an address block cannot be changed; if you are given the 192.168.1.0/24 address block, you can't assign 192.168.2.1 (or any other IP address not included in the 192.168.1.0/24 range) to a host. However, the host portion is fair game-you can use the last 8 bits to make various IP addresses to assign to hosts. This is the key to subnetting; to make subnets, you "borrow" bits from the host portion and add them to the network portion. You are then free to change the binary value of those borrowed bits between 0 and 1 to make different subnets. Figure 11.2 demonstrates how 1 bit can be borrowed from the host portion of 192.168.1.0/24 to make two different subnets: 192.168.1.0/25 and 192.168.1.128/25.

> [!translation] 逐句繁體中文翻譯
> - Address block 的 network portion 不能更改；如果你拿到 192.168.1.0/24 address block，就不能把 192.168.2.1 或任何不在 192.168.1.0/24 範圍內的 IP 位址指派給 host。
> - 不過，host portion 可以使用；你可以用最後 8 bits 建立不同 IP 位址來指派給 hosts。
> - 這就是 subnetting 的關鍵：要建立 subnets，你會從 host portion「借」bits，並把它們加到 network portion。
> - 接著你可以自由把這些 borrowed bits 的 binary 值在 0 與 1 之間改變，以建立不同 subnets。
> - Figure 11.2 示範如何從 192.168.1.0/24 的 host portion 借 1 bit，建立兩個不同 subnets：192.168.1.0/25 與 192.168.1.128/25。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-194_569_1161_1361_348.jpg)
Figure 11.2 Borrowing 1 bit from the host portion of 192.168.1.0/24 allows us to create two subnets: 192.168.1.0/25 and 192.168.1.128/25. The borrowed bit was part of the host portion of the original address block, but it is part of the network portion of each subnet (as indicated by the /25 prefix length).

NOTE In previous examples, the network address always ended in .0. However, when subnetting, because the boundary between the network portion and host portion can lie in the middle of an octet, the network address does not necessarily end in .0. 192.168.1.0 is the network address of the 192.168.1.0/25 subnet, and 192.168.1.128 is the network address of the 192.168.1.128/25 subnet.

> [!translation] 逐句繁體中文翻譯
> - NOTE：在前面的例子中，network address 總是以 .0 結尾。
> - 不過在 subnetting 時，因為 network portion 與 host portion 的邊界可以落在 octet 中間，network address 不一定會以 .0 結尾。
> - 192.168.1.0 是 192.168.1.0/25 subnet 的 network address，而 192.168.1.128 是 192.168.1.128/25 subnet 的 network address。

With the borrowed bit set to 0, we get the first subnet: 192.168.1.0/25. If we change the borrowed bit to 1 , we get the second subnet: 192.168.1.128/25. That's how subnets are made: by changing the binary value of the borrowed bit(s).

> [!translation] 逐句繁體中文翻譯
> - 當 borrowed bit 設為 0 時，我們得到第一個 subnet：192.168.1.0/25。
> - 如果把 borrowed bit 改成 1，就得到第二個 subnet：192.168.1.128/25。
> - Subnets 就是這樣產生的：改變 borrowed bit 或 borrowed bits 的 binary 值。

Borrowing a single bit from the host portion allows us to make two subnets, so how many subnets can we make if we borrow 2 bits? We covered this in section 11.1: each bit added to the prefix length (each bit borrowed from the host portion) doubles the number of subnets. The formula is $2^{\mathrm{x}}$, where $x$ is the number of borrowed bits, and therefore borrowing 2 bits allows us to make $4\left(2^{2}\right)$ subnets. Figure 11.3 shows the four subnets that can be made by borrowing 2 bits from the host portion of the 192.168.1.0/24 block.

> [!translation] 逐句繁體中文翻譯
> - 從 host portion 借 1 bit 可建立兩個 subnets，那麼借 2 bits 可以建立多少 subnets？我們在 11.1 節已經講過：prefix length 每增加 1 bit，也就是從 host portion 每借 1 bit，subnet 數量就會加倍。
> - 公式是 $2^{\mathrm{x}}$，其中 x 是 borrowed bits 的數量，因此借 2 bits 可以建立 $4\left(2^{2}\right)$ 個 subnets。
> - Figure 11.3 顯示從 192.168.1.0/24 block 的 host portion 借 2 bits 可建立的四個 subnets。

192.168.1.0/24 address block
![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-195_577_1108_873_384.jpg)

> [!translation] 逐句繁體中文翻譯
> - 192.168.1.0/24 address block。

Figure 11.3 Borrowing 2 bits from 192.168.1.0/24 allows for four subnets: 192.168.1.0/26, 192.168.1.64/26, 192.168.1.128/26, and 192.168.1.192/26.

> [!translation] 逐句繁體中文翻譯
> - Figure 11.3：從 192.168.1.0/24 借 2 bits 可建立四個 subnets：192.168.1.0/26、192.168.1.64/26、192.168.1.128/26 與 192.168.1.192/26。

NOTE In the first subnet, the borrowed bits are 00. How can you know what the next subnet is? Just count up in binary: the number after 00 is 01, then 10, and then 11. As we covered in chapter 7, counting in binary is the same process as counting in decimal, except there are only two digits to work with: 0 and 1.

> [!translation] 逐句繁體中文翻譯
> - NOTE：在第一個 subnet 中，borrowed bits 是 00。
> - 你怎麼知道下一個 subnet 是什麼？只要用 binary 往上數：00 之後是 01，接著是 10，再來是 11。
> - 正如第 7 章所說，binary counting 與 decimal counting 是同樣的流程，只是可用的數字只有 0 與 1。

## Calculating the five attributes of a /24+ subnet

In chapter 7, we covered five attributes of an IPv4 network: network address, broadcast address, first usable address, last usable address, and maximum number of hosts. Those same attributes apply when dividing networks into subnets. Here's a quick review of each attribute:

> [!translation] 逐句繁體中文翻譯
> - 在第 7 章，我們介紹了 IPv4 network 的五個 attributes：network address、broadcast address、first usable address、last usable address，以及 maximum number of hosts。
> - 把 networks 切成 subnets 時，這些 attributes 同樣適用。
> - 以下快速複習每個 attribute。

- Network address-The first address of a subnet, with a host portion of all 0s.
- Broadcast address-The last address of a subnet, with a host portion of all 1s.
- First usable address-The first address in the subnet that can be assigned to a host. It can be calculated by adding 1 to the network address (changing the last bit to 1).
- Last usable address-The last address in the subnet that can be assigned to a host. It can be calculated by subtracting 1 from the broadcast address (changing the last bit to 0).
- Maximum number of hosts-The number of IP addresses available to assign to hosts. The formula is $2^{\mathrm{y}}-2$, where $y$ is the number of bits in the host portion. 2 is subtracted because the network and broadcast addresses cannot be assigned to hosts.

Figure 11.4 shows the five attributes of one of the subnets from figure 11.3: the 192.168.1.64/26 subnet. The calculations are the same as we covered in chapter 7-just keep in mind that the borrowed bits are now part of the network portion. Because the prefix length in this example is /26, only the last 6 bits are the host portion.

> [!translation] 逐句繁體中文翻譯
> - Figure 11.4 顯示 Figure 11.3 中其中一個 subnet，也就是 192.168.1.64/26 subnet 的五個 attributes。
> - 計算方式與第 7 章相同；只要記得 borrowed bits 現在已經屬於 network portion。
> - 因為這個例子的 prefix length 是 /26，所以只有最後 6 bits 是 host portion。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-196_636_1353_946_223.jpg)
Figure 11.4 The five attributes of the 192.168.1.64/26 subnet. The first 26 bits are the network portion, and the final 6 bits are the host portion. The network address is 192.168.1.64 (host portion all Os). The broadcast address is 192.168.1.127 (host portion all 1s). The first usable address is 192.168.1.65 (network address + 1). The last usable address is 192.168.1.126 (broadcast address - 1). The maximum number of hosts in the subnet is $62\left(2^{6}-2\right)$.

A common subnetting problem is something like this: "PC1 has IP address 172.16.20.27/28. What is the network address of the subnet it belongs to?" To solve a question like this, you can follow these three steps:

> [!translation] 逐句繁體中文翻譯
> - 常見的 subnetting 問題可能像這樣：PC1 的 IP address 是 172.16.20.27/28，它所屬 subnet 的 network address 是什麼？要解這類問題，可以依照三個步驟。

1 Write the address in binary: 10101100.00010000.00010100.00011011
${ }_{2}$ Change the host portion to all 0s: 10101100.00010000.00010100.00010000
3 Convert back to dotted decimal: 172.16.20.16. That's the answer!

EXAM TIP You should be able to identify any of the five attributes of a particular subnet, not just the network address. Such problems could appear on the CCNA exam as standalone questions or as part of more complex questions.

> [!translation] 逐句繁體中文翻譯
> - EXAM TIP：你應該能找出特定 subnet 的五個 attributes 中任何一個，而不只是 network address。
> - 這類題目可能在 CCNA 考試中獨立出現，也可能作為更複雜題目的一部分。

## /24+ SUBNET MASKS

Ideally, we would be able to just write prefix lengths in CIDR notation, without having to worry about subnet masks. However, because you have to use subnet masks when configuring IP addresses and static routes in Cisco IOS, they are necessary to learn.

> [!translation] 逐句繁體中文翻譯
> - 理想情況下，我們可以只用 CIDR notation 寫 prefix length，而不用擔心 subnet masks。
> - 不過，因為在 Cisco IOS 中設定 IP addresses 與 static routes 時必須使用 subnet masks，所以仍然需要學會它們。

To review, a subnet mask is a series of 32 bits that indicates which bits in an IP address are part of the network portion and which are part of the host portion. A bit set to 1 in the subnet mask means that the bit in the same position of the IP address is part of the network portion, and a bit set to 0 in the subnet mask means that the bit in the same position of the IP address is part of the host portion. And because an IP address consists of the network portion followed by the host portion, a subnet mask is a series of 1s followed by a series of 0s (unless it is all 0s, as in /0, or all 1s, as in /32).

> [!translation] 逐句繁體中文翻譯
> - 複習一下，subnet mask 是一串 32 bits，用來指出 IP address 中哪些 bits 屬於 network portion，哪些 bits 屬於 host portion。
> - Subnet mask 中設為 1 的 bit，表示 IP address 同一位置的 bit 屬於 network portion；設為 0 的 bit，表示同一位置的 bit 屬於 host portion。
> - 因為 IP address 是 network portion 接著 host portion，所以 subnet mask 是一串 1 後面接一串 0，除非它是 /0 的全 0 或 /32 的全 1。

When only dealing with /8, /16, and /24 prefix lengths, subnet masks are simple: 255.0.0.0, 255.255.0.0, or 255.255.255.0, respectively. When using CIDR, however, the boundary between network and host portion can lie in the middle of an octet, which results in other possible subnet masks. Table 11.1 lists prefix lengths from /24 to /32 and their equivalent subnet masks written in binary and dotted decimal. You should familiarize yourself with these subnet masks; you'll need to know them when configuring Cisco routers. For reference, table 11.1 also lists the maximum number of hosts in a subnet of each size.

> [!translation] 逐句繁體中文翻譯
> - 只處理 /8、/16 與 /24 prefix lengths 時，subnet masks 很簡單：分別是 255.0.0.0、255.255.0.0 或 255.255.255.0。
> - 不過使用 CIDR 時，network 與 host portion 的邊界可以落在 octet 中間，因此會產生其他 subnet masks。
> - Table 11.1 列出 /24 到 /32 的 prefix lengths，以及它們用 binary 與 dotted decimal 表示的等效 subnet masks。
> - 你應該熟悉這些 subnet masks；設定 Cisco routers 時會需要知道它們。
> - Table 11.1 也列出每種大小 subnet 的 maximum number of hosts 供參考。

Table 11.1 /24+ prefix lengths and subnet masks
| Prefix length | Subnet mask (binary) | Subnet mask (decimal) | Maximum number of hosts (2¹-2) |
| :--- | :--- | :--- | :--- |
| /24 | 11111111.11111111.11111111.00000000 | 255.255.255.0 | 254 |
| /25 | 11111111.11111111.11111111.10000000 | 255.255.255.128 | 126 |
| /26 | 11111111.11111111.11111111.11000000 | 255.255.255.192 | 62 |
| /27 | 11111111.11111111.11111111.11100000 | 255.255.255.224 | 30 |
| /28 | 11111111.11111111.11111111.11110000 | 255.255.255.240 | 14 |
| /29 | 11111111.11111111.11111111.11111000 | 255.255.255.248 | 6 |
| /30 | 11111111.11111111.11111111.11111100 | 255.255.255.252 | 2 |
| /31 | 11111111.11111111.11111111.11111110 | 255.255.255.254 | 2 (see the following) |
| /32 | 11111111.11111111.11111111.11111111 | 255.255.255.255 | 1 (see the following) |

> [!translation] 逐句繁體中文翻譯
> - Table 11.1 列出 /24 以上 prefix lengths 與對應 subnet masks。
> - 表格顯示每個 prefix length 的 binary subnet mask、decimal subnet mask，以及最大 host 數。
> - /24 對應 255.255.255.0 且有 254 hosts；/25 對應 255.255.255.128 且有 126 hosts；/26 對應 255.255.255.192 且有 62 hosts；後續一路到 /32，host 數會隨 host bits 減少而下降。


Prefix lengths of /31 and /32 are special cases when it comes to calculating the maximum number of hosts in a subnet. A /31 prefix length, for example, leaves a single
host bit. If we use the formula $2^{\mathrm{y}}-2$ to calculate the maximum number of hosts in the subnet, the result is 0; a single host bit only allows for two addresses, and those are taken by the network and broadcast addresses, resulting in no usable addresses. For this reason, /31 prefix lengths were unused for a long time.

> [!translation] 逐句繁體中文翻譯
> - /31 與 /32 prefix lengths 在計算 subnet 的 maximum number of hosts 時是特殊案例。
> - 以 /31 為例，它只留下 1 個 host bit。
> - 如果用 $2^{\mathrm{y}}-2$ 公式計算最大 host 數，結果會是 0；1 個 host bit 只能形成兩個位址，而這兩個位址會被 network 與 broadcast addresses 佔用，因此沒有可用位址。
> - 因為這個原因，/31 prefix lengths 曾經很長一段時間沒有被使用。

However, for the purpose of further preserving the IPv4 address space, an exception to the normal rules was made for / 31 prefix lengths: they can be used for point-to-point links-connections between two routers, which only require two IP addresses. In this case, the subnet does not have a network address or broadcast address. Before this exception was made, point-to-point links used / 30 prefix lengths, leaving two host bits and therefore two usable addresses $\left(2^{2}-2=2\right)$. This works fine, and / 30 prefix lengths are still commonly used for point-to-point links today, but /31 prefix lengths are more efficient; they only consume two IP addresses, rather than four.

> [!translation] 逐句繁體中文翻譯
> - 不過，為了進一步保留 IPv4 address space，/31 prefix lengths 被允許作為正常規則的例外：它們可以用於 point-to-point links，也就是兩台 routers 之間的連線，而這種連線只需要兩個 IP addresses。
> - 在這種情況下，subnet 沒有 network address 或 broadcast address。
> - 在這個例外出現之前，point-to-point links 使用 /30 prefix lengths，留下 2 個 host bits，因此有兩個可用位址 $\left(2^{2}-2=2\right)$。
> - 這樣也沒問題，而且 /30 prefix lengths 至今仍常用於 point-to-point links，但 /31 prefix lengths 更有效率；它只消耗兩個 IP addresses，而不是四個。

NOTE Although /31 subnets are more efficient, /30 subnets are still the more common choice since /31 technically breaks the network/broadcast address rule.

> [!translation] 逐句繁體中文翻譯
> - NOTE：雖然 /31 subnets 更有效率，/30 subnets 仍然是較常見的選擇，因為 /31 技術上打破了 network/broadcast address 規則。

Figure 11.5 demonstrates a point-to-point link between two routers, providing two options for the subnet used for the connection: 203.0.113.0/30 can be used, which consumes a total of four addresses. Or 203.0.113.0/31 can be used, which consumes only two addresses.

> [!translation] 逐句繁體中文翻譯
> - Figure 11.5 示範兩台 routers 之間的 point-to-point link，並提供該連線 subnet 的兩個選項：可使用 203.0.113.0/30，總共消耗四個位址；也可使用 203.0.113.0/31，只消耗兩個位址。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-198_339_1414_1127_223.jpg)
Figure 11.5 A point-to-point link connecting R1 to R2. Traditionally, a /30 subnet would be used for a connection like this, as in option 1. In modern networks, a /31 subnet, as in option 2, is also valid (and more efficient).

In the following example, I configure R1 G0/0's IP address using a /31 prefix length (subnet mask 255.255.255.254). The router then displays a message warning that /31 prefix lengths should be used cautiously:

> [!translation] 逐句繁體中文翻譯
> - 在下面例子中，我用 /31 prefix length（subnet mask 255.255.255.254）設定 R1 G0/0 的 IP address。
> - 接著 router 會顯示一則警告，提醒 /31 prefix lengths 應謹慎使用。

```
    Configures a /31 prefix length
R1(config-if)# ip address 203.0.113.0 255.255.255.254
% Warning: use /31 mask on non point-to-point interface cautiously
```

NOTE A /32 subnet mask can be used to specify a single IP address in a route, as covered in chapter 9. However, a /32 subnet mask is rarely configured on an interface (although we will cover an exception in chapter 18, on OSPF).

> [!translation] 逐句繁體中文翻譯
> - NOTE：/32 subnet mask 可以用來在 route 中指定單一 IP address，如第 9 章所述。
> - 不過 /32 subnet mask 很少設定在 interface 上，雖然我們會在第 18 章 OSPF 中介紹一個例外。

### 11.2.2 Subnetting /16 address blocks

Subnetting an address block with a prefix length shorter than /24 can seem intimidating at first; you can no longer focus only on the final octet of the address. However, let me assure you that the process of subnetting does not change at all:

> [!translation] 逐句繁體中文翻譯
> - Subnetting prefix length 短於 /24 的 address block 一開始可能看起來嚇人；你不能再只專注於位址最後一個 octet。
> - 不過請放心，subnetting 的流程完全沒有改變。

- The number of subnets you can make is still $2^{\mathrm{x}}$, where $x$ is the number of borrowed bits.
- The maximum number of hosts per subnet is still $2^{y}-2$, where $y$ is the number of host bits.
- The subnet's network address is still the address with a host portion of all 0s.

I think you get the idea! The only difference is that converting between decimal and binary takes a little more care. Because there's no need to introduce any new concepts, in this section, I'll demonstrate how the previous concepts apply to address blocks with a /16+ prefix length. Table 11.2 summarizes some ways a /16 address block can be subnetted. As before, each borrowed bit doubles the amount of subnets that can be made but halves the total number of addresses per subnet (table 11.2 displays the maximum number of hosts per subnet, rather than the total number of addresses).

> [!translation] 逐句繁體中文翻譯
> - 我想你已經抓到概念了！唯一的差別是 decimal 與 binary 之間的轉換需要更小心一些。
> - 因為不需要引入任何新概念，本節會示範前面概念如何套用到 /16 以上的 address blocks。
> - Table 11.2 摘要列出 /16 address block 的幾種 subnetting 方式。
> - 和前面一樣，每借 1 bit，可建立的 subnets 數量會加倍，但每個 subnet 的總位址數會減半；Table 11.2 顯示的是每個 subnet 的最大 host 數，而不是總位址數。

Table 11.2 Subnetting a /16 address block
| Prefix length | Subnet mask (decimal) | Borrowed bits | Number of subnets | Maximum number of hosts per subnet (2¹-2) |
| :--- | :--- | :--- | :--- | :--- |
| /16 | 255.255.0.0 | 0 | 1 | 65,534 |
| /17 | 255.255.128.0 | 1 | 2 | 32,766 |
| /18 | 255.255.192.0 | 2 | 4 | 16,382 |
| /19 | 255.255.224.0 | 3 | 8 | 8190 |
| /20 | 255.255.240.0 | 4 | 16 | 4094 |
| /21 | 255.255.248.0 | 5 | 32 | 2046 |
| /22 | 255.255.252.0 | 6 | 64 | 1022 |
| /23 | 255.255.254.0 | 7 | 128 | 510 |
| /24 | 255.255.255.0 | 8 | 256 | 254 |
| /25 | 255.255.255.128 | 9 | 512 | 126 |

> [!translation] 逐句繁體中文翻譯
> - Table 11.2 顯示 /16 address block 的 subnetting。
> - 表格列出 prefix length、decimal subnet mask、borrowed bits、subnet 數量，以及每個 subnet 的最大 hosts。
> - 從 /16 的 1 個 subnet、65,534 hosts 開始，每增加 1 個 borrowed bit，subnet 數加倍而 host 容量下降，例如 /24 可產生 256 個 subnets，每個 254 hosts。


NOTE Table 11.2 only shows prefix lengths up to /25, but / 16 address blocks can be subnetted up to /32 as well.

> [!translation] 逐句繁體中文翻譯
> - NOTE：Table 11.2 只顯示到 /25 prefix lengths，但 /16 address blocks 也可以一路 subnet 到 /32。

In figure 11.3, we saw how borrowing two bits from the 192.168.1.0/24 address block allows for four subnets to be made. Figure 11.6 demonstrates the same, but this time using the 192.168.0.0/16 address block.

> [!translation] 逐句繁體中文翻譯
> - 在 Figure 11.3 中，我們看到從 192.168.1.0/24 address block 借 2 bits 可建立四個 subnets。
> - Figure 11.6 示範相同概念，但這次使用 192.168.0.0/16 address block。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-200_575_1298_350_225.jpg)
Figure 11.6 Borrowing two bits from 192.168.0.0/16 allows for four subnets: 192.168.0.0/18, 192.168.64.0/18, 192.168.128.0/18, and 192.168.192.0/18.

Calculating the five attributes is the same process as before. Figure 11.7 takes one of the subnets from figure 11.6 (192.168.128.0/18) and shows the five attributes of that subnet.

> [!translation] 逐句繁體中文翻譯
> - 計算五個 attributes 的流程與之前相同。
> - Figure 11.7 取 Figure 11.6 中其中一個 subnet，也就是 192.168.128.0/18，並顯示該 subnet 的五個 attributes。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-200_666_1412_1248_225.jpg)
Figure 11.7 The five attributes of the 192.168.128.0/18 subnet. The first 18 bits are the network portion, and the final 14 bits are the host portion. The network address is 192.168.128.0 (host portion all 0s). The broadcast address is 192.168.191.255 (host portion all 1s). The first usable address is 192.168.128.1 (network address + 1). The last usable address is 192.168.191.254 (broadcast address - 1). The maximum number of hosts in the subnet is $\mathbf{1 6 , 3 8 2}\left(\mathbf{2}^{\mathbf{1 4}}-\mathbf{2}\right)$.

NOTE /18 is a very large subnet size, containing 16,382 host addresses per subnet. You will probably never configure a /18 subnet on an interface, but for the CCNA exam, you should be able to create subnets of any size.

> [!translation] 逐句繁體中文翻譯
> - NOTE：/18 是非常大的 subnet size，每個 subnet 包含 16,382 個 host addresses。
> - 你大概永遠不會真的在 interface 上設定 /18 subnet，但為了 CCNA 考試，你應該能建立任何大小的 subnets。

### 11.2.3 Subnetting /8 address blocks

Subnetting a /8 address block is, once again, the same process we have seen up to this point. However, a /8 address block means there are 24 host bits-lots of bits to either borrow and make lots of subnets, or use to make a few very large subnets. Table 11.3 summarizes some ways that a /8 address block can be subnetted.

> [!translation] 逐句繁體中文翻譯
> - Subnetting /8 address block 再一次是我們目前看過的相同流程。
> - 不過 /8 address block 表示有 24 個 host bits，也就是有很多 bits 可以借來建立大量 subnets，或用來建立少數非常大的 subnets。
> - Table 11.3 摘要列出 /8 address block 的幾種 subnetting 方式。

Table 11.3 Subnetting a /8 address block
| Prefix length | Subnet mask (decimal) | Borrowed bits | Number of subnets | Maximum number of hosts per subnet (2¹-2) |
| :--- | :--- | :--- | :--- | :--- |
| /8 | 255.0.0.0 | 0 | 1 | 16,777,214 |
| /9 | 255.128.0.0.0 | 1 | 2 | 8,388,606 |
| /10 | 255.192.0.0 | 2 | 4 | 4,194,302 |
| /11 | 255.224.0.0 | 3 | 8 | 2,097,150 |
| /12 | 255.240.0.0 | 4 | 16 | 1,048,574 |
| /13 | 255.248.0.0 | 5 | 32 | 524,286 |
| /14 | 255.252.0.0 | 6 | 64 | 262,142 |
| /15 | 255.254.0.0 | 7 | 128 | 131,070 |
| /16 | 255.255.0.0 | 8 | 256 | 65,534 |
| /17 | 255.255.128.0 | 9 | 512 | 32,766 |

> [!translation] 逐句繁體中文翻譯
> - Table 11.3 顯示 /8 address block 的 subnetting。
> - 表格列出從 /8 到 /17 的 prefix lengths、decimal subnet masks、borrowed bits、subnet 數量與每 subnet 最大 hosts。
> - 隨著 prefix length 增加，borrowed bits 與 subnet 數量增加，而每個 subnet 的 host 容量降低。


NOTE Because of the number of host bits in a /8 address block, the number of hosts per subnet for each prefix length listed in table 11.3 is extremely large. When actually subnetting a /8 block, you will probably borrow many bits to make smaller subnets; a subnet with millions of addresses is never necessary.

> [!translation] 逐句繁體中文翻譯
> - NOTE：由於 /8 address block 的 host bits 很多，Table 11.3 所列各 prefix length 的每 subnet hosts 數都非常大。
> - 實際 subnetting /8 block 時，你通常會借很多 bits 來建立較小 subnets；擁有數百萬位址的 subnet 永遠沒有必要。

In figure 11.8, I borrow 12 bits from the 10.0.0.0/8 address block, which allows for 4,096 separate subnets to be made. Of course, I'm not going to write out all 4,096 subnets, so figure 11.8 shows only the first subnet and the final two subnets.

> [!translation] 逐句繁體中文翻譯
> - 在 Figure 11.8 中，我從 10.0.0.0/8 address block 借 12 bits，這可建立 4,096 個獨立 subnets。
> - 當然，我不會把全部 4,096 個 subnets 都寫出來，所以 Figure 11.8 只顯示第一個 subnet 與最後兩個 subnets。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-202_502_1300_183_348.jpg)
Figure 11.8 Borrowing 12 bits from 10.0.0.0/8 allows for 4,096 subnets. 10.0.0.0/20 is the first subnet, and 10.255.224.0/20 and 10.255.240.0/20 are the final two.

NOTE Figure 11.8 differs from the previous examples in that the borrowed bits cross between octets (all of the second octet and the first four bits of the third octet). This doesn't change anything about how subnetting works! Keep in mind that the octet divisions only exist to make addresses more human-readable; to a computer, an IP address is just a series of 32 bits-no octet divisions.

> [!translation] 逐句繁體中文翻譯
> - NOTE：Figure 11.8 與前面例子的不同點在於，borrowed bits 跨越了 octets，也就是整個第二個 octet 加上第三個 octet 的前四個 bits。
> - 這完全不會改變 subnetting 的運作方式！請記住，octet 分隔只是為了讓人類更容易閱讀；對電腦來說，IP address 只是一串 32 bits，沒有 octet 分隔。

Calculating the five attributes of a subnet doesn't change, regardless of the size of the original address block or how many bits you borrow, so we won't go through a third example here. For some additional practice, I recommend taking one of the subnets shown in figure 11.8 and calculating the five attributes: network address, broadcast address, first usable address, last usable address, and maximum number of hosts.

> [!translation] 逐句繁體中文翻譯
> - 不論原始 address block 大小如何，或你借了多少 bits，計算 subnet 五個 attributes 的方式都不會改變，所以這裡不再做第三個範例。
> - 若想額外練習，我建議你挑 Figure 11.8 中的一個 subnet，計算它的五個 attributes：network address、broadcast address、first usable address、last usable address，以及 maximum number of hosts。

### 11.2.4 FLSM scenarios

As mentioned at the beginning of this chapter, subnetting is a skill that takes practice to become proficient. At the end of this chapter, I will give some recommendations for free websites where you can find subnetting practice questions. Before that, let's go through a couple of scenarios that resemble what you might find on those websites (and on the CCNA exam itself).

> [!translation] 逐句繁體中文翻譯
> - 如本章開頭所說，subnetting 是需要練習才會熟練的技能。
> - 在本章最後，我會推薦一些免費網站，讓你找 subnetting practice questions。
> - 在那之前，我們先走過幾個類似那些網站與 CCNA 考試可能出現的情境。

Figure 11.9 provides a practice scenario in which we will use FLSM to divide the 172.25.190.0/23 address block into four subnets of equal size, calculate the maximum number of hosts in each subnet, and configure the first usable address of each subnet on R1's interfaces.

> [!translation] 逐句繁體中文翻譯
> - Figure 11.9 提供一個練習情境：我們會用 FLSM 把 172.25.190.0/23 address block 切成四個大小相同的 subnets，計算每個 subnet 的 maximum number of hosts，並把每個 subnet 的 first usable address 設定到 R1 interfaces 上。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-203_492_945_181_190.jpg)
1. Subnet the 172.25.190.0/23 address block into four subnets, and identify each subnet.
2. How many host addresses are available in each subnet?
3. Configure the first usable address of each subnet on R1's interfaces.

NOTE The example in figure 11.9 is the first time we are beginning with an address block that is not /8, /16, or /24, but the subnetting process remains the same.

> [!translation] 逐句繁體中文翻譯
> - NOTE：Figure 11.9 的例子是我們第一次從不是 /8、/16 或 /24 的 address block 開始，但 subnetting 流程仍然相同。

To begin, let's identify the four subnets. How many bits do we need to borrow to divide an address block into four subnets? As we've seen in a couple of previous examples, we need to borrow 2 bits to make four subnets because $2^{2}=4$. Then, we can just count up with those 2 borrowed bits to create four subnets. Figure 11.10 shows the four subnets that can be created by borrowing 2 bits from the 172.25.190.0/23 address block. Because we borrow 2 bits from the host portion, each subnet's prefix length is / $25(/ 23+2)$.

> [!translation] 逐句繁體中文翻譯
> - 首先，我們找出四個 subnets。
> - 要把一個 address block 分成四個 subnets，需要借多少 bits？如前面幾個例子所見，我們需要借 2 bits，因為 $2^{2}=4$。
> - 接著只要用這 2 個 borrowed bits 往上數，就能建立四個 subnets。
> - Figure 11.10 顯示從 172.25.190.0/23 address block 借 2 bits 可建立的四個 subnets。
> - 因為我們從 host portion 借 2 bits，所以每個 subnet 的 prefix length 是 /$25(/23+2)$。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-203_520_1167_1330_316.jpg)
Figure 11.10 The subnets that can be created by subnetting 172.25.190.0/23 into four equal parts: 172.25.190.0/25, 172.25.190.128/25, 172.25.191.0/25, and 172.25.191.128/25. Borrowing 2 bits from the host portion of the /23 address block results in four /25 subnets.

We have now solved the first part of the scenario: identifying the four subnets. The second part asks how many host addresses are available in each subnet. To solve this,
just use the same formula as always: $2^{\mathrm{y}}-2$ ( $y$ being the number of host bits). The original address block was /23, meaning there were 9 host bits. However, after borrowing 2 host bits to make subnets, 7 host bits remain. Therefore, there are $126\left(2^{7}-2\right)$ host addresses available in each subnet.

> [!translation] 逐句繁體中文翻譯
> - 現在我們已經解決情境的第一部分：找出四個 subnets。
> - 第二部分問每個 subnet 有多少 host addresses 可用。
> - 要解這題，只要使用一貫公式 $2^{\mathrm{y}}-2$，其中 y 是 host bits 的數量。
> - 原始 address block 是 /23，代表有 9 個 host bits；但借 2 個 host bits 建立 subnets 後，還剩 7 個 host bits，因此每個 subnet 有 $2^7-2=126$ 個可用 host addresses。

The final part of the scenario says to configure the first usable address of each subnet on R1's interfaces. The first usable address can be calculated with the usual method: add 1 to the network address of each subnet. In the following example, I configure R1's interfaces with the first usable address of each subnet:
![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-204_396_1408_588_221.jpg)

> [!translation] 逐句繁體中文翻譯
> - 情境的最後一部分要求把每個 subnet 的 first usable address 設定到 R1 interfaces。
> - First usable address 可用一般方法計算：把每個 subnet 的 network address 加 1。
> - 下面例子中，我把每個 subnet 的 first usable address 設定到 R1 的 interfaces。

We have now solved the scenario! Let's walk through one more scenario, this time text only: you have been given the 10.224.0.0/11 address block. You must create 2,000 subnets, which will be assigned to various offices and departments within a large company. What prefix length must you use to create a sufficient number of subnets? How many host addresses are in each subnet?

> [!translation] 逐句繁體中文翻譯
> - 現在我們已經解完這個情境！再走一個純文字情境：你拿到 10.224.0.0/11 address block。
> - 你必須建立 2,000 個 subnets，分配給大型公司中的各辦公室與部門。
> - 你必須使用什麼 prefix length 才能建立足夠的 subnets？每個 subnet 中有多少 host addresses？。

To solve this scenario, we first have to determine how many bits we must borrow to create 2,000 subnets. If you have learned your powers of 2, this shouldn't be too difficult: 1 bit gives 2 subnets, 2 bits gives 4 subnets, 3 bits gives 8 subnets, and so on, and then 11 bits gives 2,048 subnets. That's 48 more than we need in the scenario, but borrowing 10 bits would give only 1,024 subnets (not enough), so 11 bits is our answer. Borrowing 11 bits from the host portion results in a /22 prefix length, which is the answer to the first part of the scenario. Figure 11.11 demonstrates this and shows the first subnet and the last (2,048th) subnet.

> [!translation] 逐句繁體中文翻譯
> - 要解這個情境，首先要判斷為了建立 2,000 個 subnets 必須借多少 bits。
> - 如果你熟悉 2 的次方，這應該不難：1 bit 給 2 個 subnets，2 bits 給 4 個 subnets，3 bits 給 8 個 subnets，以此類推，11 bits 給 2,048 個 subnets。
> - 這比需求多 48 個，但借 10 bits 只會得到 1,024 個 subnets，不足夠，所以答案是 11 bits。
> - 從 host portion 借 11 bits 會得到 /22 prefix length，這就是情境第一部分的答案。
> - Figure 11.11 示範這件事，並顯示第一個 subnet 與最後一個，也就是第 2,048 個 subnet。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-204_316_1155_1656_350.jpg)
Figure 11.11 Borrowing 11 bits from the host portion of the 10.224.0.0/11 address block allows for $\mathbf{2 , 0 4 8}$ subnets. The first subnet is 10.224.0.0/22 (all borrowed bits set to 0), and the last (2048th) subnet is 10.255.252.0/22 (all borrowed bits set to 1).

A / 22 prefix length means that 10 host bits remain, so now we can calculate the number of host addresses in each subnet: $1022\left(2^{10}-2\right)$. And now we have solved the scenario!

> [!translation] 逐句繁體中文翻譯
> - /22 prefix length 表示還剩 10 個 host bits，因此現在可以計算每個 subnet 的 host addresses 數量：$1022\left(2^{10}-2\right)$。
> - 現在我們也解完這個情境了！。

NOTE Because the number of subnets increases by a power of 2 for each borrowed bit, you often won't be able to create exactly the number of subnets needed; you'll probably end up with some extra subnets, which is not a bad thing-they can be used to accommodate network expansions in the future. Likewise, you often won't be able to create subnets of exactly the size you need; you'll usually have some extra addresses in each subnet.

> [!translation] 逐句繁體中文翻譯
> - NOTE：因為每借 1 bit，subnet 數量會以 2 的次方增加，所以你通常無法剛好建立所需數量的 subnets；最後可能會多出一些 subnets，這不是壞事，未來可用於網路擴充。
> - 同樣地，你也常常無法建立大小剛好符合需求的 subnets；每個 subnet 通常會有一些多餘位址。

### 11.3 VLSM subnetting

Variable-Length Subnet Masking (VLSM) allows us to subnet an address block even more efficiently than FLSM by creating subnets of varying sizes. Although FLSM is a helpful introduction to subnetting, when actually subnetting networks in the real world, chances are you'll be doing VLSM. Figure 11.12 shows the scenario I will use to demonstrate VLSM.

> [!translation] 逐句繁體中文翻譯
> - Variable-Length Subnet Masking（VLSM）讓我們透過建立不同大小的 subnets，比 FLSM 更有效率地 subnet address block。
> - 雖然 FLSM 是學習 subnetting 的有用入門，但在真實世界做 network subnetting 時，你很可能會使用 VLSM。
> - Figure 11.12 顯示我用來示範 VLSM 的情境。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-205_540_1416_948_190.jpg)
Figure 11.12 A VLSM scenario in which you must assign subnets from the 10.89.100.0/24 address block to each LAN and the WAN connection and identify the five attributes of each subnet

A /24 address block includes 254 host addresses, and the total number of host addresses required in figure 11.12's scenario is 226, so the address space is sufficient. Using FLSM to create subnets of equal size would result in some subnets having too few addresses (Toronto LAN A requires 122 host addresses) and some subnets having too many addresses (the WAN connection only requires 2 host addresses, for R1 and R2). However, if we use VLSM, we can make some subnets smaller and some larger, allowing us to efficiently use the available address block. The high-level VLSM process is as follows:

> [!translation] 逐句繁體中文翻譯
> - /24 address block 包含 254 個 host addresses，而 Figure 11.12 情境中需要的 host addresses 總數是 226，所以 address space 足夠。
> - 若用 FLSM 建立相同大小的 subnets，有些 subnets 位址太少，例如 Toronto LAN A 需要 122 個 host addresses；有些 subnets 位址太多，例如 WAN connection 只需要 2 個 host addresses 給 R1 與 R2。
> - 不過若使用 VLSM，就能讓某些 subnets 較小、某些較大，藉此有效率使用可用 address block。
> - VLSM 的高階流程如下。

1 Assign the largest subnet at the start of the address block.

2 Assign the second-largest subnet after it.
3 Repeat the process until all subnets have been assigned.

The five subnets in figure 11.12, in order from largest to smallest, are Toronto LAN A (122 hosts), Tokyo LAN A (59 hosts), Toronto LAN B (30 hosts), Tokyo LAN B (11 hosts), and the WAN connection (2 hosts)-so let's start by assigning Toronto LAN A.

> [!translation] 逐句繁體中文翻譯
> - Figure 11.12 中的五個 subnets，依大小由大到小排列，是 Toronto LAN A（122 hosts）、Tokyo LAN A（59 hosts）、Toronto LAN B（30 hosts）、Tokyo LAN B（11 hosts），以及 WAN connection（2 hosts）；所以我們先從 Toronto LAN A 開始分配。

NOTE Router IP addresses are included in the "host" counts; any device with an IP address can be considered a host. The term end host is usually used to refer to PCs, servers, etc. to distinguish them from network infrastructure devices like routers.

> [!translation] 逐句繁體中文翻譯
> - NOTE：Router IP addresses 也包含在 host 數量中；任何有 IP address 的設備都可被視為 host。
> - End host 這個詞通常用來指 PCs、servers 等，以區分 routers 這類 network infrastructure devices。

Figure 11.13 visually represents the five subnets that will result from this process: one /25 subnet (128 addresses), one /26 subnet (64 addresses), one /27 subnet (32 addresses), one /28 subnet (16 addresses), and one /30 subnet (4 addresses). In the following sections, we'll walk through how to perform VLSM subnetting and create these five subnets.

> [!translation] 逐句繁體中文翻譯
> - Figure 11.13 視覺化呈現這個流程會得到的五個 subnets：一個 /25 subnet（128 addresses）、一個 /26 subnet（64 addresses）、一個 /27 subnet（32 addresses）、一個 /28 subnet（16 addresses），以及一個 /30 subnet（4 addresses）。
> - 接下來幾節，我們會走過如何執行 VLSM subnetting 並建立這五個 subnets。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-206_955_1018_936_348.jpg)
Figure 11.13 The 10.89.100.0/24 address block, divided into five subnets of varying sizes. (1) Toronto LAN A is 10.89.100.0/25, containing 128 addresses. (2) Tokyo LAN A is 10.89.100.128/26, containing 64 addresses. (3) Toronto LAN B is 10.89.100.192/27, containing 32 addresses. (4) Tokyo LAN B is 10.89.100.224/28, containing 16 addresses. (5) The WAN connection is 10.89.100.240/30, containing 4 addresses. After subnetting, 12 addresses remain unused: 10.89.100.244 through 10.89.100.255.

NOTE For practical reasons, figure 11.13 doesn't show the number of addresses in /30 subnets (4 addresses each) and doesn't include columns for /31 subnets (2 addresses each) and /32 subnets (1 address each).

> [!translation] 逐句繁體中文翻譯
> - NOTE：基於實務呈現原因，Figure 11.13 沒有顯示 /30 subnets 的位址數（每個 4 addresses），也沒有包含 /31 subnets（每個 2 addresses）與 /32 subnets（每個 1 address）的欄位。

### 11.3.1 Assigning Toronto LAN A's subnet

Toronto LAN A requires 122 host addresses, so the question is, what is the minimum number of host bits required to provide at least 122 host addresses? Referring to the formula we use to calculate the maximum number of host bits in a subnet $\left(2^{\mathrm{y}}-2\right)$, what is the minimum $y$ value that would serve our purpose? The answer is 7 because $2^{7}-2=126$, only a few more addresses than we need. Six host bits would give us only 62 host addresses ( $2^{6}-2$ )-not enough for the 122 hosts in Toronto LAN A. To leave 7 host bits, we have to borrow 1 bit from the /24 address block. Figure 11.14 shows the resulting subnet when we borrow a single host bit from the 10.89.100.0/24 address block: 10.89.100.0/25-Toronto LAN A's subnet! Figure 11.14 also lists the five attributes of the subnet.

> [!translation] 逐句繁體中文翻譯
> - Toronto LAN A 需要 122 個 host addresses，所以問題是：最少需要多少 host bits 才能提供至少 122 個 host addresses？參考用來計算 subnet 最大 host bits 的公式 $\left(2^{\mathrm{y}}-2\right)$，最小的 y 值是多少？答案是 7，因為 $2^{7}-2=126$，只比需求多一點點位址。
> - 6 個 host bits 只會給 62 個 host addresses（$2^{6}-2$），不足以支援 Toronto LAN A 的 122 hosts。
> - 為了留下 7 個 host bits，我們必須從 /24 address block 借 1 bit。
> - Figure 11.14 顯示從 10.89.100.0/24 address block 借 1 個 host bit 後得到的 subnet：10.89.100.0/25，也就是 Toronto LAN A 的 subnet。
> - Figure 11.14 也列出該 subnet 的五個 attributes。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-207_449_1163_917_316.jpg)
Figure 11.14 Toronto LAN A's subnet. Borrowing 1 bit from the 10.89.100.0/24 address block results in the $\mathbf{1 0 . 8 9 . 1 0 0 . 0} / \mathbf{2 5}$ subnet, supporting up to $\mathbf{1 2 6}$ host addresses-enough for the $\mathbf{1 2 2}$ hosts in the LAN.

By assigning the 10.89.100.0/25 subnet to Toronto LAN A, we have already used half of the available address space: from 10.89.100.0 through 10.89.100.127. The entire 10.89.100.0/24 address block includes 256 addresses, and a single /25 subnet takes up 128 of those addresses (keep in mind that only 126 of those 128 addresses can be assigned to hosts). We will assign the rest of the subnets from the remaining range of addresses: 10.89.100.128 through 10.89.100.255.

> [!translation] 逐句繁體中文翻譯
> - 把 10.89.100.0/25 subnet 指派給 Toronto LAN A 後，我們已經使用了可用 address space 的一半：10.89.100.0 到 10.89.100.127。
> - 整個 10.89.100.0/24 address block 包含 256 個 addresses，而單一 /25 subnet 佔用其中 128 個；請記得這 128 個 addresses 中只有 126 個可以指派給 hosts。
> - 剩下的 subnets 會從剩餘位址範圍 10.89.100.128 到 10.89.100.255 中分配。

### 11.3.2 Assigning Tokyo LAN A's subnet

After assigning Toronto LAN A's subnet and identifying its five attributes, we can easily identify one more piece of information: Tokyo LAN A's network address. The last address (not the last usable address) in Toronto LAN A is 10.89.100.127-the broadcast address. Without knowing any other information about Tokyo LAN A, we can identify
its first IP address (the first address immediately after Toronto LAN A's broadcast address), which is 10.89.100.128. This is Tokyo LAN A's network address because the first IP address of a subnet is the network address.

> [!translation] 逐句繁體中文翻譯
> - 指派 Toronto LAN A 的 subnet 並找出它的五個 attributes 後，我們可以輕鬆找出另一項資訊：Tokyo LAN A 的 network address。
> - Toronto LAN A 中最後一個 address（不是 last usable address）是 10.89.100.127，也就是 broadcast address。
> - 不需要知道 Tokyo LAN A 的其他資訊，我們就能找出它的第一個 IP address，也就是緊接在 Toronto LAN A broadcast address 之後的第一個 address：10.89.100.128。
> - 這就是 Tokyo LAN A 的 network address，因為 subnet 的第一個 IP address 就是 network address。

Now that we know Tokyo LAN A's network address we just need to figure out how many host bits are needed (which determines the prefix length), and then we can calculate the other attributes. Tokyo LAN A requires enough addresses for 59 hosts, so 6 host bits are required, giving $62\left(2^{6}-2\right)$ usable addresses-a /26 prefix length. Therefore, the subnet we should assign to Tokyo LAN A is 10.89.100.128/26. Figure 11.15 shows the subnet and its five attributes.

> [!translation] 逐句繁體中文翻譯
> - 現在知道 Tokyo LAN A 的 network address 後，只需要算出需要多少 host bits，也就是決定 prefix length，然後就能計算其他 attributes。
> - Tokyo LAN A 需要足夠支援 59 hosts 的 addresses，所以需要 6 個 host bits，可提供 $62\left(2^{6}-2\right)$ 個 usable addresses，也就是 /26 prefix length。
> - 因此應指派給 Tokyo LAN A 的 subnet 是 10.89.100.128/26。
> - Figure 11.15 顯示這個 subnet 與它的五個 attributes。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-208_304_1085_620_350.jpg)
Figure 11.15 Tokyo LAN A's subnet. The network address is 10.89.100.128-the first address after Toronto LAN A. A /26 prefix length is used to allow for up to 62 host addresses-enough for the 59 hosts in the LAN.

We have now assigned three-quarters of the 10.89.100.0/24 address block: a /25 subnet (one-half) and a /26 subnet (one-quarter). The remaining range of addresses is 10.89.100.192 through 10.89.100.255, and we will assign the remaining three subnets from that range.

> [!translation] 逐句繁體中文翻譯
> - 現在我們已經分配了 10.89.100.0/24 address block 的四分之三：一個 /25 subnet，也就是一半；以及一個 /26 subnet，也就是四分之一。
> - 剩餘位址範圍是 10.89.100.192 到 10.89.100.255，我們會從這個範圍分配剩下三個 subnets。

### 11.3.3 Assigning Toronto LAN B's subnet

The IP address immediately after Tokyo LAN A's final address (its broadcast address) is 10.89.100.192, and that is Toronto LAN B's network address. What about its prefix length? Toronto LAN B requires IP addresses for at least 30 hosts, meaning 5 host bits are required, which gives exactly $30\left(2^{5}-2\right)$ host addresses. Therefore, Toronto LAN B's subnet should use a /27 prefix length, resulting in subnet 10.89.100.192/27. Figure 11.16 shows the subnet and its five attributes.

> [!translation] 逐句繁體中文翻譯
> - Tokyo LAN A 最後一個 address，也就是 broadcast address，後面的下一個 IP address 是 10.89.100.192，這就是 Toronto LAN B 的 network address。
> - 那它的 prefix length 呢？Toronto LAN B 至少需要 30 hosts 的 IP addresses，代表需要 5 個 host bits，剛好提供 $30\left(2^{5}-2\right)$ 個 host addresses。
> - 因此 Toronto LAN B 的 subnet 應使用 /27 prefix length，形成 10.89.100.192/27 subnet。
> - Figure 11.16 顯示該 subnet 與它的五個 attributes。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-208_304_1087_1694_352.jpg)
Figure 11.16 Toronto LAN B's subnet. The network address is 10.89.100.192-the first address after Tokyo LAN A. A /27 prefix length is used to allow for up to 30 host addresses-exactly the amount needed.

NOTE In a real-world situation, you should leave a bit of room in each subnet to allow for future growth; you may need to add more hosts to the subnet at some point. However, when doing subnetting scenarios like this (and on the CCNA exam), just use the most efficient prefix length, leaving as few unused addresses in the subnet as possible.

> [!translation] 逐句繁體中文翻譯
> - NOTE：在真實世界中，你應該在每個 subnet 留一點成長空間，因為未來可能需要增加更多 hosts。
> - 不過在這類 subnetting 情境或 CCNA 考試中，只要使用最有效率的 prefix length，盡可能讓 subnet 中未使用的 addresses 最少。

We have now assigned seven-eighths of the 10.89.100.0/24 address block: a /25 subnet (one-half), a /26 subnet (one-quarter), and a /27 subnet (one-eighth). The remaining range of addresses is 10.89.100.224 through 10.89.100.255, and we will use that range to assign the remaining subnets: Tokyo LAN B and the WAN connection between R1 and R2.

> [!translation] 逐句繁體中文翻譯
> - 現在我們已經分配了 10.89.100.0/24 address block 的八分之七：一個 /25 subnet（一半）、一個 /26 subnet（四分之一），以及一個 /27 subnet（八分之一）。
> - 剩餘位址範圍是 10.89.100.224 到 10.89.100.255，我們會用這段範圍分配剩下的 subnets：Tokyo LAN B 以及 R1 與 R2 之間的 WAN connection。

### 11.3.4 Assigning Tokyo LAN B's subnet

We can use the same process to assign Tokyo LAN B's subnet. Its network address is the first address after the previous LAN's (Toronto LAN B's) broadcast address, so Tokyo LAN B's network address is 10.89.100.224. Tokyo LAN B is a small LAN, requiring only 11 host addresses. Therefore, only 4 host bits are required, allowing for up to \$14\left(2^{4}-\right.\$2) host addresses, so Tokyo LAN B's subnet is 10.89.100.224/28. Figure 11.17 shows the subnet and its five attributes.

> [!translation] 逐句繁體中文翻譯
> - 我們可以用同樣流程分配 Tokyo LAN B 的 subnet。
> - 它的 network address 是前一個 LAN，也就是 Toronto LAN B 的 broadcast address 後面的第一個 address，所以 Tokyo LAN B 的 network address 是 10.89.100.224。
> - Tokyo LAN B 是小型 LAN，只需要 11 個 host addresses。
> - 因此只需要 4 個 host bits，可提供最多 $14\left(2^{4}-2\right)$ 個 host addresses，所以 Tokyo LAN B 的 subnet 是 10.89.100.224/28。
> - Figure 11.17 顯示該 subnet 與它的五個 attributes。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-209_304_1087_1054_320.jpg)
Figure 11.17 Tokyo LAN B's subnet. The network address is 10.89.100.224-the first address after Toronto LAN B. A /28 prefix length is used to allow for up to 14 host addresses-sufficient for the 11 hosts in the LAN.

Only one-sixteenth of the 10.89.100.0/24 address block remains-addresses 10.89.100.240 through 10.89.100.255. Fortunately, that is more than enough addresses for the final subnet: the WAN connection between R1 and R2.

> [!translation] 逐句繁體中文翻譯
> - 10.89.100.0/24 address block 只剩下十六分之一，也就是 10.89.100.240 到 10.89.100.255。
> - 幸好，這已經足夠最後一個 subnet：R1 與 R2 之間的 WAN connection。

### 11.3.5 Assigning the WAN connection's subnet

The WAN connection between R1 and R2 is a point-to-point connection, requiring only two host addresses. The network address is 10.89.100.240 (the first address after Tokyo LAN B's broadcast address), but what should the prefix length be? As we covered in section 11.2.1, there are two options: a /30 prefix length (two usable addresses, four addresses in total) or a /31 prefix length (two usable addresses, without network and broadcast addresses). Either prefix length is a valid choice, but for this example, I'll use a /30 prefix length, so the WAN connection's subnet is 10.89.100.240/30. Figure 11.18 shows the subnet and its five attributes.

> [!translation] 逐句繁體中文翻譯
> - R1 與 R2 之間的 WAN connection 是 point-to-point connection，只需要兩個 host addresses。
> - Network address 是 10.89.100.240，也就是 Tokyo LAN B broadcast address 後面的第一個 address，但 prefix length 應該是多少？如 11.2.1 節所說，有兩個選項：/30 prefix length，提供兩個 usable addresses、總共四個 addresses；或 /31 prefix length，提供兩個 usable addresses，且沒有 network 與 broadcast addresses。
> - 兩種 prefix length 都有效，但在此例中我會使用 /30 prefix length，所以 WAN connection 的 subnet 是 10.89.100.240/30。
> - Figure 11.18 顯示該 subnet 與它的五個 attributes。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-210_304_1088_185_350.jpg)
Figure 11.18 The WAN connection's subnet. The network address is 10.89.100.240-the first address after Tokyo LAN B. A /30 prefix length is used to allow for two host addresses-one for R1, and one for R2.

NOTE When I visually represented these five subnets in figure 11.13, I selected a /30 prefix length for the WAN connection's subnet. The main reason for that choice was a practical one; the boxes to represent /31 subnets in the diagram would be too small. For consistency's sake, I used a /30 prefix length here, but keep in mind that a /31 prefix length is valid too and is actually superior in that it consumes fewer addresses.

> [!translation] 逐句繁體中文翻譯
> - NOTE：我在 Figure 11.13 視覺化呈現這五個 subnets 時，為 WAN connection 選了 /30 prefix length。
> - 主要原因是實務呈現：圖中代表 /31 subnets 的方塊會太小。
> - 為了一致性，這裡也使用 /30 prefix length，但請記得 /31 prefix length 也有效，而且因為消耗較少 addresses，實際上更優。

And now we are done! We have assigned all five subnets, with only a few IP addresses to spare (10.89.100.244 through 10.89.100.255); these addresses are free to be used as needed in the future as this hypothetical enterprise expands. With FLSM, this would not have been possible, but VLSM gives us the flexibility to create subnets of varying sizes.

> [!translation] 逐句繁體中文翻譯
> - 現在完成了！我們已經分配全部五個 subnets，只剩少數 IP addresses 可用，也就是 10.89.100.244 到 10.89.100.255；隨著這個假想企業未來擴張，這些 addresses 可按需使用。
> - 若使用 FLSM，這是不可能的；但 VLSM 給了我們建立不同大小 subnets 的彈性。

## Exam scenarios

Being comfortable with subnetting is a key element of CCNA exam success. Here are a couple of examples of how your knowledge of subnetting might be tested on the CCNA exam:

> [!translation] 逐句繁體中文翻譯
> - 熟悉 subnetting 是 CCNA 考試成功的關鍵元素。
> - 以下有幾個例子，展示 CCNA 考試可能如何測驗你的 subnetting 知識。

1 (multiple choice, multiple answers)
Which of the following prefix length to subnet mask pairs are correct? (Select two.)
    A $/ 25=255.255 .255 .192$
    B $/ 15=255.252 .0 .0$
    c $/ 29=255.255 .255 .248$
    D $/ 27=255.255 .255 .240$
    E $/ 18=255.255 .192 .0$
    F $/ 10=255.224 .0 .0$

Subnet masks are necessary for configuring IP addresses and static routes in Cisco IOS, so it's important that you are able to identify the correct subnet mask for a given prefix length. Fortunately, subnet masks are fairly simple-a series of binary 1s followed by a series of binary Os. In this case, (C) is the first correct option; /29 is equivalent to 255.255.255.248 (29 binary 1s followed by 3 binary 0s, written in dotted decimal). (E) is the second correct option; /18 is equivalent to 255.255.192.0. As I've said previously, being comfortable converting between binary and dotted decimal is key!
(continued)

> [!translation] 逐句繁體中文翻譯
> - 在 Cisco IOS 中設定 IP addresses 與 static routes 需要 subnet masks，所以你必須能為給定 prefix length 找出正確 subnet mask。
> - 幸好 subnet masks 相當簡單：就是一串 binary 1 後面接一串 binary 0。
> - 在這題中，C 是第一個正確選項；/29 等同 255.255.255.248，也就是 29 個 binary 1 後接 3 個 binary 0，再寫成 dotted decimal。
> - E 是第二個正確選項；/18 等同 255.255.192.0。
> - 如我前面所說，熟悉 binary 與 dotted decimal 之間的轉換是關鍵。

2 (drag and drop)
Drag each subnet on the left to its appropriate usable host address range on the right. There are more usable address ranges provided than subnets, so not all address ranges will be used.

| (A) 10.23.24.128/25 | 10.23.24.65-10.23.24.79 |
| :--- | :--- |
| (B) 10.23.24.128/27 | 10.23.24.65-10.23.24.78 |
| (C) 10.23.24.64/26 | 10.23.24.65-10.23.24.126 |
| (D) 10.23.24.64/28 | 10.23.24.129-10.23.24.254 |
|  | 10.23.24.129-10.23.24.190 |
|  | 10.23.24.129-10.23.24.158 |

Identifying a subnet's usable address range requires identifying the subnet's first and last usable addresses; they identify the start and end of the usable address range. In this question, (A)'s usable address range is 10.23.24.129 to 10.23.24.254, (B)'s usable address range is 10.23.24.129 to 10.23.24.158, (C)'s usable address range is 10.23.24.65 to 10.23.24.126, and (D)'s usable address range is 10.23.24.65 to 10.23.24.78. To solve this question, just focus on the final octet of each address; the first three octets are the same in all of them.

> [!translation] 逐句繁體中文翻譯
> - 找出 subnet 的 usable address range，需要先找出 subnet 的 first usable address 與 last usable address；它們標示 usable address range 的起點與終點。
> - 在這題中，A 的 usable address range 是 10.23.24.129 到 10.23.24.254，B 是 10.23.24.129 到 10.23.24.158，C 是 10.23.24.65 到 10.23.24.126，D 是 10.23.24.65 到 10.23.24.78。
> - 解這題時，只要專注在每個 address 的最後一個 octet，前三個 octets 都相同。

### 11.4 Additional subnetting practice

To become proficient at subnetting, you need to practice. Fortunately, there are some free websites that generate subnetting problems you can solve. One example is https://www.subnetting.net/, but you can find others with a quick Google search. I recommend spending a bit of time each day practicing subnetting for at least a week or two or until you feel confident solving questions on the practice sites.

> [!translation] 逐句繁體中文翻譯
> - 要精通 subnetting，你需要練習。
> - 幸好有一些免費網站可以產生 subnetting problems 讓你解題。
> - 其中一個例子是 https://www.subnetting.net/，你也可以快速 Google 找到其他網站。
> - 我建議每天花一點時間練習 subnetting，至少持續一兩週，或直到你能有信心解出練習網站上的題目。

Before sending you off to try out practice questions, I want to mention two points. First, some practice questions you encounter may not explicitly state the size of the address block you must subnet. Here's an example: "What is the maximum number of valid subnets and usable hosts per subnet that you can get from the network 172.26.0.0 255.255.252.0?" In such cases, assume the original address block is a classful network. In this example, the IP address is 172.26.0.0, which is a class B address (because it starts with 0b10), so you can assume that the address block is /16 (172.26.0.0/16).

> [!translation] 逐句繁體中文翻譯
> - 在讓你去做練習題之前，我想提兩點。
> - 第一，有些練習題不會明確說出你必須 subnet 的 address block 大小。
> - 例如：從 network 172.26.0.0 255.255.252.0 可得到的最大 valid subnets 數量與每 subnet usable hosts 數量是多少？遇到這種情況，假設原始 address block 是 classful network。
> - 在此例中，IP address 是 172.26.0.0，是 class B address，因為它以 0b10 開頭，所以可假設 address block 是 /16，也就是 172.26.0.0/16。

The second point is that some questions will ask about wildcard masks, which are similar to subnet masks, but not actually related to the topic of subnetting. We will cover wildcard masks in chapter 17 (Dynamic Routing), as well as chapters 23 and 24 (Access Control Lists). For now, you can skip any questions on a practice website that mention wildcard masks.

> [!translation] 逐句繁體中文翻譯
> - 第二點是，有些題目會問 wildcard masks，它們類似 subnet masks，但其實不屬於 subnetting 主題。
> - 我們會在第 17 章 Dynamic Routing，以及第 23、24 章 Access Control Lists 中介紹 wildcard masks。
> - 現在你可以跳過任何練習網站上提到 wildcard masks 的題目。

## The "magic number" method

Some instructors teach shortcuts that can help you solve subnetting scenarios without having to think about the underlying binary; one famous example is called the "magic number" method. I do not agree with these methods for CCNA candidates because I think they only serve as a crutch, helping you to solve subnetting problems without understanding how subnetting actually works. It may seem cumbersome to always be thinking about binary, but with practice, it will become effortless. And you'll become better not just at subnetting but at all of the other necessary skills that require proficiency with binary (I listed some in chapter 7).

> [!translation] 逐句繁體中文翻譯
> - 有些講師會教 shortcuts，幫你不用思考底層 binary 就解 subnetting scenarios；其中一個有名例子叫做 magic number method。
> - 我不太同意 CCNA candidates 依賴這些方法，因為我認為它們只是拐杖，幫你在不理解 subnetting 實際運作的情況下解題。
> - 一直思考 binary 可能一開始很麻煩，但透過練習會變得毫不費力。
> - 你也不只會變得更擅長 subnetting，還會更擅長其他需要 binary 熟練度的必要技能，我在第 7 章列過一些。

## Summary

- Classless Inter-Domain Routing (CIDR) replaced classful addressing, allowing prefix lengths outside of the traditional /8, /16, and /24.
- With CIDR, an address block can be divided into smaller networks called subnets. This process is called subnetting.
- Fixed-Length Subnet Masking (FLSM) subnetting divides an address block into subnets of equal size.
- Variable-Length Subnet Masking (VLSM) subnetting divides an address block into subnets of varying size.
- To subnet an address block, you "borrow" bits from the host portion of the address block and add them to the network portion. Whereas the network portion of the original address block cannot be changed, the borrowed bits can be changed to make different subnets.
- Each additional borrowed bit doubles the number of subnets that can be made: 1 borrowed bit = 2 subnets, 2 borrowed bits = 4 subnets, 3 borrowed bits = 8 subnets, etc. However, each additional borrowed bit halves the number of addresses in each subnet because there are fewer bits in the host portion.
- The five attributes of an IPv4 network are calculated in the same manner for subnets: the network address is the first address of a subnet (host portion of all 0s), the broadcast address is the last address of a subnet (host portion of all 1s), the first usable address is the first address after the network address, the last usable address is the last address before the broadcast address, and the maximum number of hosts is $2^{\mathrm{y}}-2$, where $y$ is the number of host bits.
- For point-to-point links (connections between two routers), either a /30 or a /31 prefix length can be used. /30 consumes four addresses (network address, broadcast address, and two host addresses), whereas /31 consumes only two addresses (two host addresses, without a network or broadcast address).
- To subnet an address block using VLSM, assign the largest subnet at the start of the address block, assign the second-largest subnet after it, and repeat the process until all subnets have been assigned.

- The network address of the next subnet is the address immediately after the broadcast address of the current subnet.
- In a real-world situation, you should leave some room in each subnet for future growth. When doing subnetting scenarios for practice (or for the CCNA exam), be as efficient as possible (leave as few unused addresses as possible).

## Part 3

## Layer 2 concepts

In part 3, we will build upon your foundational understanding of how switches provide connectivity within a LAN. You have already grasped the basics of MAC address learning and aging, frame forwarding and flooding, and other key operations of switches. Now, let's delve deeper into several advanced features that optimize and secure LANs. We'll start in chapter 12 by exploring virtual LANs (VLANs), which allow us to divide a single physical switch into multiple virtual switches, dividing the LAN into multiple separate segments and enhancing its security and efficiency.

> [!translation] 逐句繁體中文翻譯
> - 在第 3 部分，我們會建立在你對 switches 如何在 LAN 內提供連通性的基礎理解之上。
> - 你已經掌握 MAC address learning 與 aging、frame forwarding 與 flooding，以及 switches 的其他關鍵操作。
> - 現在，讓我們更深入探討幾個用來最佳化並保護 LAN 的進階功能。
> - 我們會從第 12 章的 virtual LANs（VLANs）開始，它能把單一 physical switch 分成多個 virtual switches，將 LAN 分割成多個獨立 segments，提升安全性與效率。

In chapter 13, we will continue on the topic of VLANs, covering two auxiliary protocols that streamline the configuration and management of VLANs on Cisco switches: Dynamic Trunking Protocol (DTP) and VLAN Trunking Protocol (VTP). We will then shift our attention to Spanning Tree Protocol (STP) in chapter 14 and Rapid Spanning Tree Protocol (RSTP) in chapter 15. These protocols are vital for preventing broadcast storms-broadcast frames that infinitely loop around the switches in the LAN, clogging up the LAN and preventing hosts in the LAN from communicating.

> [!translation] 逐句繁體中文翻譯
> - 在第 13 章，我們會延續 VLAN 主題，介紹兩個能簡化 Cisco switches 上 VLAN 設定與管理的輔助協定：Dynamic Trunking Protocol（DTP）與 VLAN Trunking Protocol（VTP）。
> - 接著我們會在第 14 章轉向 Spanning Tree Protocol（STP），並在第 15 章介紹 Rapid Spanning Tree Protocol（RSTP）。
> - 這些協定對防止 broadcast storms 非常重要；broadcast storms 是 broadcast frames 在 LAN switches 間無限循環，塞滿 LAN 並阻止 hosts 通訊的現象。

Finally, we will conclude part 3 by examining EtherChannel in chapter 16. EtherChannel allows multiple physical connections between two switches to form a single logical connection, increasing the available bandwidth in the LAN. By the end of part 3, you will understand these concepts both theoretically and practically and be able to implement them on Cisco switches; this is essential both for CCNA exam success and for any network professional who wants to design, implement, and troubleshoot modern LANs effectively.

> [!translation] 逐句繁體中文翻譯
> - 最後，我們會在第 16 章檢視 EtherChannel。
> - EtherChannel 允許兩台 switches 之間的多條 physical connections 形成單一 logical connection，增加 LAN 中可用 bandwidth。
> - 到第 3 部分結束時，你會從理論與實務兩方面理解這些概念，並能在 Cisco switches 上實作；這對 CCNA 考試成功，以及任何想有效設計、實作與排錯現代 LAN 的網路專業人士都很重要。

Licensed to Luke Chen [mic215fa@gmail.com](mailto:mic215fa@gmail.com)

> [!translation] 逐句繁體中文翻譯
> - 授權給 Luke Chen [mic215fa@gmail.com](mailto:mic215fa@gmail.com)。
