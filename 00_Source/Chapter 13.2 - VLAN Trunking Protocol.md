---
source: [[Chapter 13 - DTP 與 VTP]]
chapter: 13
section: 13.2
tags: [source-section, acting-ccna, dtp, vtp]
---

# Chapter 13.2 - VLAN Trunking Protocol

VLAN Trunking Protocol (VTP) is another Cisco-proprietary protocol that plays a role in VLAN configuration on Cisco switches. VTP allows switches to automatically synchronize their VLAN database, the file that stores information about the VLANs that exist on the switch. Using VTP, switches in a LAN send each other messages with information about the VLANs in their VLAN database, and the switches synchronize their database according to the latest version of the database.

> [!translation] 逐句繁體中文翻譯
> - VLAN Trunking Protocol (VTP) 是另一個 Cisco 專有協議，在 Cisco switch 上的 VLAN 設定中發揮作用。
> - VTP 允許switch自動同步其 VLAN 資料庫，該資料庫儲存有關switch 上存在的 VLAN 的資訊。
> - 使用 VTP，LAN 中的switch會相互傳送訊息，其中包含有關其 VLAN 資料庫中的 VLAN 的信息，並且switch根據資料庫的最新版本同步其資料庫。

NOTE The VLAN database is stored in a file called vlan.dat in flash memory; use the dir flash: or show flash: commands to view the contents of flash memory. You can use show vlan brief to view the contents of the VLAN database (as we covered in chapter 12).

> [!translation] 逐句繁體中文翻譯
> - 注意 VLAN 資料庫儲存在快閃記憶體中名為 vlan.dat 的檔案中；使用 dir flash: 或 show flash: 指令查看快閃記憶體的內容。
> - 您可以使用 show vlan Brief 來查看 VLAN 資料庫的內容（如我們在第 12 章中所介紹的）。

Figure 13.4 demonstrates why it's important for switches in a LAN to have the same VLANs in their VLAN database; PC1 sends a frame to PC2, but SW2 drops the frame because VLAN 4 isn't in SW2's VLAN database.

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-255_539_1416_554_192.jpg)
Figure 13.4 A missing VLAN on SW2 prevents PC1 from communicating with PC2. 1) PC1 sends a frame to PC2. 2) SW2 drops the frame because VLAN 4 isn't in its VLAN database.

VTP can ensure that all VLANs exist on all switches in the LAN. In a small LAN like figure 13.4, VTP might not seem necessary; manually creating VLAN 4 on SW2 wouldn't be such a hassle. However, in a large LAN with dozens of switches, VTP can both save time and reduce human error by propagating VLAN changes without requiring manual configuration on every single switch.

> [!translation] 逐句繁體中文翻譯
> - VTP 可確保所有 VLAN 都存在於 LAN 中的所有switch 上。
> - 在如圖 13.4 所示的小型 LAN 中，VTP 似乎沒有必要；在 SW2 上手動建立 VLAN 4 不會那麼麻煩。
> - 然而，在擁有數十台switch的大型 LAN 中，VTP 可以透過傳播 VLAN 變更來節省時間並減少人為錯誤，而無需在每台switch 上進行手動設定。

NOTE Like DTP, VTP is a Cisco-proprietary protocol that only runs on Cisco switches; it cannot be used to synchronize VLANs with another vendor's switches.

> [!translation] 逐句繁體中文翻譯
> - 注意 與 DTP 一樣，VTP 是 Cisco 專有協議，只能在 Cisco switch 上運作。它不能用於與其他供應商的switch同步 VLAN。

### 13.2.1 VTP synchronization

Figure 13.5 shows how VLANs created on SW1 can be propagated to SW2 and SW3 using VTP. Starting with only VLAN 1, I created VLANs 2, 3, and 4 on SW1. This causes SW1 to increment the VTP revision number-a number that keeps track of the latest version of the VLAN database. The revision number starts at 0 and is updated each time there is a change to the VLAN database, such as a VLAN being created, deleted, or renamed. This causes SW1 to send VTP messages to SW2 and SW3, informing them of the updates to the VLAN database. SW2 and SW3 then update their VLAN databases to match SW1.

NOTE VLANs 1002 to 1005 also exist by default on Cisco switches and cannot be removed. I won't mention them in this chapter because they are reserved for legacy technologies (Token Ring and FDDI) that are not used in modern networks, as mentioned in chapter 12.

