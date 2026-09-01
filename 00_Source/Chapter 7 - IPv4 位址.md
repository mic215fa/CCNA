## IPv4 addressing

## This chapter covers

- The fields of the IPv4 header
- The binary number system
- How to convert between decimal and binary
- The structure of IPv4 addresses
- How to configure IPv4 addresses on Cisco routers

In chapter 6, we focused on Layer 2 of the TCP/IP model: how switches use information in the Ethernet header to make forwarding decisions. In this chapter, we will move up a layer to Layer 3 and look at the contents of the Internet Protocol version 4 (IPv4) header, focusing on IPv4 addressing.

> [!translation] 逐句繁體中文翻譯
> 在 chapter 6，我們專注於 TCP/IP model 的 Layer 2：switches 如何使用 Ethernet header 中的資訊做 forwarding decisions。  
> 在本章中，我們會往上一層到 Layer 3，查看 Internet Protocol version 4（IPv4）header 的內容，並聚焦於 IPv4 addressing。

We are now in the realm of routers, rather than switches. Whereas switches use information in the Layer 2 header to decide how to forward messages to their proper destinations, routers use information in the Layer 3 header to make their forwarding decisions. In this chapter, we won't yet focus on exactly how routers make those forwarding decisions; we will leave that for part 2 of this book. Instead, we will first focus on the contents of the IPv4 header and the addresses used in that header.

> [!translation] 逐句繁體中文翻譯
> 我們現在進入 routers 的領域，而不是 switches。  
> Switches 使用 Layer 2 header 中的資訊決定如何將 messages forward 到正確目的地；routers 則使用 Layer 3 header 中的資訊做 forwarding decisions。  
> 在本章中，我們尚不聚焦 routers 實際如何做出這些 forwarding decisions；這會留到本書 part 2。  
> 相反地，我們會先聚焦 IPv4 header 的內容，以及該 header 中使用的 addresses。

The specific exam topic we will cover is topic 1.6: Configure and verify IPv4 addressing and subnetting. However, IPv4 addressing is not only relevant to exam topic 1.6; it is a fundamental topic that is essential to understanding nearly any other CCNA exam topic. Also note that we will cover subnetting, the second half of topic 1.6, in part 2 of this book.

> [!translation] 逐句繁體中文翻譯
> 我們會涵蓋的特定 exam topic 是 topic 1.6：Configure and verify IPv4 addressing and subnetting。  
> 不過，IPv4 addressing 不只與 exam topic 1.6 有關；它是理解幾乎任何其他 CCNA exam topic 都必要的 fundamental topic。  
> 另外請注意，topic 1.6 的後半 subnetting 會在本書 part 2 介紹。

Given the name IPv4, you may wonder what happened to previous versions. The history and characteristics of IPv0, v1, v2, and v3, although important steps in the evolution toward IPv4, are not necessary to know for the CCNA exam, so we will not cover them. It is IPv4, officially defined in RFC 791 (simply titled "Internet Protocol") that is the foundation of modern networks such as the internet.

> [!translation] 逐句繁體中文翻譯
> 看到 IPv4 這個名稱，你可能會想知道先前版本發生了什麼事。  
> IPv0、v1、v2 與 v3 的歷史與特性，雖然是演進到 IPv4 的重要步驟，但 CCNA exam 不需要知道，所以我們不會介紹。  
> 正式定義於 RFC 791（標題很簡單，就是「Internet Protocol」）的 IPv4，是 internet 等 modern networks 的基礎。

NOTE In addition to IPv4, IPv6 is another major exam topic that has its own part in this volume. IPv6 was introduced in 1995 to replace IPv4, but its adoption has been slow. Although IPv6 adoption is accelerating as the available IPv4 address pool runs out, it seems that for the foreseeable future, network engineers will have to be familiar with both IPv4 and IPv6.

> [!translation] 逐句繁體中文翻譯
> 注意：除了 IPv4 之外，IPv6 也是另一個 major exam topic，並在本卷中有自己的 part。  
> IPv6 於 1995 年推出，用來取代 IPv4，但採用速度一直很慢。  
> 雖然隨著可用 IPv4 address pool 耗盡，IPv6 adoption 正在加速，但可預見的未來中，network engineers 似乎仍必須同時熟悉 IPv4 與 IPv6。

### 7.1 The IPv4 header

Before looking at the details of IPv4 addressing, it's helpful to understand the header that contains those addresses. However, the IPv4 header doesn't just contain IPv4 addresses; it contains a variety of fields, each serving a different role in enabling the end-to-end delivery of packets (the role of Layer 3).

> [!translation] 逐句繁體中文翻譯
> 在查看 IPv4 addressing 的細節之前，先理解包含這些 addresses 的 header 會很有幫助。  
> 不過，IPv4 header 不只包含 IPv4 addresses；它包含多種 fields，每個 field 在實現 packets 的 end-to-end delivery（Layer 3 的角色）中扮演不同作用。

The IPv4 header is more complex than the Ethernet header, as you'll probably notice when looking at figure 7.1. In total, there are 14 fields (although the Options field is optional), whereas the Ethernet header and trailer only have 4 (6 if you include the Preamble and SFD).

> [!translation] 逐句繁體中文翻譯
> IPv4 header 比 Ethernet header 更複雜，當你看 figure 7.1 時應該會注意到。  
> 總共有 14 個 fields（雖然 Options field 是 optional），而 Ethernet header 與 trailer 只有 4 個 fields（若包含 Preamble 與 SFD 則是 6 個）。

|  | Byte | 0 |  |  |  |  |  |  |  | 1 |  |  |  |  |  |  |  | 2 |  |  |  |  |  |  |  | 3 |  |  |  |  |  |  |  |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Byte | Bit | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 |
| 0 | 0 | Version |  |  |  | IHL |  |  |  | DSCP |  |  |  |  |  | ECN |  | Total Length |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| 4 | 32 | Identification |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | Flags |  |  | Fragment Offset |  |  |  |  |  |  |  |  |  |  |  |  |
| 8 | 64 | Time To Live |  |  |  |  |  |  |  | Protocol |  |  |  |  |  |  |  | Header Checksum |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| 12 | 96 | Source Address |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| 16 | 128 | Destination Address |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| 20 | 160 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| ⋮ | ⋮ |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| 56 | 448 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |

Figure 7.1 The format of the IPv4 header. The header is typically 20 bytes in size (the minimum size) but can be up to 60 bytes if the Options field is used.

> [!translation] 逐句繁體中文翻譯
> Figure 7.1：IPv4 header 的格式。  
> Header 通常大小為 20 bytes（minimum size），但如果使用 Options field，最大可達 60 bytes。

Before we examine the purpose of each field of the header, I want to clarify how to read figure 7.1. The fields of the header are contained within the thick border and should be read from left to right, top to bottom; the first bit of the header is in the top-left position, and the last bit is in the bottom-right position. The numbers along the top indicate that each row is 4 bytes (32 bits) in length. The numbers on the left of each row indicate the starting byte/bit number of that row. For example, the second row starts at byte 4 (the fifth byte), which is bit 32 (the thirty-third bit).

> [!translation] 逐句繁體中文翻譯
> 在檢視 header 每個 field 的用途之前，我想先說明如何閱讀 figure 7.1。  
> Header 的 fields 位於粗框內，應由左到右、由上到下閱讀；header 的第一個 bit 位於左上角，最後一個 bit 位於右下角。  
> 上方的 numbers 表示每一列長度為 4 bytes（32 bits）。  
> 每列左側的 numbers 表示該列起始的 byte/bit number。  
> 例如，第二列從 byte 4（第五個 byte）開始，也就是 bit 32（第三十三個 bit）。

NOTE In networking, you'll have to get used to counting from 0. For example, the range from 0 to 31 includes 32 bits in total: bit 0 is the first bit, bit 1 is the second bit, etc. Likewise, byte 0 is the first byte, byte 1 is the second byte, byte 2 is the third byte, byte 3 is the fourth byte, etc.

> [!translation] 逐句繁體中文翻譯
> 注意：在 networking 中，你必須習慣從 0 開始計數。  
> 例如，0 到 31 的範圍總共包含 32 bits：bit 0 是第一個 bit，bit 1 是第二個 bit，依此類推。  
> 同樣地，byte 0 是第一個 byte，byte 1 是第二個 byte，byte 2 是第三個 byte，byte 3 是第四個 byte，依此類推。

As stated, the Options field is optional (and variable in size), so the length of the IPv4 header is variable. Without the Options field, the header is 20 bytes in length, from the first bit of the Version field to the last bit of the Destination Address field. With the Options field at its maximum size (40 bytes), the IPv4 header is 60 bytes in length. However, the Options field is rarely used and is beyond the scope of the CCNA exam.

> [!translation] 逐句繁體中文翻譯
> 如前所述，Options field 是 optional（且大小可變），所以 IPv4 header 的長度也是可變的。  
> 沒有 Options field 時，header 長度為 20 bytes，從 Version field 的第一個 bit 到 Destination Address field 的最後一個 bit。  
> 當 Options field 達到最大大小（40 bytes）時，IPv4 header 長度為 60 bytes。  
> 不過，Options field 很少使用，且超出 CCNA exam 的範圍。

EXAM TIP For the purpose of the CCNA exam, don't worry about memorizing the length and position of each field of the IPv4 header. Questions on the CCNA exam are more substantial than trivia like "What's the length of field X?" For the purpose of this chapter, it's sufficient to have a basic understanding of the purpose of each field. This chapter focuses on the IPv4 addresses in the Source Address and Destination Address fields, and in the rest of this book, we will look at other fields in greater detail as required.

> [!translation] 逐句繁體中文翻譯
> 考試提示：以 CCNA exam 為目的，不必擔心要背下 IPv4 header 每個 field 的長度與位置。  
> CCNA exam 的 questions 會比「field X 的長度是多少？」這類瑣碎記憶更有實質內容。  
> 以本章為目的，對每個 field 的用途有基本理解就足夠。  
> 本章聚焦於 Source Address 與 Destination Address fields 中的 IPv4 addresses；本書後續會在需要時更詳細介紹其他 fields。

### 7.1.1 The Version field

The first field of the IPv4 header is the Version field. It is 4 bits in length. As I mentioned previously, there are two versions of IP used in modern networks: IPv4 and IPv6. The purpose of this field is simple: to indicate which version of IP is being used. In modern networks, you can expect to find one of two values in this field:

> [!translation] 逐句繁體中文翻譯
> IPv4 header 的第一個 field 是 Version field。  
> 它長度為 4 bits。  
> 如前所述，modern networks 中使用兩個 IP versions：IPv4 與 IPv6。  
> 這個 field 的用途很簡單：指出正在使用哪個 IP version。  
> 在 modern networks 中，你可以預期在這個 field 中看到兩種 values 之一：

- A value of 0b0100 (0d4) indicates IPv4.
- A value of 0b0110 (0d6) indicates IPv6.

NOTE As mentioned in chapter 6, the prefix $O b$ indicates a binary number, and the prefix $O d$ indicates a decimal number. We will look at how to convert between the two number systems later in this chapter.

> [!translation] 逐句繁體中文翻譯
> 注意：如 chapter 6 所述，prefix $O b$ 表示 binary number，prefix $O d$ 表示 decimal number。  
> 本章稍後會介紹如何在這兩種 number systems 之間轉換。

### 7.1.2 The IHL field

The second field is the Internet Header Length (IHL) field, which is 4 bits in length. This field is used to indicate the length of the IPv4 header. The reason this field is necessary is because the IP header is variable in length, depending on whether the Options field is present or not (and the Options field itself is variable in length too).

> [!translation] 逐句繁體中文翻譯
> 第二個 field 是 Internet Header Length（IHL）field，長度為 4 bits。  
> 這個 field 用來表示 IPv4 header 的 length。  
> 這個 field 之所以必要，是因為 IP header 的 length 是可變的，取決於 Options field 是否存在（而 Options field 本身長度也可變）。

The IHL field indicates the length of the IPv4 header in 4-byte increments. For example, if the value of this field is 5, it means the header is 20 bytes in length (the minimum length of the IPv4 header).

> [!translation] 逐句繁體中文翻譯
> IHL field 以 4-byte increments 表示 IPv4 header 的 length。  
> 例如，如果這個 field 的 value 是 5，代表 header 長度為 20 bytes（IPv4 header 的 minimum length）。

NOTE A value less than 5 should not be used in this field because the IPv4 header cannot be less than 20 bytes in length.

> [!translation] 逐句繁體中文翻譯
> 注意：這個 field 不應使用小於 5 的 value，因為 IPv4 header 長度不能小於 20 bytes。

Any value greater than 5 in the IHL field indicates that the Options field is present in the header. The maximum value of the IHL field is 15, indicating that the header is 60 bytes in length (the maximum length of the IPv4 header). In that case, the Options field is 40 bytes in length, and the rest of the header is 20 bytes.

> [!translation] 逐句繁體中文翻譯
> IHL field 中任何大於 5 的 value 都表示 header 中存在 Options field。  
> IHL field 的 maximum value 是 15，表示 header 長度為 60 bytes（IPv4 header 的 maximum length）。  
> 在那種情況下，Options field 長度為 40 bytes，其餘 header 為 20 bytes。

### 7.1.3 The DSCP and ECN fields

The next two fields are Differentiated Services Code Point (DSCP), which is 6 bits in length, and Explicit Congestion Notification (ECN), which is 2 bits in length. This byte of the IPv4 header used to be called the Type of Service field and still is sometimes, but DSCP + ECN is the current definition.

