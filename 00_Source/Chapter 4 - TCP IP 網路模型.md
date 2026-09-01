## Chapter 4 - The TCP/IP networking model

## This chapter covers

- What networking models are and why we need them
- The OSI model
- The TCP/IP model and its layers
- How each layer plays a role in moving data across a network
- Data encapsulation and de-encapsulation

In the previous chapter, we looked at Ethernet; specifically, we looked at the types of physical connections defined by the Ethernet standard. Ethernet also defines rules for how devices can communicate over those connections. However, Ethernet alone isn't sufficient for two computers to communicate over a network (e.g., for a PC to retrieve a web page from a server over the internet). Communicating over a network is a complex process, and it requires a variety of protocols, each of which performs specific functions and, when brought together, enables network communications.

> [!translation] 逐句繁體中文翻譯
> 在上一章中，我們看了 Ethernet；更具體地說，我們看了 Ethernet 標準所定義的各種實體連線類型。  
> Ethernet 也定義了設備如何透過這些連線進行通訊的規則。  
> 然而，只有 Ethernet 還不足以讓兩台電腦透過網路通訊，例如讓一台 PC 從網際網路上的伺服器取得網頁。  
> 透過網路通訊是一個複雜的過程，需要多種協定；每個協定各自執行特定功能，合在一起才讓網路通訊成為可能。

In this chapter, we will look at a couple of models that define the various functions required to enable computers to communicate over a network: the Open Systems Interconnection (OSI) model and the TCP/IP model (named after two key protocols of the
model: Transmission Control Protocol and Internet Protocol). TCP/IP is the model currently used by modern networks all over the world.

> [!translation] 逐句繁體中文翻譯
> 在本章中，我們會看幾個模型，這些模型定義了讓電腦能透過網路通訊所需的各種功能：Open Systems Interconnection（OSI）model，以及 TCP/IP model。  
> TCP/IP model 的名稱來自其中兩個關鍵協定：Transmission Control Protocol 與 Internet Protocol。  
> TCP/IP 是目前全世界現代網路所使用的模型。

Neither of these models is explicitly listed as a CCNA exam topic. However, the information in this chapter is fundamental networking knowledge. We will examine the functions of various network protocols throughout the two volumes of this book, so it's important to have a framework to understand it all. That's the role of these networking models-to provide a framework to organize the various functions that make a network work.

> [!translation] 逐句繁體中文翻譯
> 這兩個模型都沒有被明確列為 CCNA 考試主題。  
> 不過，本章的資訊是基礎網路知識。  
> 在本書兩冊中，我們會檢視各種網路協定的功能，因此有一個能理解整體內容的框架很重要。  
> 這正是 networking models 的角色：提供一個框架，用來組織讓網路能運作的各種功能。

The purpose of this chapter is to provide a high-level overview of how data travels from source to destination across a network. In the rest of this book, we will fill in the gaps regarding the exact mechanisms that make network communications possible, but first we need a framework.

> [!translation] 逐句繁體中文翻譯
> 本章的目的，是提供一個高層次概觀，說明資料如何透過網路從來源端傳到目的端。  
> 在本書後續內容中，我們會補上讓網路通訊得以發生的精確機制；但首先，我們需要一個框架。

### 4.1 Conceptual models of networking

Since the beginning of computer networking, there have been several attempts to create models that define the various functions necessary for computers to communicate with each other. Several of these models were vendor-proprietary, meaning they were created by a specific vendor (i.e., IBM) to be used by their products. However, the vendor-proprietary approach was not ideal; each vendor designed its own communication protocols, so enabling communication between different vendors' products was no simple task.

> [!translation] 逐句繁體中文翻譯
> 從電腦網路發展初期開始，人們就多次嘗試建立模型，用來定義電腦彼此通訊所需的各種功能。  
> 其中有幾個模型是廠商專有的，也就是由特定廠商，例如 IBM，建立並供自家產品使用。  
> 然而，廠商專有的方法並不理想；每個廠商都設計自己的通訊協定，因此要讓不同廠商的產品互相通訊並不是一件簡單的事。

DEFINITION A protocol is a set of rules defining how data should be communicated between devices in a network. Protocols can be thought of as the languages computers use to communicate; two computers using different networking protocols are like two humans speaking different languages-they won't be able to communicate.

> [!translation] 逐句繁體中文翻譯
> 定義：protocol 是一組規則，用來定義資料應如何在網路中的設備之間通訊。  
> Protocols 可以被想成電腦用來通訊的語言；兩台使用不同網路協定的電腦，就像兩個說不同語言的人一樣，無法溝通。

These days we all enjoy the benefits of the alternative approach: vendor neutral. In a vendor-neutral model, with vendor-neutral protocols that can be used by devices of all kinds, we don't have to worry about whether an Apple MacBook will be able to access a website hosted on a Linux web server or whether a PC running Windows will be able to send an email that can be read on a smartphone running Android.

> [!translation] 逐句繁體中文翻譯
> 現在，我們都享受到另一種方法的好處：vendor neutral，也就是廠商中立。  
> 在 vendor-neutral model 中，使用各種設備都能採用的 vendor-neutral protocols，我們就不必擔心 Apple MacBook 是否能存取 Linux web server 上的網站，也不必擔心 Windows PC 寄出的 email 是否能被 Android smartphone 讀取。

Networking models are frameworks that define the various functions needed to allow data to travel from source to destination over a network. These functions are typically divided into layers, with each layer describing a certain role required to enable network communications. Then, protocols can be designed to fill those roles.

> [!translation] 逐句繁體中文翻譯
> Networking models 是一種框架，用來定義讓資料能透過網路從來源端傳到目的端所需的各種功能。  
> 這些功能通常會被分成多個 layers，每一層描述一個讓網路通訊得以發生所需的特定角色。  
> 接著，protocols 就可以被設計來填補這些角色。

Using layers allows for a modular design: at each layer of the model, there are several protocols that can fill the necessary roles of the layer. For example, in the previous chapter, we looked at some aspects of Ethernet (IEEE 802.3) and also briefly mentioned wireless LANs as defined by IEEE 802.11 (best known as Wi-Fi). Both protocols serve the same purpose: they define how data should be sent over a particular physical medium (UTP/fiber cables for Ethernet, radio waves for Wi-Fi). An email application
on a computer doesn't need to care about whether a message will be sent over the network via a wired Ethernet connection or a wireless Wi-Fi connection; as long as the email application performs its role, it can expect the other layers to perform their roles as well.

> [!translation] 逐句繁體中文翻譯
> 使用 layers 可以形成模組化設計：在模型的每一層，都有數個 protocols 可以填補該層所需的角色。  
> 例如，在上一章中，我們看了 Ethernet（IEEE 802.3）的一些面向，也簡短提到由 IEEE 802.11 定義的 wireless LANs，也就是最常稱為 Wi-Fi 的技術。  
> 這兩種 protocols 服務相同目的：它們定義資料應如何透過特定實體媒介傳送，例如 Ethernet 使用 UTP / fiber cables，而 Wi-Fi 使用 radio waves。  
> 電腦上的 email application 不需要在意訊息會透過有線 Ethernet connection 還是無線 Wi-Fi connection 傳送；只要 email application 完成自己的角色，它就可以期待其他 layers 也完成各自的角色。