> [!translation] 逐句繁體中文翻譯
> - 說明 VLAN 1002 至 1005 在 Cisco switch 上也預設存在，且無法刪除。
> - 我不會在本章中提及它們，因為它們是為現代network中未使用的遺留技術（令牌環和 FDDI）保留的，如第 12 章所述。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-256_717_1416_421_223.jpg)
Figure 13.5 VLANs created on SW1 are propagated to other switches in the VTP domain "Manning." (1) SW1 creates VLANs 2, 3, and 4, updating its revision number to 3 (incrementing by 1 each time it creates a VLAN). (2) SW1 sends VTP messages to other switches in the VTP domain. (3) SW2 and SW3 add VLANs 2, 3, and 4 to their VLAN databases, updating their revision number to match SW1's.

NOTE VTP messages are only sent out of trunk ports, not access ports.

> [!translation] 逐句繁體中文翻譯
> - 注意 VTP 訊息僅從中繼port傳送，不從存取port傳送。
Figure 13.5 also introduced the concept of the VTP domain-the group of switches in a LAN that all share the same VTP domain name. By default, switches do not have a VTP domain name; in this state, VTP is not active. You can configure VLANs on the device, but it will not send VTP messages to other switches in the LAN. The following example shows the output of show vtp status-a useful command to view the current state of VTP on the switch-before configuring VTP or any VLANs on SW1. We will cover the relevant fields of this output throughout the rest of this chapter.

NOTE A switch that does not have a VTP domain name is said to be in domain NULL.

> [!translation] 逐句繁體中文翻譯
> - 注意 沒有 VTP 網域的switch稱為位於 NULL 域中。

```
SW1# show vtp status
VTP Version capable                       : 1 to 3
VTP version running                       : 1
VTP Domain Name                           :
VTP Pruning Mode                          : Disabled
VTP Traps Generation                      : Disabled
Device ID                                 : 5254.0008.8000
Configuration last modified by 0.0.0.0 at 4-25-23 03:25:46
Local updater ID is 0.0.0.0 (no valid interface found)

Feature VLAN:
--------------
VTP Operating Mode                        : Server
Maximum VLANs supported locally           : 1005
Number of existing VLANs                  : 5
Configuration Revision                    : 0
MD5 digest                                : 0x57 0xCD 0x40 0x65 0x63 0x59 0x47 0xBD
                                            0x56 0x9D 0x4A 0x3E 0xA5 0x69 0x35 0xBC
```

The revision number starts at 0.

> [!translation] 逐句繁體中文翻譯
> - Revision number 從 0 開始。

If you configure a VTP domain name on one switch, it will send VTP messages to other switches, and all switches without a VTP domain name will adopt the new domain name; the command to do so is **vtp domain** *domain-name*. In the following examples, I configure the VTP domain name “Manning” and VLANs 2, 3, and 4 on SW1. Then, I confirm that all switches in the LAN have joined the “Manning” domain and share the same Configuration Revision number—this is the revision number that I mentioned previously:

> [!translation] 逐句繁體中文翻譯
> - 如果在一台 switch 上設定 VTP domain name，它會向其他 switches 傳送 VTP messages；尚未設定 VTP domain name 的 switches 都會採用這個新 domain name。使用的指令是 **vtp domain** *domain-name*。
> - 在以下範例中，我在 SW1 設定 VTP domain name「Manning」，並建立 VLAN 2、3 與 4。
> - 接著確認 LAN 中所有 switches 都已加入「Manning」domain，並具有相同的 Configuration Revision number；這就是前面提過的 revision number：

```
SW1(config)# vtp domain Manning
Changing VTP domain name from NULL to Manning
SW1(config)# vlan 2
SW1(config-vlan)# vlan 3
SW1(config-vlan)# vlan 4
SW1(config-vlan)# end
SW1# show vtp status
. . .
VTP Domain Name                           : Manning
. . .
Number of existing VLANs                  : 8
Configuration Revision                    : 3
SW2# show vtp status
. . .
VTP Domain Name                           : Manning
. . .
Number of existing VLANs                  : 8
Configuration Revision                    : 3
SW3# show vtp status
. . .
VTP Domain Name                           : Manning
. . .
Number of existing VLANs                  : 8
Configuration Revision                    : 3
```

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-257_228_694_1263_929.jpg)

Because the revision number is used to keep track of the latest version of the VLAN database, switches will only synchronize their VLAN database if they receive a VTP message from a switch with a higher revision number, not a lower one; a VTP message with a lower (or equal) revision number is considered old information.

> [!translation] 逐句繁體中文翻譯
> - Revision number 用來追蹤 VLAN database 的最新版本，因此 switch 只會在收到來自較高 revision number switch 的 VTP message 時同步 VLAN database，而不會接受較低的版本。
> - Revision number 較低或相同的 VTP message 會被視為舊資訊。

