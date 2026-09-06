# Exit Interface

tags: #concept #acting-ccna #routing #interface

> [!info] Concept role
> [[Routing Table]] 的 forwarding-instruction 子概念：指出 packet 從本地哪個 Layer 3 interface 離開。

## Definition

Exit Interface 是 router 要把 packet 送出去的本地 interface。

## Why It Exists

Route 不只需要知道 destination network，也需要指出本 router 應從哪個 interface 將 packet 送到下一段 link。

## Prerequisites

- [[Network Interface]]
- [[Routing Table]]
- [[Route Selection]]

## Related Concepts

- [[Next Hop]]
- [[Static Route]]
- [[Connected Route]]

## Mechanism

Connected route 會指出該 network directly connected 到哪個 interface；static route 也可指定 exit interface，或同時指定 exit interface 與 next hop。

## Contrast With

- [[Next Hop]]：exit interface 是本機資源；next hop 是下一台 Layer 3 device 的 address。
- Point-to-point link 通常只有一個可能 neighbor；Ethernet multiaccess link 可能有多個 neighbors，因此只指定 exit interface 不能清楚指出應交付給誰。

## Verification

Selected route 顯示 exit interface 後，仍應用 `show ip interface brief`／`show interfaces` 確認 interface 與 line protocol state；route 的方向正確不代表 link 一定可用。

## Appears In

- [[02_Source_Notes/Unit4-RouterSwitch-Packet|Unit04：Router / Switch 介面、路由與封包生命週期]]