There are two networking models that network professionals should be familiar with: OSI and TCP/IP. Although the TCP/IP model is the model used in modern networks, the OSI model has also had a large influence on how we think and talk about networks and is still considered core knowledge for anyone involved in networking (despite not being in use in modern networks).

> [!translation] 逐句繁體中文翻譯
> 網路專業人員應該熟悉兩個 networking models：OSI 與 TCP/IP。  
> 雖然 TCP/IP model 是現代網路使用的模型，但 OSI model 也深深影響了我們思考與談論網路的方式。  
> 即使 OSI model 並未在現代網路中實際使用，它仍被視為任何網路相關人員都應具備的核心知識。

### 4.2 The OSI reference model

The Open Systems Interconnection reference model is a conceptual model of networking developed by the International Organization for Standardization (ISO). Most people simply call it the OSI model.

> [!translation] 逐句繁體中文翻譯
> Open Systems Interconnection reference model 是由 International Organization for Standardization（ISO）發展出的網路概念模型。  
> 多數人直接稱它為 OSI model。

## International Organization for Standardization

The ISO publishes standards related to various aspects of technology. Looking at the name, you may wonder why it's abbreviated as ISO and not IOS. The ISO decided upon the abbreviation to have one shared abbreviation regardless of language. Rather than being an acronym for International Organization for Standardization, the organization states that ISO is derived from the Greek word isos, meaning "equal."

> [!translation] 逐句繁體中文翻譯
> ISO 發布與各種技術面向相關的標準。  
> 看這個名稱時，你可能會疑惑為什麼縮寫是 ISO，而不是 IOS。  
> ISO 選擇這個縮寫，是為了不論語言為何，都能共用同一個縮寫。  
> 根據該組織的說法，ISO 並不是 International Organization for Standardization 的首字母縮寫，而是源自希臘字 isos，意思是「相等」。

The OSI model defines seven layers, each with its own functions that contribute to the process of communicating over a network. Table 4.1 lists the seven layers of the OSI model.

> [!translation] 逐句繁體中文翻譯
> OSI model 定義了七個 layers，每一層都有自己的功能，並共同參與透過網路通訊的過程。  
> Table 4.1 列出了 OSI model 的七個 layers。

Table 4.1 The seven layers of the OSI model

> [!translation] 逐句繁體中文翻譯
> Table 4.1：OSI model 的七個 layers。

| Layer | Name |
| :--- | :--- |
| 7 | Application |
| 6 | Presentation |
| 5 | Session |
| 4 | Transport |
| 3 | Network |
| 2 | Data Link |
| 1 | Physical |


Because this chapter focuses on the TCP/IP model, we won't cover the role of each of the seven layers listed in table 4.1. The OSI model is a relic of the past that I don't recommend digging too deeply into unless you're interested in the history of how networks developed.

> [!translation] 逐句繁體中文翻譯
> 因為本章聚焦於 TCP/IP model，所以我們不會逐一說明 Table 4.1 中七個 layers 的角色。  
> OSI model 是過去留下來的產物；除非你對網路發展史有興趣，否則我不建議太深入研究它。

EXAM TIP Although we will focus on the TCP/IP model in this chapter, the terminology of the OSI model is still widely used, so it's worth remembering the seven layers and their names. Most students use a mnemonic to help with this: for example, "Please Do Not Teach Students Pointless Acronyms," using the first letter of each layer's name from Layers 1 to 7.

> [!translation] 逐句繁體中文翻譯
> 考試提示：雖然本章會聚焦於 TCP/IP model，但 OSI model 的術語仍被廣泛使用，所以值得記住七個 layers 及其名稱。  
> 多數學生會用 mnemonic 來幫助記憶，例如「Please Do Not Teach Students Pointless Acronyms」，這句話使用 Layer 1 到 Layer 7 各層名稱的第一個字母。

### 4.3 The TCP/IP model

The TCP/IP model was born out of research and development funded by the US Department of Defense (DOD) Defense Advanced Research Projects Agency (DARPA). It was then called the ARPANET reference model, but it has since evolved into the Internet Protocol Suite, which was defined in Request for Comments (RFC) 1122. RFCs are documents published by the Internet Engineering Task Force (IETF) to define standard protocols for the internet. Some more common names for this model are the TCP/IP suite, TCP/IP model, or just TCP/IP. TCP and IP are two of the foundational protocols included in the model, so they are often used to refer to it.

> [!translation] 逐句繁體中文翻譯
> TCP/IP model 源自美國 Department of Defense（DOD）旗下 Defense Advanced Research Projects Agency（DARPA）資助的研究與開發。  
> 它當時被稱為 ARPANET reference model，但後來演進為 Internet Protocol Suite，並在 Request for Comments（RFC）1122 中被定義。  
> RFCs 是由 Internet Engineering Task Force（IETF）發布的文件，用來定義網際網路的標準協定。  
> 這個模型更常見的名稱包括 TCP/IP suite、TCP/IP model，或直接稱為 TCP/IP。  
> TCP 與 IP 是包含在此模型中的兩個基礎協定，因此常被用來指稱整個模型。

## RFCs and the IETF

