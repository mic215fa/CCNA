---
source: [[Chapter 13 - DTP 與 VTP]]
chapter: 13
section: 13.1
tags: [source-section, acting-ccna, dtp, vtp]
---

# Chapter 13.1 - Dynamic Trunking Protocol

Dynamic Trunking Protocol (DTP) is a Cisco-proprietary protocol that allows Cisco switches to automatically determine the operational mode of their ports. A port's operational mode is the mode the port operates in (access or trunk), as opposed to the administrative mode, which is the port's configured mode (using the switchport mode command). If a switchport is manually configured as an access port or trunk port, the port's administrative and operational modes will be identical:

> [!translation] 逐句繁體中文翻譯
> - Dynamic Trunking Protocol (DTP) 是 Cisco 專有協議，可讓 Cisco switch自動確定其port的operational mode。
> - port的operational mode是port運作的模式（存取或中繼），而不是administrative mode，administrative mode是port的設定模式（使用 switchport mode 指令）。
> - 如果手動將switch端口配置為接入端口或中繼端口，則該端口的管理和operational mode將相同：

- A port configured with switchport mode access (administrative mode) will always operate as an access port (operational mode).
- A port configured with switchport mode trunk (administrative mode) will always operate as a trunk port (operational mode).

When using DTP, neighboring switches send each other DTP messages, informing each other of their port's administrative mode. Depending on the combination of administrative modes of the connected ports, the switches decide the appropriate operational mode for their ports.

> [!translation] 逐句繁體中文翻譯
> - 使用 DTP 時，鄰近switch會相互傳送 DTP 訊息，告知對方其port的administrative mode。
> - 根據所連接port的administrative mode組合，switch決定其port的適當operational mode。

NOTE Only switches use DTP; to make a switch port connected to a router operate as a trunk port (when using router on a stick), you must manually configure trunk mode on the port.

> [!translation] 逐句繁體中文翻譯
> - 注意 只有switch使用 DTP；要使連接到router的switchport作為中繼port（使用 Router on a Stick時），必須在port 上手動設定中繼模式。

DTP was developed to streamline the deployment of switches by requiring less manual configuration of port modes, but it was found to be a security vulnerability (more on that later in this chapter), so today it is generally considered best practice to disable DTP on switch ports. However, DTP is active on Cisco switches by default, so it's important to understand how it works, even if only to know how to disable it.

> [!translation] 逐句繁體中文翻譯
> - DTP 的開發目的是透過減少對port模式的手動配置來簡化switch的部署，但人們發現它是一個安全漏洞（本章稍後會詳細介紹），因此目前通常認為在switchport 上停用 DTP 是最佳實踐。
> - 但是，預設情況下，DTP 在 Cisco switch 上處於活動狀態，因此了解它的工作原理非常重要，即使只是知道如何停用它。

NOTE As a Cisco-proprietary protocol, DTP only works on Cisco switches. If you want your Cisco switch to have a trunk connection with a switch from another vendor, you must manually configure trunk mode with switchport mode trunk.

> [!translation] 逐句繁體中文翻譯
> - 說明 DTP 是 Cisco 專有協議，僅適用於 Cisco switch。
> - 如果要讓 Cisco switch 與其他廠商的 switch 建立 trunk connection，必須使用 `switchport mode trunk` 手動設定 trunk mode。

### 13.1.1 DTP negotiation

In chapter 12, we manually configured access and trunk ports. However, there are two other options for the switchport mode command: switchport mode dynamic auto and switchport mode dynamic desirable. Rather than explicitly specifying which mode the port should operate in, these administrative modes tell the switch to use DTP to determine the port's operational mode. By default, a port in one of these modes will operate as an access port, but if the connected switches both agree, a trunk link will be formed.

