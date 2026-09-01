# Exit Interface

tags: #concept #acting-ccna #routing #interface

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

## Appears In

- [[02_Source_Notes/Unit4-RouterSwitch-Packet|Unit04：Router / Switch 介面、路由與封包生命週期]]