> [!translation] 逐句繁體中文翻譯
> 接下來兩個 fields 是 Differentiated Services Code Point（DSCP，長度 6 bits）與 Explicit Congestion Notification（ECN，長度 2 bits）。  
> IPv4 header 的這個 byte 過去稱為 Type of Service field，有時現在仍這麼稱呼，但 DSCP + ECN 是目前的定義。

These fields are used for Quality of Service (QoS), which is a network feature used to prioritize specific types of network traffic over other types. A common use case for QoS is to prioritize delay-sensitive network traffic-network traffic for which it is very important to reach the destination as soon as possible, without delay. One example of this is voice and video traffic; I think most of us know how frustrating it can be to have a Zoom call (or a call using any similar application) with poor quality. QoS helps ensure that this traffic is forwarded with as little delay as possible.

> [!translation] 逐句繁體中文翻譯
> 這些 fields 用於 Quality of Service（QoS），這是一項 network feature，用來讓特定類型的 network traffic 優先於其他類型。  
> QoS 的常見 use case 是優先處理 delay-sensitive network traffic，也就是非常需要盡快、無延遲到達 destination 的 network traffic。  
> Voice 與 video traffic 就是一個例子；我想多數人都知道品質很差的 Zoom call（或任何類似 application 的通話）有多令人挫折。  
> QoS 幫助確保這類 traffic 盡可能以最少 delay 被 forwarded。

NOTE QoS is another CCNA exam topic, and we will cover it in chapter 10 of volume 2 of this book.

> [!translation] 逐句繁體中文翻譯
> 注意：QoS 是另一個 CCNA exam topic，我們會在本書 volume 2 的 chapter 10 介紹。

### 7.1.4 The Total Length field

The Total Length field is a 16-bit field that indicates the total length of the packet-the IPv4 header and its payload. Don't confuse this with the IHL field, which indicates the length of the IPv4 header alone. Figure 7.2 illustrates the difference between the IHL and Total Length fields.

> [!translation] 逐句繁體中文翻譯
> Total Length field 是 16-bit field，表示 packet 的 total length，也就是 IPv4 header 加上 payload。  
> 不要把它與 IHL field 混淆，IHL field 只表示 IPv4 header 本身的 length。  
> Figure 7.2 說明 IHL 與 Total Length fields 的差異。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-116_245_637_185_350.jpg)
Figure 7.2 The difference between the IHL and Total Length fields. The IHL field indicates the length of the IPv4 header (Layer 3 header), whereas the Total Length field indicates the length of the entire packet. The Layer 2 header and trailer are shown to emphasize that a packet will always be encapsulated in a frame before being sent; a packet alone is not ready to be sent over the physical medium.

> [!translation] 逐句繁體中文翻譯
> Figure 7.2：IHL 與 Total Length fields 的差異。  
> IHL field 表示 IPv4 header（Layer 3 header）的 length，而 Total Length field 表示整個 packet 的 length。  
> 圖中顯示 Layer 2 header 與 trailer，是為了強調 packet 在送出前一定會被 encapsulated in a frame；單獨的 packet 尚未準備好透過 physical medium 傳送。

Another difference between the IHL and Total Length fields is that the value of the Total Length field indicates the length of the packet in bytes, rather than 4-byte increments. For example, a value of 100 in the Total Length field means the packet is 100 bytes in length, and a value of 1,000 in the Total Length field means the packet is 1,000 bytes in length.

> [!translation] 逐句繁體中文翻譯
> IHL 與 Total Length fields 的另一個差異是，Total Length field 的 value 以 bytes 表示 packet length，而不是以 4-byte increments 表示。  
> 例如，Total Length field 中 value 為 100，代表 packet 長度為 100 bytes；value 為 1,000，代表 packet 長度為 1,000 bytes。

### 7.1.5 The Identification, Flags, and Fragment Offset fields

The Identification, Flags, and Fragment Offset fields, 32 bits in total, are used together to support packet fragmentation-when a packet is broken up into multiple smaller packets called fragments. IPv4 uses a concept called maximum transmission unit (MTU) to indicate the maximum size a packet should be, and any packet larger than the MTU will be fragmented. Then, the final destination host of the packet reassembles the fragments to restore the original packet.

> [!translation] 逐句繁體中文翻譯
> Identification、Flags 與 Fragment Offset fields 總共 32 bits，會一起用來支援 packet fragmentation，也就是 packet 被拆成多個較小 packets，稱為 fragments。  
> IPv4 使用 maximum transmission unit（MTU）這個概念，表示 packet 應有的 maximum size；任何大於 MTU 的 packet 都會被 fragmented。  
> 接著，packet 的 final destination host 會重新組合 fragments，以還原 original packet。

The typical MTU is 1,500 bytes, and this should be supported on all modern devices. However, if for some reason a router in the packet's path to the destination has a lower MTU, it will fragment the packet. Another possibility is that a host sends packets larger than the standard 1,500-byte size (sometimes packet sizes up to 9,000 bytes are used). If a router in the path to the destination doesn't support those larger packets, it will fragment them. Let's briefly examine the role of each of these three fields.

> [!translation] 逐句繁體中文翻譯
> Typical MTU 是 1,500 bytes，所有 modern devices 都應該支援。  
> 然而，如果 packet 到 destination 的 path 中某台 router 因某種原因有較低 MTU，它會 fragment 該 packet。  
> 另一種可能是 host 傳送大於標準 1,500-byte size 的 packets（有時會使用高達 9,000 bytes 的 packet size）。  
> 如果通往 destination 的 path 中某台 router 不支援這些較大的 packets，它會 fragment 它們。  
> 我們簡短檢視這三個 fields 各自的角色。

## Identification field

This field is 16 bits in length and is used to identify which original packet a fragment belongs to. When a packet is fragmented, all of its fragments must have the same value in this field.

> [!translation] 逐句繁體中文翻譯
> 這個 field 長度為 16 bits，用來識別某個 fragment 屬於哪個 original packet。  
> 當 packet 被 fragmented 時，它的所有 fragments 在這個 field 中必須有相同 value。

## Flags field

This field is 3 bits in length and is used to control and identify fragments. The 3 bits of this field (bit 0 , bit 1 , and bit 2 ) are defined as follows:

> [!translation] 逐句繁體中文翻譯
> 這個 field 長度為 3 bits，用來控制與識別 fragments。  
> 這個 field 的 3 個 bits（bit 0、bit 1、bit 2）定義如下：

- Bit 0: Reserved-This bit's use hasn't been defined, so it is always set to 0.
- Bit 1: Don't Fragment (DF) bit-If this bit is set to 1, it means the packet should not be fragmented. In that case, if the packet's size is greater than the MTU, it will be discarded.

- Bit 2: More Fragments (MF) bit-If this bit is set to 1, it means there are more fragments remaining-this one isn't the last. The final fragment of the packet will have a value of 0 in this field (indicating that there are no more fragments). An unfragmented packet will always have a value of 0 for this bit.

## Fragment Offset field

This field is 13 bits in length and is used to indicate the position of the fragment within the original packet. This allows fragmented packets to be reassembled even if the fragments arrive out of order. This is rare, but if there are multiple paths to a destination, different fragments might take different paths, in which case they may arrive at the destination out of order.

> [!translation] 逐句繁體中文翻譯
> 這個 field 長度為 13 bits，用來表示 fragment 在 original packet 中的位置。  
> 這讓 fragmented packets 即使 fragments 抵達順序不同，也能被重新組合。  
> 這很罕見，但如果到 destination 有 multiple paths，不同 fragments 可能走不同 paths，在這種情況下它們可能會以錯序抵達 destination。

### 7.1.6 The TTL field

The Time To Live (TTL) field is an 8-bit field. When a host sends a packet, it will set an initial value in this field (a common value is 64), and then each router that forwards the packet will decrease the value in this field by 1. If the value reaches 0, the router will drop the packet.

> [!translation] 逐句繁體中文翻譯
> Time To Live（TTL）field 是 8-bit field。  
> 當 host 傳送 packet 時，它會在這個 field 設定 initial value（常見 value 是 64），接著每台 forward 該 packet 的 router 都會將此 field 的 value 減 1。  
> 如果 value 到達 0，router 會 drop 該 packet。

The reason for this mechanism is to prevent packets from looping around the network infinitely. A loop is when a message travels around the network without being able to find its destination. For example, if there are three routers (R1, R2, and R3), a looping packet might be passed from R1 to R2, from R2 to R3, from R3 to R1, from R1 to R2, from R2 to R3, etc. in a loop. Figure 7.3 shows an example of a looped packet being dropped thanks to the TTL field.

> [!translation] 逐句繁體中文翻譯
> 這個 mechanism 的原因是防止 packets 在 network 中無限 looping。  
> Loop 是指 message 在 network 中移動卻無法找到 destination。  
> 例如，如果有三台 routers（R1、R2、R3），looping packet 可能會從 R1 傳到 R2、從 R2 傳到 R3、從 R3 傳到 R1、再從 R1 到 R2、從 R2 到 R3，如此不斷 loop。  
> Figure 7.3 顯示一個因 TTL field 而被 dropped 的 looped packet 範例。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-117_525_598_1252_318.jpg)
Figure 7.3 A looped packet is dropped due to the TTL mechanism. (1) R1 forwards the packet to R2 with a TTL of 5. (2) R2 forwards the packet to R3 with a TTL of 4. (3) R3 forwards the packet to R1 with a TTL of 3. (4) R1 forwards the packet to R2 with a TTL of 2. (5) R2 forwards the packet to R3 with a TTL of 1. (6) R3 wants to forward the packet to R1 but drops the packet because it must decrement the TTL to 0.

> [!translation] 逐句繁體中文翻譯
> Figure 7.3：looped packet 因 TTL mechanism 而被 dropped。  
> （1）R1 以 TTL 5 將 packet forward 給 R2。  
> （2）R2 以 TTL 4 將 packet forward 給 R3。  
> （3）R3 以 TTL 3 將 packet forward 給 R1。  
> （4）R1 以 TTL 2 將 packet forward 給 R2。  
> （5）R2 以 TTL 1 將 packet forward 給 R3。  
> （6）R3 想將 packet forward 給 R1，但因為必須將 TTL 遞減為 0，所以 drop 該 packet。

Loops should not occur in a properly configured network, but mistakes can happen. The TTL field prevents packets from looping indefinitely; once the packet's TTL reaches 0, it will be dropped.

> [!translation] 逐句繁體中文翻譯
> 在 properly configured network 中不應該發生 loops，但錯誤仍可能出現。  
> TTL field 防止 packets 無限 looping；一旦 packet 的 TTL 到達 0，它就會被 dropped。

### 7.1.7 The Protocol field

The Protocol field is 8 bits in length and is used to indicate what kind of message is encapsulated inside of the packet. This is similar to the Ethernet header's EtherType field, which indicates the type of message encapsulated in the frame (for example, an IPv4 packet or an IPv6 packet).

> [!translation] 逐句繁體中文翻譯
> Protocol field 長度為 8 bits，用來表示 packet 內 encapsulated 的 message 類型。  
> 這類似 Ethernet header 的 EtherType field，該 field 表示 frame 中 encapsulated 的 message type（例如 IPv4 packet 或 IPv6 packet）。

In the previous chapter, we covered the ping utility, which is a component of ICMP. If a packet contains an ICMP message, that is indicated with a value of 1 in this field. The following are the Protocol field values of some protocols we will cover in this book:

> [!translation] 逐句繁體中文翻譯
> 在前一章，我們介紹了 ping utility，它是 ICMP 的一個 component。  
> 如果 packet 包含 ICMP message，會在這個 field 中以 value 1 表示。  
> 以下是本書會介紹的一些 protocols 的 Protocol field values：

- 1-ICMP
- 6-Transmission Control Protocol (TCP)
- 17-User Datagram Protocol (UDP)
- 89-Open Shortest Path First (OSPF)

### 7.1.8 The Header Checksum field

The Header Checksum field is 16 bits in length and is used to check for errors in the IPv4 header. The mechanism is similar to the FCS in the Ethernet trailer. However, a major difference is that the Header Checksum field only checks for errors in the IPv4 header, not in the entire packet. On the other hand, the Ethernet FCS field doesn't just check for errors in the Ethernet header; it checks for errors in the entire frame.

> [!translation] 逐句繁體中文翻譯
> Header Checksum field 長度為 16 bits，用來檢查 IPv4 header 中的 errors。  
> 其 mechanism 類似 Ethernet trailer 中的 FCS。  
> 不過，一個主要差異是 Header Checksum field 只檢查 IPv4 header 中的 errors，而不是整個 packet。  
> 另一方面，Ethernet FCS field 不只檢查 Ethernet header 中的 errors；它會檢查整個 frame 中的 errors。

### 7.1.9 The Source Address and Destination Address fields

These two fields contain the IP address of the host sending the packet (Source Address field) and the intended recipient of the packet (Destination Address field). Each of these fields is 32 bits in length-the length of an IPv4 address. We will cover the structure of IPv4 addresses in detail later in this chapter.

> [!translation] 逐句繁體中文翻譯
> 這兩個 fields 包含 sending packet 的 host IP address（Source Address field）以及 packet 預定 recipient 的 IP address（Destination Address field）。  
> 這兩個 fields 各自長度為 32 bits，也就是 IPv4 address 的長度。  
> 本章稍後會詳細介紹 IPv4 addresses 的結構。

### 7.1.10 The Options field

