---
source: [[Chapter 15 - RSTP]]
chapter: 15
section: 15.4
tags: [source-section, acting-ccna, rstp, stp]
---

# Chapter 15.4 - Root Guard, Loop Guard, and BPDU Filter

In chapter 14, we covered two optional STP features: PortFast and BPDU Guard. While these are the two most common features in the so-called STP toolkit, the CCNA exam expects you to know a few more tools that expand STP's (or RSTP's) functionality. In this section, we will discuss Root Guard, Loop Guard, and BPDU Filter.

> [!translation] 逐句繁體中文翻譯
> - 在第 14 章中，我們介紹了兩個可選的 STP 功能：PortFast 和 BPDU Guard。
> - 雖然這些是所謂的 STP 工具包中最常見的兩個功能，但 CCNA 考試希望您了解更多一些擴展 STP（或 RSTP）功能的工具。
> - 在本節中，我們將討論根防護、環路防護和 BPDU 過濾器。

NOTE All of these optional STP features-PortFast, BPDU Guard, Root Guard, Loop Guard, and BPDU Filter-work in both STP and RSTP.

> [!translation] 逐句繁體中文翻譯
> - 注意 所有這些可選的 STP 功能（PortFast、BPDU Guard、Root Guard、Loop Guard 和 BPDU Filter）都可以在 STP 和 RSTP 中運作。

### 15.4.1 Root Guard

Root Guard is a feature that enhances the stability of the STP topology by preventing external switches from becoming the root bridge. A common scenario where Root Guard is useful is when a LAN of switches is controlled by two different entities, such as a service provider and a customer (who connects to the service provider's network). The service provider can use Root Guard to ensure that one of its switches remains the root bridge, maintaining a consistent STP topology even if a customer connects a switch with a lower bridge ID. Figure 15.8 illustrates this scenario.

> [!translation] 逐句繁體中文翻譯
> - 根防護是一種透過防止外部switch成為root bridge來增強 STP 拓樸穩定性的功能。
> - Root Guard 有用的常見場景是當switch LAN 由兩個不同的實體控制時，例如服務提供者和客戶（連接到服務提供者的network）。
> - 服務供應商可以使用 Root Guard 來確保其一台switch仍然是root bridge，從而保持一致的 STP topology，即使客戶連接具有較低橋接 ID 的switch也是如此。
> - 圖 15.8 說明了這種情況。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-307_634_1180_1290_320.jpg)
Figure 15.8 Root Guard prevents a newly connected customer network from affecting the service provider's STP topology. (1) The customer's SW4 and SW5 are connected to the service provider's SW2 and SW3. (2) SW4's bridge ID is lower than SW1's, so the BPDUs sent by SW4 and SW5 are superior to SW1's BPDUs. (3) SW2 and SW3 use Root Guard to block their ports that receive the superior BPDUs.

When a Root Guard-enabled port receives a superior BPDU, the port will enter the root-inconsistent state; in effect, this disables the port, preventing the switch from accepting the superior BPDU. In this state, the customer is unable to access the service provider's network. To fix this problem, the customer needs to configure a higher bridge ID on SW4, making the BPDUs it sends inferior to those of SW1. Once the superior BPDUs are no longer received, SW2's and SW3's ports automatically recover and return to their normal state.

> [!translation] 逐句繁體中文翻譯
> - 當啟用Root Guard的port收到上級BPDU時，port將進入根不一致狀態；實際上，這會停用端口，從而阻止switch接受高級 BPDU。
> - 在此狀態下，客戶無法存取服務提供者的network。
> - 為了解決這個問題，客戶需要在SW4上設定更高的橋接器ID，使其發送的BPDU不如SW1。
> - 一旦不再接收到上級 BPDU，SW2 和 SW3 的port將自動恢復並返回正常狀態。

The following example shows how to enable Root Guard on a port with the spanning-tree guard root command and the error message that appears when Root Guard blocks the port.