### 13.2.2 VTP modes

A Cisco switch can operate in one of four VTP modes: server, client, transparent, and off, each mode with its own characteristics. The VTP mode can be configured with the vtp mode mode command. Switches in server mode and client mode actively participate in VTP and synchronize their VLAN databases to match each other, whereas switches in transparent mode and off mode do not. Table 13.3 summarizes each mode.

> [!translation] 逐句繁體中文翻譯
> - Cisco switch可以在四種 VTP 模式之一下運作：伺服器、用戶端、透明和關閉，每種模式都有自己的功能。
> - VTP 模式可以使用 vtp mode mode 指令進行設定。
> - 伺服器模式和用戶端模式下的switch主動參與 VTP 並同步其 VLAN 資料庫以相互匹配，而透明模式和關閉模式下的switch則不會。
> - 表 13.3 總結了每種模式。

**Table 13.3 — VTP modes／VTP 模式**

| Mode／模式      | Description／說明 |
| :------------- | :---------------- |
| `server`       | This is the default mode. The switch can create, modify, and delete VLANs. It advertises VLAN database changes and synchronizes its database when it receives an advertisement with a higher revision number.<br>這是預設模式。Switch 可以建立、修改及刪除 VLAN；它會通告 VLAN database 的變更，並在收到具有較高 revision number 的 advertisement 時同步自己的 database。 |
| `client`       | The switch cannot create, modify, or delete VLANs, but otherwise behaves like a server.<br>Switch 無法建立、修改或刪除 VLAN；除此之外，其行為與 server 相同。 |
| `transparent`  | The switch can create, modify, and delete VLANs locally, but it neither advertises its own VLAN database changes nor synchronizes with other switches. It does not directly participate in the VTP domain, but it forwards VTP messages between switches in the same domain.<br>Switch 可以在本機建立、修改及刪除 VLAN，但不會通告自己的 VLAN database 變更，也不會與其他 switches 同步。它不直接參與 VTP domain，但會在同一 domain 的 switches 之間轉送 VTP messages。 |
| `off`          | The switch can create, modify, and delete VLANs locally, but it neither advertises nor synchronizes its VLAN database. It does not participate in VTP and does not forward VTP messages.<br>Switch 可以在本機建立、修改及刪除 VLAN，但不會通告或同步 VLAN database。它完全不參與 VTP，也不會轉送 VTP messages。 |


Switches are in VTP server mode by default. In this mode, a switch can create, modify (ie. rename), and delete VLANs, and those changes will be advertised to other switches in the domain. A VTP server will also synchronize its own VLAN database if it receives a VTP message with a higher revision number.

> [!translation] 逐句繁體中文翻譯
> - switch預設為 VTP 伺服器模式。
> - 在此模式下，switch可以建立、修改（即重新命名）和刪除 VLAN，並且這些變更將通告給網域中的其他switch。
> - 如果 VTP 伺服器收到具有較高修訂號的 VTP 訊息，它也會同步其自己的 VLAN 資料庫。

VTP client mode is similar to server mode, except for one difference: the switch cannot create/modify/delete VLANs. I demonstrate this in the following example by configuring SW3 VTP client mode and then attempting to create VLAN 5:

> [!translation] 逐句繁體中文翻譯
> - VTP 用戶端模式與伺服器模式類似，但有一點不同：switch無法建立/修改/刪除 VLAN。
> - 我在以下範例中透過設定 SW3 VTP 用戶端模式然後嘗試建立 VLAN 5 來示範這一點：
![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-258_240_1256_1614_348.jpg)

NOTE Cisco recommends that all switches in the domain be in server mode if they have sufficient memory resources. Any modern switch should have sufficient resources to store VLAN information, so you can safely leave all switches in VTP server mode.

> [!translation] 逐句繁體中文翻譯
> - 注意 如果網域中的所有switch有足夠的記憶體資源，Cisco 建議將其設定為伺服器模式。
> - 任何現代switch都應該有足夠的資源來儲存 VLAN 訊息，以便您可以安全地將所有switch置於 VTP 伺服器模式。

In a VTP domain, in most cases, all switches will be in server (or client) mode; they are the modes that take advantage of VTP's VLAN database synchronization. Transparent mode prevents the switch from synchronizing its VLAN database with other switches. You can create, modify, and delete VLANs on the switch, but it will not advertise those changes to other switches. However, the switch will forward VTP messages between switches in the same domain. Transparent mode should be used in cases where a switch needs to be managed independently from other switches, without interrupting VTP's operation on the rest of the switches in the LAN. Figure 13.6 demonstrates how VTP transparent mode works; SW2 has a VLAN database separate from SW1 and SW3 but forwards SW1's VTP messages to SW3.