The final field of the IPv4 header is the Options field. As mentioned previously, this field is optional and variable in length-from 0 bytes (if not used) to 40 bytes in length; this is the reason the IPv4 header requires a field to indicate the length of the header itself. This field is rarely used, and its use cases are beyond the scope of the CCNA exam.

> [!translation] 逐句繁體中文翻譯
> IPv4 header 的最後一個 field 是 Options field。  
> 如前所述，這個 field 是 optional 且長度可變，從 0 bytes（不使用時）到 40 bytes；這就是 IPv4 header 需要 field 來表示 header 本身長度的原因。  
> 這個 field 很少使用，其 use cases 也超出 CCNA exam 範圍。

### 7.2 The binary number system

To understand IPv4 addresses, you have to understand the binary number system, as well as how to convert between binary and decimal numbers. And to understand how binary numbers work, let's first review how decimal numbers work. We're all familiar with decimal numbers because we use them in our daily lives, but many of us don't think about how the decimal number system actually works.

> [!translation] 逐句繁體中文翻譯
> 若要理解 IPv4 addresses，你必須理解 binary number system，以及如何在 binary 與 decimal numbers 之間轉換。  
> 若要理解 binary numbers 如何運作，我們先回顧 decimal numbers 如何運作。  
> 我們都熟悉 decimal numbers，因為日常生活會用到它們，但許多人並不會思考 decimal number system 實際如何運作。

### 7.2.1 Decimal

The decimal number system uses 10 digits: 0, 1, 2, 3, 4, 5, 6, 7, 8, and 9 . All values are expressed using those 10 digits. For this reason, the decimal number system is also called base 10. Values from 0 to 9 can be expressed with a single digit, but to express greater values, we have to use more digits. For example, the number after 0d9 is 0d10-a 1 in the tens position and a 0 in the ones position.

> [!translation] 逐句繁體中文翻譯
> Decimal number system 使用 10 個 digits：0、1、2、3、4、5、6、7、8、9。  
> 所有 values 都使用這 10 個 digits 表示。  
> 因此，decimal number system 也稱為 base 10。  
> 0 到 9 的 values 可以用單一 digit 表示，但若要表示更大的 values，就必須使用更多 digits。  
> 例如，0d9 之後的 number 是 0d10，也就是 tens position 的 1 與 ones position 的 0。

After counting up to 0d99 (9 in the tens position and 9 in the ones position), we have to add a third digit; the number after 0d99 is 0d100-a 1 in the hundreds position and a 0 in both the tens and ones positions. Because decimal uses 10 digits, the value of each additional position increases tenfold as you add more digits: 1, then 10, then 100, then 1,000, etc. That's why, in the number 1,009 (for example), the 1 on the left has a greater value than the 9 on the right, even though on its own, the number 9 has a greater value than the number 1.

> [!translation] 逐句繁體中文翻譯
> 數到 0d99（tens position 是 9，ones position 是 9）之後，我們必須加入第三個 digit；0d99 之後的 number 是 0d100，也就是 hundreds position 的 1，以及 tens 與 ones positions 的 0。  
> 因為 decimal 使用 10 個 digits，隨著 digits 增加，每個額外 position 的 value 會增加十倍：1、接著 10、接著 100、接著 1,000，依此類推。  
> 這就是為什麼在 number 1,009 中（例如），左邊的 1 比右邊的 9 有更大的 value，雖然單獨來看 number 9 的 value 大於 number 1。

### 7.2.2 Binary

Counting in the binary system follows the same process but with only two digits: 0 and 1. For this reason, the binary number system is also called base 2. Only the values 0 and 1 can be expressed with a single digit; to express greater values, we have to add more digits.

> [!translation] 逐句繁體中文翻譯
> Binary system 的計數遵循相同 process，但只使用兩個 digits：0 與 1。  
> 因此，binary number system 也稱為 base 2。  
> 只有 values 0 與 1 可以用單一 digit 表示；若要表示更大的 values，就必須加入更多 digits。

The value after 0b1 is 0b10-a 1 in the twos position and a 0 in the ones position; this is equivalent to 0d2. After 0b10 is 0b11 (equivalent to 0d3), and then once again, both positions have reached their maximum value, so a third digit is needed. This results in 0b100 (equivalent to 0d4). Whereas the value of each decimal position increases 10-fold, the value of each binary position doubles because binary uses two digits. Figure 7.4 shows an eight-digit binary number with the value of each position above each bit (binary digit).

> [!translation] 逐句繁體中文翻譯
> 0b1 之後的 value 是 0b10，也就是 twos position 的 1 與 ones position 的 0；這等同於 0d2。  
> 0b10 之後是 0b11（等同於 0d3），接著兩個 positions 再次都達到最大 value，因此需要第三個 digit。  
> 結果是 0b100（等同於 0d4）。  
> Decimal 每個 position 的 value 增加 10 倍；binary 因為使用兩個 digits，所以每個 position 的 value 會加倍。  
> Figure 7.4 顯示一個 eight-digit binary number，每個 bit（binary digit）上方標出該 position 的 value。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-119_169_755_1416_341.jpg)
Figure 7.4 An 8-bit (1-byte) number with the value of each bit written above. The decimal equivalent of 0b10101101 is 0d173. This can be calculated by adding the value of each bit that is set to 1.

> [!translation] 逐句繁體中文翻譯
> Figure 7.4：一個 8-bit（1-byte）number，每個 bit 的 value 寫在上方。  
> 0b10101101 的 decimal equivalent 是 0d173。  
> 這可以透過加總每個 set to 1 的 bit value 來計算。

NOTE The rightmost digit of a binary number is called the least-significant bit, because it has the least value. The leftmost digit is called the most-significant bit, because it has the greatest value.

> [!translation] 逐句繁體中文翻譯
> 注意：binary number 最右邊的 digit 稱為 least-significant bit，因為它的 value 最小。  
> 最左邊的 digit 稱為 most-significant bit，因為它的 value 最大。

Table 7.1 lists some decimal numbers and their binary equivalents. With only two digits available, binary numbers quickly grow in size (the value after 0b11111 would be 0b100000). That is why, although computers use binary numbers,, we convert those
binary values to other number systems (decimal and hexadecimal) to make them more human-readable.

> [!translation] 逐句繁體中文翻譯
> Table 7.1 列出一些 decimal numbers 與其 binary equivalents。  
> 因為只有兩個 digits 可用，binary numbers 很快就會變長（0b11111 之後的 value 會是 0b100000）。  
> 這就是為什麼雖然 computers 使用 binary numbers，我們仍會將那些 binary values 轉換為其他 number systems（decimal 與 hexadecimal），讓它們更 human-readable。

Table 7.1 Decimal numbers and their binary equivalents

> [!translation] 逐句繁體中文翻譯
> Table 7.1：Decimal numbers 與其 binary equivalents。
| Dec. | Bin. | Dec. | Bin. | Dec. | Bin. | Dec. | Bin. |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 0 | 0 | 8 | 1000 | 16 | 10000 | 24 | 11000 |
| 1 | 1 | 9 | 1001 | 17 | 10001 | 25 | 11001 |
| 2 | 10 | 10 | 1010 | 18 | 10010 | 26 | 11010 |
| 3 | 11 | 11 | 1011 | 19 | 10011 | 27 | 11011 |
| 4 | 100 | 12 | 1100 | 20 | 10100 | 28 | 11100 |
| 5 | 101 | 13 | 1101 | 21 | 10101 | 29 | 11101 |
| 6 | 110 | 14 | 1110 | 22 | 10110 | 30 | 11110 |
| 7 | 111 | 15 | 1111 | 23 | 10111 | 31 | 11111 |


Although IPv4 addresses are 32 bits in length, the good news is that you only have to be able to convert between binary and decimal for numbers up to 8 bits in length. That is because IPv4 addresses are divided into four groups of 8 bits, making them more manageable.

> [!translation] 逐句繁體中文翻譯
> 雖然 IPv4 addresses 長度為 32 bits，好消息是你只需要能轉換最多 8 bits 長度的 binary 與 decimal numbers。  
> 這是因為 IPv4 addresses 被分成四組 8 bits，因此更容易處理。

## Converting binary numbers to decimal

Converting binary numbers to decimal is a simple process-just add up the values of the bits that are set to 1. Figure 7.5 demonstrates this process.

> [!translation] 逐句繁體中文翻譯
> 將 binary numbers 轉成 decimal 是簡單的 process；只要加總所有 set to 1 的 bits 的 values。  
> Figure 7.5 示範這個 process。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-120_228_866_1245_367.jpg)
Figure 7.5 The binary number 00101111 is equal to 47 in decimal. To calculate this, add the value of each bit that is set to $1: \mathbf{3 2}+\mathbf{8}+\mathbf{4}+\mathbf{2}+\mathbf{1}=\mathbf{4 7}$.

> [!translation] 逐句繁體中文翻譯
> Figure 7.5：binary number 00101111 等於 decimal 的 47。  
> 若要計算，請加總每個 set to $1$ 的 bit value：$\mathbf{32}+\mathbf{8}+\mathbf{4}+\mathbf{2}+\mathbf{1}=\mathbf{47}$。

I highly recommend spending some time practicing this. To do so, write some random 8-bit numbers (11011010, 01011100, 11101110, etc.), and practice converting them to decimal. With some practice, you should be able to convert from binary to decimal in your head, without writing down the value of each bit. To check your answers, you can do a quick internet search for "binary to decimal converter"; there are plenty of free tools available.

> [!translation] 逐句繁體中文翻譯
> 我強烈建議花些時間練習這件事。  
> 方法是寫下一些隨機 8-bit numbers（11011010、01011100、11101110 等），並練習將它們轉換成 decimal。  
> 經過一些練習後，你應該能在腦中完成 binary 到 decimal 的轉換，而不必寫下每個 bit 的 value。  
> 若要檢查答案，可以快速搜尋「binary to decimal converter」；有很多免費工具可用。

NOTE The minimum value of an 8-bit number (with all bits set to 0) is 0d0. The maximum value of an 8-bit number (with all bits set to 1) is 0d255. Therefore, 8 bits provide 256 possible values: from 0d0 (0b00000000) to 0d255 (0b11111111).

> [!translation] 逐句繁體中文翻譯
> 注意：8-bit number 的 minimum value（所有 bits 都 set to 0）是 0d0。  
> 8-bit number 的 maximum value（所有 bits 都 set to 1）是 0d255。  
> 因此，8 bits 提供 256 種 possible values：從 0d0（0b00000000）到 0d255（0b11111111）。

## Converting decimal numbers to binary

Converting from decimal to binary takes a few more steps. There are a few methods to do this, but figure 7.6 demonstrates the process I use. First, attempt to subtract the value of the most significant bit (128) from the decimal number. If the result is a positive number, note the remainder, and write a 1 in that bit position. If the subtraction would result in a negative value, do not subtract; just write a 0 in that bit position. Then, subtract the value of the second-most significant bit (64) from the remainder of the previous subtraction (or the original number, if you couldn't subtract 128 from the number), and repeat the process until you reach 0 . Figure 7.6 shows how this works:

> [!translation] 逐句繁體中文翻譯
> 從 decimal 轉成 binary 需要多幾個 steps。  
> 有幾種方法可以做到，但 figure 7.6 示範我使用的 process。  
> 首先，嘗試從 decimal number 減去 most significant bit 的 value（128）。  
> 如果結果是 positive number，記下 remainder，並在該 bit position 寫 1。  
> 如果相減會得到 negative value，就不要相減；只在該 bit position 寫 0。  
> 然後，從前一次相減的 remainder（或如果無法從 number 減去 128，則從 original number）減去 second-most significant bit 的 value（64），並重複此 process 直到到達 0。  
> Figure 7.6 顯示這如何運作：

1 Subtracting 128 from 206 gives a remainder of 78. Write a 1 in the 128 position.
2 Subtracting 64 from 78 gives a remainder of 14. Write a 1 in the 64 position.
332 cannot be subtracted from 14. Write a 0 in the 32 position.
416 cannot be subtracted from 14. Write a 0 in the 16 position.
${ }^{5}$ Subtracting 8 from 14 gives a remainder of 6. Write a 1 in the 8 position.
${ }_{6}$ Subtracting 4 from 6 gives a remainder of 2. Write a 1 in the 4 position.
7 Subtracting 2 from 2 gives a remainder of 0 . Write a 1 in the 2 position.
8 We have reached 0, so write a 1 in the remaining position.

We now have the answer: 0d206 is equivalent to 0b11001110.

> [!translation] 逐句繁體中文翻譯
> 現在我們得到答案：0d206 等同於 0b11001110。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-121_276_807_1132_316.jpg)
Figure 7.6 The process of converting a decimal number (206) to a binary number (11001110) by subtracting each bit's decimal value

> [!translation] 逐句繁體中文翻譯
> Figure 7.6：透過減去每個 bit 的 decimal value，將 decimal number（206）轉換為 binary number（11001110）的 process。

Instead of using subtraction, you can convert from decimal to binary using addition if you prefer. Begin with a running total of 0, and progressively add the values of each bit, starting from the leftmost (most significant) bit, without exceeding the value of the decimal number you're converting. Here are the steps for converting the decimal number 206 to binary:

> [!translation] 逐句繁體中文翻譯
> 如果你偏好，也可以不用 subtraction，而改用 addition 從 decimal 轉成 binary。  
> 從 running total 0 開始，並從最左邊（most significant）的 bit 開始逐步加上每個 bit 的 value，但不要超過你正在轉換的 decimal number value。  
> 以下是將 decimal number 206 轉成 binary 的 steps：