The IETF is an organization that defines the standard protocols used by the internet. RFCs are the documents published by the IETF that define these protocols. Many of these documents are informational or experimental and sometimes humorous (e.g., check out RFC 1149 at https://datatracker.ietf.org/doc/html/rfc1149, which describes how to send network messages using birds).

> [!translation] 逐句繁體中文翻譯
> IETF 是一個定義網際網路所使用標準協定的組織。  
> RFCs 是 IETF 發布的文件，用來定義這些協定。  
> 其中許多文件是資訊性或實驗性的，有時甚至帶有幽默感；例如你可以看看 RFC 1149，它描述如何使用鳥來傳送網路訊息。

However, some RFCs go on to be recognized as Internet Standards; these are the RFCs that define the protocols that make up the TCP/IP model. For example, TCP, IP, and other well-known protocols like HTTPS (which you'll see at the beginning of the previous URL I copied) are Internet Standards.

> [!translation] 逐句繁體中文翻譯
> 然而，有些 RFCs 後來會被認定為 Internet Standards；這些 RFCs 定義了組成 TCP/IP model 的 protocols。  
> 例如 TCP、IP，以及其他知名 protocols，如 HTTPS，也就是你會在前面那個 URL 開頭看到的協定，都是 Internet Standards。

The TCP/IP model as defined in RFC 1122 has four layers; however, network engineers typically reference a five-layer TCP/IP model. The five-layer version of the model, as indicated by the thick border in table 4.2, is what we will be using in this book. The table lists the layers of the TCP/IP model, their equivalent OSI model layers, and some example protocols that belong to each layer of the model.

> [!translation] 逐句繁體中文翻譯
> RFC 1122 所定義的 TCP/IP model 有四個 layers；然而，network engineers 通常會參考五層 TCP/IP model。  
> Table 4.2 中以粗框標示的五層版本，就是本書會使用的模型。  
> 這個表列出 TCP/IP model 的 layers、它們對應的 OSI model layers，以及屬於模型各層的一些 example protocols。

Table 4.2 The TCP/IP model

> [!translation] 逐句繁體中文翻譯
> Table 4.2：TCP/IP model。

| OSI model | Four-layer TCP/IP model | Five-layer TCP/IP model | Example protocols |
| :--- | :--- | :--- | :--- |
| Application | Application | Application | HTTP |
| Presentation |  |  | HTTPS |
| Session |  |  | FTP |
|  |  |  | SSH |
| Transport | Transport | Transport | TCP |
|  |  |  | UDP |
| Network | Internet | Network | IPv4 |
|  |  |  | IPv6 |
| Data Link | Link | Data Link | Ethernet 802.11 (Wi-Fi) |
| Physical |  | Physical |  |


NOTE The similar layers of the OSI model and TCP/IP model are not entirely equivalent; although they have similarities, they are two independent models.

> [!translation] 逐句繁體中文翻譯
> 注意：OSI model 與 TCP/IP model 中相似的 layers 並不完全等價；雖然它們有相似之處，但它們是兩個獨立的模型。

As table 4.2 shows, instead of the three upper layers (Application, Presentation, and Session) of the OSI model, TCP/IP uses a single layer called the Application Layer. Additionally, in the four-layer version of the TCP/IP model, the concerns of the bottom two layers of the five-layer version are addressed by a single layer called the Link Layer. However, for the purpose of the CCNA and understanding networking, the fivelayer model is generally more useful, and it is the one we will refer to throughout this book.

> [!translation] 逐句繁體中文翻譯
> 如 Table 4.2 所示，TCP/IP 沒有像 OSI model 那樣使用三個上層 layers，也就是 Application、Presentation 與 Session，而是使用單一一層，稱為 Application Layer。  
> 此外，在四層版 TCP/IP model 中，五層版最下面兩層的關注事項由單一的 Link Layer 處理。  
> 然而，對 CCNA 與理解網路而言，五層模型通常更有用，本書也會全程參考這個版本。

The example protocols listed in table 4.2 are some of the protocols we will cover in this book; they are just a few of the protocols you should know for the CCNA exam. I included them in the table for reference, but we will cover how they function in the rest of this book. In this chapter, we will focus on understanding the role of each layer of the TCP/IP model.

> [!translation] 逐句繁體中文翻譯
> Table 4.2 中列出的 example protocols，是本書會涵蓋的一部分 protocols；它們只是 CCNA 考試中你應該知道的少數幾個協定。  
> 我把它們放在表中作為參考，但我們會在本書後續內容中說明它們如何運作。  
> 在本章中，我們會聚焦於理解 TCP/IP model 每一層的角色。

EXAM TIP The layers of the TCP/IP model can be referred to by their names or their numbers: the Physical Layer is Layer 1, the Data Link Layer is Layer 2, the Network Layer is Layer 3, the Transport Layer is Layer 4, and the Application Layer is Layer 7. As I mentioned previously, the terminology of the OSI model is still widely used (for better or for worse!), so even when referring to the TCP/IP model, the Application Layer is typically called Layer 7 rather than Layer 5 or 4.

> [!translation] 逐句繁體中文翻譯
> 考試提示：TCP/IP model 的 layers 可以用名稱或編號來稱呼：Physical Layer 是 Layer 1，Data Link Layer 是 Layer 2，Network Layer 是 Layer 3，Transport Layer 是 Layer 4，而 Application Layer 是 Layer 7。  
> 如前所述，OSI model 的術語仍被廣泛使用，不論好壞都是如此。  
> 因此，即使在談 TCP/IP model 時，Application Layer 通常也會被稱為 Layer 7，而不是 Layer 5 或 Layer 4。

### 4.3.1 The layers of the TCP/IP model

Each layer of the TCP/IP model provides an essential function in enabling computers to communicate over a network. The end goal is for an application on one computer to be able to communicate with an application on another computer over a network (e.g., a PC's web browser communicating with a web server). Figure 4.1 demonstrates this process; a PC (PC1) accesses a web page hosted on a server (SRV1). As we examine each layer of the TCP/IP model in the following pages, we will see how the layers work together to enable this communication.

> [!translation] 逐句繁體中文翻譯
> TCP/IP model 的每一層都提供一個必要功能，使電腦能透過網路通訊。  
> 最終目標是讓一台電腦上的 application 能透過網路與另一台電腦上的 application 通訊，例如 PC 的 web browser 與 web server 通訊。  
> Figure 4.1 展示了這個過程：一台 PC（PC1）存取 server（SRV1）上 hosted 的網頁。  
> 當我們在接下來幾頁檢視 TCP/IP model 的每一層時，會看到這些 layers 如何一起運作以促成這種通訊。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-064_580_1416_592_223.jpg)
Figure 4.1 A web browser on PC1 uses a Layer 7 protocol (HTTPS) to request a web page from the web server on SRV1. Layers 2, 3, and 4 work together to deliver the message to the appropriate application on SRV1. Layer 1 is the medium over which the communication occurs.

> [!translation] 逐句繁體中文翻譯
> Figure 4.1：PC1 上的 web browser 使用 Layer 7 protocol（HTTPS），向 SRV1 上的 web server 請求網頁。  
> Layers 2、3、4 一起運作，將訊息傳遞到 SRV1 上正確的 application。  
> Layer 1 則是通訊發生時所使用的媒介。

The functions defined by each layer of the TCP/IP model include

> [!translation] 逐句繁體中文翻譯
> TCP/IP model 每一層所定義的功能包括：

- Physical specifications, such as cables and radio waves
- Communication between intermediate nodes in the path to the destination
- End-to-end communication from the original source node to the final destination node
- Addressing messages to a specific application on the destination node
- How an application should interface with the network

Now let's examine each layer of the TCP/IP model one by one to see how they enable network communications. The goal of this chapter is to provide a framework we can build upon in the rest of this book with details of how the different protocols of each layer fulfill their roles.

> [!translation] 逐句繁體中文翻譯
> 現在，讓我們逐一檢視 TCP/IP model 的每一層，看看它們如何讓網路通訊成為可能。  
> 本章的目標是提供一個框架，讓本書後續內容可以在此基礎上補充各層不同 protocols 如何完成其角色的細節。

## Layer 1: The Physical Layer

The Physical Layer is fairly self-explanatory; it defines the physical requirements for transmitting data (a series of bits) from one node to another. Those bits could be
encoded as electrical signals traveling along a copper cable, light signals on a fiberoptic cable, or radio waves in a wireless connection.

> [!translation] 逐句繁體中文翻譯
> Physical Layer 相當直觀；它定義了將資料，也就是一連串 bits，從一個 node 傳送到另一個 node 所需的實體需求。  
> 這些 bits 可以被編碼為沿著 copper cable 傳遞的 electrical signals、fiber-optic cable 上的 light signals，或 wireless connection 中的 radio waves。

We covered this in chapter 3: IEEE 802.3 (Ethernet) and IEEE 802.11 (Wi-Fi) both define specifications at the Physical Layer. For example, Ethernet defines connector and cable types, how data should be encoded into electrical (or light) signals, and countless other minutiae about how to communicate over UTP and fiber-optic cables. Likewise, Wi-Fi defines what radio frequencies should be used for wireless LAN communication, how radio waves should be modulated to encode data, etc.

> [!translation] 逐句繁體中文翻譯
> 我們在 Chapter 3 已經涵蓋這點：IEEE 802.3（Ethernet）與 IEEE 802.11（Wi-Fi）都在 Physical Layer 定義規格。  
> 例如，Ethernet 定義 connector 與 cable types、資料應如何被編碼成 electrical 或 light signals，以及許多關於如何透過 UTP 與 fiber-optic cables 通訊的細節。  
> 同樣地，Wi-Fi 定義 wireless LAN communication 應使用哪些 radio frequencies、radio waves 應如何被調變以編碼資料等等。

To summarize, the Physical Layer of the TCP/IP model defines the physical requirements to enable a series of bits to travel from one node to another over a physical medium.

> [!translation] 逐句繁體中文翻譯
> 總結來說，TCP/IP model 的 Physical Layer 定義了實體需求，使一連串 bits 能透過 physical medium 從一個 node 傳到另一個 node。

## Layer 2: The Data Link Layer

Ethernet and Wi-Fi do not only define physical specifications; they also specify how data should be addressed and sent to another node connected to the same physical medium within a LAN. The Data Link Layer's job is to prepare data for transmission over that physical medium so it can be received by the next node in the path to the final destination. That next node could be the final destination itself or the next router in the path. The journey from one node to the next in the path is called a hop, and the job of the Data Link Layer is to provide hop-to-hop delivery of messages.

> [!translation] 逐句繁體中文翻譯
> Ethernet 與 Wi-Fi 不只定義 physical specifications；它們也指定資料應如何被定址，並傳送給 LAN 中連接到同一 physical medium 的另一個 node。  
> Data Link Layer 的工作，是準備資料，使其能透過該 physical medium 傳輸，並被通往 final destination 路徑中的下一個 node 接收。  
> 這個下一個 node 可能就是 final destination 本身，也可能是路徑中的下一台 router。  
> 從路徑中的一個 node 到下一個 node 的旅程稱為 hop，而 Data Link Layer 的工作就是提供 messages 的 hop-to-hop delivery。

Figure 4.2 demonstrates the concept of network hops. PC1 sends a message to SRV1, perhaps a request to access a file hosted on the server. For PC1's message to reach SRV1, it must make three hops through the network: from PC1 to R1, from R1 to R2, and from R2 to SRV1. The Data Link Layer's job is to forward the message from one hop to the next until the message reaches the destination host: SRV1. Notice that a message traveling through a switch does not count as a hop. We will examine why this is when we look at Ethernet LAN switching in chapter 6.

> [!translation] 逐句繁體中文翻譯
> Figure 4.2 展示了 network hops 的概念。  
> PC1 傳送一個 message 給 SRV1，可能是請求存取 server 上 hosted 的某個檔案。  
> 為了讓 PC1 的 message 到達 SRV1，它必須透過網路完成三個 hops：從 PC1 到 R1、從 R1 到 R2、再從 R2 到 SRV1。  
> Data Link Layer 的工作，是將 message 從一個 hop forward 到下一個 hop，直到 message 到達 destination host，也就是 SRV1。  
> 請注意，message 經過 switch 並不算一個 hop。  
> 當我們在 Chapter 6 看 Ethernet LAN switching 時，會檢視為什麼如此。

NOTE PC1, R1, R2, and SRV1 are examples of hostnames. A hostname is a name used to identify each device in the network. The hostname of each device in figure 4.2 follows the pattern I will use throughout this book: PC X for PCs, SWX for switches, $\mathrm{R} X$ for routers, and SRV $X$ for servers.

> [!translation] 逐句繁體中文翻譯
> 注意：PC1、R1、R2 與 SRV1 是 hostnames 的例子。  
> Hostname 是用來識別網路中每個設備的名稱。  
> Figure 4.2 中每個設備的 hostname 遵循本書會使用的模式：PC 使用 PC X，switches 使用 SWX，routers 使用 R X，servers 使用 SRV X。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-065_339_1165_1637_318.jpg)
Figure 4.2 TCP/IP Layer 2. A message sent from PC1 to SRV1 takes three hops through the network: from PC1 to R1, from R1 to R2, and from R2 to SRV1. At each hop, the message is addressed to the next hop's MAC address. A message traveling through a switch does not count as a hop.

> [!translation] 逐句繁體中文翻譯
> Figure 4.2：TCP/IP Layer 2。  
> 從 PC1 傳送到 SRV1 的 message 會透過網路經過三個 hops：從 PC1 到 R1、從 R1 到 R2、再從 R2 到 SRV1。  
> 在每個 hop，message 都會被定址到 next hop 的 MAC address。  
> 經過 switch 的 message 不算一個 hop。

The Data Link Layer achieves this hop-to-hop delivery by using media access control (MAC) addresses, a kind of network address assigned to each port of a device. At each hop, the message is sent to the MAC address of the next hop. In the first hop, PC1 addresses the message to R1's MAC address. In the second hop, R1 addresses the message to R2's MAC address. In the final hop, R2 addresses the message to SRV1's MAC address.

> [!translation] 逐句繁體中文翻譯
> Data Link Layer 透過 media access control（MAC）addresses 來達成這種 hop-to-hop delivery；MAC address 是一種指派給設備每個 port 的 network address。  
> 在每個 hop，message 都會被送到 next hop 的 MAC address。  
> 在第一個 hop，PC1 將 message 定址到 R1 的 MAC address。  
> 在第二個 hop，R1 將 message 定址到 R2 的 MAC address。  
> 在最後一個 hop，R2 將 message 定址到 SRV1 的 MAC address。

NOTE The roles of SW1 and SW2 may seem unclear in figure 4.2. As covered in chapter 2, the role of a switch is to provide many ports for end hosts to connect to the LAN. For the sake of avoiding clutter, I only show one end host connected to each switch (PC1 to SW1 and SRV1 to SW2). However, in reality, there could be 40+ end hosts connected to each of them. In chapter 6, we will examine how switches function.

> [!translation] 逐句繁體中文翻譯
> 注意：在 Figure 4.2 中，SW1 與 SW2 的角色可能看起來不太清楚。  
> 如 Chapter 2 所述，switch 的角色是提供許多 ports，讓 end hosts 連接到 LAN。  
> 為了避免圖面雜亂，我只顯示一個 end host 連接到每台 switch，也就是 PC1 到 SW1、SRV1 到 SW2。  
> 然而在現實中，每台 switch 可能連接 40 個以上的 end hosts。  
> 在 Chapter 6，我們會檢視 switches 如何運作。

## Layer 3: The Network Layer

We just looked at how the Data Link Layer is used to forward a message from hop to hop until it reaches the final destination. At each hop, the message is sent to the MAC address of the next hop. However, we still need a way for the original source host to address the message to the final destination host. That is the role of the Network Layer: end-to-end delivery.

> [!translation] 逐句繁體中文翻譯
> 我們剛剛看了 Data Link Layer 如何用來將 message 從一個 hop forward 到下一個 hop，直到它到達 final destination。  
> 在每個 hop，message 都會被送到 next hop 的 MAC address。  
> 然而，我們仍然需要一種方式，讓 original source host 能把 message 定址到 final destination host。  
> 這就是 Network Layer 的角色：end-to-end delivery。

The type of address used at the Network Layer is the Internet Protocol (IP) address. Chances are you've heard of IP addresses before, although you might be unsure about how they work. We will cover IP addresses in chapter 7. Figure 4.3 shows how PC1 addresses a message to SRV1 by addressing it to SRV1's IP address. The destination IP address of the message remains the same throughout the journey, whereas the destination MAC address is different at each hop.

> [!translation] 逐句繁體中文翻譯
> Network Layer 使用的 address 類型是 Internet Protocol（IP）address。  
> 你很可能以前聽過 IP addresses，雖然你可能還不確定它們如何運作。  
> 我們會在 Chapter 7 涵蓋 IP addresses。  
> Figure 4.3 顯示 PC1 如何透過把 message 定址到 SRV1 的 IP address，來把 message 送給 SRV1。  
> Message 的 destination IP address 在整段 journey 中保持不變，而 destination MAC address 在每個 hop 都不同。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-066_394_1260_1366_350.jpg)
Figure 4.3 TCP/IP Layer 3. PC1 addresses a message to SRV1's IP address. Layer 3 is responsible for the end-to-end delivery of the message, whereas Layer 2 is responsible for the hop-to-hop delivery. The destination MAC address of the message changes at each hop, but the destination IP address remains the same throughout the journey.