> [!translation] 逐句繁體中文翻譯
> - 在第 12 章中，我們手動設定了存取port和中繼port。
> - 不過，`switchport mode` 指令還有兩個選項：`switchport mode dynamic auto` 與 `switchport mode dynamic desirable`。
> - 這些administrative mode不是明確指定port應在哪種模式下運行，而是告訴switch使用 DTP 來確定port的operational mode。
> - 預設情況下，處於這些模式之一的port將作為access port運行，但如果連接的switch都同意，則會形成trunk link。

Figure 13.1 shows two Cisco switches connected by their G0/0 ports, each with an administrative mode of dynamic auto. The result is an access link; the switches agree
that they will not form a trunk. This process of exchanging DTP messages and agreeing upon an operational mode is called DTP negotiation.

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-250_341_872_303_350.jpg)
Figure 13.1 Two connected switches negotiate their ports' operational mode by sending DTP messages. Both ports use the default administrative mode of dynamic auto, resulting in an access link; SW1 G0/0 and SW2 G0/0 operate as access ports.

NOTE dynamic auto is the default administrative mode for all Cisco switch ports, so by default, two Cisco switches that are connected will not form a trunk; the connection will remain in access mode.

> [!translation] 逐句繁體中文翻譯
> - 注意 dynamic auto是所有 Cisco switchport的預設administrative mode，因此預設情況下，連接的兩台 Cisco switch不會形成中繼；連線將保持在存取模式。

To view a port's administrative and operational modes, use the show interfaces interface-name switchport command, as in the following example. Note that the operational mode is static access; this means it is an access port that is assigned to a specific VLAN (the VLAN specified in the switchport access vlan command, or the default of VLAN 1). There are also dynamic access ports, in which the switch decides the port's VLAN based on the connected device, but dynamic access ports are beyond the scope of the CCNA:

> [!translation] 逐句繁體中文翻譯
> - 若要查看 port 的 administrative mode 與 operational mode，請使用 `show interfaces interface-name switchport` 指令，如下例所示。
> - 注意operational mode為靜態存取；這表示它是指派給特定 VLAN（在 switchport access vlan 指令中指定的 VLAN，或預設 VLAN 1）的存取port。
> - 還有動態存取端口，其中switch根據連接的設備決定端口的 VLAN，但動態訪問端口超出了 CCNA 的範圍：

```
SW1# show interfaces g0/0 switchport
Name: Gig0/O
Switchport: Enabled
Administrative Mode: dynamic auto
Operational Mode: static access
. . .
```

Although a port with administrative mode dynamic auto uses DTP to negotiate its operational mode, it does not actively try to form a trunk link with its neighbor; that's why two connected ports in dynamic auto mode do not form a trunk, as we saw in figure 13.1.

> [!translation] 逐句繁體中文翻譯
> - 儘管具有administrative modedynamic auto的port使用 DTP 來協商其operational mode，但它不會主動嘗試與其鄰居形成trunk link；這就是為什麼dynamic auto模式下的兩個port不會形成中繼，如圖 13.1 所示。

If we change the administrative mode of SW1 G0/0 to dynamic desirable, the result is different; a port in dynamic desirable mode actively attempts to form a trunk. Figure 13.2 shows the result: a trunk link between SW1 and SW2.

> [!translation] 逐句繁體中文翻譯
> - 如果將SW1 G0/0的administrative mode改為dynamic desirable，則結果不同；處於dynamic desirable模式的port主動嘗試形成中繼。
> - 圖 13.2 顯示了結果：SW1 和 SW2 之間的trunk link。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-251_341_872_183_318.jpg)
Figure 13.2 SW1 and SW2 negotiate to form a trunk link. SW1 G0/0's administrative mode is dynamic desirable, and SW2 G0/0's is dynamic auto. The result is an operational mode of trunk.

As shown in the following example, SW1 G0/0's operational mode is now trunk:

> [!translation] 逐句繁體中文翻譯
> - 如下例所示，SW1 G0/0 的operational mode現在為 trunk：

```
SW1# show interfaces g0/0 switchport
Name: Gig0/O
Switchport: Enabled
Administrative Mode: dynamic desirable
Operational Mode: trunk
. . .
```