$10+128=128$. Write a 1 in the 128 position.
$2128+64=192$. Write a 1 in the 64 position.
$3192+32=224$, which is greater than 206. Write a 0 in the 32 position.
$4192+16=208$, which is greater than 206. Write a 0 in the 16 position.
$5192+8=200$. Write a 1 in the 8 position.
$6200+4=204$. Write a 1 in the 4 position.

> [!translation] 逐句繁體中文翻譯
> $10+128=128$。在 128 position 寫 1。  
> $2128+64=192$。在 64 position 寫 1。  
> $3192+32=224$，大於 206。在 32 position 寫 0。  
> $4192+16=208$，大於 206。在 16 position 寫 0。  
> $5192+8=200$。在 8 position 寫 1。  
> $6200+4=204$。在 4 position 寫 1。

$7204+2=206$. Write a 1 in the 2 position.
8 We have reached the original value (206). Write a 0 in the remaining position.

> [!translation] 逐句繁體中文翻譯
> $7204+2=206$。在 2 position 寫 1。  
> 8 我們已到達 original value（206）。在剩餘 position 寫 0。

Like converting from binary to decimal, this process becomes much easier with practice, and eventually you should be able to do it in your head. To practice, write some random numbers from 0 to 255 (56, 127, 201, 199, etc.), and try converting them into binary.

> [!translation] 逐句繁體中文翻譯
> 和 binary 轉 decimal 一樣，這個 process 透過練習會變得容易許多，最後你應該能在腦中完成。  
> 若要練習，請寫下一些 0 到 255 的 random numbers（56、127、201、199 等），並嘗試將它們轉成 binary。

For additional practice with converting between decimal and binary (in both directions), you can try the Binary Game on Cisco Learning Network: https://learningnetwork.cisco.com/s/binary-game. Try it a few times each day; as you practice and improve, your scores in the Binary Game should increase, and you'll find yourself able to do the necessary calculations in your head.

> [!translation] 逐句繁體中文翻譯
> 若要額外練習 decimal 與 binary 之間的雙向轉換，可以嘗試 Cisco Learning Network 上的 Binary Game：https://learningnetwork.cisco.com/s/binary-game。  
> 每天試幾次；隨著練習與進步，你在 Binary Game 中的分數應該會提高，也會發現自己能在腦中完成必要計算。

EXAM TIP Being able to quickly convert between decimal and binary is a big help on the CCNA exam, especially when it comes to subnetting (the topic of chapter 11). The CCNA exam has a 2-hour time limit-don't unnecessarily spend time doing binary-to-decimal and decimal-to-binary conversions. A bit of practice with Cisco's Binary Game goes a long way.

> [!translation] 逐句繁體中文翻譯
> 考試提示：能快速在 decimal 與 binary 之間轉換，對 CCNA exam 很有幫助，尤其是 subnetting（chapter 11 的 topic）。  
> CCNA exam 有 2 小時 time limit；不要不必要地花時間做 binary-to-decimal 與 decimal-to-binary conversions。  
> 稍微練習 Cisco 的 Binary Game 會很有幫助。

## Exam applications

Binary is a fundamental topic with applications to various CCNA exam topics. In addition to IPv4 addressing (the topic of this chapter), the following are some other topics that require you to be proficient with binary, including converting between binary and decimal:

> [!translation] 逐句繁體中文翻譯
> Binary 是 fundamental topic，會應用於各種 CCNA exam topics。  
> 除了 IPv4 addressing（本章 topic）之外，以下還有一些 topics 也要求你熟練 binary，包含 binary 與 decimal 之間的轉換：

- IPv4 subnetting-Subnetting is the process of dividing networks into smaller networks and is the second half of exam topic 1.6: Configure and verify IPv4 addressing and subnetting. To subnet IPv4 networks, you need to be able to convert IPv4 addresses from decimal to binary and vice versa. We will cover subnetting in chapter 11 of this book.
- IPv6 addressing-This is exam topic 1.8: Configure and verify IPv6 addressing and prefix. To understand IPv6 addresses, you need to be able to convert between binary, decimal, and hexadecimal (because IPv6 addresses are usually written in hexadecimal). We will cover IPv6 in part 5 of this book.
- IPv4 and IPv6 routing-This includes nearly all of domain 3.0 of the CCNA exam topics (IP Connectivity) and is 25\% of the entire CCNA exam. For example, to know how a router will forward a packet, you must identify the most specific matching route-the route with the most bits that match the packet's destination IP address. To do that, you must understand binary numbers. We will cover the concept of the most specific matching route in chapter 9 and other topics in domain 3.0 in parts 4 and 5 of this volume.
Access Control Lists (ACLs)-ACLs are exam topic 5.6: Configure and verify access control lists. ACLs are used to permit or deny specific network traffic, and they do that by comparing bits in the configured ACL to the bits of a packet's source and/or destination IP addresses. To configure appropriate ACLs, you must understand the binary system. We will cover ACLs in part 6 of this book.

### 7.3 IPv4 addressing

An IPv4 address is a 32-bit number that identifies a host at Layer 3 of the TCP/IP Model. IP addresses (whether IPv4 or IPv6) are used to address a message to its final intended recipient, unlike MAC addresses, which are used to address a message to the next hop. Whereas switches are said to be Layer 2 devices, routers are said to be Layer 3 devices or to operate at Layer 3 because they make forwarding decisions based on the destination IP address of messages (located in the Layer 3 header).

> [!translation] 逐句繁體中文翻譯
> IPv4 address 是 32-bit number，用來在 TCP/IP Model 的 Layer 3 識別 host。  
> IP addresses（無論 IPv4 或 IPv6）用來將 message 定址到其 final intended recipient；這不同於 MAC addresses，MAC addresses 用來將 message 定址到 next hop。  
> Switches 被稱為 Layer 2 devices；routers 則被稱為 Layer 3 devices，或說它們 operate at Layer 3，因為它們根據 messages 的 destination IP address（位於 Layer 3 header）做 forwarding decisions。

NOTE In this chapter, we will look at how to configure IPv4 addresses on routers, but we will cover how routers forward packets in part 2 of this book.

> [!translation] 逐句繁體中文翻譯
> 注意：在本章中，我們會看如何在 routers 上 configure IPv4 addresses，但 routers 如何 forward packets 會在本書 part 2 介紹。

### 7.3.1 The structure of an IPv4 address

IPv4 addresses are 32 bits in length, but a 32-bit string of 1s and 0s isn't very humanreadable or easy to remember. To make them easier to read, IPv4 addresses are represented using decimal numbers instead of binary. To simplify it even further, we first split the 32-bit IPv4 address into four groups of 8 bits called octets, separated by a period, and then convert each of the octets to decimal; this is called dotted decimal notation. This is why, for the purpose of the CCNA, you only need to be able to convert between binary and decimal for numbers of up to 8 bits. Figure 7.7 shows an IPv4 address written in dotted decimal as well as in binary.

> [!translation] 逐句繁體中文翻譯
> IPv4 addresses 長度為 32 bits，但一串 32-bit 的 1s 與 0s 並不容易 human-readable，也不容易記住。  
> 為了讓它們更容易閱讀，IPv4 addresses 使用 decimal numbers 表示，而不是 binary。  
> 為了進一步簡化，我們先將 32-bit IPv4 address 分成四組 8 bits，稱為 octets，並用 period 分隔，然後將每個 octet 轉成 decimal；這稱為 dotted decimal notation。  
> 這就是為什麼以 CCNA 為目的，你只需要能在 binary 與 decimal 之間轉換最多 8 bits 的 numbers。  
> Figure 7.7 顯示一個同時以 dotted decimal 與 binary 書寫的 IPv4 address。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-123_261_1300_1144_318.jpg)
Figure 7.7 An IPv4 address written in both dotted decimal and binary. The 32-bit address is split into four octets consisting of 8 bits each. The address is divided into two parts: the network portion and the host portion. The prefix length indicates the size of the network portion in bits, and the remainder is the host portion.

> [!translation] 逐句繁體中文翻譯
> Figure 7.7：同時以 dotted decimal 與 binary 書寫的 IPv4 address。  
> 32-bit address 被分成四個 octets，每個 octet 含 8 bits。  
> Address 分為兩個 parts：network portion 與 host portion。  
> Prefix length 以 bits 表示 network portion 的大小，其餘部分是 host portion。

## Octet and byte

You may wonder what the difference is between an octet and a byte, both of which I have defined as a group of 8 bits. An octet always means 8 bits. However, a byte isn't necessarily 8 bits; a byte is the minimum unit of data that a computer can read from or write to at one time. This is almost always 8 bits, but in the past, there have been computers that use 6-, 7-, and 9-bit bytes. Therefore, the term octet is sometimes used instead to refer to a group of 8 bits. In the context of IPv4 addresses, octet is preferred.

> [!translation] 逐句繁體中文翻譯
> 你可能會想知道 octet 與 byte 有什麼差異，兩者我都定義為一組 8 bits。  
> Octet 永遠表示 8 bits。  
> 然而，byte 不一定是 8 bits；byte 是 computer 一次能 read from 或 write to 的 data 最小單位。  
> 這幾乎永遠是 8 bits，但過去曾有 computers 使用 6-bit、7-bit 與 9-bit bytes。  
> 因此，octet 這個 term 有時被用來明確表示一組 8 bits。  
> 在 IPv4 addresses 的 context 中，較偏好使用 octet。

## Prefix length

The size of the network portion of an IP address can be indicated with a prefix length in the format /X, where X is the number of bits in the network portion. In figure 7.7, the IPv4 address is followed by /24, indicating that the network portion of the address is 24 bits in length. From that, we can infer that the remaining 8 bits are the host portion.

> [!translation] 逐句繁體中文翻譯
> IP address 中 network portion 的大小，可以用 /X 格式的 prefix length 表示，其中 X 是 network portion 的 bits 數。  
> 在 figure 7.7 中，IPv4 address 後面接著 /24，表示 address 的 network portion 長度為 24 bits。  
> 由此可推論剩餘 8 bits 是 host portion。

NOTE The network portion of an IPv4 address is often called the prefix or network prefix.

> [!translation] 逐句繁體中文翻譯
> 注意：IPv4 address 的 network portion 通常稱為 prefix 或 network prefix。

All hosts in the same LAN as the host with IPv4 address 192.168.100.100 will share the same network portion; the first three octets of their IPv4 addresses will be the same (192.168.100). However, each host will have a unique host portion; the final octet will be unique. Some possible addresses of other hosts in the LAN could be 192.168.100.1, 192.168.100.178, 192.168.100.234, etc.

> [!translation] 逐句繁體中文翻譯
> 與 IPv4 address 192.168.100.100 的 host 位於同一 LAN 的所有 hosts，都會共用相同 network portion；它們 IPv4 addresses 的前三個 octets 會相同（192.168.100）。  
> 不過，每個 host 都會有唯一的 host portion；最後一個 octet 會是唯一的。  
> LAN 中其他 hosts 的可能 addresses 可能是 192.168.100.1、192.168.100.178、192.168.100.234 等。

Figure 7.8 shows two networks: LAN 1 and LAN 2. Notice that the IP address of each host in LAN 1 begins with 192.168.1, and the IP address of each host in LAN 2 begins with 192.168.2. The router (R1) serves to connect the two LANs; its G0/0 interface has IP address 192.168.1.1/24, and its G0/1 interface has IP address 192.168.2.1/24. Hosts in the separate LANs can communicate with each other via R1. Notice that the switches do not have IP addresses-this is because switches are not Layer 3 aware. Switches operate at Layer 2 of the TCP/IP model and do not get involved with Layer 3.

> [!translation] 逐句繁體中文翻譯
> Figure 7.8 顯示兩個 networks：LAN 1 與 LAN 2。  
> 請注意，LAN 1 中每個 host 的 IP address 都以 192.168.1 開頭，而 LAN 2 中每個 host 的 IP address 都以 192.168.2 開頭。  
> Router（R1）用來連接這兩個 LANs；其 G0/0 interface 的 IP address 是 192.168.1.1/24，G0/1 interface 的 IP address 是 192.168.2.1/24。  
> 位於不同 LANs 的 hosts 可以透過 R1 彼此溝通。  
> 請注意，switches 沒有 IP addresses；這是因為 switches 不具備 Layer 3 awareness。  
> Switches operate at TCP/IP model 的 Layer 2，不會參與 Layer 3。

NOTE When talking about routers, the term interface is typically used instead of port.

> [!translation] 逐句繁體中文翻譯
> 注意：談到 routers 時，通常使用 interface 這個 term，而不是 port。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-124_445_1374_1275_225.jpg)
Figure 7.8 Two networks (LAN 1 and LAN 2) connected via a router (R1). IP addresses of hosts in each LAN share the same network portion: 192.168.1 in LAN 1 and 192.168.2 in LAN 2.

> [!translation] 逐句繁體中文翻譯
> Figure 7.8：兩個 networks（LAN 1 與 LAN 2）透過 router（R1）連接。  
> 每個 LAN 中 hosts 的 IP addresses 共用相同 network portion：LAN 1 為 192.168.1，LAN 2 為 192.168.2。

NOTE The exact meaning of the term network can vary. You could say figure 7.8 depicts a network consisting of two LANs. However, in the context of IP addresses and prefix lengths, you can think of a network as being synonymous with a LAN-a group of devices that can communicate directly with each other, without the use of a router.

> [!translation] 逐句繁體中文翻譯
> 注意：network 這個 term 的精確意思可能不同。  
> 你可以說 figure 7.8 描繪了一個由兩個 LANs 組成的 network。  
> 不過，在 IP addresses 與 prefix lengths 的 context 中，可以把 network 視為與 LAN 同義，也就是一群不使用 router 就能彼此直接溝通的 devices。