> [!translation] 逐句繁體中文翻譯
> Figure 4.3：TCP/IP Layer 3。  
> PC1 將 message 定址到 SRV1 的 IP address。  
> Layer 3 負責 message 的 end-to-end delivery，而 Layer 2 負責 hop-to-hop delivery。  
> Message 的 destination MAC address 在每個 hop 都會改變，但 destination IP address 在整段 journey 中保持不變。

## IPv4 and IPv6

There are two versions of IP in use today: IP version 4 (IPv4) and IP version 6 (IPv6). Network engineers must be familiar with both, and both are part of the CCNA exam. IPv4 and IPv6 use different address formats. The following is an example of an IPv4 address and an IPv6 address:

> [!translation] 逐句繁體中文翻譯
> 目前使用中的 IP 有兩個版本：IP version 4（IPv4）與 IP version 6（IPv6）。  
> Network engineers 必須熟悉兩者，而且兩者都是 CCNA 考試的一部分。  
> IPv4 與 IPv6 使用不同的 address formats。  
> 以下是 IPv4 address 與 IPv6 address 的例子：

- IPv4 address: 203.0.113.255
- IPv6 address: 2001:db8:1:1:2fe3:1:32a:af01

Although IPv4 has been the dominant version of IP for a long time, IPv6 is steadily gaining popularity. In recent years, IPv6's adoption has accelerated as the number of available IPv4 addresses is running out. We will cover both address types in this book.