> [!translation] 逐句繁體中文翻譯
> - 在一個VTP域中，大多數情況下，所有switch都會處於伺服器（或用戶端）模式；它們是利用 VTP 的 VLAN 資料庫同步的模式。
> - 透明模式會阻止switch與其他switch同步其 VLAN 資料庫。
> - 您可以在switch 上建立、修改和刪除 VLAN，但它不會將這些變更通告給其他switch。
> - 但是，switch將在同一網域中的switch之間轉送 VTP 訊息。
> - 如果需要獨立於其他switch來管理交換機，而不會中斷 LAN 中其餘switch 上的 VTP 操作，則應使用透明模式。
> - 圖 13.6 示範了 VTP 透明模式的工作原理； SW2 具有與 SW1 和 SW3 分開的 VLAN 資料庫，但將 SW1 的 VTP 訊息轉送到 SW3。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-259_845_1414_662_192.jpg)
Figure 13.6 SW2, in transparent mode, forwards VTP messages but does not synchronize its VLAN database. (1) SW1 creates VLAN 5 and updates its revision number. (2) SW2 forwards SW1's VTP messages to SW3 but doesn't synchronize its VLAN database. (3) SW3 syncs its VLAN database to match SW1 and updates its revision number.

NOTE A switch in VTP transparent mode will always have revision number 0.
The final VTP mode is off, which disables VTP on the switch. Like transparent mode, the switch won't synchronize its VLAN database with other switches, but it also won't forward VTP messages between switches using VTP. If VTP is not being used in the LAN, you should configure vtp mode off to disable VTP on all switches.

> [!translation] 逐句繁體中文翻譯
> - 注意處於 VTP 透明模式的switch的修訂號碼始終為 0。
> - 最終 VTP 模式處於關閉狀態，這會停用switch 上的 VTP。
> - 與透明模式一樣，switch不會將其 VLAN 資料庫與其他switch同步，但它也不會使用 VTP 在switch之間轉送 VTP 訊息。
> - 如果 LAN 中未使用 VTP，則應設定 vtp 模式關閉以停用所有switch 上的 VTP。

### 13.2.3 VTP versions

You may have noticed the following lines when I showed the complete output of show vtp status in section 13.2.1:

> [!translation] 逐句繁體中文翻譯
> - 當我在第 13.2.1 節中顯示 show vtp status 的完整輸出時，您可能已經注意到以下幾行：

```
SW1# show vtp status
VTP Version capable : 1 to 3
VTP version running : 1
. . .
```

There are three different versions of VTP available, and version 1 is the default. You can configure the VTP version with the vtp version version command. Versions 1 and 2 are very similar; one difference is that version 2 supports Token Ring, which is not relevant to modern networks, so there isn't much reason to use version 2 over version 1.

> [!translation] 逐句繁體中文翻譯
> - VTP 有三個不同版本可用，版本 1 是預設版本。
> - 您可以使用 vtp version version 指令設定 VTP 版本。
> - 版本 1 和 2 非常相似；一個差異是版本 2 支援令牌環，這與現代network無關，因此沒有太多理由使用版本 2 而不是版本 1。

Version 3, on the other hand, brings various improvements over the previous two versions and should always be preferred if using VTP. Let's look at a few of those improvements that are relevant to the CCNA. We already covered one of the improvements: off mode. Before version 3, VTP only had three modes: server, client, and transparent. There was no way to actually disable VTP on a switch; the closest thing was to configure all switches in transparent mode.

> [!translation] 逐句繁體中文翻譯
> - 另一方面，版本 3 比前兩個版本帶來了各種改進，如果使用 VTP，則應始終首選。
> - 讓我們來看看與 CCNA 相關的一些改進。
> - 我們已經介紹了其中一項改進：關閉模式。
> - 在版本 3 之前，VTP 只有三種模式：伺服器、客戶端和透明。
> - 無法在switch 上實際停用 VTP；最接近的是將所有switch配置為透明模式。

NOTE Although off mode was added in VTP version 3, switches that support version 3 can use off mode even if they are running version 1 or 2.

> [!translation] 逐句繁體中文翻譯
> - 注意 儘管 VTP 版本 3 中新增了關閉模式，但支援版本 3 的switch即使運行版本 1 或 2 也可以使用關閉模式。