## Netmasks

Instead of indicating the prefix length with /X, another common method is to use a netmask-another string of 32 bits that is paired with an IP address to indicate which bits of the IP address are the network portion and which are the host portion. A bit in the netmask that is set to 1 means the bit in the same position of the IP address is part of the network portion; a bit in the netmask that is set to 0 means the bit in the same position of the IP address is part of the host portion.

> [!translation] 逐句繁體中文翻譯
> 除了用 /X 表示 prefix length，另一個常見方法是使用 netmask，也就是與 IP address 配對的另一串 32 bits，用來表示 IP address 的哪些 bits 是 network portion、哪些是 host portion。  
> Netmask 中 set to 1 的 bit 表示 IP address 中相同位置的 bit 屬於 network portion。  
> Netmask 中 set to 0 的 bit 表示 IP address 中相同位置的 bit 屬於 host portion。

Like IPv4 addresses, netmasks are usually written in dotted decimal notation. Figure 7.9 shows an IPv4 address (172.16.20.21) with a netmask (255.255.0.0). The first 16 bits of the netmask are 1, meaning the first 16 bits of the IPv4 address are the network portion.

> [!translation] 逐句繁體中文翻譯
> 和 IPv4 addresses 一樣，netmasks 通常以 dotted decimal notation 書寫。  
> Figure 7.9 顯示一個 IPv4 address（172.16.20.21）及其 netmask（255.255.0.0）。  
> Netmask 的前 16 bits 是 1，表示 IPv4 address 的前 16 bits 是 network portion。

| Network portion |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| IP address | 172 |  |  |  |  |  | 16 |  |  |  |  |  |  |  | 20 |  |  |  |  |  |  |  | 21 |  |  |  |  |  |
|  | 1 | 0 | 1 | 1 | 0 | 0 | 0 |  | 0 | 0 | 0 |  | 0 | 0 | 0 | 0 | 1 |  | 0 | 1 | 0 | 0 | 0 | 0 | 1 | 1 | 0 | 1 |
| Netmask | 255 |  |  |  |  |  | 255 |  |  |  |  |  |  |  | 0 |  |  |  |  |  |  |  | 0 |  |  |  |  |  |
|  | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 0 | 0 | 0 | 0 | 0 |  | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |

Figure 7.9 An IPv4 address (top) and its netmask (bottom). The first 16 bits of the netmask are set to 1, indicating that the first 16 bits of the IPv4 address are the network portion. This is equivalent to a /16 prefix length.

> [!translation] 逐句繁體中文翻譯
> Figure 7.9：IPv4 address（上方）與它的 netmask（下方）。  
> Netmask 的前 16 bits 被 set to 1，表示 IPv4 address 的前 16 bits 是 network portion。  
> 這等同於 /16 prefix length。

NOTE A netmask is often called a subnet mask; we will cover the topic of subnets in chapter 11.

> [!translation] 逐句繁體中文翻譯
> 注意：netmask 通常也稱為 subnet mask；我們會在 chapter 11 介紹 subnets 這個 topic。

For the CCNA, you should be familiar with both methods of indicating the length of the network portion of an IPv4 address: using /X notation and using a netmask. I will usually use /X notation because it's simpler, but as you'll see later in this chapter, configuring IPv4 addresses in Cisco IOS requires you to use netmasks. The following are some prefix lengths and their equivalent netmasks for comparison:

> [!translation] 逐句繁體中文翻譯
> 對 CCNA 來說，你應該熟悉表示 IPv4 address network portion 長度的兩種方法：使用 /X notation，以及使用 netmask。  
> 我通常會使用 /X notation，因為它比較簡單。  
> 但如本章稍後會看到的，在 Cisco IOS 中 configure IPv4 addresses 時，必須使用 netmasks。  
> 以下列出一些 prefix lengths 與其 equivalent netmasks 供比較：

- Prefix length: $/ 8=$ netmask: 255.0.0.0
- Prefix length: $/ \mathrm{l} 6=$ netmask: 255.255.0.0
- Prefix length: /24 = netmask: 255.255.255.0

NOTE A netmask is always a series of 1s followed by a series of 0s; this is because IPv4 addresses are always structured to have the network portion on the left (the most significant bits) and the host portion on the right (the least significant bits). Netmasks like 0.0.0.255 or 255.0.255.0 are not possible.

> [!translation] 逐句繁體中文翻譯
> 注意：netmask 永遠是一串 1s 後面接一串 0s。  
> 這是因為 IPv4 addresses 的結構永遠是 network portion 在左側（most significant bits），host portion 在右側（least significant bits）。  
> 像 0.0.0.255 或 255.0.255.0 這樣的 netmasks 是不可能的。

### 7.3.2 Configuring IPv4 addresses on a router

Unlike MAC addresses, which are assigned to a device by its manufacturer, IP addresses must be assigned by the engineer or admin configuring the device. Let's look at how to configure IP addresses on a Cisco router.

> [!translation] 逐句繁體中文翻譯
> 不同於由 manufacturer 指派給 device 的 MAC addresses，IP addresses 必須由 configure device 的 engineer 或 admin 指派。  
> 接著來看如何在 Cisco router 上 configure IP addresses。

NOTE End hosts like PCs usually receive their IP addresses automatically using Dynamic Host Configuration Protocol (DHCP), the topic of chapter 4 of volume 2. However, the IP addresses of network infrastructure devices like routers are usually manually configured.

> [!translation] 逐句繁體中文翻譯
> 注意：像 PCs 這類 end hosts 通常會使用 Dynamic Host Configuration Protocol（DHCP）自動取得 IP addresses；這是 volume 2 chapter 4 的 topic。  
> 不過，像 routers 這類 network infrastructure devices 的 IP addresses 通常會手動設定。

Figure 7.10 zooms in on R1 from figure 7.8 and shows how to configure IP addresses on and enable R1's G0/0 and G0/1 interfaces. In the rest of this section, we will analyze these configurations and use show commands to verify the status of R1's interfaces before and after configuration. Here are the basic steps:

> [!translation] 逐句繁體中文翻譯
> Figure 7.10 放大 figure 7.8 中的 R1，並顯示如何在 R1 的 G0/0 與 G0/1 interfaces 上 configure IP addresses 並啟用它們。  
> 在本節剩餘部分，我們會分析這些 configurations，並使用 show commands 驗證 R1 interfaces 在 configuration 前後的 status。  
> 以下是 basic steps：

1 From user EXEC mode, move to privileged EXEC mode and then global configuration mode.
2 Access interface configuration mode for the G0/0 interface, configure an IP address and netmask, and enable the interface.
3 Access interface configuration mode for the G0/1 interface, configure an IP address and netmask, and enable the interface.

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-126_632_1022_1165_346.jpg)
Figure 7.10 How to configure IP addresses and enable router interfaces. R1 is connected to two LANs: 192.168.1.1/24 (G0/0) and 192.168.2.1/24 (G0/1).

NOTE The switches and PCs present in figure 7.8 have been replaced with perpendicular lines at the end of the connections in figure 7.10. This is a common technique in network diagrams to indicate that a LAN is connected to an interface, but its details are not important to the diagram. Figure 7.10 focuses on R1, so there is no need to show the switches and PCs.

> [!translation] 逐句繁體中文翻譯
> 注意：figure 7.8 中出現的 switches 與 PCs，在 figure 7.10 中被連線末端的垂直線取代。  
> 這是 network diagrams 中常見的技巧，用來表示某個 LAN 連接到 interface，但該 LAN 的細節對此圖不重要。  
> Figure 7.10 聚焦於 R1，所以不需要顯示 switches 與 PCs。

## Preverification

Let's confirm the default state of R1's interfaces before we configure them-before configuring a device, it's best to confirm the device's current state. A convenient command to view a router's interfaces is show ip interface brief, executed in user EXEC or privileged EXEC mode. You will be using this command a lot! The following example shows the output of the command on R1 before configuring its interfaces:

> [!translation] 逐句繁體中文翻譯
> 在 configure R1 interfaces 之前，先確認它們的 default state；在 configure device 前，最好先確認 device 目前的狀態。  
> 查看 router interfaces 的方便 command 是 show ip interface brief，可在 user EXEC 或 privileged EXEC mode 中執行。  
> 你會很常用到這個 command！  
> 下列範例顯示在 configure R1 interfaces 之前，於 R1 上執行此 command 的 output：

```
Views information
Views information
        about R1’s interfaces
        about R1’s interfaces
R1# show ip interface brief
Interface IP-Address OK? Method Status Protocol
GigabitEthernet0/0 unassigned YES unset administratively down down
GigabitEthernet0/1 unassigned YES unset administratively down down
GigabitEthernet0/2 unassigned YES unset administratively down down
GigabitEthernet0/3 unassigned YES unset administratively down down
    R1’s four interfaces are listed.
```

The Interface column lists R1's interfaces-it has four, and we will configure two of them. The IP-Address column will list the IP address of each interface after we have configured them, but currently it just states unassigned.

> [!translation] 逐句繁體中文翻譯
> Interface column 列出 R1 的 interfaces；它有四個，我們會 configure 其中兩個。  
> IP-Address column 會在我們 configure 完後列出每個 interface 的 IP address，但目前只顯示 unassigned。

The Status column lists the physical status of each interface. If the interface is connected to another device, the status will be up; if it isn't, the Status will be down, and if the interface is manually disabled, it will be administratively down (regardless of whether it is connected to another device). As shown earlier, the default state is administratively down-Cisco router interfaces are disabled by default and must be manually enabled.

> [!translation] 逐句繁體中文翻譯
> Status column 列出每個 interface 的 physical status。  
> 如果 interface 連接到另一個 device，status 會是 up；如果沒有連接，Status 會是 down。  
> 如果 interface 被手動停用，它會是 administratively down（不論它是否連接到另一個 device）。  
> 如前所示，default state 是 administratively down；Cisco router interfaces 預設為 disabled，必須手動啟用。

The Protocol column indicates whether the Layer 2 protocol of the interface is functioning properly. For an Ethernet interface, this is fairly simple-if the Status column says up, the Protocol should be up as well. If the Status column says down or administratively down, the Protocol should be down.

> [!translation] 逐句繁體中文翻譯
> Protocol column 表示 interface 的 Layer 2 protocol 是否正常運作。  
> 對 Ethernet interface 來說，這相當簡單；如果 Status column 顯示 up，Protocol 也應該是 up。  
> 如果 Status column 顯示 down 或 administratively down，Protocol 應該是 down。

## Configuration

To configure a device's interfaces, we must use a new mode in the hierarchy of the IOS CLI: interface configuration mode. To access interface configuration mode, use the interface interface-name command from the global configuration mode. The following example demonstrates this:
![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-128_270_1284_190_346.jpg)

> [!translation] 逐句繁體中文翻譯
> 若要 configure device 的 interfaces，我們必須使用 IOS CLI hierarchy 中的一個新 mode：interface configuration mode。  
> 若要進入 interface configuration mode，請在 global configuration mode 中使用 interface interface-name command。  
> 下列範例示範這件事：

Notice that the prompt changes from R1 (config) \# to R1 (config-if) \#, indicating interface configuration mode. The name of the interface you are configuring isn't shown in the prompt, so before doing any configurations, I recommend doublechecking that you used the correct interface name after the interface command.

> [!translation] 逐句繁體中文翻譯
> 請注意，prompt 從 R1 (config) \# 變成 R1 (config-if) \#，表示目前是 interface configuration mode。  
> 你正在 configure 的 interface name 不會顯示在 prompt 中，所以在進行任何 configurations 前，我建議再次確認 interface command 後面使用的是正確 interface name。

> NOTE Instead of using the interface gigabitethernet0/0 command to enter interface configuration mode for the G0/0 interface, you can use interface g0/0-there is no need to type out the full interface name.

The command to configure an interface's IP address is ip address ip-address netmask; as I mentioned previously, you need to know netmasks when configuring IP addresses in Cisco IOS. In the following example, I configure the IP address of R1's G0/0 interface. The netmask is 255.255.255.0 because the prefix length is /24-the first 24 bits of the netmask are set to 1, and the last 8 are set to 0:
![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-128_101_1199_1121_348.jpg)

> [!translation] 逐句繁體中文翻譯
> Configure interface IP address 的 command 是 ip address ip-address netmask。  
> 如前所述，在 Cisco IOS 中 configure IP addresses 時，你需要知道 netmasks。  
> 在下列範例中，我 configure R1 的 G0/0 interface 的 IP address。  
> Netmask 是 255.255.255.0，因為 prefix length 是 /24；netmask 的前 24 bits 被 set to 1，最後 8 bits 被 set to 0：

However, G0/0 still isn't ready to forward traffic; the interface is still disabled. To change that, you must use the no shutdown command, as shown in the following example. After issuing the command, two messages are displayed, indicating that the interface is up and running. The first message indicates that the interface is physically operational (the Status column of show ip interface brief), and the second message indicates that the Layer 2 protocol is operational (the Protocol column of show ip interface brief):
![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-128_247_1290_1617_346.jpg)

> [!translation] 逐句繁體中文翻譯
> 不過，G0/0 還沒準備好 forward traffic；該 interface 仍然是 disabled。  
> 若要改變這點，必須使用 no shutdown command，如下例所示。  
> 輸入 command 後，會顯示兩個 messages，表示 interface 已 up and running。  
> 第一個 message 表示 interface 在 physical 上可運作（show ip interface brief 的 Status column）。  
> 第二個 message 表示 Layer 2 protocol 可運作（show ip interface brief 的 Protocol column）：