G0/0 functions as a trunk port.

> [!translation] 逐句繁體中文翻譯
> - G0/0 作為 trunk port 運作。

Table 13.1 lists the four administrative modes that can be configured with the switchport mode command and gives a brief description of each.

**Table 13.1 — Switch port administrative modes／Switch port 管理模式**

| Mode／模式          | Description／說明                                                                                                                                                                                                                                                          | Sends DTP?／傳送 DTP？ |
| :----------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :--------------------: |
| `access`           | Manually configures an access port. Operational mode will always be access.<br>手動設定為 access port；operational mode 永遠是 access。                                                                                                                                       | No／否                 |
| `trunk`            | Manually configures a trunk port. Operational mode will always be trunk.<br>手動設定為 trunk port；operational mode 永遠是 trunk。                                                                                                                                           | Yes／是                |
| `dynamic auto`     | Uses DTP to negotiate but does not actively try to form a trunk. It forms a trunk when connected to `trunk` or `dynamic desirable`.<br>使用 DTP 協商，但不主動嘗試形成 trunk；連接到 `trunk` 或 `dynamic desirable` port 時會形成 trunk。                                         | Yes／是                |
| `dynamic desirable` | Uses DTP to negotiate and actively tries to form a trunk. It forms a trunk when connected to `trunk`, `dynamic auto`, or `dynamic desirable`.<br>使用 DTP 協商並主動嘗試形成 trunk；連接到 `trunk`、`dynamic auto` 或 `dynamic desirable` port 時會形成 trunk。                       | Yes／是                |


NOTE Although administrative mode trunk manually configures a trunk port, the port will still send DTP messages; the purpose is to ensure that the neighboring port also operates in trunk mode (if the neighbor is in dynamic auto or dynamic desirable mode).

> [!translation] 逐句繁體中文翻譯
> - 說明 雖然administrative modeTrunk 手動配置Trunk 端口，但該端口仍會發送DTP 訊息。目的是確保相鄰port也運行在中繼模式下（如果鄰居處於dynamic auto或動態理想模式）。

For the CCNA exam, it's important to understand the resulting operational mode of each combination of administrative modes; table 13.2 shows the results of each combination. Note that access + trunk is not a valid combination; either the switches will detect the mismatch and block the link, or the traffic that passes through the link will be limited to only the trunk port's native VLAN and the access port's VLAN (because both are untagged). Either way, don't use this combination!

> [!translation] 逐句繁體中文翻譯
> - 對於 CCNA 考試，了解每種administrative mode組合所產生的operational mode非常重要；表 13.2 顯示了每種組合的結果。
> - 請注意，access + trunk 不是有效的組合；switch要么檢測到不匹配並阻止link，要么通過link的traffic將僅限於中繼端口的本機 VLAN 和接入端口的 VLAN（因為兩者都未標記）。
> - 無論哪種方式，都不要使用此組合！

**Table 13.2 — Resulting operational mode／協商後的 operational mode**

| Local administrative mode／本地設定 | Neighbor: `access` | Neighbor: `trunk` | Neighbor: `dynamic desirable` | Neighbor: `dynamic auto` |
| :---------------------------------- | :----------------: | :---------------: | :-----------------------------: | :----------------------: |
| `access`                            | `access`           | Invalid／無效     | `access`                        | `access`                 |
| `trunk`                             | Invalid／無效      | `trunk`           | `trunk`                         | `trunk`                  |
| `dynamic desirable`                 | `access`           | `trunk`           | `trunk`                         | `trunk`                  |
| `dynamic auto`                      | `access`           | `trunk`           | `trunk`                         | `access`                 |


EXAM TIP Make sure that you can identify the operational mode that results from each combination of administrative modes; it's a potential exam question.

> [!translation] 逐句繁體中文翻譯
> - 考試提示 確保您能夠識別每種administrative mode組合所產生的operational mode；這是一個潛在的考試問題。

## Trunk encapsulation negotiation