Now let's cover two other significant changes brought by VTP version 3: the primary server and extended-range VLAN support.

> [!translation] 逐句繁體中文翻譯
> - 現在讓我們介紹 VTP 版本 3 帶來的另外兩個重大變更：主伺服器和擴充範圍 VLAN 支援。

## The primary server

In VTP version 3, only one switch in the VTP domain can create, modify, and delete VLANs: the primary server. Other VTP servers (now called secondary servers) are no different than VTP clients, except that you can make a secondary server become the primary server with the command vtp primary in privileged EXEC mode.

> [!translation] 逐句繁體中文翻譯
> - 在 VTP 版本 3 中，VTP 網域中只有一台switch可以建立、修改和刪除 VLAN：主伺服器。
> - 其他 VTP 伺服器（現在稱為輔助伺服器）與 VTP 用戶端沒有什麼不同，只是您可以在特權 EXEC 模式下使用 vtp Primary 命令使輔助伺服器成為主伺服器。

NOTE Although most VTP commands are global config mode commands, the vtp primary command is a privileged EXEC mode command.

> [!translation] 逐句繁體中文翻譯
> - 注意 雖然大多數 VTP 指令是全域設定模式指令，但 vtp Primary 指令是特權 EXEC 模式指令。

In the following example, I enable VTP version 3 on SW1 and attempt to create VLAN 6, which fails. I then use the vtp primary command to make SW1 the primary server, and I am then able to create VLAN 6:

> [!translation] 逐句繁體中文翻譯
> - 在以下範例中，我在 SW1 上啟用 VTP 版本 3 並嘗試建立 VLAN 6，但失敗。
> - 然後，我使用 vtp Primary 命令使 SW1 成為主伺服器，然後我就可以建立 VLAN 6：
![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-260_352_1225_1772_346.jpg)

```
SW1(config)# vlan 6
SW1(config-vlan)# exit
```

SW1 can now create VLANs.

> [!translation] 逐句繁體中文翻譯
> - SW1 現在可以建立 VLAN。

NOTE In this example, I executed the vtp primary command in global config mode by adding do in front of the command. The vtp primary command on its own does not work in global config mode; it's a privileged EXEC mode command.

> [!translation] 逐句繁體中文翻譯
> - 注意 在本例中，我透過在命令前面新增 do 在全域設定模式下執行 vtp Primary 命令。
> - vtp 主指令本身在全域設定模式下不起作用；這是一個特權執行模式指令。

Only one switch in the domain can be the primary server; if you use the vtp primary command on a second switch, the first one will revert to being a secondary server. The reason for allowing only one switch in the domain to create, modify, and delete VLANs is to avoid the problem of a newly-connected switch overwriting the VLAN database for the domain-a problem we'll cover in section 13.2.4.

> [!translation] 逐句繁體中文翻譯
> - 網域內只能有一台switch作為主伺服器；如果您在第二台switch 上使用 vtp Primary 指令，第一台switch將恢復為輔助伺服器。
> - 只允許網域中的一台switch建立、修改和刪除 VLAN 的原因是為了避免新連線的switch覆蓋域的 VLAN 資料庫的問題，我們將在第 13.2.4 節中討論這個問題。

## Extended-range VLANs

In old versions of Cisco IOS, only VLANs 1 to 1005 were available for use; these are called the normal-range VLANs. In those versions of IOS, VLANs 1006 to 4094 were reserved for internal use by applications on the switch; a user could not create them or assign ports to those VLANs. In a later version of IOS, VLANs 1006 to 4094 were made available and called the extended-range VLANs; these days, all Cisco switches support both the normal- and extended-range VLANs.

> [!translation] 逐句繁體中文翻譯
> - 在舊版的 Cisco IOS 中，只有 VLAN 1 到 1005 可用；這些稱為正常範圍 VLAN。
> - 在這些版本的 IOS 中，VLAN 1006 到 4094 保留供switch 上的應用程式內部使用；使用者無法建立它們或將port指派給這些 VLAN。
> - 在 IOS 的更高版本中，VLAN 1006 至 4094 可用並稱為擴充範圍 VLAN；目前，所有 Cisco switch都支援一般 VLAN 和擴充範圍 VLAN。

Even after extended-range VLANs were made available for use, VTP versions 1 and 2 only supported normal-range VLANs. The only way to create extended-range VLANs on switches before VTP version 3 was to configure the switch in transparent mode, rendering it unable to participate in the VTP domain.