NOTE As mentioned in chapter 5, no can be used in front of a command to remove it from the configuration. Router interfaces are disabled by default because they have the shutdown command applied to them; the no shutdown command removes it and therefore enables the interface.

> [!translation] 逐句繁體中文翻譯
> 注意：如 chapter 5 所述，no 可以放在 command 前面，用來將它從 configuration 中移除。  
> Router interfaces 預設為 disabled，因為它們套用了 shutdown command；no shutdown command 會移除它，因此啟用 interface。

R1's G0/0 interface now has an IP address and is enabled-it's ready to forward traffic. Next let's configure the G0/1 interface, as in the following example:
![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-129_248_1429_511_190.jpg)

> [!translation] 逐句繁體中文翻譯
> R1 的 G0/0 interface 現在已有 IP address 且已 enabled；它已準備好 forward traffic。  
> 接著依下列範例 configure G0/1 interface：

NOTE To access interface configuration mode for another interface, you don't have to return to global configuration mode; you can do it directly from interface configuration mode. Note that there is no indication that I switched from configuring G0/0 to G0/1-again, always double-check that you used the correct interface name after the interface command.

> [!translation] 逐句繁體中文翻譯
> 注意：若要進入另一個 interface 的 interface configuration mode，不必回到 global configuration mode；可以直接從 interface configuration mode 進入。  
> 請注意，沒有任何提示指出我已從 configuring G0/0 切換到 G0/1。  
> 再提醒一次，務必確認 interface command 後面使用的是正確 interface name。

## Final verification

R1's G0/0 and G0/1 are both configured and enabled. After configuration, it's always a good idea to verify that the configurations are correct. In the following example, I once again used the show ip interface brief command to verify:

> [!translation] 逐句繁體中文翻譯
> R1 的 G0/0 與 G0/1 都已 configured 並 enabled。  
> Configuration 完成後，驗證 configurations 是否正確一直都是好習慣。  
> 在下列範例中，我再次使用 show ip interface brief command 進行驗證：

```
R1# show ip interface brief
Interface IP-Address OK? Method Status Protocol
GigabitEthernet0/0 192.168.1.1 YES manual up up
GigabitEthernet0/1 192.168.2.1 YES manual up up
GigabitEthernet0/2 unassigned YES unset administratively down down
GigabitEthernet0/3 unassigned YES unset administratively down down
```

G0/0 and G0/1 each have an IP address and are up/up.

> [!translation] 逐句繁體中文翻譯
> G0/0 與 G0/1 各自都有 IP address，且狀態為 up/up。

Notice that $\mathrm{G} 0 / 0$ and $\mathrm{G} 0 / 1$ both have the correct IP addresses and are up in both the Status and Protocol columns. However, show ip interface brief doesn't display the netmask. To double-check that the netmask is correct, you can use the show ip interface [interface-name] command, as in the following example. Notice that, although you must use a netmask when configuring IP addresses, the prefix length is displayed as /X in the output of this command. This command shows a lot of output, so I am only including the first few lines of each interface:
![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-130_571_1241_183_345.jpg)

> [!translation] 逐句繁體中文翻譯
> 請注意，$\mathrm{G} 0 / 0$ 與 $\mathrm{G} 0 / 1$ 都有正確 IP addresses，且在 Status 與 Protocol columns 中都是 up。  
> 不過，show ip interface brief 不會顯示 netmask。  
> 若要再次確認 netmask 正確，可以使用 show ip interface [interface-name] command，如下例所示。  
> 請注意，雖然 configure IP addresses 時必須使用 netmask，但此 command 的 output 會以 /X 顯示 prefix length。  
> 這個 command 會顯示大量 output，所以我只列出每個 interface 的前幾行：

NOTE When stating the format of a command, keywords and arguments in square brackets are optional. The show ip interface command is valid on its own and shows information for all interfaces; however, show ip interface g0/0 limits the output to only the stated interface.

> [!translation] 逐句繁體中文翻譯
> 注意：說明 command 格式時，方括號中的 keywords 與 arguments 是 optional。  
> show ip interface command 本身就是 valid，並會顯示所有 interfaces 的資訊。  
> 不過，show ip interface g0/0 會將 output 限制為只顯示指定 interface。

In addition to the prefix length, there are two more things I would like to point out about the previous output, related to other topics covered in this chapter: first, the line Broadcast address is 255.255.255.255 indicates the IP address that will be used to send a message to all hosts in the local network: 255.255.255.255. This is a specially reserved IP address for broadcast packets. If R1 wants to send a message to all hosts in LAN 1, it will send a packet addressed to 255.255.255.255 out of its G0/0 interface (encapsulated in a frame addressed to the MAC address ffff.ffff.ffff).

> [!translation] 逐句繁體中文翻譯
> 除了 prefix length，我還想指出前述 output 中另外兩件與本章其他 topics 相關的事。  
> 第一，Broadcast address is 255.255.255.255 這一行表示用來傳送 message 給 local network 中所有 hosts 的 IP address：255.255.255.255。  
> 這是為 broadcast packets 特別保留的 IP address。  
> 如果 R1 想傳送 message 給 LAN 1 中所有 hosts，它會從 G0/0 interface 送出 addressed to 255.255.255.255 的 packet（並 encapsulated in addressed to MAC address ffff.ffff.ffff 的 frame）。

Second, the final line of the included output states MTU is 1500 bytes. As mentioned when we looked at the IPv4 header, this means that if R1 has to forward a frame larger than 1,500 bytes out of either of its interfaces, it must fragment the packet first.

> [!translation] 逐句繁體中文翻譯
> 第二，所列 output 的最後一行寫著 MTU is 1500 bytes。  
> 如先前查看 IPv4 header 時所述，這表示如果 R1 必須從任一 interface forward 大於 1,500 bytes 的 frame，它必須先 fragment 該 packet。

NOTE After verifying that the configurations are correct, it's always a good idea to save the configuration with one of the commands covered in chapter 5: write,

> [!translation] 逐句繁體中文翻譯
> 注意：確認 configurations 正確後，使用 chapter 5 介紹的其中一個 command 儲存 configuration 一直都是好習慣，例如 write。

```
write memory, or copy running-config startup-config.
```

R1 is now ready to forward traffic between LAN 1 and LAN 2. Figure 7.11 shows how PC1 can send a packet to PC3 via R1; PC1 sends the packet in a frame addressed to the MAC address of R1's G0/0 interface, and then R1 forwards the packet in a frame addressed to the MAC address of PC3. Remember that Layer 3 provides end-to-end delivery, and Layer 2 provides hop-to-hop delivery. Although not shown in the diagram, before PC1 can encapsulate the packet in a frame, it must use ARP to learn R1 G0/0's MAC address. Likewise, R1 must use ARP to learn PC3's MAC address.

> [!translation] 逐句繁體中文翻譯
> R1 現在已準備好在 LAN 1 與 LAN 2 之間 forward traffic。  
> Figure 7.11 顯示 PC1 如何透過 R1 傳送 packet 給 PC3；PC1 會把 packet 放進 addressed to R1 G0/0 interface MAC address 的 frame 中傳送，接著 R1 會把 packet 放進 addressed to PC3 MAC address 的 frame 中 forward。  
> 請記住，Layer 3 提供 end-to-end delivery，而 Layer 2 提供 hop-to-hop delivery。  
> 雖然圖中未顯示，但 PC1 在把 packet encapsulate 到 frame 之前，必須使用 ARP 學習 R1 G0/0 的 MAC address。  
> 同樣地，R1 也必須使用 ARP 學習 PC3 的 MAC address。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-131_527_1414_181_192.jpg)
Figure 7.11 PC1 sends a packet to PC3 via R1. The packet is addressed to PC3's IP address. (1) PC1 sends the packet in a frame addressed to the MAC address of R1's G0/0 interface. (2) R1 forwards the packet in a frame addressed to the MAC address of PC3. For R1 to serve its purpose of connecting the two networks together, it must have an appropriate IP address on each of its interfaces, which we configured in this section.

NOTE Figure 7.11 indicates the IP address of each host differently than in previous diagrams. The network address (covered in section 7.3.3) is written next to each LAN's name, and only the host portion of each host's IP address is written next to the host. This is a common technique to reduce the amount of text in a diagram. PC1 and PC3 both have . 2 written next to them, but their IP addresses are not the same; the network portion of PC1's IP address is 192.168.1, and PC3's is 192.168.2. The same logic applies for PC2 and PC4.

> [!translation] 逐句繁體中文翻譯
> 注意：Figure 7.11 標示每個 host IP address 的方式與先前 diagrams 不同。  
> Network address（section 7.3.3 會介紹）寫在每個 LAN name 旁邊，而每個 host 旁邊只寫出其 IP address 的 host portion。  
> 這是減少 diagram 文字量的常見技巧。  
> PC1 與 PC3 旁邊都寫著 .2，但它們的 IP addresses 並不相同；PC1 IP address 的 network portion 是 192.168.1，PC3 的則是 192.168.2。  
> 同樣邏輯也適用於 PC2 與 PC4。