> [!translation] 逐句繁體中文翻譯
> 雖然 IPv4 長期以來一直是主流的 IP version，但 IPv6 正穩定地普及。  
> 近年來，隨著可用 IPv4 addresses 數量逐漸耗盡，IPv6 的採用速度已經加快。  
> 本書會涵蓋這兩種 address types。

Understanding how Layers 2 and 3 work together to deliver a message to its destination is a fundamental concept you must understand for the CCNA exam. In this chapter, I provide a high-level overview of the concepts; we will review these concepts and dig deeper in later chapters of this volume. At this point, it is enough to know the following points:

> [!translation] 逐句繁體中文翻譯
> 理解 Layers 2 與 3 如何一起運作，將 message 傳送到目的地，是 CCNA 考試必須理解的基本概念。  
> 在本章中，我提供這些概念的高層次概觀；我們會在本冊後續章節複習這些概念並深入探討。  
> 在這個階段，知道以下幾點就足夠了：

- Layer 2 uses MAC addresses to provide hop-to-hop delivery of messages.
- Layer 3 uses IP addresses to provide end-to-end delivery of messages.
- Layers 2 and 3 work together to allow a message to travel through the network to its final destination.
- The destination IP address of a message remains the same throughout the journey, whereas the destination MAC address is different at each hop.

## Layer 4: The Transport Layer

Layers 2 and 3 work together to deliver a message from the source host across a network to the destination host. You might think that's the end of the story because the message has reached its destination, but it's actually not all the way there. It's not enough for the data to reach the correct destination host; we need a way to address data to a specific application process on the destination host (e.g., a service running on a server). That is the role of Layer 4, the Transport Layer.

> [!translation] 逐句繁體中文翻譯
> Layers 2 與 3 一起運作，將 message 從 source host 透過網路傳送到 destination host。  
> 你可能會以為故事到此結束，因為 message 已經到達目的地，但其實還沒有完全到位。  
> 讓 data 到達正確的 destination host 還不夠；我們還需要一種方式，將 data 定址到 destination host 上特定的 application process，例如 server 上執行的 service。  
> 這就是 Layer 4，也就是 Transport Layer 的角色。

Like Layers 2 and 3, Layer 4 also uses its own addressing scheme: port numbers. By addressing a message to a particular port, you can send messages to a particular application process on the destination host. Computers run many different applications simultaneously, so this is a very important function. For example, a PC can simultaneously run an online game, a web browser with various tabs that each access a different website, an antivirus application that communicates with an external server for updates, and countless other applications. Port numbers allow the PC to ensure that data it receives from the network reaches the proper destination process.

> [!translation] 逐句繁體中文翻譯
> 和 Layers 2 與 3 一樣，Layer 4 也使用自己的 addressing scheme：port numbers。  
> 透過將 message 定址到特定 port，你可以把 messages 送到 destination host 上特定的 application process。  
> 電腦會同時執行許多不同 applications，因此這是一個非常重要的功能。  
> 例如，一台 PC 可以同時執行 online game、開著多個分頁且每個分頁存取不同網站的 web browser、與 external server 通訊以更新的 antivirus application，以及無數其他 applications。  
> Port numbers 讓 PC 能確保它從網路接收到的 data 會到達正確的 destination process。

NOTE Layer 4 port numbers are not related to the physical ports on a device that we connect cables to (which are an aspect of Layer 1, the Physical Layer). Same name, different concept.

> [!translation] 逐句繁體中文翻譯
> 注意：Layer 4 port numbers 與設備上用來連接 cables 的 physical ports 無關；physical ports 是 Layer 1，也就是 Physical Layer 的面向。  
> 名稱相同，但概念不同。

Figure 4.4 demonstrates this concept. Layers 2 and 3 work together to deliver PC1's message to SRV1, and Layer 4 delivers the message to the appropriate application process on SRV1. SRV1 is a server that provides a few services to clients in the network. It is a name server using the Domain Name System (DNS) to convert website names to IP addresses for clients (that's what happens when you type manning.com into a web browser). It is also a web server that uses Hypertext Transfer Protocol (HTTP) and Hypertext Transfer Protocol Secure (HTTPS) to allow clients to access the websites it hosts. DNS, HTTP, and HTTPS are Layer 7 (Application Layer) protocols, and they each accept messages using a different Layer 4 port number.