> [!translation] 逐句繁體中文翻譯
> - 即使擴充範圍 VLAN 可供使用後，VTP 版本 1 和 2 也僅支援正常範圍 VLAN。
> - 在 VTP 版本 3 之前的switch 上建立擴展範圍 VLAN 的唯一方法是將switch配置為透明模式，使其無法參與 VTP 域。

VTP version 3 brought the ability to create and propagate extended-range VLANs; the primary server can create extended-range VLANs and propagate them to other switches in the VTP domain.

> [!translation] 逐句繁體中文翻譯
> - VTP 版本 3 帶來了建立和傳播擴展範圍 VLAN 的能力；主伺服器可以建立擴展範圍的 VLAN 並將其傳播到 VTP 域中的其他switch。

### 13.2.4 Is VTP dangerous?

VTP doesn't have a very good reputation-and for good reason: it has caused a lot of network outages over the years. First, let's look at why VTP has a bad reputation, and then we'll see how version 3 fixes the problems with VTP.

> [!translation] 逐句繁體中文翻譯
> - VTP 的聲譽並不好，這是有充分理由的：多年來它造成了大量的network中斷。
> - 首先，讓我們看看為什麼 VTP 聲譽不佳，然後我們將了解版本 3 如何修復 VTP 的問題。

The danger of VTP is the potential for a newly connected switch to overwrite the VLAN database of all switches in the domain. Because switches using VTP synchronize their VLAN database to the switch with the highest revision number, if the newly connected switch has a higher revision number (and is in the same domain), all switches in the domain will synchronize to match it. Figure 13.7 illustrates how this can happen: SW4, with a revision number of 50, is connected to a VTP domain with a revision number of 5, causing the other switches to synchronize with it. As a result, hosts in VLANs 10, 20, and 30 will lose network connectivity; those VLANs no longer exist in the network!

> [!translation] 逐句繁體中文翻譯
> - VTP 的危險在於新連線的switch可能會覆蓋域中所有switch的 VLAN 資料庫。
> - 由於使用 VTP 的switch將其 VLAN 資料庫同步到具有最高修訂號的交換機，因此如果新連接的switch具有更高的修訂號（並且位於同一域中），則該域中的所有switch都將進行同步以匹配它。
> - 圖 13.7 說明了這種情況是如何發生的：版本號為 50 的 SW4 連接到版本號為 5 的 VTP 域，導致其他switch與其同步。
> - 因此，VLAN 10、20 和 30 中的host將失去network連線；這些 VLAN 不再存在於network中！

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-262_727_1421_179_219.jpg)
Figure 13.7 SW4 is connected to the network and overwrites the VLAN databases of SW1, SW2, and SW3. (1) SW4 is connected to the LAN. (2) SW4 sends VTP messages to the other switches. (3) Because SW4 has a higher revision number (50 vs. 5), SW1, SW2, and SW3 synchronize their VLAN databases to match SW4. Hosts in VLANs 10,20, and 30 will be unable to communicate over the network because their VLANs no longer exist.

NOTE You may be wondering, how could a newly added switch have a high revision number in the first place? One possibility is that it was used as a lab switch for testing and verifying before being added to the corporate network.

> [!translation] 逐句繁體中文翻譯
> - 注意您可能想知道，新新增的switch怎麼可能具有高版本號碼？
> - 一種可能性是，它在添加到公司network之前被用作測試和驗證的實驗室switch。

This is a very careless mistake, and standardized procedures for adding new devices to the network would prevent it from happening; one recommended procedure is to reset the VTP revision number of a switch to 0 before connecting it to the network. There are three methods for doing so:

> [!translation] 逐句繁體中文翻譯
> - 這是一個非常粗心的錯誤，在network上新增設備的標準化程序可以防止這種情況發生；一種建議的步驟是在將switch連接到network之前將switch的 VTP 修訂號重設為 0。
> - 可透過三種方法執行此操作：

- Change the VTP domain name to a different one and then back to the original name.
- Change the VTP mode to transparent and then back to server or client (only works in versions 1 and 2).
- Change the VTP mode to off and then back to server or client (only works in versions 1 and 2).

EXAM TIP Remember these three methods for resetting the revision number.
Resetting the revision number to 0 eliminates the risk of a newly added switch overwriting the VLAN database of switches in the network. However, it's an unfortunate truth that many corporations have few, if any, standardized procedures for such things, and
in any case, people can get careless. As a result, many people have been burned by VTP, giving it a bad reputation.