> [!translation] 逐句繁體中文翻譯
> - 以下範例顯示如何使用 spanning-tree Guard root 指令在port 上啟用根防護，以及根防護阻止port時出現的錯誤訊息。
![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-308_576_1431_652_220.jpg)

By using Root Guard, you can ensure that your network's STP topology remains stable and consistent, preventing external or misconfigured devices from disrupting the network.

> [!translation] 逐句繁體中文翻譯
> - 透過使用 Root Guard，您可以確保network的 STP topology保持穩定和一致，從而防止外部或配置錯誤的裝置中斷network。

### 15.4.2 Loop Guard

Loop Guard, as the name implies, guards against Layer 2 loops in the LAN. The entire point of STP is to prevent loops, but Loop Guard provides an additional layer of protection against a switch port erroneously transitioning from the discarding state (the blocking state in classic STP) to the forwarding state. This can happen when the discarding port stops receiving BPDUs, causing the switch to believe that the port can move to the forwarding state without causing a loop. Figure 15.9 shows how this can cause a loop.

> [!translation] 逐句繁體中文翻譯
> - 環路防護，顧名思義，可以防止區域network中的二層環路。
> - STP 的全部目的是防止環路，但環路防護提供了額外的一層保護，防止switchport錯誤地從丟棄狀態（經典 STP 中的阻塞狀態）轉換到轉送狀態。
> - 當丟棄port停止接收 BPDU 時，可能會發生這種情況，導致switch認為該port可以轉至轉送狀態而不會導致迴路。
> - 圖 15.9 顯示了這如何導致循環。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-309_533_1306_179_313.jpg)
Figure 15.9 A loop is caused when SW3 stops receiving BPDUs from SW2. (1) A software malfunction on SW2 prevents it from sending BPDUs. (2) After the max age timer counts down, SW3's port becomes a designated port and transitions to the forwarding state. (3) All three links between the switches are active, resulting in a Layer 2 loop.

To avoid such a problem, you can configure Loop Guard on SW3's alternate port using the spanning-tree guard loop command. If a Loop Guard-enabled port stops receiving BPDUs, the port will move into the loop-inconsistent state, disabling the port instead of allowing it to transition to the forwarding state; this prevents a loop from forming. However, if SW2 recovers and starts sending BPDUs again, SW3's port will automatically recover and transition back to its normal state (discarding).

> [!translation] 逐句繁體中文翻譯
> - 為了避免此類問題，您可以使用spanning tree保護環命令在 SW3 的備用port 上設定環路防護。
> - 如果啟用環路防護的port停止接收 BPDU，則該port將進入環路不一致狀態，從而停用該port而不是允許其轉換到轉送狀態；這可以防止形成環路。
> - 但是，如果 SW2 恢復並再次開始發送 BPDU，SW3 的port將自動恢復並轉換回其正常狀態（丟棄）。

The following example shows how to enable Loop Guard and the error message that appears when Loop Guard blocks the port to prevent a loop.

> [!translation] 逐句繁體中文翻譯
> - 以下範例顯示如何啟用環路防護以及環路防護阻止port以防止環路時出現的錯誤訊息。

```
SW2(config)# interface g0/1
SW2(config-if)# spanning-tree guard loop
*May 18 01:29:31.261: %SPANTREE-2-LOOPGUARD_BLOCK: Loop guard blocking
port GigabitEthernet0/1 on VLAN0001.
SW2(config-if) # do show spanning-tree
. . .
Interface Role Sts Cost Prio.Nbr Type
GiO/O
    Root FWD 4 128.1 P2p
Gi0/1
    Desg BKN*4 128.2 P2p *LOOP_Inc
            The port’s status is BKN (broken)
            and LOOP_Inc (loop-inconsistent)
```

With Loop Guard, you can provide an additional safeguard against potential network loops, ensuring the stability and reliability of the LAN. A Layer 2 loop can bring down any LAN, so avoiding them is critical.