> [!translation] 逐句繁體中文翻譯
> Figure 4.4 展示了這個概念。  
> Layers 2 與 3 一起運作，將 PC1 的 message 傳送到 SRV1，而 Layer 4 則把 message 傳送到 SRV1 上正確的 application process。  
> SRV1 是一台 server，為網路中的 clients 提供幾項 services。  
> 它是一台 name server，使用 Domain Name System（DNS）為 clients 將 website names 轉換成 IP addresses；這就是你在 web browser 輸入 manning.com 時發生的事。  
> 它也是一台 web server，使用 Hypertext Transfer Protocol（HTTP）與 Hypertext Transfer Protocol Secure（HTTPS），讓 clients 存取它 hosted 的 websites。  
> DNS、HTTP 與 HTTPS 都是 Layer 7，也就是 Application Layer protocols，而且它們各自使用不同的 Layer 4 port number 接收 messages。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-068_402_1414_782_223.jpg)
Figure 4.4 Layers 2 and 3 work together to deliver PC1's message to SRV1. At Layer 4, PC1 addresses the message to port 443, which is used by the HTTPS protocol. Three ports are open on SRV1 (53, 80, 443), meaning it will accept messages addressed to any of those ports.

> [!translation] 逐句繁體中文翻譯
> Figure 4.4：Layers 2 與 3 一起運作，將 PC1 的 message 傳送到 SRV1。  
> 在 Layer 4，PC1 將 message 定址到 port 443，這是 HTTPS protocol 使用的 port。  
> SRV1 上有三個 ports 開啟：53、80、443，表示它會接受定址到這些 ports 的 messages。

NOTE All three addresses-the MAC address (Layer 2), the IP address (Layer 3), and the port number (Layer 4)-are included in the same message. We will examine how this works in section 4.3.2.

> [!translation] 逐句繁體中文翻譯
> 注意：三種 addresses，也就是 MAC address（Layer 2）、IP address（Layer 3）與 port number（Layer 4），都包含在同一個 message 中。  
> 我們會在 section 4.3.2 檢視這是如何運作的。

## TCP and UDP

The two most common Layer 4 protocols are Transmission Control Protocol (TCP)-the "TCP" in TCP/IP-and User Datagram Protocol (UDP). Both protocols allow computers to address messages to specific application services on the destination host, but there are several differences between the two.

> [!translation] 逐句繁體中文翻譯
> 最常見的兩個 Layer 4 protocols 是 Transmission Control Protocol（TCP），也就是 TCP/IP 中的「TCP」，以及 User Datagram Protocol（UDP）。  
> 這兩個 protocols 都允許電腦將 messages 定址到 destination host 上特定的 application services，但兩者之間有幾個差異。

For example, TCP implements checks to ensure that each message reaches its destination and is used by Application Layer protocols such as HTTP and HTTPS (used for accessing websites). UDP, on the other hand, takes a "send it and forget it" approach; it doesn't check to ensure that every message reaches the destination. UDP is used by Voice over IP (VoIP) protocols-used for phone calls-and live video streaming protocols, among others. We will cover TCP and UDP in chapter 22 of this book.

> [!translation] 逐句繁體中文翻譯
> 例如，TCP 實作了檢查機制，以確保每個 message 都到達目的地；HTTP 與 HTTPS 這類 Application Layer protocols 會使用 TCP，它們用於存取 websites。  
> 另一方面，UDP 採取「送出後就不管」的方法；它不會檢查每個 message 是否都到達目的地。  
> UDP 被 Voice over IP（VoIP）protocols 使用，例如電話通話，也被 live video streaming protocols 等使用。  
> 我們會在本書 Chapter 22 涵蓋 TCP 與 UDP。

## Layer 7: The Application Layer

The Application Layer is the interface between the applications running on a computer and the network. Using Layer 7 protocols, an application running on a computer can prepare a message to be sent over the network. This message could be, for example, a request from a web browser to retrieve a web page that is hosted on a web server. Layers 2, 3, and 4 are then responsible for delivering that message to the appropriate application on the destination computer.

> [!translation] 逐句繁體中文翻譯
> Application Layer 是電腦上執行的 applications 與 network 之間的介面。  
> 使用 Layer 7 protocols，電腦上執行的 application 可以準備一個要透過網路傳送的 message。  
> 這個 message 可能是 web browser 發出的 request，用來取得 web server 上 hosted 的 web page。  
> 接著，Layers 2、3、4 負責將該 message 傳送到 destination computer 上正確的 application。

NOTE Although the TCP/IP model only has five layers (or four, in the original definition), Layer 7 is the most common term used for the Application Layer, so that is what I will use throughout this book. That is due to the influence of the OSI model, as mentioned previously.

> [!translation] 逐句繁體中文翻譯
> 注意：雖然 TCP/IP model 只有五層，或在原始定義中只有四層，但 Layer 7 是 Application Layer 最常用的稱呼，因此本書會全程使用這個稱呼。  
> 如前所述，這是受到 OSI model 影響的結果。

Layer 7 protocols such as HTTPS are not user applications themselves; rather, they provide services for those applications to enable them to communicate with applications on other computers over the network. Figure 4.1 shows the complete process that enables a web browser on PC1 to send a message to request a web page from the web server running on SRV1. The process that the message goes through to reach SRV1 is as follows:

> [!translation] 逐句繁體中文翻譯
> HTTPS 這類 Layer 7 protocols 本身不是 user applications；相反地，它們為那些 applications 提供 services，使它們能透過網路與其他電腦上的 applications 通訊。  
> Figure 4.1 顯示完整流程，說明 PC1 上的 web browser 如何傳送 message，向 SRV1 上執行的 web server 請求 web page。  
> Message 到達 SRV1 所經過的流程如下：

- Layer 7-PC1's web browser uses HTTPS to request the web page.
- Layer 4-PC1 addresses the message to port 443, which is used by the HTTPS protocol. This ensures that the message reaches the correct application on SRV1.
- Layer 3-PC1 addresses the message to the IP address of SRV1, and the destination IP address of the message remains the same as the message travels from PC1 across the network to SRV1.
- Layer 2-PC1 addresses the message to the next hop in the path to SRV1, which is R1. After receiving the message, R1 forwards it to the next hop (R2) by addressing the message to R2's MAC address. Finally, R2 forwards the message to the final destination (SRV1) by addressing the message to SRV1's MAC address. Unlike the destination IP address of the message, the destination MAC address is changed at each hop.

DEFINITION To forward a message is to send it to the next node in the path to the destination, whether that is the final destination node itself or the next router in the path to the destination. In later chapters of this volume, we will examine how routers and switches make forwarding decisions to deliver messages to the correct destination.

> [!translation] 逐句繁體中文翻譯
> 定義：forward a message 是指將 message 傳送到通往 destination 路徑中的下一個 node，不論那是 final destination node 本身，還是通往 destination 路徑中的下一台 router。  
> 在本冊後續章節中，我們會檢視 routers 與 switches 如何做出 forwarding decisions，以將 messages 傳送到正確目的地。

### 4.3.2 Data encapsulation and de-encapsulation

In this section, we'll see how the layers of the TCP/IP model work together to allow computers to communicate with each other. By now, you should be familiar with the basic purpose of each layer of the TCP/IP model:

> [!translation] 逐句繁體中文翻譯
> 在本節中，我們會看到 TCP/IP model 的 layers 如何一起運作，使電腦能彼此通訊。  
> 到目前為止，你應該已經熟悉 TCP/IP model 每一層的基本目的：

- Layer 7 (Application)-The interface between applications and the network
- Layer 4 (Transport)-Provides application-to-application delivery of messages
- Layer 3 (Network)-Provides end-to-end delivery of messages
- Layer 2 (Data Link)-Provides hop-to-hop delivery of messages
- Layer 1 (Physical)-The physical medium over which communication happens