> [!translation] 逐句繁體中文翻譯
> - 考試提示 請記住這三種重設修訂版本號碼的方法。
> - 將修訂號重設為 0 可消除新新增的switch覆蓋network中switch的 VLAN 資料庫的風險。
> - 然而，不幸的是，許多公司幾乎沒有針對此類事情的標準化程序，而且在任何情況下，人們都可能會粗心大意。
> - 結果，許多人被 VTP 傷害了，給它帶來了壞名聲。

However, the primary server mechanism in version 3 eliminates this risk; switches will only synchronize to the primary server, so even if a new switch with a higher revision number is connected to the LAN, switches in the LAN won't synchronize to it. When using version 3, there's no need to be afraid of VTP, and it can be a great tool for automating some of the workflows of configuring a network.

> [!translation] 逐句繁體中文翻譯
> - 然而，版本3中的主伺服器機制消除了這種風險；switch只會同步到主伺服器，因此即使具有更高版本號碼的新switch連接到 LAN，LAN 中的switch也不會同步到它。
> - 使用版本 3 時，無需擔心 VTP，它可以成為自動化某些network設定工作流程的絕佳工具。

## Summary

- Dynamic Trunking Protocol (DTP) allows Cisco switches to automatically determine the operational mode of their ports.
- A port's administrative mode is how it is configured with the switchport mode command, and its operational mode is the mode it operates in (access or trunk).
- Use the show interfaces interface-name switchport command to view the administrative and operational modes of a port.
- A port configured with switchport mode access will always operate as an access port, and a port configured with switchport mode trunk will always operate as a trunk port.
- switchport mode dynamic auto and switchport mode dynamic desirable configure the port to use DTP to determine its operational mode.
- dynamic auto mode does not actively try to form a trunk with its neighbor but will form a trunk if the neighbor's mode is trunk or dynamic desirable.
- dynamic desirable mode actively tries to form a trunk with its neighbor and will form a trunk if the neighbor's mode is trunk, dynamic auto, or dynamic desirable.
- DTP is considered a security vulnerability and should be disabled. It is considered best practice to manually configure each port's mode and disable DTP with switchport nonegotiate on each port.
- VLAN Trunking Protocol (VTP) allows Cisco switches to synchronize their VLAN database-the file that stores information about VLANs on the switch (vlan.dat).
- The VTP revision number is used to keep track of the latest version of the VLAN database. Each time a change is made, the revision number is incremented by 1. A switch will synchronize to match a higher revision number but not a lower (or equal) one.
- The VTP domain is the group of switches in a LAN that share the same VTP domain name; a switch will only synchronize with another switch in the same domain.
- By default, a switch has no domain name; it is said to be in domain NULL. In this state, the switch can create/modify/delete VLANs, but it won't send VTP messages to other switches.

- You can configure a switch's VTP domain name with the vtp domain domain-name command.
- Use the show vtp status command to view the current state of VTP on the switch.
- A switch can operate in one of four VTP modes: server, client, transparent, and off. Use the vtp mode mode command to configure the mode (server is the default).
- A switch in VTP server mode can create/modify/delete VLANs. It will advertise changes to its VLAN database and synchronize its VLAN database upon receiving an advertisement with a higher revision number.
- A switch in VTP client mode cannot create/modify/delete VLANs but otherwise behaves the same as a server.
- A switch in VTP transparent mode can create/modify/delete VLANs but operates independently from other switches in the VTP domain. It will forward VTP messages between switches in the VTP domain but will not send its own VTP messages or synchronize its VLAN database with other switches.
- A switch in VTP off mode can create/modify/delete VLANs but does not participate in VTP at all.
- There are three versions of VTP: 1, 2, and 3. Versions 1 and 2 are very similar, but version 3 brought many improvements.
- VTP version 3 introduced off mode; before, VTP couldn't be disabled. The closest thing was to configure all switches in VTP transparent mode.
- In VTP version 3, only one switch in the domain can create, modify, and delete VLANs: the primary server. Switches in the domain will only synchronize with the primary server. Other servers are called secondary servers; they function the same as VTP clients.
- Use the vtp primary command (in privileged EXEC mode) on a VTP server to make it the primary server. There can only be one; if you configure vtp primary on a second server, the first one will revert to being a secondary server.
- VTP version 3 is the only version that supports extended-range VLANs (VLANs 1006 to 4094). Versions 1 and 2 only support normal-range VLANs (VLANs 1 to 1005).
- One risk of VTP is that a newly connected switch with a higher revision number can overwrite the VLAN database of all switches in the LAN. Version 3 solves this problem because switches will only synchronize with the primary server.
- To reset a switch's VTP revision number to 0, you can change the domain name to a different one and then back to the original. Alternatively, you can change the mode to transparent or off and then back to server or client, but this only works in versions 1 and 2.