> [!translation] 逐句繁體中文翻譯
> - 透過環路防護，您可以針對潛在的network迴路提供額外的保護，確保 LAN 的穩定性和可靠性。
> - Layer 2環路可能會導致任何 LAN 癱瘓，因此避免它們至關重要。

NOTE Root Guard and Loop Guard are mutually exclusive; you can't enable both of them on the same port simultaneously. This is because they serve different roles: Root Guard takes action based on receiving superior BPDUs, while Loop Guard takes action based on not receiving BPDUs.

> [!translation] 逐句繁體中文翻譯
> - 注意 根保護和環路保護是互斥的；您不能同時在同一port 上啟用它們。
> - 這是因為它們扮演不同的角色：根防護根據接收到優質 BPDU 採取行動，而環路防護則根據未接收 BPDU 採取行動。

### 15.4.3 BPDU Filter

BPDU Filter is a feature that can be used to prevent BPDUs from being sent or received on specific ports. This can be desirable on ports where STP isn't necessary, such as those connected to end hosts, since there is no risk of causing a loop. BPDU Filter can be enabled in two ways, with different behaviors depending on how you activate it:

> [!translation] 逐句繁體中文翻譯
> - BPDU 過濾器是可用於阻止在特定port 上傳送或接收 BPDU 的功能。
> - 這對於不需要 STP 的port（例如連接到最終host的port）來說可能是理想的，因為不存在導致環路的風險。
> - BPDU 濾鏡可以透過兩種方式啟用，根據您的啟動方式，具有不同的行為：

- Enabling BPDU Filter on a specific port
    - Use spanning-tree bpdufilter enable in interface config mode.
    - This enables BPDU Filter on the specific port.
    - The port will not send BPDUs and will ignore any BPDUs it receives.
    - This effectively disables STP on the port.
- Enable BPDU Filter globally for all PortFast-enabled ports
    - Use spanning-tree portfast bpdufilter default in global config mode.
    - This enables BPDU Filter on all PortFast-enabled ports (RSTP edge ports).
    - The port will not send BPDUs.
    - If the port receives a BPDU, PortFast and BPDU Filter will be disabled. The port will then operate as a normal STP or RSTP port.

Of the optional STP features that we have covered in this chapter and the previous one-PortFast, BPDU Guard, Root Guard, Loop Guard, and BPDU Filter-BPDU Filter has the fewest use cases; I recommend avoiding it in general. Especially when enabled in interface config mode, BPDU Filter poses the risk of causing a Layer 2 loop if the port is connected to another switch; STP is effectively disabled on the BPDU Filter-enabled port, so it won't move to the discarding state even if there is a loop in the LAN. Enabling BPDU Filter carelessly is a great way to bring down a network-use with extreme caution!

> [!translation] 逐句繁體中文翻譯
> - 在本章和上一章介紹的可選 STP 功能中，PortFast、BPDU Guard、Root Guard、Loop Guard 和 BPDU Filter（BPDU 過濾器）的使用案例最少；我建議一般情況下避免使用它。
> - 特別是在interface設定模式下啟用時，如果port連接到另一台交換機，BPDU Filter 會帶來導致Layer 2迴路的風險；STP 在啟用 BPDU Filter 的port 上被有效停用，因此即使 LAN 中存在環路，它也不會進入迴路，它也不會進入狀態。
> - 不小心啟用 BPDU 過濾器是非常小心地降低network使用的好方法！

## Summary

- STP, standardized by IEEE 802.1D, was modified by Cisco to make PVST+. Likewise, RSTP, standardized by IEEE 802.1w, was modified by Cisco to make Rapid PVST+. PVST+ and Rapid PVST+ both run separate spanning tree instances for each VLAN.
- Whether a switch runs PVST+ or Rapid PVST+ by default depends on the switch model and IOS version.
