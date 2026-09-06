# CSMA-CD

tags: #concept #acting-ccna #ethernet

## Aliases

- Carrier-sense multiple access with collision detection

## Definition

CSMA-CD 是 shared Ethernet medium 中用來偵測媒介是否空閒、處理 collision，並在碰撞後等待再重傳的方法。

## Why It Exists

在 hub / half-duplex Ethernet 中，多台設備可能同時使用同一媒介。CSMA-CD 用來降低與處理 collision。

## Prerequisites

- [[Collision Domain]]
- [[Duplex]]

## Related Concepts

- [[Ethernet]]
- [[Interface Error]]

## Mechanism

設備先 listen，看媒介是否有 traffic；若空閒就 transmit。若偵測到 collision，停止傳送、等待隨機時間，再嘗試重傳。

## Appears In

- [[02_Source_Notes/Unit4-RouterSwitch-Packet|Unit04：Router / Switch 介面、路由與封包生命週期]]