For a router to forward packets to remote networks (that aren't directly connected to the router itself), additional configurations are required; we will cover those configurations in later chapters of this volume. However, in the example network shown in figure 7.11, LAN 1 and LAN 2 are both directly connected to R1-no more configurations are needed. PC1 and PC2 in LAN 1 can now communicate with PC3 and PC4 in LAN 2 via R1.

> [!translation] 逐句繁體中文翻譯
> 若 router 要將 packets forward 到 remote networks（也就是未直接連到 router 本身的 networks），需要額外 configurations；本卷後續章節會介紹這些 configurations。  
> 不過，在 figure 7.11 所示的 example network 中，LAN 1 與 LAN 2 都直接連到 R1，因此不需要更多 configurations。  
> LAN 1 中的 PC1 與 PC2 現在可以透過 R1 與 LAN 2 中的 PC3 與 PC4 溝通。

### 7.3.3 Attributes of an IPv4 network

Each IPv4 network has a few attributes you should be able to identify: the network address, broadcast address, maximum number of hosts, first usable address, and last usable address of the network.

> [!translation] 逐句繁體中文翻譯
> 每個 IPv4 network 都有幾個你應該能識別的 attributes：network address、broadcast address、maximum number of hosts、first usable address，以及 last usable address。

## Network address

The network address is the first address of any network, and it is used to identify the network; it cannot be assigned to a host. An IPv4 address is a network address if all bits of its host portion are set to 0. Figure 7.12 shows an example of a network address: 192.168.100.0/24.

> [!translation] 逐句繁體中文翻譯
> Network address 是任何 network 的第一個 address，用來識別該 network；它不能被 assigned to host。  
> 如果某個 IPv4 address 的 host portion 所有 bits 都 set to 0，它就是 network address。  
> Figure 7.12 顯示 network address 的例子：192.168.100.0/24。

|  | Network portion |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| IP address | 192 |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 100 |  |  |  |  |  |  |  | 0 |  |  |  |  |  |  |  |
|  | 1 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 1 | 1 | 0 | 1 | 0 | 0 | 0 | 0 | 1 | 1 | 0 | 0 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| Netmask | 255 |  |  |  |  |  |  |  | 255 |  |  |  |  |  |  | 255 |  |  |  |  |  |  |  | 0 |  |  |  |  |  |  |  |
|  | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |

Figure 7.12 192.168.100.0 is a network address, as indicated by the host portion of 00000000. This address is used to identify the 192.168.100.0/24 network as a whole and cannot be assigned to a host. 192.168.100.100 (used in figure 7.7) is a host address in the 192.168.100.0/24 network.

> [!translation] 逐句繁體中文翻譯
> Figure 7.12：192.168.100.0 是 network address，host portion 00000000 顯示了這一點。  
> 這個 address 用來識別整個 192.168.100.0/24 network，不能 assigned to host。  
> 192.168.100.100（figure 7.7 使用的 address）是 192.168.100.0/24 network 中的 host address。

## Broadcast address

The broadcast address is the last address of any network, and like the network address, it can't be assigned to a host. The broadcast address can be used to address a message to all hosts in the local network. An IPv4 address is a broadcast address if all bits of its host portion are set to 1. Figure 7.13 shows the broadcast address of the 192.168.100.0/24 network.

> [!translation] 逐句繁體中文翻譯
> Broadcast address 是任何 network 的最後一個 address，而且和 network address 一樣，不能 assigned to host。  
> Broadcast address 可用來將 message 定址給 local network 中所有 hosts。  
> 如果某個 IPv4 address 的 host portion 所有 bits 都 set to 1，它就是 broadcast address。  
> Figure 7.13 顯示 192.168.100.0/24 network 的 broadcast address。

|  | Network portion |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| IP address | 192 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 100 |  |  |  |  |  |  |  |  | 255 |  |  |  |  |  |  |  |
|  | 1 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 1 | 1 | 0 | 1 | 0 |  | 0 | 0 | 0 | 1 | 1 | 0 |  | 0 | 1 | 0 | 0 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |
| Netmask | 255 |  |  |  |  |  |  |  | 255 |  |  |  |  |  |  |  | 255 |  |  |  |  |  |  |  |  | 0 |  |  |  |  |  |  |  |
|  | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |  | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |

Figure 7.13 192.168.100.255 is a broadcast address, as indicated by the host portion of 11111111. This address can be used to address a message to all hosts in the 192.168.100.0/24 network.

> [!translation] 逐句繁體中文翻譯
> Figure 7.13：192.168.100.255 是 broadcast address，host portion 11111111 顯示了這一點。  
> 這個 address 可用來將 message 定址給 192.168.100.0/24 network 中所有 hosts。

NOTE To send a message to all devices on the local network, hosts will usually address messages to 255.255.255.255, rather than the broadcast address of their local network. 255.255.255.255 is a specially reserved broadcast address. However, the broadcast address 192.168.100.255 can be used by hosts in other networks to send a message to all hosts in the 192.168.100.0/24 network.

> [!translation] 逐句繁體中文翻譯
> 注意：若要傳送 message 給 local network 上所有 devices，hosts 通常會將 messages 定址到 255.255.255.255，而不是定址到其 local network 的 broadcast address。  
> 255.255.255.255 是特別保留的 broadcast address。  
> 不過，其他 networks 中的 hosts 可以使用 broadcast address 192.168.100.255，將 message 傳送給 192.168.100.0/24 network 中所有 hosts。

## Maximum number of hosts

The maximum number of hosts in a network is the number of IP addresses available to assign to hosts connected to the network. To calculate the total number of IP addresses in a network, the formula is $2^{\mathrm{y}}$, where $y$ is the number of host bits. For example, with a
/24 prefix length, there are eight host bits; $2^{8}$ is equal to 256, so there are 256 total IP addresses in a /24 network (such as 192.168.100.0/24).

> [!translation] 逐句繁體中文翻譯
> Network 的 maximum number of hosts，是可 assigned to 連接該 network 的 hosts 的 IP addresses 數量。  
> 若要計算 network 中 IP addresses 的總數，公式是 $2^{\mathrm{y}}$，其中 $y$ 是 host bits 的數量。  
> 例如，/24 prefix length 有 8 個 host bits；$2^{8}$ 等於 256，所以 /24 network（例如 192.168.100.0/24）共有 256 個 IP addresses。

However, because the network and broadcast addresses of each network can't be assigned to hosts, we have to subtract 2 from the total number of addresses in the network to find the maximum number of hosts. Therefore, the formula to determine the maximum number of hosts in a network is actually $2^{\mathrm{y}}-2$. For example, the maximum number of hosts of a /24 network is $254\left(2^{8}-2\right)$. The following are the maximum number of hosts in networks with /8, / 16, and / 24 prefix lengths:

> [!translation] 逐句繁體中文翻譯
> 然而，因為每個 network 的 network address 與 broadcast address 都不能 assigned to hosts，所以必須從 network address 總數中減去 2，才能得到 maximum number of hosts。  
> 因此，判斷 network 中 maximum number of hosts 的公式其實是 $2^{\mathrm{y}}-2$。  
> 例如，/24 network 的 maximum number of hosts 是 $254\left(2^{8}-2\right)$。  
> 以下是 /8、/16 與 /24 prefix lengths networks 中的 maximum number of hosts：

- $/ 8: 2^{24}-2=16,777,214$ hosts
- / $16: 2^{16}-2=65,534$ hosts
- $/ 24: 2^{8}-2=254$ hosts

## First and last usable addresses

The first usable address of a network is the first IP address that can be assigned to a host; in other words, it's the first IP address after the network address. It is simple to calculate-just add one to the network address (change the least significant bit to 1). Figure 7.14 shows the first usable address of the 192.168.100.0/24 network.

> [!translation] 逐句繁體中文翻譯
> Network 的 first usable address 是第一個可 assigned to host 的 IP address。  
> 換句話說，它是 network address 後面的第一個 IP address。  
> 它很容易計算；只要將 network address 加 1（把 least significant bit 改成 1）。  
> Figure 7.14 顯示 192.168.100.0/24 network 的 first usable address。

| Network portion |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| IP address | 192 |  |  |  |  |  | 168 |  |  |  |  |  |  | 100 |  |  |  |  |  |  |  |  | 1 |  |  |  |  |  |
|  | 1 | 1 | 0 | 0 | 0 | 0 | 1 |  | 0 | 1 | 0 | 0 | 0 | 1 | 1 | 0 | 0 |  | 1 | 0 | 0 |  | 0 |  | 0 | 0 | 0 | 0 |
| Netmask | 255 |  |  |  |  |  | 255 |  |  |  |  |  |  | 255 |  |  |  |  |  |  |  |  | 0 |  |  |  |  |  |
|  | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |  | 1 | 1 | 1 | 0 |  | 0 | 0 | 0 | O | 0 |

Figure 7.14 192.168.100.1 is the first usable address of the 192.168.100.0/24 network. It is the first address after the network address.

> [!translation] 逐句繁體中文翻譯
> Figure 7.14：192.168.100.1 是 192.168.100.0/24 network 的 first usable address。  
> 它是 network address 後面的第一個 address。

NOTE The first usable address of a network is often assigned to that network's router. For example, in the previous section, we assigned IP addresses 192.168.1.1 and 192.168.2.1 to R1s interfaces-the first usable addresses of their respective networks.

> [!translation] 逐句繁體中文翻譯
> 注意：network 的 first usable address 通常會 assigned to 該 network 的 router。  
> 例如，在前一節中，我們將 IP addresses 192.168.1.1 與 192.168.2.1 assigned to R1 的 interfaces；它們分別是各自 networks 的 first usable addresses。

The last usable address of a network is the last IP address that can be assigned to a host; it's the last IP address before the broadcast address. This address is also simple to find-subtract 1 from the broadcast address (change the least significant bit to 0). Figure 7.15 shows the last usable address of the 192.168.100.0/24 network.

> [!translation] 逐句繁體中文翻譯
> Network 的 last usable address 是最後一個可 assigned to host 的 IP address。  
> 它是 broadcast address 前面的最後一個 IP address。  
> 這個 address 也很容易找到；只要從 broadcast address 減 1（把 least significant bit 改成 0）。  
> Figure 7.15 顯示 192.168.100.0/24 network 的 last usable address。

|  | Network portion |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| IP address | 192 |  |  |  |  |  |  |  |  |  |  |  |  |  |  | 100 |  |  |  |  |  |  |  | 254 |  |  |  |  |  |  |  |
|  | 1 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 1 | 1 | 0 | 1 | 0 | 0 | 0 | 0 | 1 | 1 | 0 | 0 | 1 | 0 | 0 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 0 |
| Netmask | 255 |  |  |  |  |  |  |  | 255 |  |  |  |  |  |  | 255 |  |  |  |  |  |  |  | 0 |  |  |  |  |  |  |  |
|  | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |

Figure 7.15 192.168.100.254 is the last usable address of the 192.168.100.0/24 network. It is the last address before the broadcast address.

> [!translation] 逐句繁體中文翻譯
> Figure 7.15：192.168.100.254 是 192.168.100.0/24 network 的 last usable address。  
> 它是 broadcast address 前面的最後一個 address。

If you know the first and last usable addresses, you know the range of usable addresses: from the first usable address to the last usable address. For example, the range of usable addresses in the 192.168.100.0/24 network is from 192.168.100.1 to 192.168.100.254: 254 addresses in total.

> [!translation] 逐句繁體中文翻譯
> 如果你知道 first 與 last usable addresses，就知道 usable addresses 的 range：從 first usable address 到 last usable address。  
> 例如，192.168.100.0/24 network 中 usable addresses 的 range 是從 192.168.100.1 到 192.168.100.254，總共 254 個 addresses。

## Exam scenario

On the CCNA exam, you may be asked questions that require you to identify one or more of these attributes of a network. The following is an example question:

> [!translation] 逐句繁體中文翻譯
> 在 CCNA exam 中，你可能會遇到要求你識別 network 一個或多個 attributes 的 questions。  
> 以下是一個 example question：

Q: PC1's IP address is 172.20.20.127/16. What is the usable address range of the network PC1 belongs to?

> [!translation] 逐句繁體中文翻譯
> 問：PC1 的 IP address 是 172.20.20.127/16。  
> PC1 所屬 network 的 usable address range 是什麼？

A 172.20.20.1-172.20.20.254
B 172.20.20.0-172.20.20.255
c 172.20.0.1-172.20.255.254
D 172.20.0.0-172.20.255.255

> [!translation] 逐句繁體中文翻譯
> A 172.20.20.1-172.20.20.254  
> B 172.20.20.0-172.20.20.255  
> C 172.20.0.1-172.20.255.254  
> D 172.20.0.0-172.20.255.255

To find the usable address range of a network, you need to know the first and last usable addresses of the network. This is fairly simple when using a prefix length of /8, /16, or /24; the division between the network portion and host portion is between octets (in this case, between the second and third octets, because the prefix length is $/ 16$ ).

> [!translation] 逐句繁體中文翻譯
> 若要找出 network 的 usable address range，你需要知道該 network 的 first 與 last usable addresses。  
> 使用 /8、/16 或 /24 prefix length 時，這相當簡單；network portion 與 host portion 的分界位於 octets 之間。  
> 在本例中，分界位於第二與第三個 octets 之間，因為 prefix length 是 $/16$。

To find the first usable address, simply change the octet(s) of the host portion to 0 (this is the network address), and then add 1 to the last octet: PC1's address is 172.20.20.127, the network address is 172.20.0.0, and the first usable address is 172.20.0.1.

> [!translation] 逐句繁體中文翻譯
> 若要找出 first usable address，只要把 host portion 的 octet(s) 改成 0（這就是 network address），然後將最後一個 octet 加 1。  
> PC1 的 address 是 172.20.20.127，network address 是 172.20.0.0，first usable address 是 172.20.0.1。

To find the last usable address, change the octet(s) of the host portion to 255 (this is the broadcast address), and then subtract 1 from the last octet: PC1's address is 172.20.20.127, the broadcast address is 172.20.255.255, and the last usable address is 172.20.255.254. Now you know the usable address range: from 172.20.0.1 to 172.20.255.254. Therefore, the answer to this question is C.

> [!translation] 逐句繁體中文翻譯
> 若要找出 last usable address，將 host portion 的 octet(s) 改成 255（這就是 broadcast address），然後從最後一個 octet 減 1。  
> PC1 的 address 是 172.20.20.127，broadcast address 是 172.20.255.255，last usable address 是 172.20.255.254。  
> 現在你知道 usable address range：從 172.20.0.1 到 172.20.255.254。  
> 因此，此題答案是 C。

This process will become more challenging when we cover subnetting in chapter 11 of this book. When subnetting, we use prefix lengths that do not fit neatly between octets of an IP address, such as /19, /23, /28, etc. In that case, it is important to be proficient at converting between decimal and binary so you can identify the network and host bits, convert the host bits to 0 or 1 as necessary, convert them back to decimal, etc.

> [!translation] 逐句繁體中文翻譯
> 當本書 chapter 11 介紹 subnetting 時，這個 process 會變得更具挑戰。  
> Subnetting 時，我們會使用不剛好落在 IP address octets 之間的 prefix lengths，例如 /19、/23、/28 等。  
> 在那種情況下，熟練 decimal 與 binary 之間的轉換很重要，這樣你才能識別 network 與 host bits、必要時將 host bits 轉成 0 或 1、再轉回 decimal 等。

### 7.3.4 IPv4 address classes

Originally, all IPv4 addresses used a /8 prefix length; the first octet identified the network, and the last three octets identified the specific host within that network. However, that system was soon abandoned; because only the first 8 bits could be used to make different networks, there could only be $256\left(2^{8}\right)$ different networks (LANs): from 0.x.x.x to 255.x.x.x. In the modern world where the internet is ubiquitous, that is not nearly enough networks.

> [!translation] 逐句繁體中文翻譯
> 最初，所有 IPv4 addresses 都使用 /8 prefix length；第一個 octet 識別 network，最後三個 octets 識別該 network 內的 specific host。  
> 然而，這個 system 很快就被放棄。  
> 因為只有前 8 bits 可用來建立不同 networks，所以最多只能有 $256\left(2^{8}\right)$ 個不同 networks（LANs）：從 0.x.x.x 到 255.x.x.x。  
> 在 internet 無所不在的 modern world 中，這遠遠不夠。

NOTE The formula to calculate the number of available networks is $2^{\mathrm{x}}$, where $x$ is the number of bits in the network portion.

> [!translation] 逐句繁體中文翻譯
> 注意：計算 available networks 數量的公式是 $2^{\mathrm{x}}$，其中 $x$ 是 network portion 的 bits 數量。

To improve that system and allow for more networks of various sizes, IPv4 addresses were organized into five classes: class A, class B, class C, class D, and class E. Table 7.2 lists the five IPv4 address classes and some information about them.

> [!translation] 逐句繁體中文翻譯
> 為了改善該 system，並允許更多不同 sizes 的 networks，IPv4 addresses 被組織成五個 classes：class A、class B、class C、class D 與 class E。  
> Table 7.2 列出五個 IPv4 address classes 以及它們的一些資訊。

Table 7.2 IPv4 address classes
| Class | First octet bit pattern | First octet decimal range | Prefix length | Note |
| :--- | :--- | :--- | :--- | :--- |
| A | Oxxxxxxx | 0-127 | /8 | Address range: 0.0.0.0-127.255.255.255 |
| B | 10xxxxxx | 128-191 | /16 | Address range: 128.0.0.0-191.255.255.255 |
| C | 110xxxxx | 192-223 | /24 | Address range: 192.0.0.0 to 223.255.255.255 |
| D | 1110xxxx | 224-239 |  | Reserved for multicast addresses |
| E | 1111xxxx | 240-255 |  | Reserved for experimental purposes |

> [!translation] 逐句繁體中文翻譯
> Table 7.2：IPv4 address classes。  
> Class 欄位列出 A 到 E 五種 classes。  
> First octet bit pattern 欄位顯示每個 class 的第一個 octet binary pattern。  
> First octet decimal range 欄位顯示每個 class 的第一個 octet decimal range。  
> Prefix length 欄位顯示 class A、B、C 的預設 prefix length。  
> Note 欄位說明 address range，或指出 class D 保留給 multicast addresses、class E 保留給 experimental purposes。


The class of an IPv4 address is determined by the first 1 to 4 bits of the address; class A addresses begin with 0, class B addresses begin with 10, class C addresses begin with 110, class D addresses begin with 1110, and class E addresses begin with 1111. Classes A, B, and C are the ranges from which hosts are assigned IPv4 addresses. For example, the IP addresses we configured on R1 in this chapter are from the class C range. Classes D and E are reserved for particular purposes; we won't cover them in this book, except for a few mentions of multicast IP addresses (class D).

> [!translation] 逐句繁體中文翻譯
> IPv4 address 的 class 由 address 的前 1 到 4 bits 決定。  
> Class A addresses 以 0 開頭，class B addresses 以 10 開頭，class C addresses 以 110 開頭，class D addresses 以 1110 開頭，class E addresses 以 1111 開頭。  
> Classes A、B、C 是 hosts 會被 assigned IPv4 addresses 的 ranges。  
> 例如，本章中我們在 R1 上 configure 的 IP addresses 來自 class C range。  
> Classes D 與 E 保留給特定用途；除了少數提到 multicast IP addresses（class D）之外，本書不會介紹它們。

NOTE Some addresses in each class are reserved for special purposes and can't be assigned to hosts. For example, class A addresses with a first octet of 0 or 127 are reserved.

> [!translation] 逐句繁體中文翻譯
> 注意：每個 class 中都有一些 addresses 保留給特殊用途，不能 assigned to hosts。  
> 例如，first octet 為 0 或 127 的 class A addresses 是 reserved。

Classes A, B, and C each use a specific prefix length: class A addresses use a /8 prefix length (netmask 255.0.0.0), class B addresses use a /16 prefix length (netmask 255.255.0.0), and class C addresses use a /24 prefix length (netmask 255.255.255.0). Because an IPv4 address is always 32 bits in length, if the network portion is larger, the host portion is smaller (and vice versa). This gives some characteristics to each class:

> [!translation] 逐句繁體中文翻譯
> Classes A、B、C 各自使用特定 prefix length。  
> Class A addresses 使用 /8 prefix length（netmask 255.0.0.0），class B addresses 使用 /16 prefix length（netmask 255.255.0.0），class C addresses 使用 /24 prefix length（netmask 255.255.255.0）。  
> 因為 IPv4 address 永遠是 32 bits 長，如果 network portion 較大，host portion 就較小（反之亦然）。  
> 這賦予每個 class 一些特性：

- Few class A networks exist (128), but each class A network contains many addresses (16,777,216).
- Class B networks are a middle ground. There are 16,384 class B networks, each containing 65,536 addresses.
- Many class C networks exist (2,097,152), but each class C network contains relatively few addresses (256).

Figure 7.16 represents these characteristics visually. A larger network portion means a smaller host portion and vice versa. There is a tradeoff between the two.

> [!translation] 逐句繁體中文翻譯
> Figure 7.16 以視覺方式呈現這些特性。  
> 較大的 network portion 代表較小的 host portion，反之亦然。  
> 兩者之間存在 tradeoff。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-136_295_780_617_348.jpg)
Figure 7.16 The network portion and host portion sizes of class A, class B, and class C IPv4 addresses