In addition to negotiating a port's operational mode, DTP can also negotiate which protocol a port uses to tag frames if it becomes a trunk: 802.1Q or ISL. Of course, this only applies to switches that support both 802.1Q and ISL. As I mentioned in chapter 12, modern Cisco switches only support 802.1Q, so I wouldn't expect any questions related to ISL on the CCNA exam.

> [!translation] 逐句繁體中文翻譯
> - 除了協商port的operational mode之外，DTP 還可以協商port在成為中繼時使用哪個協定來標記frame：802.1Q 或 ISL。
> - 當然，這僅適用於同時支援 802.1Q 和 ISL 的switch。
> - 正如我在第 12 章中提到的，現代 Cisco switch僅支援 802.1Q，因此我預期 CCNA 考試中不會出現任何與 ISL 相關的問題。

If a switch supports both protocols, its default setting is switchport trunk encapsulation negotiate. If both switches are using negotiate, the result will be ISL. If one side specifies a protocol (dot1q or is1), the side using negotiate will agree to use the same encapsulation. An encapsulation mismatch (dot1q on one side, isl on the other) is a misconfiguration. If you encounter a switch that supports both 802.1Q and ISL, you should manually configure 802.1Q with switchport trunk encapsulation dot1q, as we covered in chapter 12.

> [!translation] 逐句繁體中文翻譯
> - 如果switch支援這兩種協議，則其預設為switchport中繼封裝協商。
> - 如果兩台switch都使用協商，結果將為 ISL。
> - 如果一方指定了協議（dot1q 或 is1），則使用協商的一方將同意使用相同的封裝。
> - 封裝不符（一側為 dot1q，另一側為 isl）是一種錯誤配置。
> - 如果您遇到同時支援 802.1Q 和 ISL 的交換機，則應使用switchport中繼封裝 dot1q 手動設定 802.1Q，如我們在第 12 章所述。

### 13.1.2 Disabling DTP

As I mentioned previously, DTP was developed to streamline the deployment of switches by allowing them to automatically determine the operational status of the ports. However, it is also a security vulnerability; an attacker can take advantage of DTP to form a trunk link with a switch, gaining access to all VLANs in the LAN.

> [!translation] 逐句繁體中文翻譯
> - 正如我之前提到的，DTP 的開發目的是透過允許switch自動確定port的運作狀態來簡化switch的部署。
> - 然而，這也是一個安全漏洞；攻擊者可以利用 DTP 與switch形成中繼link，從而獲得對 LAN 中所有 VLAN 的存取權。

In the default administrative mode of dynamic auto, a switch port connected to an end host will operate in access mode; end hosts don't use DTP, so they can't negotiate
a trunk. This means that the end host will have access only to a single VLAN, and communication between that VLAN and other VLANs can be controlled by configuring security policies on the router.

> [!translation] 逐句繁體中文翻譯
> - 在dynamic auto的預設administrative mode下，連接到終端host的switchport將以存取模式運作；終端host不使用 DTP，因此它們無法協商中繼。
> - 這表示終端host只能存取單一 VLAN，並且可以透過在router 上設定安全性原則來控制該 VLAN 與其他 VLAN 之間的通訊。

However, if a malicious user uses something like Yersinia (a hacking tool) to send DTP messages out of their PC, they can negotiate to form a trunk link with the switch port, giving them access to all VLANs in the LAN, presenting a security threat. Figure 13.3 depicts an attacker who has used DTP to negotiate a trunk with a switch.

> [!translation] 逐句繁體中文翻譯
> - 然而，如果惡意使用者使用耶爾森氏菌（一種駭客工具）之類的東西從其 PC 發送 DTP 訊息，他們可以協商與switchport形成中繼link，使他們能夠存取 LAN 中的所有 VLAN，從而構成安全威脅。
> - 圖 13.3 描述了使用 DTP 與switch協商中繼的攻擊者。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-253_387_871_529_318.jpg)
Figure 13.3 An attacker sends malicious DTP messages to SW1 to form a trunk. This gives the attacker access to all VLANs in the LAN, presenting a security threat.

