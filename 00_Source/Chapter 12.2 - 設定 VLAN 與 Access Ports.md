---
source: [[Chapter 12 - VLAN]]
chapter: 12
section: 12.2
tags: [source-section, acting-ccna, vlan]
---

# Chapter 12.2 - 設定 VLAN 與 Access Ports

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