Class A networks were intended for very large organizations such as Internet Service Providers and the United States Department of Defense (DoD); the vast majority of organizations don't need anywhere near 16,777,216 IP addresses. Class B networks were intended for medium- to large-sized businesses, and class C for small- to mediumsized businesses.

> [!translation] 逐句繁體中文翻譯
> Class A networks 原本設計給非常大型的 organizations，例如 Internet Service Providers 與 United States Department of Defense（DoD）。  
> 絕大多數 organizations 根本不需要接近 16,777,216 個 IP addresses。  
> Class B networks 原本設計給中大型 businesses，而 class C 則設計給中小型 businesses。

Table 7.3 summarizes the characteristics of classes A, B, and C. You don't have to memorize the number of networks and addresses per network for each address class; just understand that a smaller network portion means fewer networks with more hosts in each network, and a larger network portion means more networks with fewer hosts in each network.

> [!translation] 逐句繁體中文翻譯
> Table 7.3 摘要 classes A、B、C 的 characteristics。  
> 你不必背下每個 address class 的 networks 數量與每個 network 的 addresses 數量。  
> 只要理解較小的 network portion 代表 networks 較少但每個 network hosts 較多，而較大的 network portion 代表 networks 較多但每個 network hosts 較少。

Table 7.3 Characteristics of classes A, B, and C
| Class | First octet | Size of network portion | Size of host portion | Number of networks | Addresses per network |
| :--- | :--- | :--- | :--- | :--- | :--- |
| A | 0xxxxxxx | 8 bits | 24 bits | $128\left(2^{7}\right)$ | 16,777,216 $\left(2^{24}\right)$ |
| B | 10xxxxxx | 16 bits | 16 bits | 16,384 $\left(2^{14}\right)$ | $65,536\left(2^{16}\right)$ |
| C | 110xxxxx | 24 bits | 8 bits | 2,097,152 (2 ${ }^{21}$ ) | $256\left(2^{8}\right)$ |

> [!translation] 逐句繁體中文翻譯
> Table 7.3：Classes A、B、C 的 characteristics。  
> Class A 的 first octet pattern 是 0xxxxxxx，network portion 為 8 bits，host portion 為 24 bits。  
> Class B 的 first octet pattern 是 10xxxxxx，network portion 與 host portion 各為 16 bits。  
> Class C 的 first octet pattern 是 110xxxxx，network portion 為 24 bits，host portion 為 8 bits。  
> 表格也比較每個 class 的 networks 數量與每個 network 可用的 addresses 數量。


NOTE The reason why there are only $2^{7}$ class A networks, even though the network portion is 8 bits in length, is that the first bit is fixed as 0-only 7 bits are available to change and make different networks. The same reasoning applies for why there are $2^{14}$ class B networks (not $2^{16}$ ) and $2^{21}$ class C networks (not $2^{24}$ ).

> [!translation] 逐句繁體中文翻譯
> 注意：雖然 class A 的 network portion 長度是 8 bits，但只有 $2^{7}$ 個 class A networks，原因是第一個 bit 固定為 0，只有 7 bits 可用來變化並建立不同 networks。  
> 同樣邏輯也解釋了為什麼 class B networks 是 $2^{14}$ 個（不是 $2^{16}$），class C networks 是 $2^{21}$ 個（不是 $2^{24}$）。

Networks that follow the class A, B, and C rules are called classful networks. Although important to study and understand even today, this system is now obsolete and has been replaced with classless networking, a system in which prefix lengths are not restricted by class. We will cover this in chapter 11 when we look at subnetting.

> [!translation] 逐句繁體中文翻譯
> 遵循 class A、B、C rules 的 networks 稱為 classful networks。  
> 雖然即使在今天仍重要且值得學習理解，但這個 system 現在已 obsolete，並被 classless networking 取代。  
> Classless networking 是 prefix lengths 不受 class 限制的 system。  
> 我們會在 chapter 11 介紹 subnetting 時涵蓋這個內容。

## Reserved addresses

Within each address class, there are several ranges of IP addresses that are reserved and cannot be assigned to hosts. Here are two examples:

> [!translation] 逐句繁體中文翻譯
> 在每個 address class 中，都有幾個 IP address ranges 是 reserved，不能 assigned to hosts。  
> 以下是兩個 examples：

- 0.0.0.0/8: Any IP address that begins with the first octet 0 is reserved.
- 127.0.0.0/8: This range is reserved for loopback addresses. A message sent to any IP address in this range (i.e., ping 127.0.0.1) will be looped back to the local host-the device you are working on-without being transmitted over the network. This can be used to test the networking software on the local device.

## Summary

- The IPv4 header is 20 to 60 bytes in length and contains 14 fields.
- The Version field indicates the version of IP (IPv4 or IPv6).
- The Internet Header Length (IHL) field indicates the length of the header in 4-byte increments.
- The Differentiated Services Code Point (DSCP) and Explicit Congestion Notification (ECN) fields are used to prioritize certain kinds of traffic. This is called Quality of Service (QoS).
- The Total Length field indicates the length of the entire packet in bytes.
- The Identification, Flags, and Fragment Offset fields support packet fragmentation. If a packet is larger than an interface's Maximum Transmission Unit (MTU), the router will divide the packet into multiple smaller packets called fragments. The standard MTU is 1500 bytes.
- The Time To Live (TTL) field is used to prevent packets from looping indefinitely around the network. Each time a router forwards a packet, its TTL is decremented by 1 , and if it reaches 0 , the packet is dropped.
- The Protocol field indicates the type of message encapsulated inside of the packet, such as ICMP, TCP, UDP, or OSPF.
- The Header Checksum field is used to check for errors in the IPv4 header.
- The Source Address field contains the IPv4 address of the host that sent the packet.
- The Destination Address field contains the IPv4 address of the packet's intended recipient.
- The Options field is optional and variable in length-from 0 bytes (if not used) to a maximum of 40 bytes in length. This field is rarely used.

- The decimal number system uses 10 digits: 0, 1, 2, 3, 4, 5, 6, 7, 8, and 9. It is also called base 10. The value of each digit position increases tenfold: 1, 10, 100, 1000, etc.
- The binary number system uses two digits: 0 and 1. It is also called base 2. The value of each digit position increases twofold: 1, 2, 4, 8, 16, 32, 64, 128, etc.
- An 8-bit binary number provides 256 possible values: from 0d0 (00000000) to 0d255 (11111111).
- For the CCNA exam, you must be able to convert between binary and decimal for numbers of up to 8 bits in length. You can practice at https://learningnetwork .cisco.com/s/binary-game.
- An IPv4 address is a 32-bit number that identifies a host at Layer 3. It is divided into four groups of 8 bits called octets and written in dotted decimal notation.
- IPv4 addresses are divided into two parts: the network portion and the host portion. All hosts within a LAN will have the same network portion but a unique host portion.
- The size of the network portion can be indicated with a prefix length in the format /X, where X is the number of bits in the network portion. Any bits that are not part of the network portion are part of the host portion.
- The size of the network portion can also be indicated with a netmask (also called a subnet mask). A netmask is a string of 32 bits that is paired with an IP address to indicate which bits of the IP address are the network portion and which are the host portion.
- A 1 in the netmask means the bit in the same position as the IP address is part of the network portion. A 0 in the netmask means the bit in the same position as the IP address is part of the host portion.
- The show ip interface brief command lists a router's interface and information about their IP addresses and status.
- The show ip interface [interface-name] command shows more detail about each interface.
- Router interfaces are disabled by default and must be enabled with the no shutdown command.
- Interface configuration mode can be accessed with the interface interface -name command from global configuration mode.
- An interface's IPv4 address can be configured with the ip address ip-address netmask command in interface configuration mode.
- The network address of a network is the first address of the network, with a host portion of all 0s. It is used to identify the network and cannot be assigned to a host.
- The broadcast address of a network is the last address of the network, with a host portion of all 1s. It can be used to send a message to all hosts in the network.

However, to send a message to all hosts on the local network, the address 255.255.255.255 is usually used.
- The maximum number of hosts of a network is the number of IP addresses that can be assigned to hosts. The formula is $2^{\mathrm{y}}-2$, where $y$ is the number of bits in the host portion. Two is subtracted for the network and broadcast addresses.
- The first usable address of a network is the first address that can be assigned to a host. The last usable address is the last address that can be assigned to a host.
- IPv4 addresses can be organized into five classes: A, B, C, D, and E. Class D is reserved for multicast addresses, and Class E is reserved for experimental purposes. Addresses from classes A, B, and C are assigned to network hosts.
- Class A addresses have a first octet of 0-127 and use a /8 prefix length. Class B addresses have a first octet of 128-191 and use a /16 prefix length. Class C addresses have a first octet of 192-223 and use a /24 prefix length.
- Networks that follow class A, B, and C rules are called classful networks. This system is now obsolete and has been replaced with classless networking, which is more flexible.

> [!translation] 逐句繁體中文翻譯
> 不過，若要傳送 message 給 local network 上所有 hosts，通常會使用 address 255.255.255.255。  
> Network 的 maximum number of hosts 是可 assigned to hosts 的 IP addresses 數量。  
> 公式是 $2^{\mathrm{y}}-2$，其中 $y$ 是 host portion 的 bits 數量。  
> 減去 2 是因為要扣除 network address 與 broadcast address。  
> Network 的 first usable address 是第一個可 assigned to host 的 address。  
> Last usable address 是最後一個可 assigned to host 的 address。  
> IPv4 addresses 可以被組織成五個 classes：A、B、C、D、E。  
> Class D 保留給 multicast addresses，Class E 保留給 experimental purposes。  
> Classes A、B、C 的 addresses 會 assigned to network hosts。  
> Class A addresses 的 first octet 是 0-127，並使用 /8 prefix length。  
> Class B addresses 的 first octet 是 128-191，並使用 /16 prefix length。  
> Class C addresses 的 first octet 是 192-223，並使用 /24 prefix length。  
> 遵循 class A、B、C rules 的 networks 稱為 classful networks。  
> 這個 system 現在已 obsolete，並被更有彈性的 classless networking 取代。