Because of the security implications and to reduce the amount of unnecessary traffic (DTP messages) being sent in the LAN, it is recommended that you disable DTP. There are two ways to do this:

> [!translation] 逐句繁體中文翻譯
> - 由於安全隱患以及為了減少在 LAN 中發送的不必要traffic（DTP 訊息），建議您停用 DTP。
> - 有兩種方法可以執行此操作：

- Manually configure the port as an access port with switchport mode access.
- Explicitly disable DTP with switchport nonegotiate.

In the following example, I use show interfaces g0/0 switchport. The output states Negotiation of Trunking: On; this means the port is sending DTP messages. I then configure G0/0 as an access port and check again; notice that trunk negotiation is now Off:

> [!translation] 逐句繁體中文翻譯
> - 在以下範例中，我使用 showinterfacesg0/0switchport。
> - 輸出狀態 中繼協商：開；這表示port正在傳送 DTP 訊息。
> - 然後我將 G0/0 配置為存取port並再次檢查；請注意，中繼協商現已關閉：

```
SW1# show interfaces g0/0 switchport
Name: Gi0/0 GO/O’s administrative mode
Switchport: Enabled
Administrative Mode: dynamic auto
Operational Mode: static access
. . .
Negotiation of Trunking: On ← DTP is enabled.
. . .
SW1# configure terminal
SW1(config) # interface g0/0
SW1(config-if)# switchport mode access
SW1(config-if)# do show interfaces g0/0 switchport
Name: GiO/O
Switchport: Enabled
```

```
Administrative Mode: static access
Operational Mode: static access
. . .
Negotiation of Trunking: Off ← DTP is disabled.
. . .
```

As demonstrated in the previous example, manually configuring access mode disables DTP on the port; it won't send DTP messages. However, the second method can be used to ensure that the port never sends DTP messages, regardless of its current mode (access or trunk). In the following example, I configure G0/0 as a trunk port and confirm that negotiation is On. I then use switchport nonegotiate to disable DTP and confirm again:

> [!translation] 逐句繁體中文翻譯
> - 如上例所示，手動設定存取模式會停用port 上的 DTP；它不會傳送 DTP 訊息。
> - 但是，可以使用第二種方法來確保port永遠不會發送 DTP 訊息，無論其當前模式為何（存取或中繼）。
> - 在以下範例中，我將 G0/0 配置為trunk port並確認協商已開啟。
> - 然後我使用 switchport nonegotiate 停用 DTP 並再次確認：

```
SW1(config)# interface g0/0
SW1(config-if)# switchport mode trunk
SW1(config-if)# do show interfaces g0/0 switchport
Name: GiO/O
Switchport: Enabled
Administrative Mode: trunk
Operational Mode: trunk
. . .
Negotiation of Trunking: On ← DTP is enabled.
. . .
SW1(config-if)# switchport nonegotiate < \ Disables DTP
SW1(config-if)# do show interfaces g0/0 switchport
Name: GiO/O
Switchport: Enabled
Administrative Mode: trunk
Operational Mode: trunk
. . .
Negotiation of Trunking: Off ← DTP is disabled.
. . .
```

Even if you manually configure a port in access mode, it is recommended that you also use switchport nonegotiate to ensure that DTP messages are never sent, even if you later configure the port in trunk mode.

> [!translation] 逐句繁體中文翻譯
> - 即使您手動將port配置為存取模式，建議您也使用 switchport nonegotiate 以確保永遠不會發送 DTP 訊息，即使您稍後將port配置為中繼模式也是如此。

EXAM TIP Remember that as a security best practice, use switchport nonegotiate to disable DTP on switch ports.

> [!translation] 逐句繁體中文翻譯
> - 考試提示 請記住，作為安全最佳實踐，請使用 switchport nonegotiate 停用switchport 上的 DTP。