## Data encapsulation

The process a host goes through to send data is a five-step process. It begins with the Layer 7 protocol preparing some data to be sent. In the second step, a Layer 4 protocol then adds a header to that data addressed to a certain port.

> [!translation] 逐句繁體中文翻譯
> Host 傳送 data 時會經過一個五步驟流程。  
> 這個流程從 Layer 7 protocol 準備一些要傳送的 data 開始。  
> 在第二步，Layer 4 protocol 會對該 data 加上一個 header，並將其定址到某個特定 port。

DEFINITION A header is supplemental data added to the front of a message that is to be transmitted over a network. A protocol's header contains the data used by that protocol. For example, a Layer 4 protocol will include a destination port number, as well as other information.

> [!translation] 逐句繁體中文翻譯
> 定義：header 是加在即將透過網路傳輸的 message 前方的 supplemental data。  
> 一個 protocol 的 header 包含該 protocol 所使用的 data。  
> 例如，Layer 4 protocol 會包含 destination port number，以及其他資訊。

In the third step, the message is passed to Layer 3, which adds its own header to that data. This header will be addressed to the IP address of the destination host. In the fourth step, the message will then be passed to Layer 2, which adds both a header and a trailer.

> [!translation] 逐句繁體中文翻譯
> 在第三步，message 會被傳給 Layer 3，而 Layer 3 會將自己的 header 加到該 data 上。  
> 這個 header 會被定址到 destination host 的 IP address。  
> 在第四步，message 接著會被傳給 Layer 2，而 Layer 2 會同時加上 header 與 trailer。

DEFINITION A trailer is also supplemental data added to a message that is to be transmitted over a network. Whereas a header is added to the beginning of a message, a trailer is added to the end. The Ethernet trailer contains a small block of data used to check for errors in the message. For example, errors can occur during transmission as a result of electromagnetic interference.

> [!translation] 逐句繁體中文翻譯
> 定義：trailer 也是加到即將透過網路傳輸的 message 上的 supplemental data。  
> Header 加在 message 的開頭，而 trailer 加在 message 的結尾。  
> Ethernet trailer 包含一小塊 data，用來檢查 message 中是否有錯誤。  
> 例如，傳輸過程中可能因 electromagnetic interference 而發生錯誤。

At Layer 2, the message is addressed to the next-hop device. Finally, in the fifth step, the host will transmit the bits over the physical medium, such as a UTP cable. The process of adding headers (and trailers) to data before sending it over a network is called encapsulation. To summarize that process:

> [!translation] 逐句繁體中文翻譯
> 在 Layer 2，message 會被定址到 next-hop device。  
> 最後，在第五步，host 會透過 physical medium，例如 UTP cable，傳送 bits。  
> 在透過網路傳送 data 之前，將 headers（以及 trailers）加入 data 的過程稱為 encapsulation。  
> 這個過程可以總結如下：

1 The Application Layer protocol prepares data.
2 Layer 4 encapsulates the data with a header addressed to a port number on the destination host.
3 Layer 3 encapsulates the data with a header addressed to the IP address of the destination host.

4 Layer 2 encapsulates the data with a header addressed to the MAC address of the next hop. It also encapsulates the data with a trailer, used to check for errors.
${ }^{5}$ The host transmits the bits of data over the physical medium (e.g., encoded as electrical signals over a UTP cable).

Figure 4.5 demonstrates the five-step process of encapsulation and transmission.

> [!translation] 逐句繁體中文翻譯
> Figure 4.5 展示 encapsulation 與 transmission 的五步驟流程。
![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-071_462_1389_470_206.jpg)

Figure 4.5 The five-step process of encapsulating and transmitting data: (1) the Application Layer protocol prepares some data, (2) Layer 4 encapsulates the data with a header, (3) Layer 3 encapsulates the data with a header, (4) Layer 2 encapsulates the data with a header and trailer, and (5) the host transmits the bits over the physical medium (i.e., a UTP cable).

> [!translation] 逐句繁體中文翻譯
> Figure 4.5：encapsulating 與 transmitting data 的五步驟流程。  
> （1）Application Layer protocol 準備一些 data。  
> （2）Layer 4 使用 header 封裝 data。  
> （3）Layer 3 使用 header 封裝 data。  
> （4）Layer 2 使用 header 與 trailer 封裝 data。  
> （5）Host 透過 physical medium，例如 UTP cable，傳送 bits。

NOTE The Layer 2 header is the beginning of the message; it is the first part sent. The Layer 2 trailer is the end of the message; it is the last part sent.

> [!translation] 逐句繁體中文翻譯
> 注意：Layer 2 header 是 message 的開頭；它是第一個被送出的部分。  
> Layer 2 trailer 是 message 的結尾；它是最後被送出的部分。

## Data de-encapsulation

When the destination host receives the message, it goes through the opposite process: de-encapsulation. In the de-encapsulation process, the host receiving the message inspects the information in each header/trailer and then removes them until it gets to the data inside. Like encapsulating and transmitting a message, receiving and de-encapsulating a message can also be summarized into five steps, summarized as follows (also see figure 4.6):

> [!translation] 逐句繁體中文翻譯
> 當 destination host 收到 message 時，它會經過相反的流程：de-encapsulation。  
> 在 de-encapsulation 過程中，接收 message 的 host 會檢查每個 header / trailer 中的資訊，然後移除它們，直到取得裡面的 data。  
> 就像 encapsulating 與 transmitting message 一樣，receiving 與 de-encapsulating message 也可以總結為五個步驟，如下所示；也請參見 Figure 4.6。

1 The destination host receives the message.
2 It inspects the Layer 2 header and trailer, removes them, and passes the message to Layer 3.
3 It inspects the Layer 3 header, removes it, and passes the message to Layer 4.
4 It inspects the Layer 4 header, removes it, and sends the data to the appropriate application.
5 The application receives and processes the data.

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-072_462_1391_181_230.jpg)
Figure 4.6 The five-step process of receiving and de-encapsulating data: (1) the destination host receives bits (the message), (2) the Layer 2 header/trailer is inspected and removed, (3) the Layer 3 header is inspected and removed, (4) the Layer 4 header is inspected and removed, and (5) the data is received and processed by the application.

> [!translation] 逐句繁體中文翻譯
> Figure 4.6：receiving 與 de-encapsulating data 的五步驟流程。  
> （1）Destination host 接收 bits，也就是 message。  
> （2）Layer 2 header / trailer 被檢查並移除。  
> （3）Layer 3 header 被檢查並移除。  
> （4）Layer 4 header 被檢查並移除。  
> （5）Data 被 application 接收並處理。

Protocol data units
At each stage in the encapsulation/de-encapsulation process, there is a name given to the message:

> [!translation] 逐句繁體中文翻譯
> Protocol data units。  
> 在 encapsulation / de-encapsulation 過程中的每個階段，message 都有一個名稱：

- The combination of data and a Layer 4 header is called a segment.
- The combination of a segment and a Layer 3 header is called a packet.
- The combination of a packet and a Layer 2 header/trailer is called a frame.

We can also use an alternative term to describe the message at each stage-protocol data unit (PDU):