> [!translation] Summary 逐點繁體中文翻譯
> - Dynamic Trunking Protocol（DTP）讓 Cisco switches 自動決定 ports 的 operational mode。
> - Port 的 administrative mode 是透過 `switchport mode` 指令設定的模式；operational mode 則是 port 實際運作的模式，也就是 access 或 trunk。
> - 使用 `show interfaces interface-name switchport` 查看 port 的 administrative mode 與 operational mode。
> - 設定 `switchport mode access` 的 port 永遠以 access port 運作；設定 `switchport mode trunk` 的 port 永遠以 trunk port 運作。
> - `switchport mode dynamic auto` 與 `switchport mode dynamic desirable` 會讓 port 使用 DTP 決定 operational mode。
> - `dynamic auto` 不會主動嘗試與鄰居形成 trunk；但鄰居為 `trunk` 或 `dynamic desirable` 時會形成 trunk。
> - `dynamic desirable` 會主動嘗試與鄰居形成 trunk；鄰居為 `trunk`、`dynamic auto` 或 `dynamic desirable` 時都會形成 trunk。
> - DTP 被視為安全弱點，應予以停用。最佳實務是手動設定每個 port 的 mode，並在每個 port 使用 `switchport nonegotiate` 停用 DTP。
> - VLAN Trunking Protocol（VTP）讓 Cisco switches 同步 VLAN database；這個 database 儲存 switch 上的 VLAN 資訊，檔名為 `vlan.dat`。
> - VTP revision number 用來追蹤 VLAN database 的最新版本。每次發生變更時，revision number 加 1。Switch 會同步較高的 revision number，但不會同步較低或相同的 revision number。
> - VTP domain 是 LAN 中共用相同 VTP domain name 的 switches 群組；switch 只會與相同 domain 中的其他 switch 同步。
> - Switch 預設沒有 domain name，稱為位於 domain NULL。在此狀態下，switch 可以建立、修改及刪除 VLAN，但不會向其他 switches 傳送 VTP messages。
> - 使用 `vtp domain domain-name` 設定 switch 的 VTP domain name。
> - 使用 `show vtp status` 查看 switch 目前的 VTP 狀態。
> - Switch 可在 server、client、transparent 與 off 四種 VTP modes 之一運作。使用 `vtp mode mode` 設定；server 是預設模式。
> - VTP server mode 的 switch 可以建立、修改及刪除 VLAN。它會通告 VLAN database 的變更，並在收到較高 revision number 的 advertisement 時同步自己的 VLAN database。
> - VTP client mode 的 switch 無法建立、修改或刪除 VLAN；除此之外，其行為與 server 相同。
> - VTP transparent mode 的 switch 可以建立、修改及刪除 VLAN，但獨立於 VTP domain 中的其他 switches 運作。它會在相同 domain 的 switches 之間轉送 VTP messages，但不會傳送自己的 VTP messages，也不會與其他 switches 同步 VLAN database。
> - VTP off mode 的 switch 可以建立、修改及刪除 VLAN，但完全不參與 VTP。
> - VTP 有 versions 1、2、3。Versions 1 與 2 非常相似；version 3 則帶來多項改進。
> - VTP version 3 加入 off mode；此前無法真正停用 VTP，最接近的做法是把所有 switches 設為 VTP transparent mode。
> - 在 VTP version 3 中，domain 內只有 primary server 能建立、修改及刪除 VLAN。Domain 內的 switches 只會與 primary server 同步；其他 servers 稱為 secondary servers，功能與 VTP clients 相同。
> - 在 VTP server 上以 privileged EXEC mode 使用 `vtp primary`，可使其成為 primary server。同一時間只能有一台；若在第二台 server 設定 `vtp primary`，第一台會恢復為 secondary server。
> - 只有 VTP version 3 支援 extended-range VLANs（VLAN 1006–4094）；versions 1 與 2 僅支援 normal-range VLANs（VLAN 1–1005）。
> - VTP 的風險之一，是新連接且具有較高 revision number 的 switch 可能覆寫 LAN 中所有 switches 的 VLAN database。Version 3 藉由只允許 switches 與 primary server 同步來解決此問題。
> - 若要把 switch 的 VTP revision number 重設為 0，可以先把 domain name 改成其他名稱，再改回原名稱。另一種方法是先切換為 transparent 或 off mode，再切回 server 或 client mode；但這種方法只適用於 versions 1 與 2。
