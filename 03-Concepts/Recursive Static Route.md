# Recursive Static Route

tags: #concept #acting-ccna #routing

## Definition

Recursive Static Route 是只指定 next-hop IP address 的 static route。

## Why It Exists

它讓管理者用「下一台 router 的 IP」描述路由方向，而不直接指定本地 exit interface。

## Prerequisites

- [[Static Route]]
- [[Next Hop]]
- [[Routing Table]]

## Related Concepts

- [[Exit Interface]]
- [[Fully Specified Static Route]]

## Mechanism

Router 需要先查 static route 的 next-hop，再查如何到達該 next-hop，因此稱為 recursive lookup。

## Appears In

- [[02_Source_Notes/Unit4-RouterSwitch-Packet|Unit04：Router / Switch 介面、路由與封包生命週期]]
