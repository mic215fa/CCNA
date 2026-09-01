---
source: [[Chapter 12 - VLAN]]
chapter: 12
section: 12.3
tags: [source-section, acting-ccna, vlan]
---

# Chapter 12.3 - 使用 Trunk Ports 連接 Switches

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
