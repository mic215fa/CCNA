# Fully Specified Static Route

tags: #concept #acting-ccna #routing

## Definition

Fully Specified Static Route 是同時指定 exit interface 與 next-hop IP address 的 static route。

## Why It Exists

它清楚指出本 router 要從哪個 interface 送出，以及下一台 router 是誰，避免只指定 exit interface 時可能造成的 ARP / 多重存取網路問題。

## Prerequisites

- [[Static Route]]
- [[Next Hop]]
- [[Exit Interface]]

## Related Concepts

- [[Recursive Static Route]]
- [[Proxy ARP]]

## Mechanism

設定格式同時包含 destination network、mask、exit interface、next-hop IP。它比只指定 exit interface 更明確。

## Appears In

- [[02_Source_Notes/Unit4-RouterSwitch-Packet|Unit04：Router / Switch 介面、路由與封包生命週期]]