> [!translation] 逐句繁體中文翻譯
> 我們也可以使用另一個術語來描述每個階段的 message：protocol data unit（PDU）。

- A segment is a Layer 4 PDU (L4PDU).
- A packet is a Layer 3 PDU (L3PDU).
- A frame is a Layer 2 PDU (L2PDU).

The contents of each PDU (everything encapsulated by that layer's header/trailer) are called the payload. So, a frame's payload is a packet, a packet's payload is a segment, and a segment's payload is the application data. Figure 4.7 illustrates the different PDUs and their payloads.

> [!translation] 逐句繁體中文翻譯
> 每個 PDU 的內容，也就是被該 layer 的 header / trailer 封裝起來的所有東西，稱為 payload。  
> 因此，frame 的 payload 是 packet，packet 的 payload 是 segment，而 segment 的 payload 是 application data。  
> Figure 4.7 說明不同 PDUs 與它們的 payloads。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-073_630_670_190_316.jpg)
Figure 4.7 Application data encapsulated in a Layer 4 header is a segment (L4PDU); a segment encapsulated in a Layer 3 header is a packet (L3PDU); and a packet encapsulated in a Layer 2 header/ trailer is a frame (L2PDU). The encapsulated contents of each PDU are that PDU's payload.

> [!translation] 逐句繁體中文翻譯
> Figure 4.7：被 Layer 4 header 封裝的 application data 是 segment（L4PDU）。  
> 被 Layer 3 header 封裝的 segment 是 packet（L3PDU）。  
> 被 Layer 2 header / trailer 封裝的 packet 是 frame（L2PDU）。  
> 每個 PDU 中被封裝的內容，就是該 PDU 的 payload。

Adjacent-layer and same-layer interactions
Within a computer, each layer of the TCP/IP model provides a service for the layer above it, called adjacent-layer interaction. Following is a summary of the interactions between adjacent layers of the TCP/IP model:

> [!translation] 逐句繁體中文翻譯
> Adjacent-layer 與 same-layer interactions。  
> 在一台電腦內，TCP/IP model 的每一層都為它上方的 layer 提供 service，這稱為 adjacent-layer interaction。  
> 以下是 TCP/IP model 中 adjacent layers 之間 interactions 的摘要：

- Layer 4 provides a service to Layer 7 by delivering data to the appropriate application on the destination host.
- Layer 3 provides a service to Layer 4 by delivering segments to the correct destination host.
- Layer 2 provides a service to Layer 3 by delivering packets to the next hop.
- Layer 1 provides a service to Layer 2 by providing a physical medium for frames to travel over.

There is also a related concept called same-layer interaction. This refers to the communications between the same layer on different computers. Same-layer interactions work like this:

> [!translation] 逐句繁體中文翻譯
> 另外還有一個相關概念，稱為 same-layer interaction。  
> 這是指不同電腦上相同 layer 之間的 communications。  
> Same-layer interactions 的運作方式如下：

- Application data from one computer is sent to an application on another computer.
- When data is encapsulated with a Layer 4 header, the segment is addressed to Layer 4 of the destination host, where the information in the header will be inspected.
- When a segment is encapsulated with a Layer 3 header, the packet is addressed to Layer 3 of the destination host, where the information in the header will be inspected.

- When a packet is encapsulated with a Layer 2 header and trailer, the frame is addressed to Layer 2 of the next hop, where the information in the header and trailer will be inspected.
- Signals sent out of a physical port of one device are received by a physical port of another device.

Figure 4.8 illustrates these adjacent-layer interactions between different layers on the same computer (on Host A and on Host B), and same-layer interactions between different computers that are communicating with each other (between Host A and Host B).

> [!translation] 逐句繁體中文翻譯
> Figure 4.8 說明同一台電腦上不同 layers 之間的 adjacent-layer interactions，也就是 Host A 與 Host B 各自內部的 layers。  
> 它也說明正在彼此通訊的不同電腦之間的 same-layer interactions，也就是 Host A 與 Host B 之間。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-074_480_927_628_362.jpg)
Figure 4.8 Each layer on a host provides services for the layer above it; this is called adjacent-layer interaction. When two hosts communicate, each layer on one host communicates with the same layer on the other host; this is called same-layer interaction.

> [!translation] 逐句繁體中文翻譯
> Figure 4.8：Host 上的每一層都為其上方的 layer 提供 services；這稱為 adjacent-layer interaction。  
> 當兩台 hosts 通訊時，一台 host 上的每一層都會與另一台 host 上相同的 layer 通訊；這稱為 same-layer interaction。

## Summary

- Networking models provide frameworks to define the functions necessary to enable network communications.
- Networking models are divided into layers; each layer describes a necessary function for network communications and includes multiple protocols that can fulfill the layer's role.
- The Open Systems Interconnection Reference (OSI) model is a networking model that influenced how we think and talk about networks but is not in use today.
- The OSI model has seven layers: (1) Physical, (2) Data Link, (3) Network, (4) Transport, (5) Session, (6) Presentation, and (7) Application.
- The Internet Protocol Suite (TCP/IP) model) is the networking model used in modern networks and is named after two of its key protocols: Transmission Control Protocol (TCP) and Internet Protocol (IP).
- The original TCP/IP model has four layers, but a more popular version has five: (1) Physical, (2) Data Link, (3) Network, (4) Transport, and (5) Application (called Layer 7, not Layer 5).

- Layer 1 (Physical) defines physical requirements for transmitting data, such as ports, connectors, and cables, and how data should be encoded into electrical/ light signals.
- Layer 2 (Data Link) is responsible for hop-to-hop delivery of messages. A hop is the journey from one node in the network to the next in the path to the final destination.
- Layer 2 uses media access control (MAC) addresses to address messages to the next hop.
- Layer 3 (Network) is responsible for end-to-end delivery of messages, from the source host to the destination host.
- Layer 3 uses Internet Protocol (IP) addresses to address messages to the destination host.
- The destination MAC address of a message changes at each hop in the path to the destination, but the destination IP address remains the same.
- Layer 4 (Transport) is used to address messages to the appropriate application on the destination host.
- Layer 4's addressing scheme uses port numbers (not related to physical ports). The port number identifies the Layer 7 protocol being used.
- Layer 7 (Application) is the interface between applications and the network. Layer 7 protocols such as Hypertext Transfer Protocol Secure (HTTPS) are not applications themselves but provide services for applications to enable them to communicate over the network.
- A host encapsulates application data with a Layer 4 header, Layer 3 header, and Layer 2 header/trailer before being transmitted over the physical medium (cable or radio waves).
- After a message is received by a host, the host de-encapsulates it by inspecting and removing the Layer 2 header and trailer, inspecting and removing the Layer 3 header, inspecting and removing the Layer 4 header, and finally processing the data in the message.
- The contents encapsulated inside each protocol data unit (PDU) are its payload.
- The combination of data and a Layer 4 header is called a segment (L4PDU).
- The combination of a segment and a Layer 3 header is called a packet (L3PDU).
- The combination of a packet and a Layer 2 header/trailer is called a frame (L2PDU).
- Within a computer, each layer provides a service for the layer above it; this is called adjacent-layer interaction.
- Communication between the same layer on different computers is called samelayer interaction.
