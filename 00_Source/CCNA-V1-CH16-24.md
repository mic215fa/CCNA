- Use spanning-tree bpdufilter enable in interface config mode to enable BPDU Filter on a specific port. The port will not send BPDUs and will ignore any BPDUs it receives, effectively disabling STP on the port.
- Use spanning-tree portfast bpdufilter default in global config mode to enable BPDU Filter on all PortFast-enabled ports. The port will not send BPDUs, but if it receives a BPDU, PortFast and BPDU Filter will be disabled. The port will then operate as a normal STP or RSTP port.

## EtherChannel

## This chapter covers

- How EtherChannel combines redundant links into a single logical link
- Static and dynamic EtherChannel configuration
- How traffic is load-balanced over the physical links of an EtherChannel
- Using Layer 3 EtherChannel to provide redundant Layer 3 connections

In the previous two chapters on STP, we emphasized the essential role of STP in preventing Layer 2 loops in a LAN with redundant connections. However, the downside of STP is that all redundant links are disabled; they provide essential redundancy in a LAN but will only be used to forward traffic if an active link fails.

In this chapter, we will cover EtherChannel, a technology that helps to overcome this limitation by allowing multiple physical links to be combined into a single logical link. As a result, STP and frame-forwarding logic will treat the group of links as a single unit, allowing the whole group of links to remain active without causing Layer 2 loops. This not only maximizes usage of the available bandwidth but also improves network resilience and simplifies management-advantages that we'll delve into in this chapter. EtherChannel is CCNA exam topic 2.4: Configure and verify (Layer 2/ Layer 3) EtherChannel (LACP).

### 16.1 How EtherChannel works

To see how EtherChannel works and why it's useful, think of the following situation. Two switches, SW1 and SW2, are connected by their G0/0 ports. However, the link is congested; there is a lot of traffic in the LAN. The link between SW1 and SW2 is a bottleneck that is negatively affecting networking performance, and users are complaining.

To remedy the problem, you add a second link between SW1 and SW2, connecting their G0/1 ports. However, users still complain about poor network performance. This time, you add a further two links between SW1 and SW2, resulting in a total of four links between the switches. Having quadrupled the bandwidth between SW1 and SW2 (from a single 1 Gbps connection to four), surely the issue has been fixed, right? Nope, the LAN is just as congested as ever.

NOTE Bandwidth is the total number of bits that can be transferred over a connection per second-four 1 Gbps links means 4 Gbps of bandwidth. In section 16.3, we will differentiate between bandwidth and speed-two similar but different concepts.

So, what's the culprit? It's our friend from the previous two chapters: Spanning Tree Protocol. Figure 16.1 shows why the network performance won't improve, no matter how many new links you add between the two switches. STP blocks all redundant links to avoid Layer 2 loops, leaving only a single link active.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-315_405_1412_1144_194.jpg)
Figure 16.1 Clients connected to SW1 experience poor network performance when accessing the servers connected to SW2, but adding additional links between the switches doesn't remedy the issue. This is because STP blocks redundant links, leaving only a single active link.

Having additional links between SW1 and SW2 adds redundancy but doesn't solve the problem of congestion between the switches. The real solution to the problem is EtherChannel, which combines multiple physical links into a single logical link, meaning they can all remain active without causing Layer 2 loops. Figure 16.2 shows how EtherChannel combines the four 1 Gbps physical links into a single logical link with 4 Gbps of bandwidth.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-316_539_1359_181_225.jpg)
Figure 16.2 EtherChannel combines the four 1 Gbps links between SW1 and SW2 into a logical 4 Gbps link. Frame-switching and STP logic treat the four links as one. SW1's logical Po1 (Port-channel1) interface is an STP root port connected to SW2's logical Po1 interface, which is an STP designated port.

NOTE The logical interfaces created by EtherChannel are called port channels in Cisco IOS; this term is often used interchangeably with EtherChannel. For consistency, I will use the term EtherChannel except when referring to CLI commands that use the term port-channel.

Frames forwarded between SW1 and SW2 will be distributed over the four physical links that make up the EtherChannel; this is called load balancing or load distribution, and we will cover how EtherChannel does it in section 16.3. Because the four links are treated as one logical entity, a Layer 2 loop is not created even though all ports can forward frames. Figure 16.3 shows how a broadcast frame sent by a PC reaches all hosts in the LAN but does not result in a Layer 2 loop; BUM frames received on a port that is a member of an EtherChannel will not be flooded out of other ports in the same EtherChannel.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-316_428_1359_1534_225.jpg)
Figure 16.3 (1) PC1 send a broadcast frame and (2) SW1 floods it over the EtherChannel, sending the frame out of one of the EtherChannel's member ports. (3) SW2 receives the frame on G0/0 and floods it to SRV1 and SRV2 but does not flood the frame back to SW1 because G0/1, G0/2, and G0/3 are part of the same EtherChannel as G0/0.

It is important to note that EtherChannel does not entirely remove the need for STP in a LAN. With only two switches, all links can remain active, but in the context of a larger LAN, some links may need to be disabled. Figure 16.4 shows an example. The switches in the LAN are connected by various two-link EtherChannels. However, some of the EtherChannels need to be blocked by STP to avoid Layer 2 loops in the LAN.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-317_492_920_437_318.jpg)
Figure 16.4 Four switches connected by EtherChannels, some blocked by STP. Without EtherChannel, only 3 of the 10 physical links would be active. With EtherChannel, 6 of the 10 physical links (three 2-link) EtherChannels are active.

Because all ports in an EtherChannel are treated as one logical port by STP, the output of show spanning-tree will only show the port channel interfaces, not the physical ports. I demonstrate this in the following example:

```
SW1# show spanning-tree
```

| Interface | Role Sts Cost | Prio.Nbr | Type |
| :--- | :--- | :--- | :--- |
| Po1 | Root FWD 3 | 128.65 | P2p |
| Po2 | Altn BLK 3 | 128.66 | P2p |

NOTE Po1 and Po2 have an STP cost of 3 in the example, rather than the default cost of 4 for a GigabitEthernet port. Because the bandwidth values of the two member ports of each port channel are combined, the STP cost calculation is affected.

### 16.2 EtherChannel configuration

Now that we've covered EtherChannel's purpose and how it works at a high level, let's see how to configure it on Cisco switches. There are two ways to configure an Ether-Channel: configuring a dynamic EtherChannel causes the switches to use a protocol to negotiate with each other to form an EtherChannel, and static EtherChannel forces the switch to form an EtherChannel with the specified ports, without any negotiation with the neighboring device.

NOTE Dynamic and static EtherChannel configuration can be likened to dynamic and static trunk port configuration; switch ports using DTP negotiate with each other to form a trunk (dynamic), whereas manually configured trunk ports operate in trunk mode without any negotiation (static).

### 16.2.1 Dynamic EtherChannel

A dynamic EtherChannel configuration involves enabling a protocol on a set of switch ports. The switches will use the protocol to send messages to each other to negotiate whether to form an EtherChannel; this negotiation process involves comparing configuration parameters to make sure they match. Incorrectly configured EtherChannels can result in loops in the LAN, so it is highly recommended that you use dynamic EtherChannel instead of the other option: static EtherChannel (the topic of section 16.2.2).

Cisco switches support two dynamic EtherChannel negotiation protocols: the Ciscoproprietary Port Aggregation Protocol (PAgP) and the IEEE standard Link Aggregation Control Protocol (LACP), first defined in IEEE 802.3ad. The biggest difference between the two is that PAgP only runs on Cisco switches, whereas LACP can run on any vendor's switches.

EXAM TIP Remember the key difference between PAgP and LACP; if an exam question mentions forming an EtherChannel with a non-Cisco switch, you must use LACP.

## PAGP configuration

Cisco's PAgP allows up to eight physical links to be combined into a single EtherChannel. Figure 16.5 shows a two-link EtherChannel between SW1 and SW2; you should be familiar with the desirable and auto keywords from configuring trunks with DTP. Additionally, switchport mode trunk is configured on each logical port channel interface to make the new EtherChannel a trunk link.

```
SW1(config)# interface range g0/0-1
SW1(config-if-range) # channel-group 1 mode desirable
SW1(config-if-range) # interface po1
SW1(config-if)# switchport mode trunk
```

Figure 16.5 An EtherChannel is formed between SW1 and SW2 using PAgP. SW1's G0/0 and G0/1 ports are configured in PAgP desirable mode, and SW2's GO/0 and G0/1 ports are configured in PAgP auto mode. switchport mode trunk is applied to each switch's port channel interface to make the EtherChannel a trunk.

To configure an EtherChannel using PAgP, use the channel-group group-number mode \{desirable | auto\} command on the member ports. The group-number value is used to identify the EtherChannel on the switch; a single switch can have multiple EtherChannels, and this number identifies which ports belong to which EtherChannel. Like in DTP trunk formation, the combination of desirable and auto modes on the two ends of the connection determines whether the switches will form an EtherChannel:

```
- desirable + desirable = yes
- desirable + auto = yes
- auto + auto = no
```

NOTE If the switches don't form an EtherChannel, the two links will function independently-one active and one blocked by STP.

Ports in desirable mode will actively attempt to form an EtherChannel with the neighboring switch by sending PAgP messages. Ports in auto mode won't actively attempt to form an EtherChannel but will respond upon receiving PAgP messages from a desirable-mode neighbor and agree to form an EtherChannel.

In the following example, I configure desirable mode on SW1's G0/0 and G0/1 ports, as we saw in figure 16.5. After assigning both ports to channel group 1, the logical interface Port-channell is automatically created, as shown in the output of show ip interface brief; using this logical interface, the physical G0/0 and G0/1 ports are treated as one entity by Cisco IOS:

```
SW1(config)# interface range g0/0-1
SW1(config-if-range)# channel-group 1 mode desirable
SW1(config-if-range) # do show ip interface brief
```

| Interface | IP-Address | OK? | Method | Status | Protocol |
| :--- | :--- | :--- | :--- | :--- | :--- |
| GigabitEthernet0/0 | unassigned | YES | unset | up | up |
| GigabitEthernet0/1 | unassigned | YES | unset | up | up |
| GigabitEthernet0/2 | unassigned | YES | unset | administratively down | down |
| GigabitEthernet0/3 | unassigned | YES | unset | administratively down | down |
| Port-channel1 | unassigned | YES | unset | down | down |

The Port-channel1 interface was created automatically after assigning G0/0 and G0/1 to channel group 1.

NOTE Get used to the different terms: some IOS commands use the term channel-group, some use port-channel, and some use etherchannel. They all refer to the same thing.

Note that the Port-channell interface is in a down/down state; this is because I haven't configured SW2's end of the connection yet. You can confirm the status of any EtherChannels on a switch with the show etherchannel summary command, as in the following example:
![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-320_580_1411_179_222.jpg)

A variety of flags are used to indicate the status of the EtherChannel; they are indicated in brackets next to the port channel interface and each member port. SW1's Po1 interface has the S and D flags; as indicated in the legend at the top of the output, S indicates a Layer 2 EtherChannel (think S for "switch port"). Just like regular switch ports, port channel interfaces can also be configured to operate like router interfaces with the no switchport command. We will cover that in section 16.4; for now, let's focus on Layer 2 EtherChannels. The Po1 interface also has the D flag, meaning down; in this case, it's because SW2's end hasn't been configured yet.

The G0/0 and G0/1 ports that form the EtherChannel both have the I flag, meaning stand-alone (think I for "independent"); although I configured them to be part of Port-channel1, PAgP didn't succeed in negotiating with SW2. As a result, G0/0 and G0/1 both operate as regular standalone switch ports. To complete the EtherChannel, let's configure SW2's side of the connection in auto mode, as we saw in figure 16.5. In the following example, I configure SW2's G0/0 and G0/1 ports and confirm with show

```
    etherchannel summary:
SW2(config)# interface range g0/0-1
        Configures SW2 GO/0 and
SW2(config-if-range) # channel-group 1 mode auto
        GO/1 in auto mode
SW2(config-if-range) # do show etherchannel summary
Flags: D - down P - bundled in port-channel
. . .
    R - Layer3 S - Layer2
    U - in use N - not in use, no aggregation
. . .
Group Port-channel Protocol Ports
------+-------------+-----------+-----------------------------------------------
1 Po1(SU) PAgP Gi0/0(P) Gi0/1(P)
        GO/0 and GO/1 have formed an EtherChannel.
```

NOTE I configured channel-group 1 on both SW1 and SW2, but the group number doesn't have to match on the two switches; the group number is only significant to the local switch.

After configuring SW2, notice that the flags are different from those we saw on SW1. Po1's D has changed to a U for in use; this means the EtherChannel was successfully negotiated and is working-I like to think of it as U for "up," since the opposite state is D for "down." The I next to G0/0 and G0/1 also changed to a P for bundled in port-channel; this means they are no longer standalone ports but are now members of the EtherChannel. Figure 16.6 shows the physical and logical topologies of the SW1-SW2 connection after forming the EtherChannel.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-321_425_761_528_316.jpg)
Figure 16.6 The physical and logical topologies of the SW1-SW2 connection. Physically, SW1 and SW2 are connected by two 1 Gbps links. Logically, they are connected by a single 2 Gbps link via their Po1 interfaces.

Now that the EtherChannel has been formed, we can configure each switch's port channel interface as necessary. For example, because this is a connection between switches, it probably should be a trunk link; in most cases, connections between switches need to carry traffic in multiple VLANs. In the following example, I configure SW1's Po1 interface in trunk mode and confirm with show interfaces trunk:
Configures port-channel1
as a trunk

Configurations made to the port channel interface will be automatically inherited by its member ports. In the following example, I use show running-config on SW1 to check the configurations of its ports; G0/0 and G0/1 have both inherited the switchport mode trunk command:

```
SW1# show running-config
. . .
interface Port-channel1
```

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-321_76_490_1717_1108.jpg)

```
    switchport mode trunk
!
interface GigabitEthernet0/0
    switchport mode trunk
    channel-group 1 mode desirable
!
interface GigabitEthernet0/1
    switchport mode trunk
```

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-321_340_152_1720_950.jpg)

```
    channel-group 1 mode desirable
. . .
```

Although G0/0 and G0/1 have inherited the switchport mode trunk command, only Po1 appears in the output of show interfaces trunk, as shown in the following example. This is because when part of an EtherChannel, physical ports are no longer viewed as independent logical entities by IOS; they are combined to form the logical port channel interface:

```
SW1# show interfaces trunk
Port Mode
Po1 on
. . .
```

```
Encapsulation Status Native vlan
802.1q trunking 1
```

The output only shows Po1, not G0/0 or G0/1.

Inheritance goes both ways; if you use the switchport mode trunk command on G0/0 and G0/1, the configuration will be inherited by Po1. In the following example, I demonstrate that on SW2:

```
SW2(config)# interface range g0/0-1
SW2(config-if-range) # switchport mode trunk
SW2(config-if-range) # do show running-config
. . .
interface Port-channel1
    switchport mode trunk
!
interface GigabitEthernet0/0
    switchport mode trunk
    channel-group 1 mode desirable
!
interface GigabitEthernet0/1
    switchport mode trunk
    channel-group 1 mode desirable
. . .
```

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-322_108_405_835_1236.jpg)

Order of configuration
Because inheritance goes both ways, there is some flexibility to the order in which you configure EtherChannels. Assigning ports to an EtherChannel (with the channel-group command) can come before or after configuring port settings (like switchport mode trunk), and those port settings can be configured on either the member ports or the port channel interface.

However, after the EtherChannel has been created and is active in the network, it is recommended that you make configuration changes (for example, modifying the allowed VLANs) on the port channel interface, rather than the member ports. This helps ensure configuration consistency across all member ports, which is necessary for a functioning EtherChannel. Additionally, configuration changes made to the member ports, rather than the port channel interface, can cause the EtherChannel to flap-to briefly go down before coming up again-interrupting traffic in the LAN for a few seconds.

Although show etherchannel summary is the command you'll be using most often to verify the status of a switch's EtherChannels, another command to familiarize yourself with is show pagp neighbor, as in the following example:

```
The A flag is used to indicate that the
neighbor port is in auto mode.
SW2# show pagp neighbor
Flags: S - Device is sending Slow hello. C - Device is in Consistent state.
A - Device is in Auto mode. P - Device learns on physical port.
Channel group 1 neighbors
```

|  | Partner | Partner | Partner |  | Partner | Group |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Port | Name | Device ID | Port | Age | Flags | Cap. |
| Gio/o | SW1 | 5254.0015 .8000 | Gio/o | 6s | SC | 10001 |
| Gi0/1 | SW1 | 5254.0015 . 8000 | Gi0/1 | 6 s | SC | 10001 |

SW1 G0/0 and G0/1 do not have the A flag, so we can identify that they are not in auto mode-they must be in desirable mode.

This command displays information about the switch's PAgP-enabled neighbors; because I used the command on SW2, it shows information about SW1. Notice that the A flag, which stands for auto mode, does not appear under the Partner Flags column. This means that SW1's ports are not in auto mode; they must be in desirable mode.

EXAM TIP Interpreting the output of show commands like show pagp neighbor is a major part of the CCNA exam. Although many such commands also contain information beyond the scope of the CCNA, make sure you can identify the key pieces of information.

## Link Aggregation Control Protocol

Having covered how to configure an EtherChannel using PAgP, you now know 90\% of what you need to know to configure an EtherChannel using LACP. There are only a couple of practical differences that you should know for the CCNA exam. Figure 16.7 shows an LACP EtherChannel connecting two switches. Notice that the configuration is nearly identical to the PAgP EtherChannel we looked at previously.

```
SW1(config)# interface range g0/0-1
SW1(config-if-range) # channel-group 1 mode active
SW1(config-if-range) # interface pol
SW1(config-if)# switchport mode trunk
```

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-323_327_1084_1579_510.jpg)
Figure 16.7 An EtherChannel is formed between SW1 and SW2 using LACP. SW1's GO/0 and G0/1 ports are configured in LACP active mode, and SW2's GO/0 and G0/1 ports are configured in LACP passive mode. switchport mode trunk is applied to each switch's port channel interface to make the EtherChannel a trunk.

The most relevant difference is the keywords used with the channel-group command when configuring an LACP EtherChannel; active and passive instead of PAgP's desirable and auto. The following LACP mode combinations will result in an EtherChannel:

- active + active = yes
- active + passive = yes
- passive + passive = no

LACP's active mode is equivalent to PAgP's desirable mode; ports in active mode will actively attempt to form an EtherChannel with the neighboring switch by sending LACP messages. Likewise, LACP's passive mode is equivalent to PAgP's auto mode; ports in passive mode won't actively attempt to form an EtherChannel but will respond if connected to a neighbor using active mode.

NOTE The LACP and PAgP modes cannot be used together. For example, active (LACP) + desirable (PAgP) will not result in a valid EtherChannel. Protocols are like languages: the devices have to use the same language to communicate successfully.

A second difference between LACP and PAgP is that LACP supports up to 16 links in a single EtherChannel, whereas PAgP only supports 8. However, although LACP supports up to 16 links in an EtherChannel, only up to 8 links can be used at once; the remaining links will be inactive, waiting to take over if an active link fails.

In the following example, I configure an EtherChannel between SW1 and SW2 using LACP and enable trunk mode, as we saw in figure 16.7. Aside from the use of the active and passive keywords, the configurations are identical to the PAgP configurations we did previously:

```
SW1(config)# interface range g0/0-1
SW1(config-if-range) # channel-group 1 mode active
SW1(config-if-range) # interface po1
SW1(config-if)# switchport mode trunk
SW2(config)# interface range g0/0-1
SW2(config-if-range) # channel-group 1 mode passive
SW2(config-if-range) # interface po1
SW2(config-if)# switchport mode trunk
SW2(config-if) # do show etherchannel summary
Flags: D - down P - bundled in port-channel
    I - stand-alone s - suspended
    H - Hot-standby (LACP only)
    R - Layer3 S - Layer2
    U - in use N - not in use, no aggregation
. . .
Group Port-channel Protocol Ports
------+-------------+-----------+-----------------------------------------------
1 Po1 (SU)
    LACP GiO/O(P) GiO/1(P)
```

The SU flags on Po1 and the P flag on G0/0 and G0/1 indicate an operational Layer 2 EtherChannel.

In addition to show etherchannel summary, we can verify the status of an LACP EtherChannel with the show lacp neighbor command. In the following example, I use the command on SW1, and the output shows information about SW1's neighbor (SW2):

```
Flag A means the neighbor port is in
Flag A means the neighbor port is in
active mode, and flag P means the
active mode, and flag P means the
neighbor port is in passive mode.
neighbor port is in passive mode.
Flags: S - Device is requesting Slow LACPDUs
F - Device is requesting Fast LACPDUs
A - Device is in Active mode P - Device is in Passive mode
Channel group 1 neighbors
Partner's information:
```

|  |  | LACP port |  |  | Admin | Oper | Port | Port |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Port | Flags | Priority | Dev ID | Age | key | Key | Number | State |
| Gio/o | SP | 32768 | 5254.0004 .8000 | 10s | 0x0 | 0x1 | $0 \times 1$ | 0x3C |
| Gi0/1 | SP | 32768 | 5254.0004 . 8000 | 6 s | 0x0 | 0x1 | $0 \times 2$ | 0x3C |

Figure 16.8 A static EtherChannel is formed between SW1 and SW2. Both switches' ports are configured in on mode. switchport mode trunk is applied to each switch's port channel interface to make the EtherChannel a trunk.

SW2's ports have the P flag, so they are in passive mode (as we configured).

EXAM TIP Some exam questions might expect you to read between the lines. The previous output indicates that SW1's neighbor is using passive mode, so we can deduce that SW1 must be using active mode; if both switches were using passive mode, the EtherChannel would not form.

### 16.2.2 Static EtherChannel

Another option for configuring EtherChannels is to not use any negotiation protocol at all; this is called a static EtherChannel. Figure 16.8 shows a static EtherChannel between SW1 and SW2, with the relevant configurations.

```
SW1(config) # interface range g0/0-1
SW1(config-if-range) # channel-group 1 mode on
SW1(config-if-range) # interface po1
SW1(config-if) # switchport mode trunk
```

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-325_340_1102_1552_492.jpg)
Figure 16.8 A static EtherChannel is formed between SW1 and SW2. Both switches' ports are configured in on mode. switchport mode trunk is applied to each switch's port channel interface to make the EtherChannel a trunk.

Whereas PAgP and LACP each have two separate modes, static EtherChannels use a single mode called on; for a static EtherChannel to work, both sides of the connection must be in on mode. When using on mode, neither switch sends any messages to negotiate the formation of an EtherChannel; the configured ports will form an EtherChannel regardless of the state of the neighboring device.

For this reason, it is highly recommended that you use PAgP or LACP instead of manual EtherChannels; the dynamic protocols will check to make sure the neighboring device's configurations are appropriate before forming an EtherChannel. In the worstcase scenario, an improperly configured EtherChannel can result in Layer 2 loops, which are disastrous for the LAN.

NOTE There are cases in which you have to use a static EtherChannel, such as when connecting a switch to a wireless LAN controller (WLC). We will cover WLCs in part 4 of volume 2 of this book.

Just as an EtherChannel will not form between a switch using PAgP and a switch using LACP, a static EtherChannel will not work if connected to a switch using PAgP or LACP. Table 16.1 shows which combinations of modes will result in an operational EtherChannel.

Table 16.1 EtherChannel mode combinations
| Modes | Desirable | Auto | Active | Passive | On |
| :--- | :--- | :--- | :--- | :--- | :--- |
| Desirable | Yes | Yes | No | No | No |
| Auto | Yes | No | No | No | No |
| Active | No | No | Yes | Yes | No |
| Passive | No | No | Yes | No | No |
| On | No | No | No | No | Yes |


### 16.2.3 Physical port configurations

When configuring EtherChannels, it's very important that various port settings match, both among the ports on the same switch and between neighboring switches. Following are some of the settings that must match among the ports of an EtherChannel:

- Speed
- Duplex
- Operational mode (access or trunk)
- Allowed VLANs and native VLAN (when in trunk mode)
- Access VLAN (when in access mode)

Rather than memorizing a list of settings that must match between an EtherChannel's ports, just ensure that all ports in the same EtherChannel are configured identically. Figure 16.9 shows what can happen when a port's settings don't match the other ports in the EtherChannel.
![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-327_309_822_399_513.jpg)

```
SW1# show etherchannel summary
Flags: D - down P - bundled in port-channel
    I - stand-alone s - suspended
. . .
Group Port-channel Protocol Ports
------+-------------+-----------+-----------------------------------------------
1 Po1(SU) LACP Gi0/0(s) Gi0/1(P) GiO/2(P)
    Gio/3(P)
```

Figure 16.9 Changing the native VLAN of GO/O causes it to be placed in a suspended state; it is disabled until its settings match the other ports in the EtherChannel.

NOTE Settings that don't have a direct effect on the port's operation, like a description configured with the description command, don't have to match.

Changing the native VLAN of SW1 G0/0 but not the other ports in the EtherChannel causes it to be suspended; it is unable to participate in the EtherChannel or forward frames at all until its settings match the other ports in the EtherChannel. In the following example, I restore SW1 G0/0's native VLAN to the default of 1 and check the EtherChannel's status; it is once again an active member of the EtherChannel:

```
SW1(config)# interface g0/0
SW1(config-if)# no switchport trunk native vlan 10
SW1(config-if)# do show etherchannel summary
Flags: D - down P - bundled in port-channel
    I - stand-alone s - suspended
. . .
Group Port-channel Protocol Ports
------+-------------+-----------+-----------------------------------------------
1 Po1(SU) LACP Gi0/0(P) Gi0/1(P) GiO/2(P)
    GiO/3(P)
```

SW1 G0/0's flag is now P; it is a functioning member of the EtherChannel again.

### 16.3 EtherChannel load balancing

After a switch's frame-forwarding logic has determined that a frame should be forwarded out of an EtherChannel, there is an additional check on the frame: the EtherChannel load-balancing logic examines the frame and uses certain parameters to determine which physical port the frame will be forwarded out of. Figure 16.10 demonstrates this concept.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-328_481_1260_457_348.jpg)
Figure 16.10 SW1's frame-forwarding logic determines that both frames will be forwarded out of Po1. However, the EtherChannel load-balancing logic determines that PC1's frame will be forwarded out of G0/0, and PC2's frame will be forwarded out of G0/2.

Frames forwarded over an EtherChannel are load-balanced over the EtherChannel's physical links by flow. A flow is a communication between two nodes; for example, if PC1 communicates with SRV1, all messages from PC1 to SRV1 are one flow, and all messages from SRV1 to PC1 are another flow. All messages in the same flow will be forwarded over the same physical link in the EtherChannel. Figure 16.11 shows an example; all messages from PC1 to SRV1 use the G0/0-G0/0 link, all messages from SRV1 to PC1 use the G0/1-G0/1 link, all messages from PC2 to SRV2 use the G0/2-G0/2 link, and all messages from SRV2 to PC2 use the G0/3-G0/3 link.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-328_377_1254_1530_350.jpg)
Figure 16.11 Messages forwarded over an EtherChannel are load-balanced based on flow. In this example, messages from PC1 to SRV1 use the G0/0-G0/0 link, messages from SRV1 to PC1 use the G0/1-G0/1 link, messages from PC2 to SRV2 use the G0/2-G0/2 link, and messages from SRV2 to PC2 use the G0/3-G0/3 link.

Keep in mind that figure 16.11 is just an example; EtherChannel load balancing won't always use all links of the EtherChannel equally. Fortunately, you can modify which parameter(s) the load-balancing logic takes into consideration with the port-channel load-balance parameters command in global config mode. Table 16.2 lists some possible parameters and their meanings.

Table 16.2 EtherChannel load-balancing parameters
| Parameter | Description |
| :--- | :--- |
| src-mac | All frames with the same source MAC address will use the same link. |
| dst-mac | All frames with the same destination MAC address will use the same link. |
| src-dst-mac | All frames with the same combination of source and destination MAC addresses will use the same link. |
| src-ip | All frames with the same source IP address (in the encapsulated packet) will use the same link. |
| dst-ip | All frames with the same destination IP address (in the encapsulated packet) will use the same link. |
| src-dst-ip | All frames with the same combination of source and destination IP addresses (in the encapsulated packet) will use the same link. |


NOTE The default EtherChannel load-balancing setting varies depending on the switch model and IOS version. On the switches I'm using for this chapter, the default setting is src-dst-ip. Some switches support additional parameters, too.

To check which parameters are used by the switch, use the show etherchannel load-balance command, as in the following example. Note the statement Non-IP: Source XOR Destination MAC address; this means that, for frames that don't encapsulate IP packets, the switch will use the source and destination MAC addresses instead. If there is no IP packet inside, there are no IP addresses to examine:
![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-329_348_1279_1551_316.jpg)

NOTE Although the configuration command is port-channel load-balance, the show command is show etherchannel load-balance-one of the great mysteries of Cisco IOS.

If it is determined that traffic isn't being evenly balanced over the links of an EtherChannel, changing the load-balancing parameters might help. For example, load balancing based only on the destination MAC address will cause all frames destined for the default gateway to use the same link, possibly causing congestion on that link. However, such determinations are beyond the scope of the CCNA; a general understanding of how EtherChannel load balancing works is sufficient.

## Lanes on a highway

EtherChannel can be compared to adding more lanes to a highway. Just as adding more lanes to a highway increases its total capacity, adding more links to an EtherChannel increases its total bandwidth-the total number of bits that can be transmitted over it per second. An EtherChannel's bandwidth is the total bandwidth of its member links.

However, adding more lanes to a highway doesn't increase the speed of each individual car; the speed limit remains the same. Likewise, adding more links to an EtherChannel doesn't increase the speed of each individual link; if an EtherChannel has four 1 Gbps links, the maximum speed of any particular communication over the EtherChannel is still 1 Gbps .

With that said, adding more lanes to a highway can reduce congestion and, thus, indirectly increase a car's speed by allowing it to drive at the speed limit without slowing down or stopping. Similarly, EtherChannel can reduce network congestion and thus increase the speed and efficiency of each communication flow.

In this context, the term bandwidth refers to the total capacity of a connection, and speed refers to the maximum transfer rate of a particular communication.

### 16.4 Layer 3 EtherChannel

Up to this point, we have covered Layer 2 EtherChannels-EtherChannels consisting of switch ports that switch frames rather than routed ports that route packets. However, we can also configure Layer 3 EtherChannels consisting of routed ports. Remember, on multilayer switches, you can use the no switchport command on a port to make it a routed port capable of forwarding packets like a router.

NOTE Some router models also support Layer 3 EtherChannels, but we will focus on configuring Layer 3 EtherChannels on multilayer switches. EtherChannel is most commonly used on switches rather than routers.

Because routed ports don't pose a risk of causing Layer 2 loops and, therefore, won't be disabled by STP, the benefits of Layer 3 EtherChannels are fewer than those of Layer 2 EtherChannels. However, they provide a simple way to load-balance packets over multiple links and can be easier to manage than multiple independent Layer 3 links; configuring IP addressing and routing for one logical interface is simpler than doing so for multiple independent interfaces.

Figure 16.12 shows a situation where you might want to use a Layer 3 EtherChannel: two LANs are connected via multilayer switches. Each LAN has a Layer 2 switch connected to a multilayer switch via a Layer 2 EtherChannel, and the two multilayer switches are connected via a Layer 3 EtherChannel.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-331_883_1414_392_194.jpg)
Figure 16.12 Two LANs connected via multilayer switches using Layer 3 EtherChannel. Additionally, the Layer 2 switch in each LAN uses Layer 2 EtherChannel to connect to the LAN's multilayer switch. The Layer 3 EtherChannel configuration is depicted.

Like Layer 2 EtherChannels, Layer 3 EtherChannels can be configured dynamically using PAgP, LACP, or statically with on mode. The only additions are the no switchport command to make routed ports and an IP address configured on the port channel interface. The following example shows the configuration of SW3's Layer 3 EtherChannel, as we saw in figure 16.12 using LACP as the negotiation protocol. Note that I use group 2 for this EtherChannel because group 1 is used by the Layer 2 EtherChannel connecting SW3 to SW1:
![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-331_327_1307_1783_316.jpg)

NOTE Make sure to configure the IP address on the port channel interface, not any of the member ports.

In the following example, I configure SW4's end of the connection and confirm the EtherChannel's status. Note that whereas Po1 (a Layer 2 EtherChannel connected to SW2) has the flags su, Po2 has the flags RU. R indicates a Layer 3 EtherChannel; think R for "routed port":

```
SW4(config)# interface range g0/2-3
SW4(config-if-range) # no switchport
SW4(config-if-range) # channel-group 2 mode active
SW4(config-if-range) # interface po2
SW4(config-if)# ip address 192.168.1.1 255.255.255.252
SW4(config-if-range)# do show etherchannel summary
Flags: D - down P - bundled in port-channel
    I - stand-alone s - suspended
    H - Hot-standby (LACP only)
    R - Layer3 S - Layer2
. . .
Group Port-channel Protocol Ports
------+-------------+-----------+-----------------------------------------------
1 Po1(SU) LACP Gi0/0(P) Gi0/1(P)
    Po2 (RU) LACP GiO/2(P) GiO/3(P)
        Po2 is a Layer 3
        EtherChannel.
```


## Summary

- EtherChannel combines multiple physical links into a single logical link. This allows multiple links between two switches to all remain in the STP forwarding state without causing Layer 2 loops.
- BUM frames received on a port that is a member of an EtherChannel will not be flooded out of other ports in the same EtherChannel.
- EtherChannel does not entirely remove the need for STP. In the context of a larger LAN, some EtherChannels may need to be disabled by STP.
- There are two main ways to configure EtherChannels: dynamic and static.
- Dynamic EtherChannels use one of two protocols-PAgP or LACP-to negotiate an EtherChannel between two neighboring switches.
- Port Aggregation Protocol (PAgP) is a Cisco-proprietary EtherChannel negotiation protocol that uses two modes: desirable and auto.
- Link Aggregation Control Protocol (LACP) is a standard EtherChannel negotiation protocol standardized by IEEE 802.3ad. It uses two modes: active and passive.
- desirable mode in PAgP actively tries to form an EtherChannel. auto mode does not actively try to form an EtherChannel but will form an EtherChannel with a neighbor in desirable mode.

- You can configure PAgP on a port with the channel-group group-number mode \{desirable | auto\} command in interface config mode. The group-number identifies which EtherChannel the port is a member of and is locally significant.
- After assigning a port to an EtherChannel, a logical port channel interface is automatically created. You should configure the relevant settings on the port channel interface (i.e., access or trunk mode).
- Use the show etherchannel summary command to check the status of EtherChannels on the switch.
- Commands configured on the port channel interface are inherited by the member ports, and vice versa. However, it is recommended that you configure port settings on the port channel interface to ensure consistency.
- Use the show pagp neighbor command to view information about the switch's neighbors using PAgP.
- Whereas PAgP supports EtherChannels of up to 8 links, LACP supports up to 16 links. However, only up to 8 links can be used at once; the others will act as standby links, ready to take over if an active link fails.
- active mode in LACP is equivalent to PAgP's desirable mode, and passive mode is equivalent to PAgP's auto mode. Otherwise, EtherChannel configuration and verification are identical to PAgP.
- You can configure LACP on a port with the channel-group group-number mode \{active | passive\} command in interface config mode.
- Use the show lacp neighbor command to view information about the switch's neighbors using LACP.
- Static EtherChannels do not use a protocol to negotiate EtherChannel formation. Ports are configured to statically form an EtherChannel without negotiating with the neighboring switch.
- Use the channel-group group-number mode on command to configure a port as a member of a static EtherChannel.
- An EtherChannel will not form between switch ports using a different negotiation protocol (PAgP or LACP) or between switch ports using a negotiation protocol and switch ports configured with mode on.
- The settings of an EtherChannel's member ports must match. For example, speed, duplex, operational mode (access or trunk), allowed VLANs, native VLAN, and access VLAN settings must match.
- If a switch's frame-switching logic determines that a frame should be forwarded or flooded out of an EtherChannel, an additional check determines which physical port the frame should be sent out of.
- Frames forwarded over an EtherChannel are load-balanced by flow. A flow is a communication between two nodes.

- The parameters a switch uses to identify flows can be configured with the port-channel load-balance parameters command in global config mode. Example parameters are src-dst-mac and src-dst-ip.
- Use the show etherchannel load-balance command to check which parameters the switch is using to identify flows for EtherChannel load balancing.
- EtherChannels are like highways: adding more links increases the Ether-Channel's total capacity (bandwidth) but doesn't increase the maximum speed of any communication over the EtherChannel.
- An EtherChannel consisting of switch ports (that switch frames) is a Layer 2 EtherChannel. An EtherChannel consisting of routed ports (the route packets) is a Layer 3 EtherChannel.
- To configure a Layer 3 EtherChannel, use the no switchport command on the member ports before using the channel-group command. Then, configure an IP address on the port channel interface (not the individual member ports).

Licensed to Luke Chen [mic215fa@gmail.com](mailto:mic215fa@gmail.com)

## Part 4

## Dynamic routing and first hop redundancy protocols

After focusing on Layer 2 concepts in part 3, part 4 is a return to Layer 3. In chapter 9, we covered static routes, which involve manually configuring routes on each router as necessary. That manual approach is simply not feasible in larger networks, and chapter 17 introduces a more scalable approach: dynamic routing. Dynamic routing protocols enable routers to communicate with each other and share routing information automatically, without the need to manually configure each router's routing table. Then, chapter 18 focuses on Open Shortest Path First (OSPF), one of the most commonly used dynamic routing protocols, and a major topic on the CCNA exam.

Chapter 19 moves away from the topic of dynamic routing to a different kind of protocol available on Cisco routers: first hop redundancy protocols (FHRPs). FHRPs enable multiple routers to team up to provide a resilient default gateway for hosts in a LAN; if a hardware failure or a similar issue affects one router, the other router is ready to take over and ensure that hosts in the LAN maintain continuous connectivity. FHRPs are a key tool in modern networks, which are expected to be reliably available on a 24/7 basis.

Part 4 is pivotal, as it dives into the more sophisticated aspects of routing and reliability. As always, the focus is on not just the theoretical knowledge of these important concepts but also the practical application on Cisco routers, both of which are essential for CCNA exam success. These are key CCNA exam topics for a good reason; dynamic routing protocols and FHRPs are used by enterprises of all sizes, and modern network professionals must be proficient at implementing them.

Licensed to Luke Chen [mic215fa@gmail.com](mailto:mic215fa@gmail.com)

## Dynamic routing

## This chapter covers

- The advantages of dynamic routing over static routing
- The types of dynamic routing protocols
- How a router decides which routes to enter in its routing table
- Activating dynamic routing protocols with the network command

After focusing on Layer 2 concepts for the previous several chapters, in this chapter, we return to the topic of routing-how routers forward packets between networks. In chapter 9, we learned about static routing, in which an administrator manually configures routes to build a router's routing table. When using dynamic routing, the topic of this chapter, routers communicate with each other and build their routing tables automatically. Although static routing has its uses, dynamic routing provides several advantages that we will examine in this chapter. We will cover elements of the following exam topics:

- 3.1 Interpret the components of routing table
- 3.2 Determine how a router makes a forwarding decision by default

- 3.3 Configure and verify IPv4 and IPv6 static routing
- 3.4 Configure and verify OSPFv2

### 17.1 Dynamic routing vs. static routing

Dynamic routing is a process by which routers share information about the network with each other, allowing them to build their routing tables without the need for an administrator to manually configure each route. They do this using a routing protocol-a protocol that defines how routers communicate with each other to share routing information and how they use that information to build their routing tables. We will cover the different kinds of routing protocols in section 17.2.

Figure 17.1 shows an example of how routing protocols work. R1, R2, and R3 use a routing protocol to exchange messages, informing each other of their known networks. Each router will use this information to build its routing table, without requiring an administrator to manually configure the routes.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-339_790_1414_879_192.jpg)
Figure 17.1 R1, R2, and R3 use a routing protocol to share routing information with each other. Each router will use this information to build its routing table.

NOTE Sharing of routing information is also called advertisement. R1, R2, and R3 advertise their known networks to each other.

Dynamic routing provides several advantages over static routing, such as adaptability and scalability. Let's take a closer look at those two advantages.

### 17.1.1 Adaptability

Static routes, as the name implies, are static and unchanging; they are incapable of reacting to changes in the network. If a route becomes invalid due to a hardware failure on one of the routers in the path, the static route won't adjust automatically to find an alternate path. Figure 17.2 shows an example: the R2-R3 link goes down due to a hardware failure. This causes R3 to remove its route to 192.168.30.0/24 from its routing table; it can no longer reach the next-hop IP address. R1, however, is unaware of the link's failure. As a result, R1 keeps its route to 192.168.30.0/24 via R2 in its routing table, even though it is no longer a valid route to reach the destination.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-340_624_1408_634_228.jpg)
Figure 17.2 The static route on R1 is unable to adapt to a change in the network. (1) A hardware failure causes the R2-R3 link to go down. (2) R2 removes its route to 192.168.30.0/24 because it can no longer reach the next hop. (3) R1, unaware of the hardware failure, leaves its route to 192.168.30.0/24 via R2 in its routing table, despite the existence of an alternate path.

NOTE If a router cannot reach a route's next-hop IP address, it will remove the route from the routing table. This applies to all kinds of routes and is why R2 removes its static route to 192.168.30.0/24 when the R2-R3 link fails.

Because R1's route to 192.168.30.0/24 via R2 remains in its routing table, it will continue to forward packets destined for hosts in the 192.168.30.0/24 network to R2. R2 then has no choice but to drop the packets because it has no route to 192.168.30.0/24 in its routing table. Figure 17.3 shows how dynamic routing fixes this; it allows R1 to automatically adapt to the network change, inserting a new route to the destination into its routing table.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-341_698_1414_181_194.jpg)
Figure 17.3 Dynamic routing allows routers to adapt to changes in the network by automatically calculating new routes. (1) A hardware failure causes the R2-R3 link to go down. (2) R2 removes its route to 192.168.30.0/24 via R3 and adds a new route with R1 as the next hop. (3) R1 removes its route via R2 and adds a new route with R4 as the next hop.

NOTE The route type of D in figure 17.3 indicates a route learned via the routing protocol Enhanced Interior Gateway Routing Protocol (EIGRP), which we will briefly cover in section 17.2. $D$ stands for Diffusing Update Algorithm (DUAL), the specific algorithm EIGRP uses to calculate routes.

In a matter of seconds after the failure, all routers in the network will be aware of the change and have new routes in their routing tables. And once the failed link is restored (perhaps a faulty cable is replaced), the routers will once again adapt to that change, returning their routing tables to their previous state. The adaptability of dynamic routing improves the resilience of the network; it is able to automatically recover from failures with minimal downtime. This is not possible in a network that exclusively uses static routing.

### 17.1.2 Scalability

Another major advantage of dynamic routing is scalability. While static routing may be practical for small networks, it becomes increasingly complex and unmanageable as the network grows. On the other hand, dynamic routing protocols easily scale to support very large and complex networks. Figure 17.4 shows a network of six routers, each connected to a LAN containing various subnets. Additionally, two routers have connections to an ISP. This is a fairly simple network, but even in a network of this size, manually configuring static routes to every destination on each router is not very
practical. A simpler option is to enable a routing protocol on each router and let them share the routing information themselves.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-342_615_1300_301_223.jpg)
Figure 17.4 Configuring static routes to every destination on each router, even in a fairly small network like this, is not practical; it would be both time-consuming and prone to human error.

Before we move on to examine the different types of routing protocols, it is worth mentioning that static routes have their own advantages, such as predictability and control. Because static routing involves manually configuring routes (specifying the next hop for each destination), they are useful in situations where you want to control the exact path that packets take. Fortunately, there is no need to choose between static and dynamic routing; you can use a combination of static and dynamic routing on a single router.

### 17.2 Types of routing protocols

Routing protocols can be divided into two main categories: Interior Gateway Protocols (IGP) and Exterior Gateway Protocols (EGP). IGPs are used to exchange routing information within a single autonomous system (AS)-the network of a single organization. EGPs, on the other hand, are used to exchange routing information between different autonomous systems, such as between an enterprise and an ISP or between two ISPs.

NOTE Gateway is an old term for a router. Although we call them routers these days, the term gateway is still used in some contexts (such as default gateway, as we covered in chapter 9).

Figure 17.5 demonstrates the difference between IGPs and EGPs. Each organization in the diagram uses an IGP to exchange routing information within their organization but an EGP to exchange routing information with other organizations.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-343_441_1414_181_192.jpg)
Figure 17.5 IGPs are used to exchange routing information with routers in the same AS (connected with solid lines). EGPs are used to exchange routing information with routers in a different AS (connected with dotted lines).

Although the CCNA exam focuses on one specific routing protocol (OSPF), you are also expected to understand some fundamental information about other routing protocols. Figure 17.6 lists the routing protocols that are commonly used today. In addition to being categorized as either an IGP or EGP, the routing protocols can be further categorized by algorithm type, which describes how the routers share information and calculate routes.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-343_468_1227_1088_320.jpg)
Figure 17.6 The five routing protocols in common use today. IGPs can be categorized as distancevector or link-state. The two distance-vector IGPs are RIP and EIGRP. The two link-state IGPs are OSPF and IS-IS. The only exterior gateway protocol in use today, Border Gateway Protocol, uses a path-vector algorithm.

### 17.2.1 Interior gateway protocols

IGPs use one of two algorithm types: distance-vector and link-state. In this section, we'll take a look at the basic characteristics of each.

## Distance-vector protocols

There are two main distance-vector routing protocols in use today: the industrystandard Routing Information Protocol (RIP) and the Cisco-developed Enhanced Interior

Gateway Routing Protocol (EIGRP). RIP is a very simple protocol that is usually only used in very small networks and labs. EIGRP, on the other hand, is more advanced than RIP-in fact, it's sometimes called an advanced distance-vector protocol-and is in use in many large-scale networks today.

NOTE Although EIGRP was developed by Cisco, most of its functionality was released to the public in RFC 7868, so other vendors can implement it on their devices. However, very few other vendors have implemented it, so it's generally safe to say that EIGRP only runs on Cisco routers.

Routers using a distance-vector protocol share information about their known networks and their metric to reach those networks. Metrics are a similar concept to STP's root cost. Whereas the STP root cost is a measure of the efficiency of a path to the root bridge, a metric is a measure of the efficiency of a route to the destination network; we will cover metrics in section 17.3.1. Figure 17.7 shows how a router learns of a destination network (LAN A) with a distance-vector routing protocol. R1 learns of LAN A from two neighbors: R2 and R5. Because the route via R5 has a lower metric, R1 inserts the route via R5 into its routing table; a lower metric value is preferable.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-344_495_1410_990_226.jpg)
Figure 17.7 Routers share routing information with a distance-vector routing protocol. R1 learns of LAN A from both R2 and R5 but inserts the route via R5 into its routing table due to the lower metric value.

NOTE The route type of R in figure 17.7 indicates a route learned via the routing protocol RIP. The details of RIP aren't relevant to the CCNA exam.

The key characteristic of distance-vector routing protocols is that each router doesn't have a complete map of the network; for each destination network it learns about, it only knows the metric and next-hop router. Using the example of figure 17.7, R1 knows that to reach LAN A, it can forward packets to R2, which has a metric of 2, or R5, which has a metric of 1; it doesn't know the details of the network beyond R2 and R5, as shown in figure 17.8.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-345_567_1290_185_320.jpg)
Figure 17.8 R1 learns of LAN A from R2 and R5 and selects a route based on that information. R1 is unaware of the details of the network beyond R2 and R5.

NOTE Distance-vector routing is sometimes called routing by rumor. Routers don't have a complete map of the network; they only know what their neighbors tell them.

## Link-state protocols

Like distance-vector routing protocols, there are two main link-state routing protocols in use today: Intermediate System to Intermediate System (IS-IS) and Open Shortest Path First (OSPF), both of which are industry-standard protocols. IS-IS is most commonly used in service-provider networks (such as ISP networks) and is not a topic on the CCNA exam; we won't cover it any further in this book. On the other hand, OSPF is one of the major topics of the CCNA exam, and we will cover it in detail in chapter 18.

When using a link-state routing protocol, each router creates a connectivity map of the network. To allow all routers to build their connectivity map, each router shares information about its connected links and the state of those links (the connected subnets, metric cost, etc.)-hence the name link state. This information is not shared only with directly connected neighbors; it is shared with all routers in the network, so they can all build the same connectivity map. Each router then uses this map of the network to calculate its best route to each destination in the network. Figure 17.9 demonstrates this concept; R1 builds a connectivity map and uses it to calculate a route to LAN A.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-346_613_1412_181_225.jpg)
Figure 17.9 R1 builds a connectivity map of the network and uses it to calculate a route to LAN A.

NOTE The route type of O in figure 17.9 indicates a route learned via the routing protocol OSPF, the topic of chapter 18.

Building this map of the network and using it to calculate routes requires more CPU and memory resources on the router than required by distance-vector routing protocols, which can be a concern in very large networks. The map is stored in memory using a structure called the link-state database (LSDB), and calculating routes from the LSDB can be CPU-intensive. However, there are methods to overcome this limitation, such as dividing the network into areas, which we'll cover in chapter 18.

### 17.2.2 Exterior gateway protocols

In modern networks, only a single EGP is widely used: Border Gateway Protocol (BGP). BGP uses a path-vector algorithm to calculate routes. Like the two IGP algorithm types, the name path-vector gives us a hint about how BGP works. The path is the series of autonomous systems a packet will travel through along the route to the destinationfor example, perhaps it will travel through two different ISPs before reaching the destination AS.

Figure 17.10 demonstrates path-vector logic: packets from R1 to destinations in Enterprise B will travel through ISP A and ISP B and then reach Enterprise B. R1 learns of this route by communicating with ISP A using BGP. Rather than making routing decisions based on the series of individual routers that packets will travel through, BGP makes routing decisions based on the series of autonomous systems they will travel through (each AS likely consisting of multiple routers).

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-347_352_1298_183_320.jpg)
Figure 17.10 R1's path to Enterprise B passes through ISP A and ISP B and then arrives at Enterprise B. Path-vector logic is AS-to-AS, rather than router-to-router.

NOTE Figure 17.10 is a simplification of how BGP works. BGP takes multiple attributes of the path into account, not just the number of autonomous systems.

### 17.3 Route selection

Route selection is something we already covered in chapter 9. That definition of route selection referred to packet forwarding; when a router forwards a packet, it will select the most specific matching route in the routing table.

However, route selection can also refer to the process of selecting which routes are used to populate the routing table. Routes learned via a dynamic routing protocol aren't automatically inserted into the router's routing table. Likewise, manually configured static routes don't necessarily enter the routing table either. If a route isn't in the routing table, it can't be used to forward packets. To summarize, the following are the two meanings of the term route selection:

- Routing table population-The process of selecting which routes the router will enter into its routing table
- Packet forwarding-The process of selecting the best route in the routing table to forward a particular packet

If a router learns multiple routes to the same destination, it will only insert the best route to that destination into its routing table. To determine which route is best, it will compare two parameters: metric and administrative distance.

### 17.3.1 The metric parameter

We already saw an example of how metrics work in figure 17.7. R1 learned two routes to reach LAN A: one with a metric of 2 and one with a metric of 1. Which of the two routes did R1 insert into its routing table? R1 selected the latter route because of its lower metric.

Each routing protocol's metric is calculated differently. RIP uses a simple hop count: the number of routers between the router and the destination is the route's metric. OSPF uses a cost value that is calculated from the bandwidth of each link in the path.

EIGRP uses a more complex calculation based on bandwidth and delay (how long it takes bits to travel across a link), as well as other parameters. Table 17.1 summarizes how RIP, EIGRP, and OSPF calculate metrics.

Table 17.1 IGP metrics
| IGP | Metric | Description |
| :--- | :--- | :--- |
| RIP | Hop count | Each router in the path counts as one hop. The total metric is the total number of hops to the destination. |
| EIGRP | Metric based on bandwidth and delay | A complex formula that can take into account many values. By default, bandwidth and delay are used. |
| OSPF | Cost | The cost of each link is calculated based on bandwidth. A route's metric is the total cost of each link in the path. |


Figure 17.11 shows an example of route selection in a network of routers that use OSPF to share routing information. R1 uses its connectivity map to calculate the possible routes to reach 192.168.3.0/24 and selects the best route for its routing table.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-348_598_1416_959_223.jpg)
Figure 17.11 R1 inserts the best route to 192.168.3.0/24 into its routing table. (1) R1 calculates the possible routes to 192.168.3.0/24. The route via R3 has a metric of 2, and the route via R2 has a metric of 4. (2) R1 selects the route via R3 because of its lower metric and inserts the route into its routing table.

The following example shows the route in R1's routing table. Pay attention to the two values in square brackets:

```
R1# show ip route
. . .
O 192.168.3.0/24 [110/2] via 192.168.1.6, 00:08:35, GigabitEthernet0/0
```

After the destination network of 192.168.3.0/24, the route includes two values in square brackets: [110/2]. The first value (110) is the route's administrative distance (covered in section 17.3.2), and the second value (2) is the route's metric. In the following example, I disable R1's G0/0 interface, which renders the route via R3 invalid. I then check the routing table again; the alternate route via R2, with a metric of 4, is inserted into the routing table in place of the route via R3:

```
R1(config) # interface g0/0
R1(config-if) # shutdown
R1(config-if) # do show ip route
. . .
0 192.168.3.0/24 [110/4] via 192.168.1.2, 00:00:04, GigabitEthernet0/1
```

The route via R2 (metric 4) is selected after the router via R3 (metric 2) is removed.

### 17.3.2 The administrative distance parameter

Although most routers will only run one routing protocol, there are cases where a router will run multiple protocols-for example, if two enterprises (running different routing protocols) connect their networks to enable communication between them. When running multiple routing protocols, a router can learn about the same destination network from different routing protocols. In such cases, the router needs a way to compare the routes to determine which should enter the routing table.

Each routing protocol uses different parameters to determine a route's metric: RIP uses a simple hop count, OSPF uses a cost based on bandwidth, and EIGRP's metric is calculated using a formula that can take many different factors into account. BGP's route selection process, which is beyond the scope of the CCNA exam, is far more complicated than a simple metric value. Because each routing protocol's metric is different, they can't be directly compared; it would be like asking, "Which is better: 20 kilograms or 10 kilometers?" If a router learns multiple routes to the same destination network from different routing protocols, it needs to use something else to select which route enters the routing table.

That is the role of administrative distance (AD). AD is a value that indicates how preferred a routing protocol is. A lower AD value indicates a routing protocol that IOS considers more "trustworthy"-more likely to select good routes. Whereas a metric is used to compare routes learned via the same routing protocol, AD is used to compare routes learned via different routing protocols. Table 17.2 lists the default AD values of some different routing protocols (including connected and static routes).

Table 17.2 Default AD values
| Route type/protocol | Default AD |
| :--- | :--- |
| Connected | 0 |
| Static | 1 |
| External BGP (EBGP) | 20 |
| EIGRP | 90 |
| OSPF | 110 |
| IS-IS | 115 |
| RIP | 120 |
| Internal BGP (IBGP) | 200 |
| Unusable route | 255 |


EXAM TIP Make sure you know these default AD values; I recommend using flashcards to memorize them.

As when comparing metric values, the lower AD value is preferred. Connected routes (the routes that are automatically added when you configure an IP address on an interface) have an AD of 0; they are always preferred over other route types. Static routes, with an AD of 1, are preferred over routes learned from any of the dynamic routing protocols by default; however, we'll cover how you can make them less preferred later in this section. The least-preferred AD value is 255. Cisco routers can use this value to mark a route unusable; it will be removed from the routing table.

Figure 17.12 shows an example in which AD is used to select which route enters the routing table. R1 learns of the destination network 10.0.0.0/24 via EIGRP and OSPF. Although the EIGRP route's metric value (3584) is numerically higher than the OSPF route's (4), that is irrelevant in this case; EIGRP and OSPF metric values can't be directly compared. Instead, AD is used to decide which route enters the routing table. EIGRP's AD (90) is lower than OSPF's (110), so R1 inserts the EIGRP route into its routing table.

NOTE Due to the formula EIGRP uses to calculate metrics, the metric values of EIGRP routes tend to be quite large compared to those of RIP and OSPF routes. Although this route's metric is 3584 (because R1 and the destination are only separated by a few routers), it's not rare for the metric of EIGRP routes to be in the range of tens or hundreds of thousands.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-351_475_1297_183_192.jpg)
Figure 17.12 R1 uses AD to select which route enters the routing table. (1) R1 learns two routes to 10.0.0.0/24: via EIGRP (metric 3584) and via OSPF (metric 4). (2) EIGRP's AD (90) is lower than OSPF's (110), so R1 inserts the EIGRP route into its routing table. The metric values are irrelevant in this case.

The following example shows the EIGRP route in R1's routing table. Take note of the values in square brackets-EIGRP's AD is 90, and the route's metric is 3584:

```
R1# show ip route
. . .
D 10.0.0.0 [90/3584] via 192.168.12.2, 00:33:30, GigabitEthernet0/0
. . .
```

NOTE Metrics and AD are only used to compare routes to the same destination-the same destination network address with the same prefix length. If two routes have the same destination network address but a different prefix length (i.e., 192.168.0.0/24 and 192.168.0.0/25), they are considered different destinations; both routes will be inserted into the routing table. We will clarify this point in section 17.3.3.

## ECMP

Metrics are used to select among routes to the same destination that were learned via the same routing protocol, and AD is used to select among routes to the same destination that were learned via different routing protocols. But what happens if a router learns multiple routes to the same destination via the same routing protocol and the routes have the same metric? In that case, all routes will be added to the routing table, and traffic will be load-balanced over them; half of the traffic will be sent using one route and the other half using the other route. This is called equal-cost multi-path (ECMP) routing.

NOTE By default, a maximum of four routes to the same destination can be used for ECMP routing.

Figure 17.13 shows an example of ECMP. R1 learns two routes to 10.0.0.0/24: one from R2 and one from R4. Both routes are learned via OSPF and have a metric of 4. Therefore, R1 inserts both routes into the routing table; it will load-balance traffic over the two routes.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-352_483_1328_183_225.jpg)
Figure 17.13 An example of ECMP routing. (1) R1 learns two routes to 10.0.0.0/24 via OSPF with the same metric: one from R2 and one from R4. (2) R1 inserts both routes into its routing table; it will load-balance traffic using the two routes.

## Floating static routes

By default, static routes have an AD of 1 and are thus preferred over routes learned using a dynamic routing protocol. However, there are cases where you might want to configure a static route as a backup that only enters the routing table if the main route (learned via a routing protocol) is lost. That is the role of floating static routes.

A floating static route is a static route configured with an AD greater than the default of 1 for the purpose of making it less preferred. For example, to make a static route less preferred than an OSPF route to the same destination, it should be configured with an AD greater than 110 (OSPF's AD). To configure a floating static route, just add the AD value to the end of the command. For example, you can configure a floating recursive static route with the ip route destination-network netmask next-hop ad command. Figure 17.14 shows an example in which I configure a static route with an AD of 111 to make it less preferable than a route learned via OSPF.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-352_540_1338_1492_226.jpg)
Figure 17.14 A floating static route is less preferred than an OSPF route to the same destination.

NOTE If you configure a floating static route with the same AD as a dynamic routing protocol, the static route will still be preferred. You should configure floating static routes with a higher AD than the routing protocol.

A floating static route serves as a backup route, offering a secondary path for data if the primary route fails. Although dynamic routing protocols also can provide the same functionality (by recalculating the next-best route if the current best route fails), a floating static route can provide a backup path via a router that the local router doesn't exchange routing information with-in figure 17.14, R1 has an OSPF neighbor relationship with R4 but not R2. Floating static routes also provide the benefits mentioned previously, such as control and predictability; they allow you to control exactly which path traffic will take if the primary route fails.

In the following example, I test the floating static route we saw in figure 17.14. When I first check R1's routing table, the OSPF route is present. I then disable R1's G0/1 interface (simulating a hardware failure) and check the routing table again; this time, the floating static route (with an AD of 111) has taken the place of the OSPF route:

```
R1(config)# do show ip route
. . .
O 10.0.0.0 [110/4] via 192.168.1.6, 00:25:43,
→GigabitEthernet0/1
. . .
R1(config) # interface g0/1
R1(config-if) # shutdown
R1(config)# do show ip route
. . .
S 10.0.0.0 [111/0] via 192.168.1.2
. . .
```

NOTE Static routes don't use the concept of metrics; their metric is always 0.

### 17.3.3 Route selection examples

Route selection in both of its meanings-routing table population and packet forwarding-is a very important topic for the CCNA exam. In this section, we'll look at some examples of each and clarify the concepts we have covered so far.

## Routing table population

Consider the following example. R1 learns the following routes via manual configuration and dynamic routing protocols:

- (A) 203.0.113.0/24 via static routing
- (B) 203.0.113.0/25 via RIP, metric 4
- (C) 203.0.113.0/26 via EIGRP, metric 5678
- (D) 203.0.113.0/27 via OSPF, metric 10

Which route(s) will R1 insert into its routing table? Having read up to this point, you may think that R1 will select the static route because it has the lowest AD. Or perhaps you think R1 will select the OSPF route because it has the longest prefix length (using the "most specific matching route" rule covered in chapter 9). However, the answer is that R1 will insert all four routes into its routing table.

The reason is that all four routes are to different destinations. Although they have the same destination network address (203.0.113.0), they all have different prefix lengths and are thus considered different destinations. There is no need to compare them; R1 will insert them all into the routing table.

It is worth noting that the four subnets in the example overlap, as shown in figure 17.15. The /25, /26, and /27 subnets are contained within the /24 subnet. However, when it comes to building the routing table, they are considered different destination networks and thus will all be inserted into the routing table.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-354_641_1054_801_346.jpg)
Figure 17.15 The four routes in the example overlap. 203.0.113.0/24 includes 203.0.113.0-.255, 203.0.113.0/25 includes 203.0.113.0-.127, 203.0.113.0/26 includes 203.0.113.0-.63, and 203.0.113.0/27 includes 203.0.113.0-.31. However, IOS considers them to be routes to different destinations and will insert all four routes into the routing table.

NOTE The concept of "most specific matching route" is irrelevant to this question. The most specific matching route rule is used to select which route in the routing table will be used to forward a packet; it is not used to select which routes will enter the routing table.

Let's take a look at one more example. R1 learns the following routes via dynamic routing protocols:

- (A) 10.0.0.0/8 via EIGRP, metric 2345
- (B) 10.0.0.0/8 via OSPF, metric 10
- (C) 10.0.0.0/16 via OSPF, metric 20
- (D) 10.0.0.0/16 via OSPF, metric 5

Which route(s) will R1 insert into its routing table in this example? Let's walk through the logic. In this example, R1 learns two routes to each of two different destination networks: two routes to 10.0.0.0/8 and two routes to 10.0.0.0/16. Therefore, R1 must select the best of the two routes to 10.0.0.0/8 and the best of the two routes to 10.0.0.0/16.

Which of the two routes to 10.0.0.0/8 will R1 prefer? Because R1 learns the two routes via different routing protocols, it will use AD to compare the routes. EIGRP's metric (90) is lower than OSPF's (110), so R1 will select the EIGRP route; therefore, A is one of the answers.

And which of the two routes to 10.0.0.0/16 will R1 prefer? In this case, both routes are learned via the same routing protocol, so R1 will compare their metric values to determine which route is preferable. Route D has the lower metric of the two, so it will be selected.

## Packet forwarding

The second aspect of route selection is packet forwarding-selecting which route in the routing table will be used to forward a particular packet. This process is simpler in that there is only one consideration: the most specific matching route. When forwarding a packet, the AD and metric values of routes are not considered.

However, identifying which route a router will select to forward a packet on can be difficult because the process of identifying which routes match a particular packet's destination and which of those matching routes is the most specific requires you to convert between binary and decimal. This is why I emphasized the importance of being comfortable with binary when covering IPv4 addressing and subnetting in previous chapters.

Let's look at an example of route selection in the context of packet forwarding. Examine R1's routing table in the following. Which route will it select to forward a packet destined for 203.0.113.65?

```
R1# show ip route
. . .
S 203.0.113.0/24 [1/0] via 192.168.1.2
R 203.0.113.0/25 [120/4] via 192.168.1.6
D 203.0.113.0/26 [90/5678] via 192.168.1.10
0 203.0.113.0/27 [110/10] via 192.168.1.14
```

The first step is to identify which routes match the packet's destination. As we covered in chapter 9, if the packet's destination IP address is a part of the network specified in the route, it's a match. In the following example, I have written out each route in binary, as well as the packet's destination IP address (203.0.113.65):
![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-356_398_1406_181_223.jpg)

EXAM TIP To speed up the process during the exam, you don't have to write out all 32 bits of every address. In this example, the shortest prefix length is /24, and the four routes and the destination IP address all begin with 203.0.113, so there is no need to write out the first three octets of all of them (they're all the same); you can just write out the relevant octets (the final octet, in this case).

The route to 203.0.113.0/24 matches the packet's destination, and so does the route to 203.0.113.0/25. The route to 203.0.113.0/26 doesn't match the packet's destination because the 26th bit doesn't match; it's 0 in the route but 1 in the packet's destination IP address. Likewise, the /27 route doesn't match because of the mismatched 26th bit.

Out of the two matching routes, the route to 203.0.113.0/25 is more specific; it has the longer prefix length. Therefore, it is the route that will be selected to forward the packet; R1 will forward the packet to the next hop 192.168.1.6. The fact that the route has a higher AD than the other three routes is irrelevant, and so is the route's metric; when selecting which route will be used to forward a packet, the only consideration is the most specific matching route. To summarize, here are the two aspects of route selection:

- Routing table population:
    - Metrics are used to select among routes to the same destination that are learned via the same routing protocol.
    - AD is used to select among routes to the same destination that are learned via different routing protocols.
- Packet forwarding:
    - The most specific matching route is selected.

Exam scenarios
To achieve CCNA exam success, it's essential that you understand dynamic routing-especially the route selection topics covered in this chapter. We already covered a few example questions, but here are a couple more that test your knowledge of route selection:
(continued)

1 (multiple choice, multiple answers)
R1 learns the following routes via dynamic routing protocols and manual configuration. Which routes will R1 insert into its routing table? (Select three.)
    A 10.0.0.0/16 via EIGRP, next hop 172.16.1.2, metric 7812
    B 10.0.0.0/16 via EIGRP, next hop 172.16.23.2, metric 7812
    c 10.0.0.0/16 via OSPF, next hop 10.0.0.1, metric 5
    D 10.0.0.0/24 via EIGRP, next hop 172.16.1.2, metric 1098
    E 10.0.0.0/24 via static routing, next hop 10.2.2.2, default AD

In this scenario, R1 learns routes to two different destination networks: 10.0.0.0/16 and 10.0.0.0/24. Of the three routes to 10.0.0.0/16, A and B are preferred for their lower AD-EIGRP's 90 vs OSPF's 110. Both EIGRP routes have identical metric values (7812), so R1 will insert both into its routing table-an example of ECMP. So A and B are two of the correct answers.

Of the two routes to 10.0.0.0/24, E is preferred for its lower AD-as a static route, its default AD of 1 is lower than that of any dynamic routing protocol. So the third correct answer is E.

2 (multiple-choice, single answer)
R1 has built its routing table through a combination of dynamic and static routing. When forwarding a packet, which of the following is used to select which route is used to forward the packet?
    A Longest prefix length
    B Most specific matching route
    c Lowest AD
    D Lowest metric

Although a router uses AD and metrics to select which routes to insert into its routing table, packet forwarding is simpler: the router will forward the packet according to the most specific matching route in the routing table (the matching route with the longest prefix length). Option A touches on the "longest prefix length" aspect but misses the critical "matching" aspect. Having the longest prefix length doesn't necessarily mean that a route will be selected to forward a packet; the route must also match the packet's destination IP address. So B is the best answer for this question.

### 17.4 The network command

RIP, EIGRP, and OSPF are all configured by activating the protocol on one or more of the router's interfaces. The router will then advertise the network prefix (network address and netmask) of the interface. RIP and EIGRP configuration are out of the scope of the CCNA exam, and we will cover OSPF configuration in greater detail in chapter 18, but in this section, we will look at one command that is shared by all three protocols: the network command. This command tells the router to

- Look for interfaces with an IP address that is in the specified range
- Activate the routing protocol on those interfaces
- Advertise the network prefix of the interface(s) to its neighbors

Although RIP, EIGRP, and OSPF all share the network command, there are differences in syntax between them; we will focus on OSPF, since its configuration is a CCNA exam topic. To configure OSPF on a Cisco router, use the router ospf process-id command in global config mode to enter router config mode-a new configuration mode, from which you can use the network command.

NOTE A router can run separate OSPF processes (instances), which is why you must specify a process-id in the router ospf command. However, the use cases of multiple OSPF processes are beyond the scope of the CCNA exam.

Figure 17.16 shows how the network command can be used to activate OSPF on a router's interfaces. The syntax of the network command is network ip-address wildcard-mask area area-id. The key to this command is the wildcard mask, which looks like an inverted netmask but serves a different purpose.

NOTE OSPF uses areas to logically divide up the network; we will cover OSPF areas in chapter 18. For now, we will just specify area 0 in the network command.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-358_514_1186_1146_360.jpg)
Figure 17.16 Activating OSPF on a router's interfaces with the network command. The command uses a wildcard mask, which looks like an inverted netmask.

A wildcard mask, like a netmask, is a series of 32 bits. The purpose of a wildcard mask is to indicate which bits need to match between two IP addresses and which bits don't. The wildcard mask in the network command specifies which bits have to match between the IP address in the network command and the IP address configured on a router's interface. A 0 bit in the wildcard mask means that the bits in the same position of the network command's IP address and the interface's IP address must match. A 1 bit in the wildcard mask means that the bits don't have to match.

Let's examine the three network commands used in figure 17.16. In the following example, I show the network command used to activate OSPF on R1's G0/0 interface. Note that the appropriate bits match between the network command's IP address (192.168.1.0) and R1 G0/0's IP address (192.168.1.1)-those specified by 0 bits in the wildcard mask:
![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-359_240_1406_419_192.jpg)
Next is the command I used to activate OSPF on R1's G0/1 interface. Once again, the appropriate bits match between the IP address specified in the network command and the IP address of G0/1:
![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-359_238_1429_858_192.jpg)
In these two examples, I used a wildcard mask of 0.0.0.3, which is equivalent to a /30 netmask (255.255.255.252) with the bits inverted. Figure 17.17 demonstrates this: all 1 bits in the 255.255.255.252 netmask are 0 in the 0.0.0.3 wildcard mask and vice versa.

| Netmask | 255 |  |  |  |  |  |  | 255 |  |  |  |  |  | 255 |  |  |  |  |  |  | 252 |  |  |  |  |  |  |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
|  | 1 | 1 | 1 | 1 | 1 |  | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 | 1 |  | 1 | 1 | 1 | 1 | 1 | 1 | 0 |
| Wildcard Mask | 0 |  |  |  |  |  |  | 0 |  |  |  |  |  | 0 |  |  |  |  |  |  | 3 |  |  |  |  |  |  |
|  | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 1 |

Figure 17.17 A / 30 netmask (255.255.255.252) and its equivalent wildcard mask (0.0.0.3). The 1 bits in the netmask are 0 in the wildcard mask and vice versa.

NOTE A shortcut to calculating a netmask's equivalent wildcard mask is to subtract each octet of the netmask from 255. The first three octets of a / 30 netmask are 255 and $255-255=0$. The final octet is 252 and $255-252=3$.

Finally, let's look at the command I used to activate OSPF on R1's G0/2 interface. This time, the interface's prefix length is $/ 24$, so I used the wildcard mask equivalent of a /24 netmask: 0.0.0.255:

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-360_242_1429_181_223.jpg)
Table 17.3 lists some netmasks and their equivalent wildcard masks.

Table 17.3 /24+ netmasks and wildcard masks
| Prefix length | Netmask | Wildcard mask (last octet binary) |
| :--- | :--- | :--- |
| /24 | 255.255.255.0 | 0.0.0.255 (11111111) |
| /25 | 255.255.255.128 | 0.0.0.127 (01111111) |
| /26 | 255.255.255.192 | 0.0.0.63 (00111111) |
| /27 | 255.255.255.224 | 0.0.0.31 (00011111) |
| /28 | 255.255.255.240 | 0.0.0.15 (00001111) |
| /29 | 255.255.255.248 | 0.0.0.7 (00000111) |
| /30 | 255.255.255.252 | 0.0.0.3 (00000011) |
| /31 | 255.255.255.254 | 0.0.0.1 (00000001) |
| /32 | 255.255.255.255 | 0.0.0.0 (00000000) |


## Why wildcard masks?

You might wonder why we use wildcard masks instead of netmasks. The key is to recall what netmasks are used for: they identify the size of a network prefix, distinguishing between the network portion and the host portion of an IP address. For example, configuring ip address 192.168.1.1 255.255.255.0 on a router interface tells the router that its IP address is 192.168.1.1-the first three octets are the network portion, and the last octet is the host portion. The prefix is 192.168.1.0/24.

However, when dealing with the OSPF network command, the IP address and wildcard mask are used differently; they don't define a network prefix as a netmask would. Instead, they specify a range of IP addresses, not necessarily part of the same subnet. This range is used to determine which router interfaces will take part in OSPF, meaning which ones will send and receive OSPF routing information. Here's a quick recap:

- A netmask (or subnet mask) is used to distinguish the network and host portions of an IP address. It determines the length of a subnet's network prefix.
- In the context of the OSPF network command, an IP address and wildcard mask do not define a network prefix. Instead, they define a range of IP addresses (that aren't necessarily part of the same subnet). This range is used to determine which interfaces on the router will participate in the OSPF process (i.e., which interfaces will send and receive OSPF routing information).

In the three network commands we looked at, I used the network address (host portion of all 0s) of each interface and the wildcard mask that is equivalent to the netmask of each interface. However, it's important to note that the network command is flexible: as long as the appropriate bits match between the IP address in the network command and the interface's IP address (those indicated by a 0 bit in the wildcard mask), OSPF will be activated on the interface. Figure 17.18 shows a different way to activate OSPF on R1's interfaces, this time using only two network commands.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-361_493_1286_542_327.jpg)
Figure 17.18 Activating OSPF on R1's interfaces with two network commands. The first command activates OSPF on R1's G0/0 and G0/1 interfaces, and the second activates OSPF on G0/2.

The first command activates OSPF on both G0/0 and G0/1; the appropriate bits of each interface's IP address match the IP address specified in the network command. By using a wildcard mask of 0.0.0.7-equivalent to a /29 netmask (255.255.255.248)-I tell R1 to activate OSPF on all interfaces with an IP address from 192.168.1.0 to 192.168.1.7, which includes G0/0 (192.168.1.1) and G0/1 (192.168.1.5):
![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-361_274_1433_1458_190.jpg)

The second command activates OSPF on G0/2 in a different manner than in the previous example. By specifying G0/2's exact IP address in the network command and using a wildcard mask of 0.0.0.0-equivalent to a /32 netmask (255.255.255.255)-I tell R1 to activate OSPF only on the interface with IP address 192.168.2.1, R1's G0/2 interface:
![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-362_242_1431_181_223.jpg)

As we just saw, the network command is quite flexible. The result of the previous two examples (figures 17.17 and 17.18) is the same: OSPF is activated on R1's interfaces, and R1 will advertise the network address of its interfaces to its neighbors. However, the recommended method of using the network command is the last one we looked at: specify the exact IP address of the interface and use a wildcard mask of 0.0.0.0.

The reason for this recommendation is to prevent unintended activation of OSPF on interfaces. If you use a network command with a broader range of addresses, any interface with an IP address in that range will be included in the OSPF process. By specifying the exact IP address of the interface with a wildcard mask of 0.0.0.0, you ensure that only the desired interface is included.

NOTE A shortcut to activate OSPF on all interfaces is to use network 0.0.0.0 255.255.255.255 area 0. A wildcard mask of 255.255.255.255 is equivalent to a netmask of 0.0.0.0 and matches all possible IP addresses. This is handy for quickly activating OSPF in a lab setting but is not recommended in a real network.

There are two major misconceptions that many students have about the OSPF network command. The first is that the wildcard mask in the network command has to match the netmask of the interface. This is not the case, as we saw in the previous examples; as long as the correct bits match between the IP address in the network command and the interface's IP address-the bits specified by the wildcard mask-OSPF will be activated on the interface.

The second misconception is that the network command specifies which networks OSPF should advertise. It does not; rather, it specifies which interfaces OSPF should be activated on. The router will then advertise the network prefix of the interface. To clarify, let's look at what the two commands in figure 17.18 do. Here is the first command:

```
R1(config-router) # network 192.168.1.0 0.0.0.7 area 0
```

Although 0.0.0.7 is equivalent to a /29 netmask, this command does not tell R1 to advertise the 192.168.1.0/29 network. It tells R1 to do the following:

- Look for interfaces with an IP address in the 192.168.1.0 to 192.168.1.7 range:
    - R1 G0/0 and G0/1's IP addresses are in this range.
- Activate OSPF on those interfaces.
- Advertise the network prefix of the interfaces to neighbors:
    - R1 will advertise G0/0's prefix of 192.168.1.0/30 and G0/1's prefix of 192.168.1.4/30.

And here is the second command, used to activate OSPF on R1's G0/2 interface:

```
R1(config-router) # network 192.168.2.1 0.0.0.0 area 0
```

This command does not tell R1 to advertise 192.168.2.1/32 but rather to activate OSPF on the interface with an IP address of 192.168.2.1 (G0/2) and advertise that interface's network prefix, which is 192.168.2.0/24.

EXAM TIP In chapter 18, we will look at a more straightforward method to activate OSPF on a router's interfaces. However, for the CCNA exam, you should be familiar with the network command and wildcard masks.

## Summary

- Dynamic routing is a process by which routers share information about the network with each other, allowing them to build their routing tables automatically. They do this by using a routing protocol.
- Dynamic routing provides several advantages over static routing, such as adaptability and scalability.
- Static routing also has advantages over dynamic routing, such as predictability and control. Fortunately, both can be used at the same time.
- Routing protocols can be divided into two main categories: Interior Gateway Protocols (IGP) and Exterior Gateway Protocols (EGP).
- IGPs are used to exchange routing information within a single autonomous system (AS), and EGPs are used to exchange routing information between autonomous systems.
- IGPs can be categorized by the type of algorithm they use to share routing information and calculate routes: distance-vector or link-state.
- The two distance-vector IGPs are Routing Information Protocol (RIP) and Enhanced Interior Gateway Routing Protocol (EIGRP).
- The two link-state IGPs are Intermediate System to Intermediate System (IS-IS) and Open Shortest Path First (OSPF).
- Distance-vector routing is also called routing by rumor. The router doesn't have a complete map of the network; it only knows what its neighbors tell it.
- Routers using a link-state routing protocol build a "connectivity map" of the network and use that map to calculate routes to each destination.
- The only EGP in common use today is Border Gateway Protocol (BGP). It uses a path-vector algorithm, which calculates routes using AS-to-AS logic, rather than router-to-router; it's designed for larger-scale routing.
- The term route selection has two main meanings, one regarding routing table population and one regarding packet forwarding.

- In routing table population, route selection is the process of selecting which routes will enter the routing table. The router will use AD and metrics to select which routes enter the routing table.
- In packet forwarding, route selection is the process of selecting the best route in the routing table to forward a packet. The router will select the most specific matching route in the routing table to forward the packet.
- Only the best route to each destination will be added to the routing table and, therefore, be a candidate for packet forwarding. Routes that do not enter the routing table cannot be used to forward packets.
- A metric is a measure of the efficiency of a route. It is used to select among routes to the same destination (same network address and prefix length), learned via the same routing protocol.
- Each IGP uses a different metric. RIP uses hop count, EIGRP uses a metric based on bandwidth and delay (and other parameters), and OSPF uses a cost based on the bandwidth of each link.
- Because each protocol uses a different metric, metrics cannot be used to compare routes learned via different routing protocols.
- Administrative distance (AD) is a value that indicates how preferred a routing protocol is. A lower AD value is preferred and means that IOS considers the protocol more trustworthy-more likely to select good routes.
- AD is used to select among routes to the same destination, learned via different routing protocols.
- The default AD values are Connected = 0, Static = 1, eBGP = 20, EIGRP = 90, OSPF = 110, IS-IS = 115, RIP = 120, iBGP = 200, unusable = 255.
- If multiple routes to the same destination are learned via the same routing protocol and have the same metric, they will all be inserted into the routing table, and the router will load-balance traffic over the routes. This is called equal-cost multipath (ECMP) routing.
- A floating static route is a static route configured with an AD greater than the default of 1 for the purpose of making it less preferred. The command syntax is ip route destination-network netmask next-hop ad.
- Even if multiple routes have the same destination network address, their destinations are considered different if they have different prefix lengths. They will all be inserted into the routing table (i.e. 192.168.0.0/16, 192.168.0.0/17, and 192.168.0.0/18).
- The network command is used by RIP, EIGRP, and OSPF to activate the protocol on the router's interfaces. It is configured in router config mode.
- To enter router config mode for OSPF, use the command router ospf process-id. The syntax of the OSPF network command is network ip-address wildcard-mask area area-id.

- The network command tells the router to
    - Look for interfaces with an IP address in the specified range
    - Activate the routing protocol on those interfaces
    - Advertise the network prefix of the interface(s) to neighbors
- A wildcard mask is a series of 32 bits that indicates which bits have to match between two IP addresses. A 0 bit in the wildcard mask means the bits have to match, and a 1 bit in the wildcard mask means the bits don't have to match.
- A wildcard mask looks like an inverted netmask but serves a different purpose. A netmask indicates the network and host portions of an IP address, and a wildcard mask is used to specify a range of IP addresses (not necessarily in the same subnet).
- All 1 bits of a netmask are 0 in the equivalent wildcard mask and vice versa. For example, a /24 netmask is 255.255.255.0, and the equivalent wildcard mask is 0.0.0.255.
- If the appropriate bits match between the IP address in the network command and an interface's IP address, OSPF will be activated on the interface.
- The network command is flexible. The IP address and wildcard mask of the command don't have to be the same as the IP address and netmask of the interface. If the correct bits match between the network command's IP address and the interface's IP address, OSPF will be activated on the interface.
- It is recommended to specify the interface's exact IP address and use a /32 wildcard mask (0.0.0.0) in the network command to ensure that OSPF is activated only on the intended interface.
- A shortcut to activate OSPF on all interfaces is to use network 0.0.0.0 255.255.255.255 area 0, because this matches all possible IP addresses. This is a handy shortcut in a simulated lab, but it is not recommended in a real network.
- The IP address and wildcard mask in the network command do not determine which prefixes the router advertises. They determine which interfaces OSPF is activated on, and then the router advertises those interface's prefixes.

## Open Shortest Path First

## This chapter covers

- Open Shortest Path First link-state advertisements and database
- How OSPF routers calculate routes
- Configuring OSPF on Cisco routers
- How OSPF routers become neighbors and form adjacencies

Open Shortest Path First (OSPF) is an interior gateway protocol (IGP) that serves as a key building block of modern enterprise networks. In chapter 17, we covered dynamic routing protocols in general and also examined how to use the network command to activate OSPF on router interfaces. In this chapter, we will dig deeper into the topic of OSPF and see how it actually works, including how OSPF-enabled routers become neighbors with each other, share routing information, calculate routes, and many other details.

These days, two versions of OSPF are in use: OSPFv2, which is primarily used for IPv4 networks, and OSPFv3, which is primarily used for IPv6 networks (although it can be used for IPv4 as well). For the purpose of the CCNA exam, the version we are concerned with is OSPFv2; all mentions of OSPF in this book are specifically referring to OSPFv2, as stated in exam topic 3.4: Configure and verify single area OSPFv2.

The name Open Shortest Path First has two aspects: Open means that it is an open standard protocol; OSPF is not Cisco proprietary. All vendors are free to implement

OSPF on their network devices. Shortest Path First (SPF) is the name of the algorithm used to calculate routes; it is also called Dijkstra's algorithm after its creator, Edsger Dijkstra.

### 18.1 OSPF foundations

There are three main steps that OSPF routers go through to share routing information and build their routing tables:

1 Form neighbor relationships with other OSPF-enabled routers.
2 Exchange routing information to build a connectivity map of the network.
3 Calculate the best routes to each destination.

Let's begin by examining the second and third steps of that process, and then we'll examine the details of how OSPF routers become neighbors in section 18.3.

### 18.1.1 The link-state database

When using a dynamic routing protocol, routers exchange routing information with each other and then use that information to calculate routes automatically. In OSPF, routers exchange routing information using data structures called link-state advertisements (LSA). Each router organizes the LSAs it receives in a database called the link-state database (LSDB). The LSDB serves as the router's map of the network topology, which is used for calculating the shortest path to each destination network; this is the "connectivity map" of the network that I mentioned in chapter 17.

Figure 18.1 shows how OSPF-enabled routers share LSAs to build the LSDB. Each router creates an LSA that includes information about its connected networks and then sends it to its neighboring routers, which will proceed to forward it to other routers in the OSPF area; OSPF areas are logical divisions of the network that we'll cover in section 18.1.2. The process of sending LSAs to all other routers in the OSPF area is called LSA flooding.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-367_478_1260_1500_194.jpg)
Figure 18.1 Routers flood LSAs within the OSPF area to ensure all routers have an identical LSDB. The LSAs that make up the LSDB contain information about each router's connected networks, such as their cost. R1's LSA indicates that it is connected to 10.0.0.0/30,10.0.0.4/30, and 10.1.1.0/24, each with a cost of 1.

It is essential that all routers in the area have the same LSDB, so each router can use the SPF algorithm to calculate routes to all destination networks. For example, the routers in figure 18.1 will use the LSDB to calculate a route to the 10.1.1.0/24 network. Using the LSDB, it is as if the routers are looking at the same diagram as us but with more detailed information about each link. This is in contrast to distance-vector routing protocols like RIP and EIGRP, in which each router does not have a complete map of the network.

### 18.1.2 OSPF areas

The network we saw in figure 18.1 was an example of single-area OSPF; the network is a single logical unit, and all routers in it share the same LSDB. In small networks, a single-area approach is nice and simple and doesn't have any major downsides. However, in a large network with dozens or hundreds of routers (instead of the six in figure 18.1), a single-area design has multiple negative effects:

- The larger LSDB takes up more memory resources on the routers.
- The SPF algorithm takes more time and CPU resources to calculate routes.
- A single change in the network (i.e., an interface going up or down) causes LSAs to flood the network and causes every router to run the SPF algorithm again.

By dividing a large OSPF network into multiple smaller areas, we minimize those negative effects. Smaller LSDBs mean OSPF uses fewer resources on routers, and the SPF algorithm doesn't take as much time to calculate routes. Network changes are only advertised within the local area, and if there is network instability (interfaces going up and down), its effects are limited to a single area.

An OSPF area can be defined as a set of routers that share the same LSDB. Although the CCNA exam topics list states that you should be able to "configure and verify single area OSPFv2," you also need a basic understanding of what an area is and the benefits of multi-area OSPF.

Figure 18.2 shows a multi-area OSPF network consisting of four areas. OSPF employs a two-level hierarchical structure consisting of a backbone area (area 0) and other non-backbone areas. All non-backbone areas must be connected to area 0, and traffic between areas must pass through area 0.

NOTE In single-area OSPF, it's highly recommended that you use area 0, although it is technically possible to use another area number. Using area 0 simplifies the process of expanding into multi-area OSPF in the future, if needed.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-369_580_1290_181_192.jpg)
Figure 18.2 A multi-area OSPF network consisting of four areas. Area 0 is the backbone area to which other areas must connect. Areas 1, 2, and 3 are non-backbone areas, and traffic between them must pass through area 0.

OSPF distinguishes between four different types of routers. Table 18.1 summarizes these four router types and lists which routers in figure 18.2 are of each type; note that some routers fit into multiple categories.

Table 18.1 OSPF router types
| Router type | In figure 18.2 | Description |
| :--- | :--- | :--- |
| Internal router | R1, R2, R6, R7, R8, R9, R10, R11, R12, R13 | All OSPF-enabled interfaces are in the same area. |
| Backbone router | R1, R2, R3, R4, R5 | At least one interface is in area 0. |
| Area border router (ABR) | R3, R4, R5 | Routers that connect one (or more) areas to area 0 |
| Autonomous system boundary router (ASBR) | R1 | Routers that connect the OSPF autonomous system to external networks (i.e., the internet) |


NOTE Although R1 has an interface connected to the internet, that interface does not use OSPF and therefore does not affect its classification as an internal OSPF router.

An internal router has all of its OSPF-enabled interfaces in the same area, whether that is area 0 or a non-backbone area. A backbone router has at least one interface in the backbone area. An area border router connects one (or more) areas to area 0; because non-backbone areas cannot connect directly to each other, all ABRs are also backbone routers (although backbone routers aren't necessarily ABRs).

NOTE ABRs maintain a separate LSDB for each area they are connected to.
The final router type is an autonomous system boundary router (ASBR)-a router that connects the OSPF autonomous system (AS) to external networks, such as the internet or another enterprise's network. Note that this type is independent of the others; an ASBR can be a backbone router or not and can be an internal router or an ABR. As listed in table 18.1, figure 18.2's R1 is an internal router, a backbone router, and an ASBR, all at once. In section 18.2.4, we'll see how to configure a default route on an ASBR and advertise it to other routers in the OSPF AS.

OSPF routes can be categorized as intra-area, inter-area, or external. Intra-area routes are routes to destinations in the same OSPF area as the router; for example, if the router is an internal router in area 1, all routes to destinations within area 1 are intraarea routes. Inter-area routes are routes to destinations in an area the router does not connect to; for example, if an ABR connected to areas 0 and 1 learns a route to area 2. Finally, routes to external destinations-advertised by an ASBR-are called external routes.

EXAM TIP Although the exam topics list only specifies single-area OSPF, you should know the different router and route types for the CCNA exam.

### 18.1.3 OSPF cost

As we covered in chapter 17, OSPF's metric is called cost, and it's the value OSPF uses to determine the best path to each destination (the shortest in OSPF). Each OSPFenabled interface has an associated cost value, and a route's cost is the cumulative cost of each interface a packet must be sent out of to reach the destination. Figure 18.3 demonstrates this concept; R1's cost to reach 192.168.3.0/24 is the cumulative cost of R1 G0/0, R2 G0/1, and R3 G0/1 (but not R2 G0/0 and R3 G0/0).

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-370_284_1218_1423_350.jpg)
Figure 18.3 A route's cost is the cumulative cost of each interface out of which a packet must be sent to reach the destination. R1's cost to reach 192.168.3.0/24 is 3: 1 for R1 G0/0, plus 1 for R2 G0/1, plus 1 for R3 G0/1.

Although figure 18.3's depiction of OSPF cost is accurate, it's more common to refer to the cost of a link rather than the cost of each interface that makes up the link. Because both sides of the link should have the same cost-it is considered a misconfiguration if they don't-and a router considers only one side of the link when doing route cost calculations, it's simpler to think of the link as a single entity.

Using figure 18.3's example, it doesn't matter if R1 is calculating a route to 192.168.3.0/24 or R3 is calculating a route to 192.168.1.0/24; the cost is the same in either direction. Except when specifically referring to an interface's cost, I will refer to a link's cost rather than each individual interface's cost.

## Reference bandwidth

The cost of a link is calculated by dividing a reference bandwidth value by the link's bandwidth; the default reference bandwidth is 100 Mbps. If this calculation results in a value less than 1, the OSPF cost is assigned as 1 because OSPF does not accept fractional or decimal values for cost. This gives the following default OSPF cost values:

- 10 Mbps link = 10 ( $100 / 10$ )
- 100 Mbps link = 1 (100/100)
- $1,000 \mathrm{Mbps}(1 \mathrm{Gbps})$ link $=1(100 / 1000=0.1)$
- 10,000 Mbps (10 Gbps) link = 1 (100/10000 = 0.01)

You may have noticed a problem: with the default reference bandwidth of 100 Mbps, links with a bandwidth of 100 Mbps or greater all have the same cost. If OSPF considers a 100 Mbps link just as preferable as a 1 Gbps or 10 Gbps link, it's likely to calculate suboptimal routes. Figure 18.4 shows an example: although R1's route to 10.1.3.0/24 via R4 uses FastEthernet links, it is considered equal to R1's route via R2, which uses GigabitEthernet links.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-371_575_1414_1165_192.jpg)
Figure 18.4 Although the route to 10.1.3.0/24 via R2 is superior to the route via R4, the OSPF cost of both routes is the same. R1 adds both routes to the routing table and will load-balance over them with ECMP.

R1 adds both routes to the routing table and will use ECMP to load-balance traffic over them. While ECMP itself isn't a bad thing, attempting to forward equal amounts of traffic over links with vastly different bandwidths could lead to congestion on the slower links, while the faster links are underutilized.

OSPF was developed decades ago when FastEthernet was considered a very highspeed link; that's why the default reference bandwidth is 100 Mbps. However, in modern networks it's better to adjust the reference bandwidth value to allow OSPF to differentiate between links of greater bandwidths. To adjust the reference bandwidth value, use the auto-cost reference-bandwidth mbps command in OSPF router config mode.

In the following example, I confirm the cost of R1's interfaces with the show ip ospf interface brief command. I then adjust the reference bandwidth to 1000 Mbps, so GigabitEthernet links have a cost of 1, and FastEtherent links have a cost of 10. Although there is no specific recommended value to use for the reference bandwidth, it's common to set it to match the bandwidth of the fastest link in the network. Another option is to set the reference bandwidth to a value greater than the current fastest link's bandwidth to allow even faster links to be added to the network without adjusting the reference bandwidth again:
![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-372_605_1464_795_220.jpg)

As the warning message in the previous example states, it's important that you configure the same reference bandwidth on all routers to ensure consistent route selection. In the worst-case scenario, having different reference bandwidths could result in routing loops: if router A believes the best route to a destination is via router B, but router B believes the best route is via router A, packets for that destination will loop between the two routers until their TTL expires-never reaching their intended destination.

## Modifying the cost of a link

Although modifying the reference bandwidth is usually sufficient to allow OSPF to select the most efficient path to a destination, in some cases, you may want to modify the cost of a particular link to make it more or less preferable. There are two methods to do so:

- Configure the cost with the ip ospf cost cost command in interface config mode.

- Modify the interface's bandwidth value with the bandwidth kbps command in interface config mode.

NOTE Although the reference bandwidth is configured in megabits per second, the interface bandwidth value is configured in kilobits per second.

Of the two, the first method is preferred if you want to modify the OSPF cost of a particular link: it allows you to directly configure the interface's OSPF cost. The second method, instead, is used to influence the automatic cost calculation we covered previously: reference bandwidth/interface bandwidth. However, an interface's bandwidth value affects more than just OSPF cost values, such as QoS (quality of service) mechanisms, so it's usually best not to modify it.

NOTE Modifying an interface's bandwidth value doesn't actually affect how the interface operates; it's just a value used for calculations like OSPF cost, EIGRP metric, etc. To change the speed at which an interface operates, use the speed command.

### 18.2 OSPF configuration

In chapter 17, we looked at how to activate OSPF on router interfaces with the network command. That command alone is enough to get OSPF working at its most basic level; after activating OSPF on a router's interfaces, the router will attempt to form neighbor relationships and share LSAs with other routers connected to those interfaces. In this section, we'll look at how to configure various other aspects of OSPF, including a simpler way to activate OSPF on a router's interfaces.

The first step in configuring OSPF is to create an OSPF process with the router ospf process-id command; this is the command used to enter router config mode, from which you can use the network command and many other OSPF-related commands. Similar to how PVST+ creates multiple STP instances on a switch, each calculating a unique spanning tree, you can create multiple OSPF processes. Each of those processes independently calculates its own routes to destination networks. However, running multiple OSPF processes on a router is extremely rare, and the specific use cases are beyond the scope of the CCNA exam. For the purposes of this chapter, we will just use process ID 1 (although you can pick any other number you'd like).

NOTE The OSPF process ID is locally significant; it doesn't matter if the process ID matches neighboring routers. For example, a router running OSPF process ID 1 can become a neighbor of a router running OSPF with a process ID of 65535 (the highest possible value).

Figure 18.5 shows the topology we will configure in this section: a single-area OSPF network of four routers. The 10.1.3.0/24 subnet is connected to R3 G0/1, which we will configure as a passive interface-more about that in section 18.2.3. R1 connects to the internet, and we will configure it to advertise a default route to the other routers.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-374_408_1426_181_224.jpg)
Figure 18.5 A single-area OSPF network of four routers. R3 connects to the 10.1.3.0/24 subnet on its G0/1 interface, a passive interface. R1 connects to the internet and advertises a default route to the other routers.

### 18.2.1 The router ID

When you first use the router ospf command to create the OSPF process, the router will assign itself a router ID (RID)-a unique 32-bit value that identifies the router in the OSPF AS. Unlike the process ID, the RID must be unique; there can't be two routers with the same RID in the AS. To view a router's RID, you can use the show ip protocols command; this command shows information about active routing protocols on the router. In the following example, I create OSPF process 1 on R1 and check its RID:

```
R1(config)# router ospf 1
R1(config-router) # do show ip protocols
        Creates OSPF process 1
. . .
Routing Protocol is "ospf 1"
    Outgoing update filter list for all interfaces is not set
    Incoming update filter list for all interfaces is not set
    Router ID 203.0.113.1
    Number of areas in this router is 1. 1 normal 0 stub 0 nssa
        . . .
```

R1 selected the IP address of its G0/2 interface-203.0.113.1-as its RID. The RID is assigned in the following order of priority:

1 Manual configuration with the router-id command
${ }_{2}$ Highest IP address on an operational (up/up) loopback interface (we'll cover loopback interfaces in this section)
3 Highest IP address on an operational (up/up) physical interface

As indicated in the first point, you can manually configure the RID with the router-id command. However, this command can only be configured once you've entered router config mode. To access router config mode, you have to first create the OSPF process with the router ospf command.

Therefore, when you first create the OSPF process, the router can't consider any manual RID configuration: one hasn't been set yet. Instead, it only considers the second
and third options to assign the RID: the highest IP address on an operational loopback interface (if any exist) or, failing that, the highest IP address on an operational physical interface. In this case, R1 selected the highest IP address on an operational physical interface-that of G0/2.

NOTE If you create an OSPF process and the router has no operational interfaces with an IP address, the process won't be able to start until you configure the RID or an interface with an IP address becomes operational.

## Loopback interfaces

A loopback interface is a virtual router interface that is always up and reachable as long as the device is operational (although you can disable the interface with shutdown). Unlike physical interfaces (i.e., GigabitEthernet0/1), which rely on physical ports and connections, loopback interfaces are entirely software-based. The benefit of a loopback interface is that it provides a stable, reliable interface that you can use to identify and connect to the router without relying on any particular physical port. Figure 18.6 shows how R1's loopback interface provides a stable interface for an admin to connect to with Secure Shell (SSH).

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-375_468_1414_1005_192.jpg)
Figure 18.6 An admin uses SSH to connect to R1's Loopback0 interface. Loopback0 doesn't rely on the status of a particular physical interface, such as GO/0, which is down due to a hardware failure.

NOTE SSH is a protocol used to remotely access a device's CLI in a secure manner; we will cover it in chapter 5 of volume 2 of this book.

If the admin had, for example, attempted to connect to the IP address of R1's G0/0 interface, the connection would have failed; the $\mathrm{G} 0 / 0$ interface is down due to a hardware failure. On the other hand, the Loopback0 interface provides a stable IP address that the admin can connect to regardless of the status of R1's physical ports. However, it's important to note that the admin's PC still needs a valid physical path to reach R1. If both G0/0 and G0/1 were down, the PC would not be able to connect to R1 despite the loopback interface.

Although loopback interfaces are not required, it is highly recommended that you configure them on routers and that you activate OSPF on them so all routers can reach each other's loopback interfaces. To create a loopback interface, use the interface loopback number command-it's common to start with Loopback0. In the following example, I configure a loopback interface on R1, configure an IP address on it, and check the OSPF RID once again:
![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-376_306_1302_483_346.jpg)

NOTE It's standard to configure loopback interfaces with a /32 netmask (255.255.255.255). This is because a loopback interface, by its nature, doesn't need to communicate with other devices on the same subnet (as a physical interface would). Instead, it represents a single device and thus doesn't need a range of addresses.

Despite configuring a loopback interface, R1's RID remains the same; to keep the RID stable, IOS doesn't reselect it when a new interface is configured. It is possible to reset the OSPF process with the clear ip ospf process command in privileged EXEC mode, but even that won't cause R1 to select the loopback interface's IP address as the RID, as shown in the following example:
![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-376_304_1227_1351_348.jpg)

NOTE The point to remember here is that once the router has selected its RID, it will maintain that RID even if you configure a loopback interface and reset OSPF. A loopback interface's IP address will only be used for the initial RID selection when you first create the OSPF process. To make a router change its RID after it has already selected one, you must manually configure the RID; we'll cover that next.

## Changing the RID

After R1 has selected its RID, the best way to change it is to manually configure it and then reset the OSPF process (if needed). In any case, hardcoding the router's RID with manual configuration is considered a best practice to ensure predictable RIDs on all routers. In the following example, I configure the router-id router-id command on R1, causing it to change its RID:

```
R1(config)# router ospf 1
R1(config-router) # router-id 172.16.1.1 ← \
R1(config-router) # do show ip protocols
. . .
    Router ID 172.16.1.1
. . .
```

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-377_78_529_613_814.jpg)

NOTE If the router has already established a neighbor relationship with one or more OSPF neighbors, you will have to reset the OSPF process with clear ip ospf process for the newly configured RID to take effect. But at this point in the example, I haven't activated OSPF on any of R1's interfaces yet, so it has no OSPF neighbors. That is why the new RID took effect immediately.

Although the OSPF RID is often derived from one of the router's IP addresses, it's important to note that the RID is not an IP address; it's just a 32-bit value that is formatted similarly to an IP address (dotted decimal notation). As long as the RID is unique in the OSPF AS, it can be any 32-bit value.

In the following example, I configure a loopback interface on R2 and then create the OSPF process. In this case, R2 takes the IP address of Loopback0 as its RID because I configured the loopback interface before creating the OSPF process:
![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-377_407_1305_1340_316.jpg)

NOTE For the sake of space, I won't show the configurations, but I also configured Loopback0 on R3 (172.16.1.3/32) and R4 (172.16.1.4/32) and created the OSPF process.

## Loopback addresses and loopback interfaces

In chapter 7, I briefly introduced the concept of loopback addresses, which are IP addresses in the reserved 127.0.0.0/8 range. Messages sent to any address in this range are "looped back" to the local device without being transmitted across the network. For example, if you issue the ping 127.0.0.1 command on a PC, you'll get a response back from the PC itself. This can be used to test the device's network software.

Although they share the term loopback, loopback interfaces are a different concept from loopback addresses. Loopback interfaces are virtual interfaces in a router that can be assigned any valid IP address (which does not include the reserved loopback address range). Loopback interfaces provide a stable and reliable IP address that can be used to reach the router and isn't dependent on the status of a particular physical interface.

EXAM TIP The OSPF RID is exam topic 3.4.d; you should know how the RID is initially determined and how to change it.

### 18.2.2 Activating OSPF on interfaces

Activating OSPF on interfaces is possibly the most important part of configuring OSPF; it's what tells the router to make OSPF neighbors and share routing information. In chapter 17, we covered how to activate OSPF on interfaces with the network command. In the following example, I use the command to activate OSPF on R1's G0/0, G0/1, and L0 interfaces:

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-378_544_1245_1229_349.jpg)
NOTE I did not activate OSPF on R1 G0/2, which is connected to the internet. Later, we will configure a default route via G0/2 on R1 and advertise it to R1's neighbors, but there is no need to activate OSPF on the interface.

However, there is a more straightforward method of activating OSPF on interfaces: the ip ospf process-id area area command in interface config mode. Whereas the network command specifies a range of IP addresses, and the router activates OSPF
on all interfaces with an IP address in that range, this command allows you to explicitly specify which interfaces OSPF should be activated on. In the following example, I enable OSPF on R2's G0/0, G0/1, and L0 (Loopback0) interfaces:
![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-379_482_1256_356_190.jpg)

I think you'll agree that this method is much simpler than using the network command, although you should know both for the CCNA exam. In the following example, I use this method to activate OSPF on R3 and R4's interfaces as well:

```
R3(config)# interface range g0/0-2,10
R3(config-if-range) # ip ospf 1 area 0
R4(config) # interface range g0/0-1,10
R4(config-if-range)# ip ospf 1 area 0
```

EXAM TIP If a lab simulation in the exam expects you to use one OSPF configuration method over another, it will tell you so. Cisco exam questions can be difficult, but they aren't unfair.

### 18.2.3 Passive interfaces

Activating OSPF on an interface causes the router to perform two key actions:

- It attempts to form neighbor relationships with routers connected to the interface.
- It advertises the network prefix of the interface to neighbors.

However, there are cases where you might want the router to perform the second action-advertising the network prefix-without attempting to form network relationships on the interface. To form neighbor relationships, OSPF-enabled routers send hello messages out of their interfaces. If an interface isn't connected to another router, these hello messages are wasted-using up CPU and memory resources on the router and consuming network bandwidth. Furthermore, these unnecessary OSPF messages can pose a security risk, as malicious users could gather information about the network by examining them.

NOTE We will cover OSPF hello messages and other message types in section 18.3.

This is where passive interfaces come into play. A passive interface in OSPF is one that doesn't send OSPF messages to initiate neighbor relationships, even though it's OSPF enabled. However, the router will still advertise the network prefix of the interface to its OSPF neighbors, allowing them to forward packets to destinations in the network.

Figure 18.7 shows a situation in which you should configure a passive interface. R3 G0/1 connects to the 10.1.3.0/24 network, but there are no other routers connected to the interface. To allow R1, R2, and R4 to learn of 10.1.3.0/24 while preventing R3 from sending OSPF hello messages out of the interface, it should be configured as a passive interface.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-380_560_1411_737_224.jpg)
Figure 18.7 R3 advertises the network prefix of G0/1-a passive interface-but does not send OSPF hello messages out of it.

Configuring loopback interfaces as passive is also considered good practice in OSPF. Because a loopback interface is a virtual interface that isn't physically connected to any network, there's no practical need for it to send OSPF hello messages to try to form neighbor relationships. To configure a passive interface, use the passive-interface interface-name command in router config mode. In the following example, I configure R3 G0/1 and L0 as passive interfaces:

```
R3(config)# router ospf 1
R3(config-router) # passive-interface g0/1
R3(config-router) # passive-interface 10
R3(config-router) # do show ip protocols
. . .
Routing on Interfaces Configured Explicitly (Area 0):
    Loopback0
    FastEthernet1/0
    GigabitEthernet0/0
    GigabitEthernet0/1
```

Configures G0/1 and L0 as
passive interfaces

```
Passive Interface(s):
    GigabitEthernet0/1
Loopback0
```

G0/1 and L0 are now passive.
Another method of configuring passive interfaces is to use the passive-interface default command, which makes all interfaces passive by default. You can then use the no passive-interface interface-name command to specify which interfaces should not be passive. This method of configuration can be convenient if a router has many OSPF-enabled interfaces but only a few interfaces that it needs to form OSPF neighbor relationships on. Although this is not the case for R2, in the following example, I use this method to configure L0 as a passive interface and G0/0 and G0/1 as nonpassive:

```
R2(config) # router ospf 1
R2(config-router) # passive-interface default
R2(config-router) # no passive-interface g0/0
R2(config-router) # no passive-interface g0/1
```

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-381_266_667_660_880.jpg)

### 18.2.4 Advertising a default route

In most cases, hosts connected to routers in an OSPF AS don't just need to communicate with each other; they also need to communicate with hosts in external networks, such as the internet. To enable this communication, you can configure a default route on your ISP-connected router and then configure it to share that default route with the other routers in the OSPF AS. Figure 18.8 shows how to configure this.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-381_484_1409_1310_197.jpg)
Figure 18.8 R1 functions as an ASBR, advertising a default route into the OSPF AS. After configuring a default route, use default-information originate to advertise the route to other routers in the OSPF AS.

To make R1 advertise a default route to R2, R3, and R4, you should first configure a static default route on R1. If you configure default-information originate on R1
without configuring a default route, R1 won't advertise a default route to other routers; it must have a default route in its own routing table first.

After configuring the default route, use the default-information originate command in router config mode to make R1 advertise the default route to the other routers; I do so in the following example. Notice the additional statement added to the output of show ip protocols R1 is now an ASBR:
![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-382_390_1406_463_219.jpg)

NOTE The default-information originate always command allows a router to advertise a default route, even if it doesn't have one in its own routing table. However, the use cases of this command are beyond the scope of the CCNA exam.

In the following example, I use show ip route ospf on R2 to view routes that R2 learned via OSPF. R2 has learned a default route from R1, in addition to the other routes it has learned via OSPF:

```
R2# show ip route ospf
. . .
O*E2 0.0.0.0/0 [110/1] via 10.0.0.1, 00:37:34,
    GigabitEthernet0/0
    10.0.0.0/8 is variably subnetted, 7 subnets, 3 masks
O 10.0.0.4/30 [110/2] via 10.0.0.1, 00:02:09,
    GigabitEthernet0/0
O 10.0.0.12/30 [110/2] via 10.0.0.10, 01:11:30,
    GigabitEthernet0/1
O 10.1.3.0/24 [110/2] via 10.0.0.10, 01:11:30,
    GigabitEthernet0/1
    172.16.0.0/32 is subnetted, 4 subnets
0 172.16.1.1 [110/2] via 10.0.0.1, 01:11:30,
    GigabitEthernet0/0
0 172.16.1.3 [110/2] via 10.0.0.10, 01:11:30,
    GigabitEthernet0/1
O 172.16.1.4 [110/3] via 10.0.0.10, 00:13:54,
    GigabitEthernet0/1
        [110/3] via 10.0.0.1, 00:02:09,
    GigabitEthernet0/0
```

NOTE Loopback interfaces add a cost of 1 to a route. For example, R2's cost to reach R1's loopback interface (172.16.1.1) is 2: 1 for the R2-R1 link, plus 1 for R1 L0 (Loopback0).

Notice that R2 has inserted two paths to 172.16.1.4 into its routing table; this is an example of ECMP, which we covered in chapter 17. By default, OSPF will insert up to four equal-cost routes to a destination into the routing table. This value can be modified with the maximum-paths number command in router config mode, although the default setting of 4 is usually sufficient for most network scenarios.

### 18.3 Neighbors and adjacencies

For OSPF routers to exchange routing information, they first need to form neighbor relationships with each other. In this section, we'll examine that crucial first step of OSPF. For review, the three fundamental steps of the OSPF process are forming neighbor relationships, exchanging routing information, and calculating routes. Table 18.2 summarizes the five different message types that OSPF uses in this process; we will examine each one's role in this section as we cover how OSPF routers form neighbor relationships.

Table 18.2 OSPF message types
| Type | Name | Purpose |
| :--- | :--- | :--- |
| 1 | Hello | Neighbor discovery and maintenance |
| 2 | Database Description (DBD) | Summary of the router's LSDB. Used to check whether the LSDB of each router is the same. |
| 3 | Link-State Request (LSR) | Requests specific LSAs from a neighbor |
| 4 | Link-State Update (LSU) | Sends specific LSAs to a neighbor |
| 5 | Link-State Acknowledgment (LSAck) | Used to acknowledge that the router received an LSU |


### 18.3.1 Neighbor states

For OSPF routers to exchange routing information with each other, they have to pass through a series of neighbor states, in which the routers verify that various configuration parameters match between them. Figure 18.9 outlines the OSPF neighbor states from Down to Full.

We could spend several pages covering this process. If you continue your studies of OSPF beyond the CCNA, the details of this process are essential for understanding the OSPF protocol and how to troubleshoot it. However, for the purpose of the CCNA exam, a general understanding of each state's purpose and the sequence of states is sufficient.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-384_537_1349_183_223.jpg)
Figure 18.9 The OSPF neighbor states: Down, Init, 2-Way, ExStart, Exchange, Loading, and Full. Routers that reach the 2 -Way state are OSPF neighbors. Some routers remain in this state, and some proceed to establish a full adjacency.

When OSPF is activated on an interface, the router regularly sends OSPF hello messages, which are used for dynamically discovering OSPF neighbors and for maintaining neighbor relationships after they have been established. These hello messages include various pieces of information, two of which are the local router's RID and the RIDs of any neighbor routers it is aware of on the interface.

OSPF hello messages are sent to IP address 224.0.0.5, which is a multicast IP address. Whereas unicast packets are one-to-one (from one host to one other host), and broadcast packets are one-to-all, multicast packets are one-to-multiple (but not necessarily all). A packet sent to the multicast IP address 224.0.0.5 will be flooded by a switch and, therefore, received by all hosts on the segment. However, only router interfaces with OSPF activated will be interested in the contents of the packet; other hosts will simply ignore it, as shown in figure 18.10.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-384_489_868_1480_350.jpg)
Figure 18.10 An OSPF hello message from R1 is addressed to multicast address 224.0.0.5. The switch floods the frame, and OSPF-enabled R2 G0/0 and R4 G0/0 accept the hello message. R3 ignores the hello because GO/O is not OSPF-enabled.

The first OSPF neighbor state is Down, although it's not really a neighbor state-it means the router hasn't received any hello messages on the interface. Then, using an example from the neighbor states shown in figure 18.9, when R2 first receives a hello message from R1, R2 will create an OSPF neighbor entry for R1 in the Init state, as shown in the following example:

```
Use this command to display
the OSPF neighbor table.
Interface
GigabitEthernet0/0
```

R2 has an entry for R1 in the Init state.
show ip ospf neighbor is a very useful command to check the router's OSPF neighbors and their states. The Init state means that the router has received an OSPF hello message from a neighboring router, but that hello message did not include the local router's own RID. R2 will then send its own hello message to R1, including R1's RID in the message, having learned it from R1's hello. When R1 receives this hello from R2, R1 will create an OSPF neighbor entry for R2 in the 2-Way state-bypassing the Init state. The following example shows R1's OSPF neighbor table entry for R2:

```
Displays the OSPF neighbor table
R1# show ip ospf neighbor
Neighbor ID Pri State Dead Time Address Interface
172.16.1.2 1 2WAY/DROTHER 00:00:39 10.0.0.2 GigabitEthernet0/0
```

Having received a hello from R2-and thus having learned R2's RID-R1 sends another hello to R2, including R2's RID in the message this time. R2 then moves its neighbor table entry for R1 to the 2-Way state. At this point, R1 and R2 are considered OSPF neighbors-they see and acknowledge each other but have not exchanged any routing information.

NOTE In some cases, a designated router (DR) and backup designated router (BDR) election is held in the 2-Way state-more on that in section 18.3.2.

For OSPF routers to exchange routing information, they must establish an OSPF adjacency by progressing toward the Full state. To do so, they will proceed to the ExStart and Exchange states, where they exchange database description (DBD) messages, which are summaries of the contents of each router's LSDB. In the ExStart state, they determine which router will lead the DBD exchange; the router with the higher RID will become the Leader, and the router with the lower RID will become the Follower. Then, the actual DBD exchange takes place in the Exchange state.

NOTE Leader/Follower is a recent update to OSPF terminology, replacing the previous Master/Slave. Because it's a recent update, the old terms are still more common (and used in Cisco IOS software), so you should be aware of them.

After exchanging DBDs, the routers are aware of the contents of each other's LSDBs. They then proceed to the Loading state, where they use link-state request (LSR) messages to request specific LSAs from each other, link-state update (LSU) messages to send LSAs, and link-state acknowledgment (LSAck) messages to acknowledge receipt of LSUs.

NOTE LSAs are data structures that contain OSPF routing information, and they are sent in LSU messages. You can think of LSUs as the envelopes that carry LSAs.

The routers then enter the Full state, meaning that they have synced their LSDBs. At this point, they are called adjacent neighbors or fully adjacent neighbors, and their neighbor relationship is now called an adjacency or full adjacency. As long as the connection between the routers remains stable, they should remain in the Full state, sharing LSAs as the network changes.

Even after a neighbor relationship or adjacency is established, routers will continue sending hello messages at regular intervals, as determined by the hello timer, which is 10 seconds by default. The purpose of these hello messages is to maintain neighbor relationships. If a router stops receiving hellos from a neighbor, it will remove the neighbor from the neighbor table; this is determined by the dead timer, which is 40 seconds by default. This means that a router will remove a neighbor if the router hasn't received a hello from the neighbor for the past 40 seconds.

### 18.3.2 OSPF network types

OSPF uses an interface setting called network type to determine how OSPF behaves over a particular network. In this context, a network is a connection between two or more routers-a segment. The network type influences things like the OSPF message timers, whether a DR and BDR are elected, and whether all neighbor relationships will become full adjacencies.

There are various OSPF network types: broadcast, non-broadcast multi-access (NBMA), point-to-point, point-to-multipoint, point-to-multipoint non-broadcast, and loopback. The CCNA exam topics explicitly state the two network types you need to know for the exam in topics 3.3.b, Point-to-point, and 3.3.c, Broadcast (DR/BDR selection). Table 18.3 summarizes those two network types.

Table 18.3 OSPF network types
| Broadcast | Point-to-point |
| :--- | :--- |
| DR/BDR elected | No DR/BDR |
| Establish full adjacency only with DR \& BDR | Establish full adjacency |
| Neighbors dynamically discovered | Neighbors dynamically discovered |
| Default timers: hello $=10$, dead $=40$ | Default timers: hello = 10; dead = 40 |


The two shared characteristics of these network types are dynamic neighbor discovery and the default timers. By sending hello messages out of OSPF-enabled interfaces, routers are able to dynamically discover which neighbors are connected to the interface, as opposed to requiring an admin to manually configure neighbor IP addresses (as required on some OSPF network types).

The default hello timer on both of these network types is 10 seconds, so interfaces send hello messages at 10-second intervals. The default dead timer is 40 seconds, so a neighbor is removed from the neighbor table if no hello message is received for 40 seconds. Other network types use longer default timers of 30 and 120 seconds.

NOTE If the router detects a physical link failure (placing the interface in the down/down state), OSPF will immediately remove the neighbor-no need to wait for the dead timer to count down. The dead timer is only relevant if the interface remains up but the router stops receiving hello messages from the neighbor (for whatever reason).

## Broadcast network type

Broadcast is the default OSPF network type of Ethernet interfaces (of all speeds-FastEthernet, GigabitEthernet, etc.). You can confirm that with the show ip ospf interface command, as in the following example:

```
R1# show ip ospf interface g0/0
GigabitEthernet0/0 is up, line protocol is up
    Internet Address 10.0.0.1/30, Area 0, Attached via Network Statement
    Process ID 1, Router ID 172.16.1.1, Network Type BROADCAST, Cost: 1
    The network type of G0/0 is broadcast.
```

The main defining characteristic of the broadcast network type is that a designated router (DR) and backup designated router (BDR) are elected in the 2-Way neighbor state; other routers connected to the segment become DROthers (usually pronounced D-R-other). While the DR and BDR establish full adjacencies with all routers in the segment, DROthers only establish a full adjacency with the DR and BDR of the segment and remain neighbors in the 2-Way state with fellow DROthers-they don't exchange LSAs with each other.

The purpose of electing a DR and BDR is to reduce the amount of OSPF traffic in the segment by only requiring routers to exchange LSAs (in LSU messages) with the DR and BDR of the segment. Figure 18.11 shows how the number of adjacencies (and, therefore, LSA exchanges) is reduced, limiting the amount of OSPF traffic in the segment and the amount of resources used on the routers.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-388_455_913_181_346.jpg)
Figure 18.11 Six routers are connected to the same network segment. Without the DR/BDR, 15 OSPF adjacencies (and 15 LSA exchanges) would be required. With the DR/BDR, only 9 are required, reducing the amount of OSPF traffic.

NOTE To address messages to only the DR and BDR, DROthers send the packets to multicast IP address 224.0.0.6. Remember the two OSPF multicast IP addresses: 224.0.0.5 (all OSPF routers) and 224.0.0.6 (DR and BDR only).

Depending on the number of routers connected to the segment, the DR/BDR feature of the broadcast network type can greatly reduce the resources used by OSPF on the segment. This could be further reduced by electing only a DR, but the BDR is important for providing stability and resiliency; if the DR fails for some reason, the BDR takes over automatically as the new DR.

Figure 18.12 shows an example OSPF AS with DRs, BDRs, and DROthers labeled. Note that the DR and BDR are elected per segment, not per area. For example, R2 is the DR for the R1-R2 segment, but a DROther for the R2-R3-R4-R5 segment.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-388_561_1214_1412_350.jpg)
Figure 18.12 An OSPF AS consisting of six segments. The DR/BDR election is held per segment, not per area. RIDs are in parentheses.

The G0/1 interfaces of R1, R3, R4, and R5 in figure 18.12 are labeled as DRs. Because there are no other routers connected to those segments, no DR/BDR election is held; the routers simply declare themselves the DR for those segments. However, no adjacencies will be established, and no LSAs will be exchanged out of those interfaces; DR is just a title in this case. Furthermore, these interfaces should be configured in passive mode, so no OSPF hello messages will be sent out of them.

The following example shows the output of show ip ospf neighbor on R5. As the DR of the R2-R3-R4-R5 segment, R5 has a full adjacency with all other routers:
![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-389_289_1426_573_190.jpg)

Although the output doesn't explicitly state that R5 is the DR, because none of the three other routers connected to the segment are the DR, you can conclude that R5 must be the DR for the segment. Let's confirm by using the same command on R2:
![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-389_481_1433_1081_188.jpg)

As the output shows, R5 is indeed the DR for the segment. Another point worth noting is that R2 and R3 do not form a full adjacency; as DROthers, they remain neighbors in the 2-Way state. This means that they don't exchange LSAs with each other.

NOTE Although DROthers don't directly exchange LSAs with each other, they will learn of each other's LSAs via the DR/BDR. Remember, all routers in the OSPF area must have the same LSDB.

By now, you're probably wondering how the DR and BDR are elected. The DR/BDR are elected with the following two criteria:

- The highest interface priority
- The highest RID

The router with the highest interface priority will become the DR for the segment, and the router with the second highest will become the BDR. The OSPF interface priority is a configurable value (default 1) that can be configured with the ip ospf priority priority command in interface config mode.

NOTE If you want to ensure that a router never becomes the DR or BDR for a segment, configure ip ospf priority 0 on its interface.

In the example we saw in figure 18.12, all interfaces had the default priority of 1, so the second parameter was used to decide the DR of each segment: the OSPF RID. If interface priorities are tied, the router with the highest RID will become the DR (R5, 172.16.1.5), and the router with the second highest RID the BDR (R4, 172.16.1.4).

Let's change the interface priority of R2 G0/1 to test out the DR/BDR election. In the following example, I set R2 G0/1's interface priority to 100 and confirm the results:

```
            Changes R2 GO/1’s priority to 100
            This means OSPF was
            activated directly on the
        interface, not with the
        network command.
    #Attached via Interface Enable
    Process ID 1, Router ID 172.16.1.2, Network Type BROADCAST, Cost: 1
. . .
    Transmit Delay is 1 sec, State DROTHER, Priority 100
R2 G0/1 is still a
. . .
        DROther.
R2(config-if)# do show ip ospf neighbor
Neighbor ID Pri State Dead Time Address Interface
172.16.1.1 1 FULL/BDR 00:00:30 10.0.0.9 GigabitEthernet0/0
172.16.1.3 1 2WAY/DROTHER 00:00:34 10.0.0.2 GigabitEthernet0/1
172.16.1.4 1 FULL/BDR 00:00:30 10.0.0.3 GigabitEthernet0/1
172.16.1.5 1 FULL/DR 00:00:31 10.0.0.4 GigabitEthernet0/1
The DR and BDR of the segment are unchanged.
```

Although R2 G0/1 now has the highest priority of the segment, R5 and R4 remain the DR and BDR. That's because, once the DR and BDR have been elected, they won't be preempted-their roles won't be taken-even if the priority of an existing router is increased or a router with a higher priority connects to the segment. To make a DR or BDR give up its role, you can use the clear ip ospf process command to reset the router's OSPF process, as I do on R5 in the following example:

```
R5# clear ip ospf process
Reset ALL OSPF processes? [no]: yes
R5# show ip ospf neighbor
Neighbor ID Pri State Dead Time Address Interface
```

| 172.16.1.2 | 100 | FULL/BDR | 00:00:35 | 10.0.0.1 | GigabitEthernet0/0 | ◁ |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| 172.16.1.3 | 1 | 2WAY/DROTHER | 00:00:31 | 10.0.0.2 | GigabitEthernet0/0 |  |
| 172.16.1.4 | 1 | FULL/DR | 00 : 00 : 31 | 10.0.0.3 | GigabitEthernet0/0 | 4 |
| R4 becomes the new DR, and R2, the BDR. |  |  |  |  |  |  |

After resetting R5's OSPF process, R4 (formerly the BDR) becomes the new DR, and R2 becomes the new BDR, despite having the highest priority. These results reveal an important point about how the DR and BDR function: if the DR goes down, routers connected to the segment don't hold an election for the new DR. Instead, the BDR immediately takes over as the new DR; that's why R4 becomes the new DR. Then, an election is held to determine the new BDR; R2 wins this election and becomes the new BDR. To make R2 the DR, you would have to then reset R4's OSPF process, making R2 (the BDR) immediately take over R4's role.

EXAM TIP The broadcast network type is exam topic 3.4.c. You should understand the DR/BDR election in particular.

## Point-to-point network type

Although the DR/BDR feature of the broadcast network type can reduce the amount of resources used by OSPF, electing a DR and BDR extends the amount of time it takes for routers to establish a full adjacency; if there are only two routers connected to the segment, this is unnecessary and inefficient.

A point-to-point connection is a direct link between two routers. When OSPF is enabled on this link with the default network type of broadcast, one router becomes the DR, and the other, the BDR. However, these designations do not offer any advantage in this case because both routers establish a full adjacency, regardless.

NOTE The point-to-point network type is the default on serial interfaces, which used to be common for WAN connections. Due to the greater speed and lower cost of fiber-optic Ethernet, serial connections are now considered a legacy technology. To use this network type on Ethernet links, you must manually configure it.

By using the OSPF point-to-point network type, we eliminate the DR/BDR election process, allowing the routers to establish a full adjacency in less time. This network type is designed for direct connections between two routers, and it is the most efficient option in these situations. Figure 18.13 shows a situation where the point-to-point network type should be configured. R1 and R2 have a point-to-point connection, so there is no need to elect a DR and BDR.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-392_638_1303_183_222.jpg)
Figure 18.13 Electing a DR and BDR on a connection between two routers is unnecessary, so the R1-R2 link should use the point-to-point network type.

To configure the point-to-point network type, use the ip ospf network point-topoint command in interface config mode-make sure you do it on both sides of the connection! In the following example, I configure the point-to-point network type on R1 G0/0 and R2 G0/0 and then confirm. Notice that R1 and R2 establish a full adjacency, but neither is a DR or BDR, as indicated by the state of FULL/ -:

```
R1(config) # interface g0/0
R1(config-if)# ip ospf network point-to-point
R2(config) # interface g0/0
R2(config-if) # ip ospf network point-to-point
R2(config-if) # do show ip ospf neighbor
```

| Neighbor ID | Pri | State | Dead Time | Address | Interface |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 172.16.1.3 | 1 | 2WAY/DROTHER | 00:00:35 | 10.0.0.2 | GigabitEthernet0/1 |
| 172.16.1.4 | 1 | FULL/BDR | 00:00:38 | 10.0.0.3 | GigabitEthernet0/1 |
| 172.16.1.5 | 1 | FULL/DR | 00:00:36 | 10.0.0.4 | GigabitEthernet0/1 |
| 172.16.1.1 | 0 | FULL/ - | 00:00:38 | 10.0.0.9 | GigabitEthernet0/0 |

R1 and R2 establish a full adjacency with no DR/BDR.

EXAM TIP The point-to-point network type is exam topic 3.4.b. You should understand the advantage it provides (no DR/BDR election) and how to configure it.

### 18.3.3 Neighbor requirements

Although the OSPF broadcast and point-to-point network types allow routers to dynamically discover neighbors, that's no guarantee that they will actually become neighbors; there is a set of requirements that needs to be met, including various parameters that must match (and one that must not match):

- Area number must match.
- Subnet (network address, netmask) must match.
- OSPF process must not be shutdown.
- RIDs must be unique.
- Hello and dead timers must match.
- Authentication settings must match.
- IP MTU settings must match.*
- Network type must match.*

A mismatch of the final two, which I've marked with asterisks (*), will not prevent routers from becoming neighbors but will prevent OSPF from operating properly. Let's go through these requirements one by one, and see how OSPF is affected. I'll use a simple connection between two routers: R1 (192.168.1.1/30) and R2 (192.168.1.2/30). In the following example, I configure an area number mismatch on R1 and R2, and they fail to become OSPF neighbors. After fixing the mismatch, the issue is resolved:
![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-393_518_1408_1191_190.jpg)

In the next example, I change the netmask of R2's interface. Even though I didn't change the IP address; just having a mismatched netmask causes the adjacency to go from Full to Down.
![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-393_207_1412_1878_190.jpg)

```
R2(config-if)# ip address 192.168.1.2 255.255.255.252
*Jun 9 04:37:36.071: %OSPF-5-ADJCHG: Process 1,
■Nbr 192.168.1.1 on GigabitEthernet0/0 from LOADING to
```

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-394_94_313_181_1232.jpg)

```
-FULL, Loading Done
```

R1 and R2 reestablish a full adjacency.

The third requirement is that the OSPF process must not be shutdown, referring to the command you can use in router config mode to disable OSPF, much like how you use the shutdown command to disable an interface. I demonstrate this in the following example:

```
R2(config) # router ospf 1
R2(config-router) # shutdown
*Jun 9 05:02:01.379: %OSPF-5-ADJCHG: Process 1,
#Nbr 192.168.1.1 on GigabitEthernet0/0 from FULL to
-DOWN, Neighbor Down: Interface down or detached
R2(config-router) # no shutdown
*Jun 9 05:02:07.114: %OSPF-5-ADJCHG: Process 1,
#Nbr 192.168.1.1 on GigabitEthernet0/0 from LOADING to
-FULL, Loading Done
```

R1 and R2 reestablish a full adjacency.

The fourth requirement is that the routers' RIDs must be unique-the one parameter that must not match. In the following example, I modify R2's RID to match R1's, reset the OSPF process, and they fail to become neighbors. Removing the R2's RID configuration fixes the issue:

```
R2(config-router) # router-id 192.168.1.1
R2(config-router) # do clear ip ospf process
Reset ALL OSPF processes? [no]: yes
*Jun 9 05:06:09.209: %OSPF-5-ADJCHG: Process 1,
#Nbr 192.168.1.1 on GigabitEthernet0/0 from FULL to
-DOWN, Neighbor Down: Interface down or detached
*Jun 9 05:06:11.679: %OSPF-4-DUP_RTRID_NBR: OSPF
-detected duplicate router-id 192.168.1.1 from
-192.168.1.1 on interface GigabitEthernet0/0
R2(config-router) # no router-id 192.168.1.1
*Jun 9 05:06:29.284: %OSPF-5-ADJCHG: Process 1,
■Nbr 192.168.1.1 on GigabitEthernet0/0 from LOADING to
-FULL, Loading Done
```

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-394_313_730_1435_799.jpg)

The fifth requirement is that the hello and dead timers must match. The default hello timer is 10 seconds, and the default dead timer is 40 seconds, but they can be configured with the ip ospf hello-interval seconds and ip ospf dead-interval seconds commands in interface config mode. In the following example, I modify R2's hello timer, causing the adjacency with R1 to go down:
![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-395_411_1402_179_188.jpg)

The sixth requirement is that the authentication settings must match. You can configure a password to authenticate OSPF neighbors-to ensure that only the intended routers form neighbor relationships. OSPF authentication is beyond the scope of the CCNA exam, but you can use the ip ospf authentication command in interface config mode to enable authentication and the ip ospf authentication-key password command to configure the password.

The seventh requirement is that the routers' IP maximum transmission unit (MTU) settings must match. We briefly mentioned the IP MTU in chapter 7 when covering the IPv4 header; it determines the maximum size of an IPv4 packet. Although an IP MTU mismatch will not prevent two OSPF routers from becoming neighbors, they will be unable to establish a full adjacency; they will not be able to advance beyond the ExStart/Exchange states. Like authentication, the details of IP MTU are beyond the scope of the CCNA exam, but you can modify an interface's IP MTU with the ip mtu bytes command in interface config mode; the default setting on Ethernet interfaces is 1,500 bytes.

The final requirement is that the OSPF network types match. This requirement is different from the others we have covered so far: a router with the broadcast network type and a router with the point-to-point network type will be able to establish a full adjacency. However, the problem is that the routers will be unable to synchronize their LSDBs; they won't learn each other's routes.

In the following example, I configure a loopback interface on R2 and enable OSPF on it. I then configure the point-to-point network type on R2 G0/0, resulting in a network type mismatch—R1's interface is still using the broadcast network type. Despite the network type mismatch, R2's OSPF neighbor table shows a full adjacency with R1. I then check R1's neighbor and routing tables:

```
R2(config) # interface 10
R2(config-if)# ip address 10.10.10.2 255.255.255.255
R2(config-if) # ip ospf 1 area 0
R2(config-if) # interface g0/0
R2(config-if) # ip ospf network point-to-point
R2(config-if) # do show ip ospf neighbor
```

```
Neighbor ID Pri State Dead Time Address Interface
192.168.1.2 1 FULL/ - 00:00:34 192.168.1.2 GigabitEthernet0/0
R1# show ip ospf neighbor
Neighbor ID Pri State Dead Time Address Interface
192.168.1.2 1 FULL/DR 00:00:34 192.168.1.2 GigabitEthernet0/0
R1# show ip route ospf
            R1 and R2 have a full adjacency.
```

No routes display; R1 does not learn a route to R2's loopback interface, despite having a full adjacency.

The OSPF neighbor tables for R1 and R2 both indicate a full adjacency, but there is something strange: R2's output shows a state of FULL/ - (as expected when using the point-to-point network type), but R1's output shows FULL/DR-R1 believes that R2 is the DR. Furthermore, although OSPF is activated on R2's loopback interface, R1's routing table shows no route to the interface's address (10.10.10.2/32). Despite achieving full adjacency, R1 and R2 weren't actually able to synchronize their LSDBs. The solution is to ensure that both sides of the connection are using the same network type, whether that is broadcast or point-to-point.

EXAM TIP Neighbor adjacencies are exam topic 3.4.a; make sure you know these requirements for the exam.

### 18.4 LSA types

OSPF routers share routing information by sending LSU messages, which contain LSAs. There are various different kinds of LSAs, each with its own purpose, but for the CCNA exam, you should be aware of three:

- Type 1 (Router LSA)-Generated by all routers, this LSA describes the router's links.
- Type 2 (Network LSA)-Generated by the DR of a broadcast network, this LSA lists all routers on the segment.
- Type 5 (AS External LSA)-Generated by ASBRs, this LSA advertises routes to external networks (i.e., a default route to the internet).

Figure 18.14 shows the OSPF AS we looked at in section 18.3.2, modified with a connection to the internet, which R1 advertises with default-information originate. Notice the contents of the LSDB, which are shared by all five routers.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-397_647_1417_181_192.jpg)
Figure 18.14 An OSPF AS and the contents of its LSDB. Each router advertises a type 1 (Router) LSA. R5, as the DR of the R2-R3-R4-R5 segment, advertises a type 2 (Network) LSA. R1, as an ASBR, advertises a type 5 LSA.

NOTE R3, R4, and R5 don't advertise a type 2 LSA for their G0/1 interfaces. As mentioned before, with no neighbors on those interfaces, they are DRs in name only.

To view the OSPF LSDB, use the show ip ospf database command. The output of this command should be the same on all routers; they should all have the same LSAs in their LSDB. In the following example, I use the command on R1. The specifics of how to read the output are beyond the scope of the CCNA; just note that each router advertises a type 1 LSA, R5 advertises a single type 2 LSA, and R1 advertises a single type 5 LSA:

```
R1# show ip ospf database
        OSPF Router with ID (172.16.1.1) (Process ID 1)
            Router Link States (Area 0)
Link ID ADV Router Age Seq# Checksum Link count
172.16.1.1 172.16.1.1 1851 0x80000008 0x0015D5 3
172.16.1.2 172.16.1.2 1852 0x8000000A 0x003688 4
172.16.1.3 172.16.1.3 1825 0x80000014 0x002510 3 advertises a type1
172.16.1.4 172.16.1.4 1824 0x80000018 0x0052D9 3 (Router) LSA.
172.16.1.5 172.16.1.5 1824 0x80000015 0x008D9C 3
        Net Link States (Area 0)
Link ID ADV Router Age Seq# Checksum
10.0.0.4
        Type-5 AS External Link States R2-R3-R4-R5 segment.
Link ID ADV Router Age Seq# Checksum Tag
0.0.0.0
            172.16.1.1 1850 0x80000001 0x009B58 1
R1 advertises a type 5 (AS External) LSA.
```


## Summary

- Two versions of OSPF are used: OSPFv2 (for IPv4) and OSPFv3 (mainly for IPv6).
- OSPF uses the Shortest Path First (SPF) algorithm, also called Dijkstra's algorithm.
- OSPF routers go through three main steps: forming neighbor relationships, exchanging routing information, and calculating routes.
- OSPF routers exchange routing information using link-state advertisements (LSAs), which are organized in the link-state database (LSDB). The process of sending LSAs to all other routers in the same OSPF area is called LSA flooding.
- An OSPF network can be logically divided into areas. Routers in an OSPF area all share the same LSDB. Area 0 is the backbone area to which all other areas must connect.
- In large networks, multi-area OSPF provides advantages: smaller LSDBs take up fewer memory resources, the SPF algorithm takes less time and CPU resources to calculate routes, and network changes/instability are isolated to a single area.
- An internal router has all OSPF-enabled interfaces in the same area. A backbone router has at least one interface in area 0 . An area border router (ABR) connects one (or more) areas to area 0. An autonomous system border router (ASBR) connects the OSPF AS to external networks.
- Intra-area routes are routes to destinations in an area the router is connected to. Inter-area routes are routes to destinations in an area the router is not connected to. External routes are routes to destinations outside of the OSPF AS.
- OSPF's metric is called cost, and a route's cost is the cumulative cost of each interface a packet must be sent out of to reach the destination.
- A link's cost is calculated by dividing the reference bandwidth (default 100 Mbps) by the link's bandwidth. Values less than 1 are assigned as 1 , meaning links with a bandwidth of 100 Mbps or greater all have a cost of 1 by default.
- You can change the reference bandwidth with the auto-cost referencebandwidth mbps command in router config mode.
- You can modify a link's cost with ip ospf cost cost on each interface or by modifying the interfaces' bandwidth with bandwidth kbps.
- Use show ip ospf interface brief to view OSPF-enabled interfaces.
- The OSPF process ID is specified with the router ospf process-id command and is locally significant; it doesn't have to match between routers.
- Use show ip protocols to view information about routing protocols on the router, such as the OSPF RID, OSPF-enabled interfaces, etc.
- Each router's OSPF router ID (RID) must be unique. It is determined in the following order: (1) manual configuration with the router-id command, (2) highest IP address on a loopback interface, and (3) highest IP address on a physical interface.

- A loopback interface is a virtual interface that is not dependent on the status of a particular physical interface. You can create a loopback interface with the interface loopback number command and configure an IP address like a physical interface.
- In addition to the network command, you can activate OSPF on interfaces with the ip ospf process-id area area command in interface config mode.
- A passive interface is OSPF-enabled but does not send OSPF hello messages.
- To configure a passive interface, use passive-interface interface-name in router config mode or passive-interface default and then no passiveinterface interface-name to make specific interfaces nonpassive.
- Use default-information originate in router config mode to make a router advertise its default route to its OSPF neighbors (making it an ASBR).
- OSPF uses five message types: hello, database description (DBD), link-state request (LSR), link-state update (LSU), and link-state acknowledgment (LSAck).
- Hellos are used for neighbor discovery and maintenance. DBDs provide a summary of the router's LSDB. LSRs request specific LSAs from a neighbor. LSUs send specific LSAs to a neighbor. LSAcks acknowledge receipt of an LSU.
- OSPF hello messages are sent to multicast IPv4 address 224.0.0.5. This multicast address is used to send messages to all OSPF routers on the segment.
- There are seven OSPF neighbor states: Down, Init, 2-Way, ExStart, Exchange, Loading, and Full.
- OSPF routers in the 2-Way state are neighbors but have not yet exchanged LSAs.
- Routers in the Full state are adjacent/fully adjacent; they have an adjacency/ full adjacency. They have exchanged LSAs and synced their LSDBs.
- Use show ip ospf neighbor to view information about OSPF neighbors.
- OSPF uses an interface setting called network type to determine how OSPF behaves over a particular network (a segment-a link between one or more routers).
- Use show ip ospf interface interface to view details about a particular interface.
- Ethernet interfaces use the broadcast network type by default. This network type allows neighbors to dynamically discover each other and uses these default timers: hello $=10$, dead = 40.
- OSPF routers elect a designated router (DR) and backup designated router (BDR) on each broadcast network segment. The remaining routers are DROthers. The DR/ BDR election occurs in the 2-Way state.
- All routers on the segment establish a full adjacency with the DR/BDR, but DROthers remain neighbors in the 2-Way state with each other.
- The DR and BDR are determined using (1) the highest interface priority or (2) the highest RID. The default interface priority is 1 . It can be configured with ip ospf priority.

- Increasing a router's interface priority/RID or connecting a new router with a higher interface priority/RID will not cause the router to preempt the DR/BDR. To make the DR or BDR give up its role, use clear ip ospf process.
- If the DR is lost, the BDR immediately takes over its role, and an election is held for the new BDR.
- The point-to-point network type is ideal for connections between two routers. The routers establish a full adjacency without a DR/BDR election. Use ip ospf network point-to-point in interface config mode to configure this network type.
- For routers to become OSPF neighbors, the OSPF process must not be shutdown, the RIDs must be unique, and the following parameters must match: area number, subnet, hello/dead timers, authentication settings, IP MTU settings, and network type.
- If the IP MTU settings don't match, the routers will be stuck in the ExStart/ Exchange states. If the network type doesn't match, the routers will establish a full adjacency but will not sync their LSDBs.
- There are various kinds of LSAs, including type 1 (Router LSA), type 2 (Network LSA), and type 5 (AS External LSA).
    - Type 1 (Router LSA)-Generated by all routers, this LSA describes the router's links.
    - Type 2 (Network LSA)-Generated by the DR of a broadcast network, this LSA lists all routers on the segment.
    - Type 5 (AS External LSA)-Generated by ASBRs, this LSA advertises routes to external networks (i.e., a default route to the internet).

## First hop redundancy protocols

## This chapter covers

- How FHRPs provide a redundant default gateway for end hosts
- The three FHRPs used by Cisco routers and their characteristics
- How to configure Cisco's Hot Standby Router Protocol

When covering STP in chapter 14, I emphasized the importance of redundancy in a LAN. In modern enterprise networks, redundancy plays an essential role in ensuring network reliability and resilience. As businesses increasingly rely on digital operations and processes, network downtime can result in substantial financial losses and damage to one's reputation. Redundancy safeguards against this by eliminating single points of failure.

Redundancy doesn't just mean having multiple potential paths to reach destinations within the same LAN; it also means having multiple potential paths to reach destinations outside of the LAN. First hop redundancy protocols (FHRPs), the topic of this chapter, facilitate this by enabling multiple routers to work together, providing a redundant default gateway for hosts in a LAN. FHRPs constitute CCNA exam topic 3.5: Describe the purpose, functions, and concepts of first hop redundancy protocols.

Cisco routers support three different FHRPs: Hot Standby Router Protocol (HSRP), Virtual Router Redundancy Protocol (VRRP), and Gateway Load Balancing Protocol (GLBP). They all serve the purpose of providing a redundant default gateway for hosts but also have their own unique characteristics. We will begin by covering FHRPs as a whole and then cover the characteristics of each FHRP that you should know for the CCNA exam.

### 19.1 FHRP concepts

First hop redundancy protocol (FHRP) is a category of protocol that allows multiple routers to work together to provide a redundant default gateway for hosts in a LAN, minimizing downtime in the event of a hardware failure. This is the reason for the name: the default gateway is the first hop in the packet's path to its destination.

### 19.1.1 Providing a redundant default gateway

An end host's default gateway is an IP address that is either manually configured or automatically learned via DHCP (the topic of chapter 4 in volume 2). End hosts use the Address Resolution Protocol (ARP) to learn the MAC address of the default gateway and then encapsulate packets to external destinations in frames addressed to the default gateway's MAC address. Because hosts rely on the default gateway to reach external destinations, the default gateway must be resilient to failures.

At first glance, the network in figure 19.1 might look like it deserves a perfect score when it comes to redundancy; there are multiple connections between switches, providing multiple paths to destinations within the LAN, and there are two routers, providing multiple paths to external destinations. However, without an FHRP, the two routers are unable to coordinate with each other to provide a redundant default gateway.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-402_592_1410_1340_223.jpg)
Figure 19.1 Despite the hardware failure on R1, hosts in the LAN still try to use R1 as their default gateway. Without an FHRP, R2 is unable to take over R1's role.

Because the end hosts still use IP address 10.0.0.1 as their default gateway, they will continue to send packets destined for external destinations in frames addressed to R1, despite the hardware failure preventing R1 from fulfilling its role as default gateway. You could reconfigure each host's default gateway to 10.0.0.2 or R2's IP address to 10.0.0.1, but this kind of manual intervention is not acceptable in modern networks; it is expected that the network will recover automatically and with minimal downtime.

Figure 19.2 shows how an FHRP allows R2 to take over as the default gateway after R1's hardware failure. R1 and R2 both share a virtual IP (VIP) address, which is configured as the default gateway of hosts in the LAN. Under normal circumstances, R1 responds to ARP requests directed to the VIP (10.0.0.1), so end hosts will use R1 as their default gateway. However, when a hardware failure prevents R1 from performing its role, R2 will take over as the default gateway; R2 will respond to ARP requests for the VIP.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-403_615_1416_799_190.jpg)
Figure 19.2 After a hardware failure on R1, R2 takes over as the default gateway. R1 and R2 share a virtual IP (VIP) address, which hosts in the LAN use as their default gateway. The hosts are unaware of the change from R1 to R2.

A set of routers configured to use an FHRP is called a group. A single router can be part of multiple FHRP groups. This comes in handy when a LAN has multiple subnets, as each subnet can have its own FHRP group-its own redundant default gateway IP address.

### 19.1.2 FHRP neighbor relationships

To coordinate with each other, routers using an FHRP communicate via multicast hello messages. Routers send hello messages at regular intervals, and these messages are used both for establishing FHRP neighbor relationships and for maintaining those relationships. If a router stops receiving hello messages from a neighbor, it will assume
that the neighbor has gone down and will act accordingly (e.g., by taking over the role of default gateway). This mechanism is similar to OSPF's mechanism for establishing and maintaining neighbor relationships that we covered in chapter 18.

NOTE The exact multicast address to which hello messages are sent depends on the FHRP. We will cover such details in section 19.2.

As mentioned in chapter 18, multicast messages are flooded by switches, so all hosts in the LAN receive them. However, only routers running the FHRP are interested in the contents of the messages; other hosts simply ignore them. On the other hand, when a device receives a broadcast message, it has to de-encapsulate it and use up processing power to determine if the message is relevant. This is why FHRPs (and dynamic routing protocols) send multicast messages, rather than broadcast; they take up fewer resources on hosts that are not interested in the contents of those messages. Figure 19.3 shows how routers use hello messages to establish and maintain their FHRP neighbor relationship and also introduces a few new concepts.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-404_666_1410_900_223.jpg)
Figure 19.3 R1 and R2 communicate with multicast hello messages to establish and maintain their FHRP neighbor relationship. R1 becomes the active router, and R2, the standby. When a host sends an ARP request to the VIP, R1 will reply with the virtual MAC address, allowing the host to create an appropriate ARP table entry. Switches will also learn the virtual MAC address on the appropriate port.

Through their hello messages, the routers elect an active router and a standby router. As the names suggest, the active router serves as the default gateway, while the standby router is ready to take over if the active router fails. Note that the terms active and standby are used by HSRP; other FHRPs use different terms that we will cover in section 19.2.

In addition to sharing a VIP, the active and standby routers also share a virtual MAC address. When a host sends an ARP request to the VIP, the active router will reply with the virtual MAC address-not the MAC of its own interface. This results in two actions:

- The host that receives the ARP reply will create an ARP table entry mapping the VIP to the virtual MAC address. To send packets to external destinations, the host will encapsulate the packets in frames destined for the virtual MAC address.
- Switches will learn the virtual MAC address on the appropriate port-the port leading toward the current active router.

NOTE Each FHRP uses a different format for the virtual MAC address. We will cover the virtual MAC address formats in section 19.2.

### 19.1.3 Failover

The process of the active router failing and the standby router taking over its role is called failover. Because the active router and the standby router both share the same IP address (the VIP) and MAC address (the virtual MAC address), there is no need for hosts in the LAN to update their default gateway IP addresses and ARP tables after a failover; they can continue as if nothing happened. However, there is one thing that does need to be updated: the switches' MAC address tables. If a failover occurs but switch MAC address tables aren't updated, switches will not be able to forward frames to the new active router.

To inform switches about its location after taking over the active role, the new active router will send gratuitous ARP (GARP) messages-ARP replies that were not prompted by ARP requests. GARP messages are sent to the broadcast MAC address (unlike regular ARP replies, which are unicast), causing switches to flood them. The source MAC address of these GARP messages is the virtual MAC address; this causes switches in the LAN to update their MAC address tables if necessary, learning the virtual MAC address on the appropriate port.

Figure 19.4 outlines what happens during a failover. After R2 detects R1's failure, it takes over as the active router and sends GARP messages, allowing the switches to re-learn the virtual MAC address on the appropriate port (if necessary-not all switches need to update their MAC address table entry for the virtual MAC). This process is transparent to the end hosts; their default gateway remains 10.0.0.1 (the VIP), and 10.0.0.1 is still mapped to the same virtual MAC address in their ARP tables.

After R1 recovers from its hardware failure-perhaps a faulty power supply is replaced-there are two things that can happen: R1 can reclaim its role as the active router, or it can become the new standby router. If a router (R1) takes over the role of the current operational active router (R2), it is called preemption-the same term is used with regard to OSPF's designated router (DR) and backup designated router (BDR) elections, as covered in chapter 18.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-406_763_1382_183_221.jpg)
Figure 19.4 An FHRP failover. (1) A hardware failure occurs on R1. (2) R2 takes over as the active router. (3) R2 sends GARP messages, sourced from the virtual MAC and destined for the broadcast MAC. (4) The switches learn the virtual MAC on the appropriate ports-those leading toward the new active router. (5) End hosts' default gateways and ARP table entries for the VIP remain the same.

### 19.2 Comparing FHRPs

Cisco routers support three different FHRPs, each with its own characteristics: Hot Standby Router Protocol (HSRP), Virtual Router Redundancy Protocol (VRRP), and Gateway Load Balancing Protocol (GLBP). For the purpose of the CCNA exam, you should understand the basic characteristics of each. Table 19.1 lists some essential characteristics of each FHRP that we will cover in this section.

Table 19.1 FHRP characteristics
|  | HSRP | VRRP | GLBP |
| :--- | :--- | :--- | :--- |
| Terminology | Active/Standby | Master/Backup | AVG/AVF |
| Multicast IP | v1: 224.0.0.2 v2: 224.0.0.102 | 224.0.0.18 | 224.0.0.102 |
| Virtual MAC format | v1: 0000.0c07.acXX v2: 0000.0c9f.fXXX | 0000.5e00.01XX | 0007.b400.XXYY |
| Cisco proprietary? | Yes | No | Yes |
| Load balancing | Per subnet | Per subnet | Per host |
| Preemption (default) | Disabled | Enabled | AVG: Disabled AVF: Enabled |


EXAM TIP You should know the information in table 19.1 for the exam. Flashcards are a useful tool for remembering information like this.

### 19.2.1 Hot Standby Router Protocol

Hot Standby Router Protocol (HSRP) is a Cisco-proprietary FHRP. As a Cisco-proprietary protocol, it only runs on Cisco routers. HSRP uses the active/standby terminology that we used in section 19.1; the active router functions as the default gateway, while the standby router waits to take over if the active router fails. Preemption is disabled by default; if the previously active router recovers from a failure, it will not retake its active role.

## Hot standby and cold standby

A hot standby is a backup system (it could be a server, a router, or some other device) that is fully operational and running in parallel with the primary (active) system. In the context of HSRP, hot standby means that the standby router is always ready to start forwarding packets. The standby router keeps track of the status of the active router, ready to automatically take over if the active router fails-no manual intervention is required.

A cold standby is a backup system that is not operational under normal circumstances. If the primary system fails, the backup system may need to be started up, configured, cabled, etc. An example of a cold standby router is a spare router kept in storage. Should the active router fail, the standby router is retrieved from storage and used to replace the failed active router. Clearly, this approach is less desirable than using an FHRP like HSRP, which ensures continuous network availability and a smooth transition from active to standby.

There are two versions of HSRP: version 1 and version 2. The two versions are largely similar, but there are some key differences that you should be aware of. HSRPv1 hello messages are sent to multicast IP address 224.0.0.2; this address is not reserved for HSRP but is used to address messages to all routers in the LAN. HSRPv2, on the other hand, uses multicast IP address 224.0.0.102, which is reserved exclusively for HSRPv2 and GLBP (the topic of section 19.2.3).

Another difference is the format of the virtual MAC address. HSRPv1 uses the format 0000.0c07.acXX, where XX is the HSRP group number. For example, HSRPv1 group 1 will use virtual MAC address 0000.0c07.ac01, and HSRPv1 group 15 will use 0000.0c07.ac0f (as covered in chapter 6, 0d15 is equivalent to 0xf). HSRPv2 uses the format 0000.0c9f.fXXX. Using the same examples as previously, HSRPv2 group 1 will use virtual MAC address 0000.0c9f.f001, and group 15 will use 0000.0c9f.f00f.

NOTE Because the HSRPv2 virtual MAC format leaves three hex digits to represent the group number, instead of HSRPv1's two hex digits, HSRPv2 supports more groups. In total, HSRPv1 supports 256 groups (0-255), and HSRPv2 supports 4,096 (0-4,095).

As mentioned in section 19.1.1, a single router can be part of multiple FHRP groups. When there are multiple subnets in a LAN, there should be one HSRP group per subnet. Instead of one router being active in all subnets, you can configure one router to be active in half of the subnets and the other to be active in the remaining subnets. This achieves load balancing, avoiding congestion on any single link in the network.

Figure 19.5 shows an example of load balancing with HSRP. There are two subnets in the LAN: subnet 1 (10.0.0.0/24) and subnet 2 (10.0.1.0/24). R1 is the active router in subnet 1 and the standby router in subnet 2, and vice versa. As a result, hosts in subnet 1 will use R1 as their default gateway (unless it fails), and hosts in subnet 2 will use R2 as their default gateway.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-408_598_1269_662_348.jpg)
Figure 19.5 HSRP load balancing. R1 is the active router in subnet 1 and the standby router in subnet 2. R2 is the standby router in subnet 1 and the active router in subnet 2.

One final major difference between versions 1 and 2 is that HSRPv2 adds support for IPv6, whereas HSRPv1 only supports IPv4. So far in this book, we have only covered IPv4, but we will begin looking at IPv6 in chapter 20.

### 19.2.2 Virtual Router Redundancy Protocol

Virtual Router Redundancy Protocol (VRRP) is the Internet Engineering Task Force's industry-standard answer to HSRP. At a high level, HSRP and VRRP are nearly identical. However, you should know the differences between them for the CCNA exam. One difference is terminology; instead of HSRP's active and standby routers, VRRP calls them master and backup routers. Furthermore, although HSRP preemption is disabled by default, VRRP preemption is enabled by default.

VRRP's multicast IP and virtual MAC addresses are also different. VRRP routers send hello messages to multicast IP address 224.0.0.18-a reserved address that only VRRP-enabled routers use. The VRRP virtual MAC address format is 0000.5e00.01XX, where XX is the VRRP group number. For example, VRRP group 12 will use the virtual MAC address 0000.5e00.010c.

Perhaps the most significant difference between HSRP and VRRP is that VRRP is an industry-standard protocol, and therefore any vendor is free to implement it on its devices. If a Cisco router and a Juniper router must cooperate to provide a redundant default gateway for a LAN, they must use VRRP; the Juniper router can't run HSRP.

Like HSRP, VRRP can provide load balancing on a per-subnet basis. This can be achieved by configuring one router to be the master router in half of the subnets and the other router to be the master router in the remaining subnets. However, neither HSRP nor VRRP can efficiently achieve load balancing within a single subnet. To achieve that, you'll have to use the next FHRP: GLBP.

### 19.2.3 Gateway Load Balancing Protocol

Gateway Load Balancing Protocol (GLBP) is another Cisco-proprietary FHRP. Of the three FHRPs covered in this chapter, GLBP is unique in that it enables load balancing within a single subnet; some hosts in a subnet will use one router as their default gateway, and others will use the other router (assuming two routers); load balancing is done on a per-host basis.

GLBP works by electing one router as the Active Virtual Gateway (AVG), which then assigns up to four Active Virtual Forwarders (AVF)-the routers that actively forward packets. The AVG itself can be an AVF too. That means that if there are four routers connected to the LAN, traffic will be load-balanced among all four routers. Preemption of the AVG role is disabled by default, but AVF preemption is enabled by default.

Figure 19.6 shows how GLBP works. R1, the AVG, assigns itself as the AVF for half of the hosts and R2 as the AVF for the remaining half. There is only a single subnet in the LAN, but load balancing is achieved on a per-host basis.

Although there is only a single VIP (10.0.0.1), load balancing is achieved by assigning each AVF a unique virtual MAC address. The AVG is responsible for answering ARP requests, but instead of replying with its own MAC address, it replies with the virtual MAC of one of the AVFs, in a round-robin manner. As a result, in a LAN with two AVFs, half of the hosts in the LAN will address frames to one AVF's virtual MAC and the other half to the other AVF's virtual MAC.

DEFINITION Round-robin is a simple method for distributing tasks evenly in a cyclical manner. In the context of GLBP, if there are two AVFs, the AVG will reply with the virtual MAC of AVF 1, then AVF 2, then AVF 1, etc.

Like HSRPv2, GLBP sends hello messages to the reserved multicast IP address 224.0.0.102. The GLBP virtual MAC address format is 0007.b400.XXYY, where XX is the GLBP group number and YY is the AVF number. For example, AVF 1 in group 1 will use virtual MAC 0007.b400.0101, and AVF 2 in group 11 will use virtual MAC 0007 .b400.0b02.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-410_889_1412_181_221.jpg)
Figure 19.6 GLBP enables load balancing among hosts in the same subnet. Half of the hosts in 10.0.0.0/24 use one AVF (R1) as their default gateway, and the other half use the other AVF (R2). Each AVF has a unique virtual MAC address.

### 19.3 Basic HSRP configuration

CCNA exam topic 3.5 uses the verb describe, not configure (i.e., Describe the purpose, functions, and concepts of first hop redundancy protocols). However, Cisco exams can occasionally be liberal in their interpretation of the exam topics. HSRP is the most commonly used FHRP on Cisco routers, and its basic configuration is worth knowing for the CCNA exam. Furthermore, getting a bit of hands-on practice configuring HSRP in a lab can help you understand the concepts that you have studied. For those reasons, in this section, we'll briefly cover how to configure HSRP.

Before we delve into how to configure HSRP, let's first consider why you would want to implement it in the first place. Think back to figure 19.1, where hosts were sending packets to a router that was down due to a hardware failure, despite there being another functional router connected to the LAN. The result of such a failure is that end users lose connectivity to destinations outside of the LAN: they can't access files on corporate servers, can't access the internet, can't join an important call with a client, etc.

In modern enterprises, the inability to access crucial resources over the network often means an inability to perform required tasks-it's more than a simple inconvenience. Although hardware failures are rare, they are almost inevitable over the lifespan
of a network. That's where HSRP comes into play. To minimize the disruption caused by router hardware failures, redundant routers should be configured with an FHRP, such as HSRP.

Figure 19.7 shows how to configure HSRP on two Cisco routers: R1 and R2. Note that although R1 and R2 share the VIP, each router still requires its own unique IP address, which is primarily used for communication between the two routers. For example, when R1 sends HSRP hello messages, it sources them from its own IP address (10.0.0.2).

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-411_839_1412_535_190.jpg)
Figure 19.7 An HSRPv2 group of two routers. R1 is the active router, and R2 is the standby. If R1 fails, R2 will become the active router. However, because preemption is enabled on R1, it will retake its role after it recovers from the failure.

Let's examine those configurations, specifically the HSRP configurations-you should be familiar with configuring an interface's IP address and enabling it with no shutdown by now. The following example shows how to configure R1, the active router of the HSRP group:

```
R1(config) # interface g0/0
R1(config-if)# ip address 10.0.0.2 255.255.255.0
R1(config-if) # standby version 2
R1(config-if) # standby 1 ip 10.0.0.1
R1(config-if) # standby 1 priority 105
R1(config-if) # standby 1 preempt
R1(config-if) # no shutdown
```

The first HSRP command is standby version 2. As you probably guessed, this command enables HSRP version 2. If you enable HSRPv2 on one router, you must enable it on the other router; HSRPv1 and HSRPv2 aren't compatible.

The second command is standby group ip vip, used to configure the VIP. When configuring an HSRP group, it is important that both of these parameters-the group number and the VIP-match on both routers. If either of these don't match, the routers won't be able to work together to function as a redundant default gateway.

The third command is standby group priority priority. This command is used to influence which router becomes active and which becomes standby. The HSRP active router is determined in the following manner:

1 The router with the highest priority
2 The router with the highest IP address

In this example, R1 G0/0's IP address is 10.0.0.2, and R2 G0/0's is 10.0.0.3. As a result, if both routers use the default HSRP priority (100), R2 will become the active router; it has a higher IP address. To ensure that R1 becomes the active router, I used the command standby 1 priority 105, raising R1's priority above R2's.

Finally, I enabled preemption-which is disabled by default-with the command standby 1 preempt. Preemption determines what happens when a higher-priority router comes back online after a failure. Enabling preemption ensures that R1 is the active router as long as it is up and running. If R1 were to fail, leading to R2 taking over as the active router, R1 would take over the active role again after recovering from the failure.

The following example shows R2's configurations, highlighting the HSRP-related commands. Two commands that I used on R1 are missing: I didn't configure R2's priority, and I didn't enable preemption. By increasing R1's priority to 105 , I ensured that it becomes the active router; there is no need to modify R2's priority. And preemption only needs to be configured on the higher-priority router that retakes its role after a failure; the command is not necessary on the lower-priority router (although it would do no harm to configure it anyway):
![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-412_190_1283_1564_346.jpg)

NOTE The commands we have covered here can also be used to configure VRRP and GLBP, replacing standby with vrrp or glbp (except standby version 2). However, I wouldn't expect any questions about VRRP or GLBP configuration on the exam.

After configuring HSRP, you can use show standby brief to verify its operation. In the following example, I use the command on R1:

```
R1# show standby brief
        P indicates configured to preempt.
        |
```

| Interface | Grp | Pri | P | State | Active | Standby | Virtual IP |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Gio/0 | 1 | 105 | P | Active | local | 10.0.0.3 | 10.0.0.1 |

The State of Active indicates that R1 is the active router. This is confirmed by the value of local in the Active column. The Standby column states 10.0 .0 .3 -R3's IP address. For comparison, in the following example, I use the same command on R2:

```
R2# show standby brief
        P indicates configured to preempt.
        |
```

| Interface | Grp | Pri P |  | P State | Active | Standby | Virtual IP |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Gio/o | 1 | 100 |  | Standby | 10.0.0.2 | local | 10.0.0.1 |

This time, the State is Standby, which is confirmed by the value of local in the Standby column. The Active column states 10.0.0.2-R1's IP address. For more detailed information about HSRP, you can remove the brief keyword; the following example shows the output of show standby on R1, pointing out some additional information not shown in the output of the previous command:

```
R1# show standby
GigabitEthernet0/0 - Group 1 (version 2)
```

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-413_163_266_1001_1066.jpg)
HSRPv2 is enabled.

```
    State is Active
        2 state changes, last state change 00:05:53
```

The HSRPv2 virtual

```
    Virtual IP address is 10.0.0.1
```

MAC address format

```
    Active virtual MAC address is 0000.0c9f.f001
        Local virtual MAC address is 0000.0c9f.f001 (v2 default)
    Hello time 3 sec, hold time 10 sec
        Next hello sent in 1.536 secs
```

The default HSRP timers. The

```
    Preemption enabled
```

hold time is equivalent to OSPF's dead time.

```
    Active router is local
    Standby router is 10.0.0.3, priority 100 (expires in 9.344 sec)
    Priority 105 (configured 105)
    Group name is "hsrp-Gi0/0-1" (default)
```


## Summary

- First hop redundancy protocol (FHRP) is a category of protocol that allows multiple routers to work together to provide a redundant default gateway for hosts in a LAN.
- The active router actively forwards packets between hosts in the LAN and external destinations. The standby router is ready to take over if the active router fails.
- Routers using an FHRP share a virtual IP (VIP) address, which is configured as the default gateway of hosts in the LAN. They also share a virtual MAC address.
- The active router uses the virtual MAC when replying to ARP requests sent to the VIP. When hosts send packets to external destinations, the frames encapsulating the packets will be sent to the active router.

- Routers using an FHRP communicate via multicast hello messages, which are sent at regular intervals. Hello messages are used to establish and maintain neighbor relationships.
- When the active router fails and the standby router takes over the active role, it is called a failover.
- After a failover, the new active router sends gratuitous ARP (GARP) messages- ARP replies that were not prompted by ARP requests.
- GARP messages are addressed to the broadcast MAC address (ffff.ffff.ffff) in order to update switches' MAC address tables.
- Because the VIP and virtual MAC addresses remain the same, FHRP failovers are transparent to end hosts; the end hosts are not aware that a failover has occurred.
- Cisco routers support three FHRPs: HSRP, VRRP, and GLBP.
- Hot Standby Router Protocol (HSRP) is a Cisco-proprietary FHRP with two versions: HSRPv1 and HSRPv2.
- Preemption is an FHRP feature that allows a higher-priority router to take over as the active router, even if there is another active router present.
- HSRP elects one active router, and other routers become standby routers. HSRP preemption is disabled by default.
- HSRPv1 uses the multicast IP address 224.0.0.2, and HSRPv2 uses 224.0.0.102.
- The HSRPv1 virtual MAC address format is 0000.0c07.acXX, where XX is the HSRP group number. The HSRPv2 format is 0000.0c09.fXXX, where XXX is the group.
- HSRPv2 adds IPv6 support, whereas HSRPv1 only supports IPv4.
- HSRP supports per-subnet load balancing by configuring a different active router for different subnets.
- Virtual Router Redundancy Protocol (VRRP) is an industry-standard FHRP, which can be implemented on any vendor's routers.
- VRRP elects one master router, and other routers become backup routers. Unlike HSRP, preemption is enabled by default.
- VRRP uses the multicast IP address 224.0.0.18.
- The VRRP virtual MAC address format is 0000.5e00.01XX, where XX is the VRRP group number.
- Like HSRP, VRRP supports per-subnet load balancing.
- Gateway Load Balancing Protocol (GLBP) is a Cisco-proprietary FHRP that enables load balancing within a single subnet; load balancing is done on a per-host basis.
- GLBP elects one Active Virtual Gateway (AVG), which assigns up to four Active Virtual Forwarders (AVF). The AVG itself can be an AVF. AVG preemption is disabled by default, but AVF preemption is enabled.

- Although there is a single VIP, GLBP achieves load balancing by assigning each AVF a unique virtual MAC address. The format is 0007.b400.XXYY, where XX is the group number and YY is the AVF number.
- The AVG replies to ARP requests using the virtual MAC of one of the AVFs in a round-robin manner.
- Although routers using an FHRP share a VIP, each router still needs its own unique IP address. It uses this IP address when communicating with other routers.
- Enable HSRP version 2 with standby version 2 in interface config mode.
- Configure the HSRP VIP with standby group ip vip in interface config mode.
- Configure the HSRP priority (default 100) with standby group priority priority in interface config mode.
- Enable HSRP preemption with standby group preempt in interface config mode.
- Use show standby brief and show standby to verify HSRP operation.

## Part 5

## IPv6

For decades, the dominant version of the Internet Protocol has been IPv4. Also for decades, however, another version has been slowly gaining adoption: Internet Protocol version 6 (IPv6), the topic of part 5 of this book. As the pool of available IPv4 addresses has all but dried up in recent years, IPv6 adoption has accelerated, and modern network professionals must be familiar with both IPv4 and IPv6.

Chapter 20 starts with a look at IPv6 addresses. We will explore the concepts of global unicast, unique local, link-local, and multicast addresses in IPv6, as well as how to configure them in Cisco IOS. For CCNA students, who have often just become comfortable with IPv4, IPv6 can seem intimidating. But rest assured that they are actually quite similar, with the most significant differences being the address sizes (32 bits for IPv4 versus 128 bits) and representation (decimal versus hexadecimal).

The similarities between IPv4 and IPv6 will become clear when we cover IPv6 routing in chapter 21, which covers familiar IPv4 concepts from an IPv6 perspective: connected and local routes, static route configuration, default routes, floating static routes, and others. The knowledge and skills you acquire in these two chapters are critical for the CCNA exam, but on top of that, they will be invaluable in your journey as a network professional; IPv6 is the future, and its adoption is consistently growing year after year.

Licensed to Luke Chen [mic215fa@gmail.com](mailto:mic215fa@gmail.com)

## IPv6 addressing

## This chapter covers

- Why IPv6 is needed
- How to convert between binary, decimal, and hexadecimal number systems
- The structure of IPv6 addresses
- How to configure IPv6 addresses on Cisco routers
- The various IPv6 address types

IPv4, the version of the Internet Protocol that we have focused on up to this point in the book, was developed at a time when no one knew the internet would be as ubiquitous as it is today; the current IPv4 standard was published in 1981. As a result, IPv4 has limitations, the most significant one being insufficient address space; there aren't enough IPv4 addresses available for all of the devices in the world that require network connectivity.

In this chapter, we will cover the next version of the Internet Protocol: IPv6. IPv6 provides several benefits over IPv4, the most significant one being a much larger address space. Although IPv4 is still the dominant version in use today, IPv6 adoption is growing, and the modern network engineer must be familiar with both. Specifically, we will cover the following CCNA exam topics:

- 1.8: Configure and verify IPv6 addressing and prefix
- 1.9: Describe IPv6 address types

## What about IPv5?

Given that IPv6 follows IPv4, "What about IPv5?" is a natural question. Internet Stream Protocol (ST), a family of experimental protocols designed to work with applications such as real-time video and voice, used version number 5 in the Version field of the IP header. Although ST was never referred to as IPv5, IPv4's successor was named IPv6 to avoid any confusion with ST.

### 20.1 The need for IPv6

The main reason IPv6 is needed is that there simply aren't enough IPv4 addresses available. IPv4 addresses' 32-bit length allows for 4,294,967,296 (232) unique IPv4 addresses. However, not all of those addresses are available to assign to hosts. For example, the class D range is reserved for multicast addresses, and the class E range is reserved for experimental purposes and research.

Even with all of the reserved addresses removed, there are still billions of IPv4 addresses available for hosts. However, in our era of unprecedented connectivity, even those billions of addresses are not enough. We're running out of IPv4 addresses and have been for a long time; this problem is called IPv4 address exhaustion. The problem is exacerbated by inefficient use of the available address space, but even with the most efficient use, it would only be a matter of time before we run out of IPv4 addresses.

IP address assignments are controlled by an organization called the Internet Assigned Numbers Authority (IANA). IANA distributes IP address space to various Regional Internet Registries (RIR), which then assign addresses to organizations (such as Internet Service Providers) as needed. Figure 20.1 shows the five RIRs and the regions they serve.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-419_563_1174_1351_396.jpg)
Figure 20.1 The five RIRs. AFRINIC serves Africa. APNIC serves the Asia-Pacific region. ARIN serves Canada, the United States, parts of the Caribbean, and Antarctica. LACNIC serves most of the Caribbean and all of Latin America. RIPE NCC serves Europe, West Asia, Central Asia, and Russia.

Unfortunately, the RIRs are running out of IPv4 addresses. For example, ARIN announced exhaustion of its IPv4 address pool in 2015, RIPE NCC announced the same in 2019, and LACNIC in 2020.

In addition to subnetting with CIDR (as we covered in chapter 11), which allows for more efficient use of the IPv4 address space, two particular technologies-private IPv4 addresses and network address translation (NAT)-have been instrumental in extending the lifespan of IPv4. We will cover private IPv4 addresses and NAT in chapter 9 of volume 2, but in this chapter, we'll focus on the long-term solution to IPv4 address exhaustion: IPv6.

IPv6 addresses are four times the size of IPv4 addresses: 128 bits. With that information, you might assume that IPv6 provides four times as many addresses as IPv4, but that is not the case; each additional bit doubles the number of addresses. Let's compare the number of IPv4 and IPv6 addresses:

- 4,294,967,296 (2 ${ }^{32}$ ) IPv4 addresses
- 340,282,366,920,938,463,463,374,607,431,768,211,456 (2 ${ }^{128}$ ) IPv6 addresses

The number of grains of sand on Earth is estimated to be between $10^{20}$ to $10^{24}$, so the number of IPv6 addresses is between 340 trillion and 3.4 quintillion times larger than the estimated number of grains of sand on Earth. This enormous number ensures that the IPv6 address space is more than sufficient to accommodate the current and foreseeable future needs of the growing internet.

Although there are various differences between IPv4 and IPv6 (the size and format of their addresses being just one of them), their general purpose is the same. Just like IPv4, IPv6 encapsulates Layer 4 segments (i.e., TCP or UDP) with a header to make packets, providing end-to-end addressing from a message's source host to its destination host. Likewise, an IPv6 packet is encapsulated in an Ethernet frame at each hop in the path to its final destination, just like an IPv4 packet.

## IPv6 adoption

Google keeps track of the percentage of users that access Google over IPv6 at https:// www.google.com/intl/en/ipv6/statistics.html. At the time of writing, it is nearly 45\%-a major increase from roughly 1\% just a decade ago.

### 20.2 Hexadecimal

Whereas IPv4 addresses are written in dotted-decimal notation, IPv6 addresses are written in hexadecimal. Before we cover the details of how IPv6 addresses are written in section 20.3, let's review hexadecimal notation and look at how to convert between decimal, binary, and hexadecimal.

As we covered in chapter 6, the hexadecimal number system uses the same 10 digits as the decimal number system (0-9), and the first 6 letters of the alphabet (A-F)-a
total of 16 digits. In effect, this means that a single hexadecimal digit contains four bits of information because $2^{4}=16$. Table 20.1 demonstrates this: 0b0000-0b1111 can be represented by 0×0-0×F (the equivalent decimal values are included for reference).

Table 20.1 Decimal, binary, and hexadecimal numbers
| Decimal | Binary | Hex. | Decimal | Binary | Hex. |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 0 | 0000 | 0 | 8 | 1000 | 8 |
| 1 | 0001 | 1 | 9 | 1001 | 9 |
| 2 | 0010 | 2 | 10 | 1010 | A |
| 3 | 0011 | 3 | 11 | 1011 | B |
| 4 | 0100 | 4 | 12 | 1100 | C |
| 5 | 0101 | 5 | 13 | 1101 | D |
| 6 | 0110 | 6 | 14 | 1110 | E |
| 7 | 0111 | 7 | 15 | 1111 | F |


NOTE A group of four bits (a half-byte) is also called a nibble.
IPv6 addresses are split into groups of four hexadecimal characters (16 bits) when written (a string of 128 bits is not very human-readable), so let's practice converting 16-bit values between binary and hexadecimal. This skill is necessary for mastering IPv6, just as converting 8-bit values between binary and decimal is necessary for IPv4.

Converting from binary to hexadecimal can be done with a three-step process. To demonstrate, let's convert 0b1101101100101111 to hexadecimal:

1 Split the number into four-bit groups: 1101, 1011, 0010, 1111.
${ }^{2}$ Convert each four-bit group into decimal: 13, 11, 2, 15.
3 Convert each decimal number into hexadecimal: D, B, 2, F.

NOTE Memorizing the decimal values of hexadecimal A-F is very helpful: 0 d10 = $0 \times \mathrm{A}, 0 \mathrm{~d} 11=0 \times \mathrm{B}, 0 \mathrm{~d} 12=0 \times \mathrm{C}, 0 \mathrm{~d} 13=0 \times \mathrm{D}, 0 \mathrm{~d} 14=0 \times \mathrm{E}, 0 \mathrm{~d} 15=0 \times \mathrm{F}$.

We now have the answer: 0b1101101100101111 is equivalent to 0xDB2F. As you can see, hexadecimal is much more compact than binary; the same value can be expressed with one quarter the number of digits.

To convert from hexadecimal to binary, you can use a similar process. Let's do that with the number 0x41AE:

1 Split up the hexadecimal digits: 4, 1, A, E.
2 Convert each hexadecimal digit to decimal: 4, 1, 10, 14.
3 Convert each decimal digit to binary: 0100, 0001, 1010, 1110.

That's the answer: 0x41AE is equivalent to 0b0100000110101110. For further practice, write some random 16-bit numbers in binary and convert them to hexadecimal. Then, do the same in the opposite direction: write some random four-digit hexadecimal numbers and convert them to binary. You can check your answers with a free converter online: try a Google search for "binary to hexadecimal converter."

NOTE Make sure you can do these conversions confidently before moving on. For some fun practice converting 1-byte numbers, which follows the same process we just covered, you can try the game "Flippy Bit And the Attack of the Hexadecimals from Base 16" at https://mng.bz/PZRv. A browser game, Android app, and iOS app are available.

### 20.3 IPv6 addressing

Now that you're comfortable converting between binary and hexadecimal, let's move on to IPv6 addressing. IPv6 addresses can be intimidating to many students at first, primarily because they are quite large and because they are written in hexadecimal, rather than decimal. However, there is no need to be afraid of IPv6; after the initial period of unfamiliarity, you'll grow to love it! I, for one, welcome our new IPv6 overlords. Both IPv4 and IPv6 are just binary represented in two different ways: dotted decimal versus hexadecimal.

### 20.3.1 IPv6 header

Before looking at IPv6 addresses themselves, let's take a brief look at the header that they are part of. As with the IPv4 header, don't expect any questions about the specifics of the IPv6 header on the CCNA exam. However, a basic understanding of the header provides valuable context for understanding IPv6 as a whole. Figure 20.2 shows the format of the IPv6 header.

|  | Byte | 0 |  |  |  |  |  |  |  | 1 |  |  |  |  |  |  |  |  | 2 |  |  |  |  |  |  |  | 3 |  |  |  |  |  |  |  |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Byte | Bit | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 | 11 | 12 | 13 |  | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 |
| 0 | 0 | Version |  |  |  | Traffic Class |  |  |  |  |  |  |  | Flow Label |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| 4 | 32 | Payload Length |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | Next Header |  |  |  |  |  |  |  | Hop Limit |  |  |  |  |  |  |  |
| 8 | 64 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| 12 | 96 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| 16 | 128 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| 20 | 160 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| 24 | 192 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| 28 | 224 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| 32 | 256 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| 36 | 288 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |

Figure 20.2 The format of the IPv6 header. The fields are Version, Traffic Class, Flow Label, Payload Length, Next Header, Hop Limit, Source Address, and Destination Address. The header is 40 bytes in size.

Just like the IPv4 header, the first field of the IPv6 header is the Version field. This field is four bits in length and is always set to 0b0110 (0d6) to indicate IPv6. The next field is the Traffic Class field, an 8-bit field that is used for Quality of Service (QoS)-the topic of chapter 10 of volume 2. This field is split up into two parts: the 6-bit Differentiated Services Code Point (DSCP) and the 2-bit Explicit Congestion Notification (ECN)-just like in the IPv4 header. We'll cover how this field is used in chapter 10 of volume 2.

Next is a 20-bit field called Flow Label. This field is, as the name suggests, used to label flows; a flow is a sequence of packets sent from a particular source to a particular destination (i.e., a TCP session between two hosts). The potential uses of this field are beyond the scope of the CCNA exam, but it can also play a role in QoS.

The fourth field is Payload Length, a 16-bit field that is used to indicate the length of the packet's encapsulated payload (i.e., the encapsulated TCP or UDP segment). Whereas the IPv4 header is variable in length (20-60 bytes), the IPv6 uses a fixed 40-byte header. As a result, whereas IPv4 uses two separate length fields-Internet Header Length to indicate the header's length and Total Length to indicate the entire packet's length-IPv6 only requires a single length field, indicating the size of the payload.

The next field of the IPv6 header is Next Header, which indicates the type of message encapsulated in the packet; this is equivalent to IPv4's Protocol field. For example, a value of 6 in this field indicates that a TCP segment is encapsulated inside. Following are some common values you can expect in the IPv6 Next Header and IPv4 Protocol fields:

- 1: ICMP
- 6: TCP
- 17: UDP
- 58: ICMPv6 (ICMP for IPv6)
- 88: EIGRP
- 89: OSPF

The final field before the addresses is the Hop Limit field, which is equivalent to IPv4's Time to Live (TTL) field. When a host sends a packet, it will set a certain value in this field, and each router that forwards the packet will decrement the value by 1. If the value reaches 0, the router will drop the packet. The purpose of this field is to prevent packets from looping around the network indefinitely (although loops should not occur in a properly configured network).

The final two fields are the Source Address and Destination Address fields-two 128bit fields that contain the packet's source and destination IPv6 addresses. In section 20.3.2, we'll explore how these addresses are structured.

### 20.3.2 IPv6 address structure

An IPv6 address is a 128-bit number that identifies a host at Layer 3 of the TCP/IP model. As the following example shows, a string of 128 bits is not very human-readable:

001000000000000100001101101110000101100100010111111010101011110101 10010101100010000101111110101011001001001011010101100110111101

IPv4 addresses are divided into four groups of 8 bits and written in dotted decimal to make them more human-readable. For demonstration purposes, here's what the previous IPv6 address would look like written in dotted decimal:
32.1.13.184.89.23.234.189.101.98.23.234.201.45.89.189

Although the dotted-decimal address is more manageable than the binary address, it's still quite long. To allow the same address to be written in even fewer digits, IPv6 addresses are divided into eight groups of 16 bits, separated by colons, and written in hexadecimal. That same IPv6 looks like this when written in hexadecimal:

2001:0db8:5917:eabd:6562:17ea:c92d:59bd

NOTE Hexadecimal can be written using uppercase or lowercase letters (A-F or a-f). However, IPv6 addresses should be written using lowercase letters-more on this later.

There is no official or standard term for each group of 16 bits in an IPv6 address. I previously preferred quartet because each group results in four hexadecimal digits. Another common term is hextet, although the term implies 6 bits, not 16 bits; the precise term is hexadectet. However, hextet is more common-probably because it's easier to pronounce. Although it is an informal term, I will use hextet as a convenient way to refer to each group of 16 bits in an IPv6 address.

Figure 20.3 shows the previous IPv6 address along with the binary of each hextet. It also introduces the IPv6 prefix length, which is always written with slash notation (/64)-no more dotted-decimal netmasks!

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-424_266_1292_1349_350.jpg)
Figure 20.3 An IPv6 address written in hexadecimal and binary. The prefix length is $/ 64$, meaning that the first half ( 64 bits) of the address is the network portion, and the second half ( 64 bits) is the host portion.

NOTE IPv6 typically uses /64 prefix lengths. Although this is extremely inefficient-each subnet contains about 18 quintillion addresses-the IPv6 address space is so large that /64 prefix lengths are preferred due to their simplicity.

### 20.3.3 Abbreviating IPv6 addresses

Although hexadecimal notation makes IPv6 addresses much more manageable than a simple string of 128 bits, it still results in 32 digits-much longer than an IPv4 addess, which is 4-12 digits in dotted decimal. Fortunately, IPv6 provides two methods for abbreviating addresses, which can significantly reduce their length and complexity:

- Removing leading zeros
- Omitting consecutive all-zero hextets

Leading zeros-any hexadecimal 0 digits on the left side of a hextet-can be removed to shorten the hextet. Here is an example:

- Original address-2001:0db8:0000:001b:20a1:0020:0080:34bd
- Abbreviated address-2001:db8:0:1b:20a1:20:80:34bd

In the abbreviated address, I removed leading zeros from five hextets. Note that the 0020 and 0080 hextets were abbreviated to 20 and 80, respectively. Trailing zeros-those on the right side of a hextet-cannot be removed. Furthermore, the 0000 octet was abbreviated to 0-only three of the four zeros can be removed, not all four.

The second method is to omit consecutive all-zero hextets-two or more hextets of all zeros (0x0000); the omitted hextets are represented by a double colon. Here is an example of this method:

- Original address-2001:2db8:0000:0000:0000:0000:1280:34bd
- Abbreviated address-2001:2db8::1280:34bd

The original address has four consecutive all-zero hextets, which were replaced with a double colon in the abbreviated address. Because IPv6 addresses are eight hextets in length, there is no ambiguity in the abbreviated address; only four hextets are displayed, so we can deduce that four all-zero hextets were abbreviated by the double colon.

NOTE A double colon can only be used once in an address. If there are multiple choices for where to use it (i.e., 2001:0db8:0000:0000:1234:0000:0000:0001), only one series of all-zero hextets can be shortened (i.e., 2001:0db8::1234:0000: 0000:0001).

The two address-abbreviation methods can be combined if the address permits it. Following is an example of abbreviating an address both by removing leading zeros and by omitting consecutive all-zero hextets:

- Original address-2001:0db8:0000:0000:002f:0001:0000:34bd
- Abbreviated address—2001:db8::2f:1:0:34bd

## RFC 5952: IPv6 address representation

IPv6 address representation-how we write IPv6 addresses-is addressed in RFC 5952. Its status is "proposed standard," meaning it has not been implemented as a true internet standard. However, it provides some useful guidelines for how to write IPv6 addresses that I recommend following for consistent and accurate IPv6 address representation:

- All leading zeros must be removed. 2001:0db8::0001 is incorrect; it must be written as 2001:db8::1
- The double colon must abbreviate the address as much as possible. For example, 2001:db8:0:0:0:0:2:1 cannot be abbreviated as 2001:db8::0:2:1; it must be abbreviated as 2001:db8::2:1.
- An individual all-zero hextet cannot be omitted with a double colon. For example, 2001:db8::1:2:3:4:5 is incorrect; it must be written as 2001:db8:0:1:2:3:4:5.
- If there are multiple choices for the placement of a double colon, it must be used to replace the longest series of all-zero hextets. For example, 2001:0:0:1:0:0:0:1 must be abbreviated as 2001:0:0:1::1.
If the two choices are of equal length, the leftmost series must be replaced. For example, 2001:db8:0:0:1:0:0:1 must be abbreviated as 2001:db8::1:0:0:1.
- Hexadecimal characters a-f must be written in lowercase, not uppercase.

Table 20.2 shows some more full and abbreviated IPv6 addresses for your reference. For the rest of this chapter and chapter 21, I will abbreviate IPv6 addresses by default.

Table 20.2 IPv6 address abbreviation
| Full address | Abbreviated address |
| :--- | :--- |
| 2e09:2100:0222:00f0:1000:0000:0000:011f | 2e09:2100:222:f0:1000::11f |
| fd00:af89:0010:01df:ac80:0000:0000:0000 | fd00:af89:10:1df:ac80:: |
| ff02:0000:0000:0000:0000:0000:0000:0002 | ff02::2 |
| fe80:0000:0000:0000:1010:000a:0bad:cafe | fe80::1010:a:bad:cafe |
| 0000:0000:0000:0000:0000:0000:0000:0001 | ::1 |


### 20.3.4 Identifying the IPv6 prefix

A network prefix is the combination of a network address and prefix length. For example, if a host has IPv4 address 192.168.1.10/24, the prefix is 192.168.1.0/24. You should be comfortable with this process by now-we covered it in detail in chapter 11.

The concept is exactly the same in IPv6: an IPv6 prefix is the combination of a network address (an IPv6 address with an all-zero host portion) and a prefix length. For example, if a host has IPv6 address 2001:db8:1:2:a:b:c:d/64, what is the network prefix?

Because IPv6 almost exclusively uses /64 prefix lengths, you don't even need to convert to binary: simply convert the final four hextets-the host portion of the address-to
all zeros: 2001:db8:1:2::/64 (abbreviating the host portion's zeros with ::). Table 20.3 lists some example IPv6 host addresses and their prefixes.

Table 20.3 IPv6 network prefixes
| Host address | Prefix |
| :--- | :--- |
| fd00:af89:1234:1200:0:123:4567:beef/64 | fd00:af89:1234:1200::/64 |
| 2001:db8:babe:cafe:2100:101:0:1/64 | 2001:db8:babe:cafe::/64 |
| fd00:1234:5678:9012::feed:dad/64 | fd00:1234:5678:9012::/64 |
| 2333::efd:1212:1:1/64 | 2333::/64 |
| 3100:ab00:1f26::2000:1/64 | 3100:ab00:1f26::/64 |


NOTE Identifying the IPv6 prefix when using non-/64 prefix lengths (i.e., /55, /73, /97) can be a useful exercise for converting between hexadecimal and binary but is not very practical in reality; IPv6 usually uses /64.

### 20.4 IPv6 address configuration

Although IPv6 involves learning many new concepts, I have some good news: many IPv6 configuration and verification commands are identical to IPv4 configuration commands, simply replacing ip with ipv6. Here are some examples:

- IPv4-ip address, IPv6-ipv6 address
- IPv4-ip route, IPv6-ipv6 route
- IPv4-show ip interface brief, IPv6-show ipv6 interface brief
- IPv4-show ip route, IPv6-show ipv6 route

In this section, we'll look at a couple of methods for configuring IPv6 addresses on Cisco router interfaces: manual configuration and a method that allows the router to automatically generate the address's host portion. Figure 20.4 shows the simple network we will use: a single router (R1) connected to three networks.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-427_377_1355_1602_190.jpg)
Figure 20.4 A router with three connected IPv6 networks. ipv6 unicast-routing allows the router to forward IPv6 packets. ipv6 address configures an IPv6 address on each interface. Modified EUI-64 is used to automatically generate the host portion of G0/2's IPv6 address.

The first command you should use when configuring IPv6 on a Cisco router is ipv6 unicast-routing in global config mode. Without this command, you will be able to configure IPv6 addresses on the router, but it won't be able to route IPv6 packets. For example, R1 won't be able to forward packets between hosts in 2001:db8::/64 and 2001:db8:0:1/64 without this command.

EXAM TIP Don't forget ipv6 unicast-routing! It's easy to overlook because IPv4 routing is enabled by default, but IPv6 routing isn't.

### 20.4.1 Manually assigning an IPv6 address

The command to manually assign an IPv6 address is ipv6 address address/prefix -length. In addition to using ipv6 instead of ip, another major difference is that you have to specify the prefix length using slash notation (/64), instead of a dotteddecimal netmask like in IPv4. In the following example, I configure R1 G0/0's IPv6 address and confirm with show ipv6 interface brief:

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-428_523_1283_904_346.jpg)
NOTE Cisco IOS show commands display IPv6 addresses using uppercase A-F. RFC 5952 isn't a full internet standard, so its recommendations aren't always followed.

There are a few main things to point out about the previous example. First, notice that you can configure abbreviated IPv6 addresses; as long as you abbreviate the address correctly, Cisco IOS will be able to interpret it as the proper address. Second, after configuring the 2001:db8::1/64 address on G0/0, a second address is automatically generated-a link-local address. We will cover this address type in section 20.5.3.

For demonstration purposes, in the following example, I configure G0/1's full, unabbreviated IPv6 address. Notice that, in the output of show ipv6 interface brief, Cisco IOS automatically abbreviates the address to its shortest possible version:

```
R1(config) # interface g0/1
R1(config-if)# ipv6 address 2001:0db8:0000:0001:0000:0000:0000:0001/64
R1(config-if) # no shutdown
R1(config-if)# do show ipv6 interface brief
GigabitEthernet0/0 [up/up]
```

Configures R1 G0/1's IPv6 address

```
    FE80::260:2FFF:FE62:B801
    2001:DB8::1
GigabitEthernet0/1 [up/up]
```

Cisco IOS automatically

```
    FE80::260:2FFF:FE62:B802
```

abbreviates the address.

```
    2001:DB8:0:1::1
GigabitEthernet0/2 [administratively down/down]
Unassigned
```

One practical difference between configuring IPv4 and IPv6 addresses in Cisco IOS is that IPv4 addresses overwrite each other, but IPv6 addresses don't. In the following example, I configure multiple IPv4 and IPv6 addresses on another router (R2); notice the results afterward:

```
R2(config) # interface g0/0
R2(config-if)# ip address 192.168.1.1 255.255.255.0
R2(config-if)# ip address 192.168.1.2 255.255.255.0
R2(config-if) # ip address 192.168.2.1 255.255.255.0
R2(config-if)# ipv6 address 2001:db8:12:34::1/64
R2(config-if)# ipv6 address 2001:db8:12:34::2/64
R2(config-if)# ipv6 address 2001:db8:56:78::1/64
R2(config-if) # no shutdown
R2(config-if) # do show running-config interface g0/0
. . .
interface GigabitEthernet0/0
ip address 192.168.2.1 255.255.255.0
duplex auto
speed auto
ipv6 address 2001:DB8:12:34::1/64
ipv6 address 2001:DB8:12:34::2/64
ipv6 address 2001:DB8:56:78::1/64
```

Notice that only the most recently configured IPv4 address (192.168.2.1/24) remains; it overwrites the previously configured addresses. However, configuring additional IPv6 addresses doesn't overwrite the previous ones; R2 now has three IPv6 addresses on the same interface. Keep this difference in mind as you practice configuring IPv6: if you want to change an interface's IPv6 address, you must use the no command to remove the existing IPv6 address.

There are use cases for configuring multiple IP addresses on an interface, but they are beyond the scope of the CCNA exam. One example use case is when you have run out of addresses in a particular subnet: you can add another subnet to the same router interface by configuring an additional IP address in the new subnet. However, for the purpose of the CCNA exam, you can assume that each router interface has one IP address.

## Multiple IPv4 addresses on an interface

You can also configure multiple IPv4 addresses on an interface by adding the secondary keyword to the end of the ip address command. For example, the first IP address might be configured as ip address 192.168.1.1 255.255.255.0, and the second ip address 192.168.10.1 255.255.255.0 secondary. There is no limit to the number of secondary addresses you can configure on an interface.

### 20.4.2 Modified EUI-64

Modified Extended Unique Identifier 64 (EUI-64) is a method of automatically generating a 64-bit IPv6 interface identifier-the host portion of the address. To configure an IPv6 address using Modified EUI-64, specify a /64 IPv6 prefix and add the eui-64 keyword to the end of the ipv6 address command. In the following example, I use EUI-64 to configure R1's G0/2 interface:

```
R1(config) # interface g0/2
R1(config-if)# ipv6 address 2001:db8:0:2::/64 eui-64
R1(config-if) # no shutdown
R1(config-if)# do show ipv6 interface brief
GigabitEthernet0/0 [up/up]
```

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-430_139_296_909_1323.jpg)

```
    FE80::260:2FFF:FE62:B801
    2001:DB8::1
GigabitEthernet0/1 [up/up]
    FE80::260:2FFF:FE62:B802
    2001:DB8:0:1::1
GigabitEthernet0/2 [up/up]
    FE80::260:2FFF:FE62:B803
```

R1 automatically generates the

```
    2001:DB8:0:2:260:2FFF:FE62:B803
```

host portion of the address.

NOTE The host portion of the address I configured on G0/2 and that of its automatically configured link-local address are the same; both use Modified EUI-64.

How does Modified EUI-64 generate a 64-bit interface identifier? It takes the interface's 48-bit MAC address and adds 0xfffe (a reserved 16-bit value) to make it 64 bits in total. Here is the three-step process:

1 Divide the MAC address in half.
2 Insert 0xfffe in the middle.
$3_{3}$ Invert the seventh bit.

Figure 20.5 demonstrates this three-step process using G0/2's MAC address (0060.2f62. b803). Let's see how R1 turned the 48-bit MAC address into the 64-bit interface identifier 0260:2fff:fe62:b803.

1. Divide the MAC address in half: 0060 2f | 62 b803
2. Insert fffe in the middle: 0060 2fff fe62 b803

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-431_110_843_308_558.jpg)
Figure 20.5 Generating a 64-bit interface identifier using Modified EUI-64. (1) Divide the MAC address in half and (2) insert fffe in the middle to make it 64 bits. Then, (3) invert the seventh bit-the third bit of the second hex digit.

The third step, in which you must invert the seventh bit of the 64-bit identifier, probably requires some explanation. Because each hexadecimal digit is 4 bits, the seventh bit of the interface identifier is the third bit of the second hexadecimal digit-the second hexadecimal 0, in this case. Inverting the seventh bit of R1 G0/2's MAC addresschanging it from 0b0 to 0b1-changes the second hexadecimal digit from 0x0 (0b0000) to 0x2 (0b0010). This results in the interface identifier 0260:2fff:fe62:b803. That interface identifier, combined with the 64-bit prefix specified in the ipv6 address command, results in IPv6 address 2001:db8:0:2:260:2fff:fe62:b803.

Table 20.4 shows some example MAC addresses and Modified EUI-64 interface identifiers. For practice, I recommend writing out each MAC address and doing the conversions yourself.

Table 20.4 Example Modified EUI-64 interface IDs
| MAC address | Modified EUI-64 Interface ID |
| :--- | :--- |
| 7f2b.cbac. 1813 | 7d2b:cbff:feac:1813 |
| 1548.4fff. 2101 | 1748:4fff:feff:2101 |
| 3150.c012.1212 | 3350:cOff:fe12:1212 |
| 84ec.4389.fd23 | 86ec:43ff:fe89:fd23 |
| 9eba.1112.9900 | 9cba:11ff:fe12:9900 |


EXAM TIP Modified EUI-64 is exam topic 1.9.d. For the CCNA exam, you should be comfortable converting a MAC address to a Modified EUI-64 interface identifier using the three-step process outlined in this section.

Why invert the seventh bit?
The term Modified in the name Modified EUI-64 refers to the inversion of the seventh bit that is done when using EUI-64 to generate a 64-bit interface identifier for an IPv6 address. Although it's beyond the scope of the CCNA exam, let's take a look at why this is done.

MAC addresses can be divided into two types: a Universally Administered Address (UAA) is a MAC address that is uniquely assigned to the device by the manufacturer, and a Locally Administered Address (LAA) is assigned locally and doesn't have to be globally unique. You can identify a UAA or LAA by the seventh bit of the MAC address, called the U/L bit (Universal/Local bit):

- U/L bit of 0 = UAA
- U/L bit of 1 = LAA

In the context of IPv6 addresses and EUI-64, the meaning of this bit is reversed:

- U/L bit of $0=$ The MAC address the EUI-64 ID was generated from was an LAA.
- U/L bit of 1 = The MAC address the EUI-64 ID was generated from was a UAA.

The exact reasoning for this reversal is not worth delving into here. Just remember that this bit is always flipped when using EUI-64 to generate an interface ID for IPv6 (the host portion of the address). This process is called Modified EUI-64, although it's often casually referred to simply as EUI-64.

### 20.5 IPv6 address types

Like IPv4, there are various IPv6 addresses and address ranges that are reserved for specific purposes. In this section, we will cover those address types and their IPv4 equivalents. For some of these IPv6 address types, we have covered the IPv4 equivalent previously in this book. In other cases, this will be their first introduction.

Figure 20.6 outlines the five main address types we will delve into in this section:

- Global unicast-Globally unique addresses that can be used for communication over the internet.
- Unique local-Addresses that don't have to be globally unique; they can be freely used in internal networks but can't be used for communication over the internet.
- Link-local-Used for communication between directly connected hosts.
- Multicast-Used for one-to-multiple communication, allowing a single packet to be addressed to multiple hosts.
- Anycast-Used for one-to-one-of-multiple communication, an anycast address is a unicast address that is assigned to multiple hosts. Packets are delivered to the nearest host configured with the address, often used on servers to provide services over the internet with low latency.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-433_611_1252_181_316.jpg)
Figure 20.6 IPv6 address types: global unicast, unique local, link-local, multicast, and anycast.

### 20.5.1 Global unicast

IPv6 global unicast addresses are globally unique addresses used for communication over the public internet. Their allocation is controlled by IANA, as mentioned in section 20.1. Because they are public and must be globally unique, an enterprise is not free to use whichever global unicast addresses they want; the enterprise will typically be assigned an address block from an ISP or from the appropriate RIR, depending on the type and size of the business.

The global unicast address range was originally defined as $2000:: / 3$, which includes all addresses from 2000:: through 3fff:ffff:ffff:ffff:ffff:ffff:ffff:ffff. However, the definition of global unicast addresses has expanded to include any address that is not specifically reserved for other purposes.

An enterprise that requests IPv6 global unicast addresses will typically be assigned a /48 address block from the RIR or ISP; this is called the global routing prefix. Because IPv6 prefix lengths are typically /64, the /48 global routing prefix leaves 16 bits free for the enterprise to use to make different subnet addresses; this 16-bit section of the address is called the subnet identifier. The remaining 64 bits are the interface identifier- the host portion. Figure 20.7 shows the structure of an IPv6 global unicast address.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-433_190_1005_1754_320.jpg)
Figure 20.7 The structure of an IPv6 global unicast address. The first 48 bits are the global routing prefix, assigned to the enterprise by the ISP. The next 16 bits are the subnet identifier, which the enterprise can use to make various subnets. The remaining 64 bits are the interface identifier-the host portion of the address.

The IPv6 subnetting process is the same as IPv4 subnetting; the bits of the subnet identifier are equivalent to the borrowed bits when subnetting IPv4 networks. Following are the first few subnets that can be made with the 2001:db8:8b00::/48 address block:

- 2001:db8:8b00::/64 (subnet identifier 0x0000)
- 2001:db8:8b00:1::/64 (subnet identifier 0x0001)
- 2001:db8:8b00:2::/64 (subnet identifier 0x0002)

The IPv4 equivalent of an IPv6 global unicast address is a public IPv4 address. The ranges for public IPv4 addresses are all the addresses that are not part of the reserved private or other special-purpose ranges. I will mention the IPv4 private address ranges in section 20.5.2 and then cover them in greater detail in chapter 9 of volume 2.

NOTE The 2001:db8::/32 address range is reserved for use in examples in books, documentation, etc. Most examples of global unicast IPv6 addresses in this book will be from this reserved range.

### 20.5.2 Unique local

IPv6 unique local addresses (ULAs) are very similar to global unicast addresses. Like global unicast addresses, they are used for unicast communications; they are meant to identify a single host on the network. The main difference in use is that ULAs are private addresses; they do not have to be globally unique, and enterprises are free to use them in their internal networks. However, they cannot be used for communications over the internet-a public network that relies on each destination having a unique address. ISPs will drop packets sourced from or destined for ULAs.

IPv6 ULAs are equivalent to private IPv4 addresses, which are addresses included in one of the following three ranges:

- 10.0.0.0/8 (10.0.0.0-10.255.255.255)
- 172.16.0.0/12 (172.16.0.0-172.31.255.255)
- 192.168.0.0/16 (192.168.0.0-192.168.255.255)

You'll probably recognize the three private IPv4 address ranges; I use them in most examples in this book. We will cover these addresses in greater detail in chapter 9 of volume 2.

The IPv6 ULA range is defined as fc00::/7, which includes all addresses from fc00:: through fdff:ffff:ffff:ffff:ffff:ffff:ffff:ffff. However, the range is divided into two / 8 blocks:

- fc00::/8-Currently reserved and not defined for any specific purpose
- fd00::/8-The active range for IPv6 ULAs

Because the fc00::/8 range is currently reserved, all ULAs should begin with fd. Figure 20.8 shows the structure of an IPv6 ULA, similar to that of a global unicast address.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-435_175_984_185_318.jpg)
Figure 20.8 The structure of an IPv6 unique local address. After the prefix fd, there is a 40-bit global ID, which should be randomly generated. The following 16 bits are the subnet identifier, and the remaining 64 bits are the interface identifier.

After the prefix fd, ULAs have a 40-bit global ID, which should be randomly generated; this makes it highly likely that the global ID will be globally unique, resulting in each enterprise having unique ULAs. The rest of the address is identical to a global unicast address: a 16-bit subnet identifier and a 64-bit interface identifier.

## Randomly generated global IDs

Randomly generating global IDs for ULAs to make them globally unique may seem to contradict the fact that ULAs are private addresses and don't have to be globally unique. Even though that is true, there are benefits to avoiding overlapping addresses between enterprises. For example, in business scenarios, mergers and acquisitions often require the integration of two or more previously separate networks. If ULAs use randomly generated global IDs, the likelihood of overlapping addresses is minimized, making the integration process much smoother.

### 20.5.3 Link-local

IPv6 link-local addresses (LLA), as we saw in section 20.4, are automatically generated on IPv6-enabled interfaces-those with an IPv6 address (i.e., a global unicast address) configured.

NOTE You can enable IPv6 on an interface without configuring an IPv6 address by using the ipv6 enable command. In this case, the interface will only have an automatically generated LLA.

LLAs are unicast addresses used to identify a single host. However, as the name implies, LLAs can only be used for communication on the local link-between devices connected to the same network segment. Devices that aren't connected to the same segment cannot communicate using LLAs. Figure 20.9 demonstrates this concept.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-435_207_790_1793_318.jpg)
Figure 20.9 Link-local addresses can only be used for communication between hosts connected to the same link (segment). R1-R2 and R2-R3 communication can be sourced from/destined for each other's LLA, but R1-R3 communication cannot.

IPv6 LLAs use the fe80::/10 range, but the standard also states that the following 54 bits must be 0 . As a result, all LLAs use the fe80::/64 prefix. The remaining 64 bits-the interface identifier-are automatically generated using Modified EUI-64 rules. However, it is also possible to manually configure the interface's LLA with the ipv6 address address link-local command (without specifying a prefix length), as in the following example:

```
R1(config)# do show ipv6 interface brief
GigabitEthernet0/0 [up/up]
    FE80::260:2FFF:FE62:B801
    2001:DB8::1
. . .
R1(config) # interface g0/0
R1(config-if)# ipv6 address fe80::1 link-local
R1(config-if)# do show ipv6 interface brief
GigabitEthernet0/0 [up/up]
    FE80::1
    2001:DB8::1
```

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-436_81_401_786_893.jpg)

NOTE Each IPv6 interface must have exactly one link-local address. Configuring a new one will overwrite the old one, unlike global unicast and unique local addresses, which do not overwrite each other.

IPv4 also uses link-local addresses; the reserved range is 169.254.0.0/16. However, the major difference between IPv4 and IPv6 is that IPv6 requires each interface to have an LLA. We'll explore the role of LLAs in IPv6 in chapter 21.

IPv4 doesn't require each interface to have an LLA, but one situation in which you may encounter IPv4 LLAs is when a host attempts to receive an IPv4 address using Dynamic Host Configuration Protocol (DHCP) but is unable to; in many operating systems, the host will then automatically generate an IPv4 LLA for itself. We'll cover DHCP in chapter 4 of volume 2.

### 20.5.4 Multicast

Unicast addresses, such as global unicast, unique local, and link-local IPv6 addresses, are used for one-to-one communication-from one host to another. Multicast addresses, on the other hand, provide one-to-multiple communication-from one host to multiple destinations; all hosts that have joined the relevant multicast group. For example, as we covered in chapter 18, OSPF routers accept packets destined for 224.0.0.5 on their OSPF-activated interfaces; in other words, they join the 224.0.0.5 multicast group. IPv4 multicast addresses are from the class D range (224.0.0.0-239.255.255.255).

IPv6 uses the ff00::/8 range for multicast addresses; if an address begins with ff, it's a multicast address. However, IPv6 defines several multicast scopes that determine how far the multicast packets should travel; these scopes are determined by the final digit of the first hextet. Table 20.5 lists and briefly describes some of the IPv6 multicast scopes.

Table 20.5 IPv6 multicast scopes
| Scope | First hextet | Description |
| :--- | :--- | :--- |
| Interface-local | ff01 | The packet doesn't leave the local device. Can be used to send traffic to a service within the local device. |
| Link-local | ff02 | The packet remains on the local segment. Routers will not route the packet. |
| Site-local | ff05 | The packet can be forwarded by routers. Should be limited to a single physical site (location). |
| Organization-local | ff08 | Wider in scope than site-local (an entire enterprise/organization). |
| Global | ff0e | No boundaries. Possible to be routed over the internet. |


NOTE Don't mix up unicast link-local addresses (LLAs) as covered in section 20.5.3 and the link-local multicast scope. The term link-local address typically refers to a unicast address, and link-local multicast refers to the multicast scope.

Figure 20.10 gives a visual representation of each of the multicast scopes (aside from interface-local), representing how far each kind of multicast packet sent by PC1 may travel.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-437_681_1269_1054_196.jpg)
Figure 20.10 IPv6 multicast scopes from the perspective of PC1. Link-local multicast remains within the local link. Site-local multicast may be forwarded within the local site. Organization-local multicast may be forwarded within the organization. Global multicast has no boundaries and may be forwarded over the internet.

EXAM TIP The specifics of how site-local, organization-local, and global multicast packets are forwarded by routers are well beyond the scope of the CCNA exam. I recommend just memorizing their names and first hextet patterns (ff05, ff08, ff0e).

The one multicast scope you should be familiar with for the CCNA exam is link-local: multicast messages that are sent to other hosts in the same network segment. Table 20.6 lists some common IPv6 link-local multicast addresses and their IPv4 equivalents (a couple of which we saw when covering OSPF in chapter 18). Switches will flood these messages to all hosts on the local segment by default, and it is up to those hosts to decide if they are interested in the contents or not.

Table 20.6 Common link-local multicast addresses
| Purpose | IPv6 address | IPv4 address |
| :--- | :--- | :--- |
| All nodes | ff02::1 | 224.0.0.1 |
| All routers | ff02::2 | 224.0.0.2 |
| All OSPF routers | ff02::5 | 224.0.0.5 |
| All OSPF DRs/BDRs | ff02::6 | 224.0.0.6 |
| All RIP routers | ff02::9 | 224.0.0.9 |
| All EIGRP routers | ff02::a | 224.0.0.10 |


NOTE In IPv6, there is no such thing as a broadcast address. Instead, IPv6 hosts can use the all-nodes multicast address (ff02::1) to send a message to all hosts on the local segment. In effect, this is like a broadcast message: from one host to all other hosts connected to the same segment.

You can check which multicast groups a Cisco router interface has joined-which types of multicast messages it is interested in-with the show ipv6 interface command. In the following example, I use the command on R1, which we configured in section 20.4:

```
R1# show ipv6 interface g0/0
GigabitEthernet0/0 is up, line protocol is up
IPv6 is enabled, link-local address is FE80::2D0:97FF:FE92:B401
No Virtual link-local address(es):
Global unicast address(es):
2001:DB8:1:1::1, subnet is 2001:DB8:1:1::/64
Joined group address(es):
FF02::1
FF02::2
FF02::1:FF00:1
FF02::1:FF92:B401
. . .
```

R1 has joined four multicast groups on its G0/0 interface: ff02::1 (all nodes), ff02::2 (all routers), as well as two solicited-node multicast addresses; we will cover those soon in chapter 21. If R1 receives a packet addressed to any of those four multicast addresses, it will examine the contents. If it receives a packet addressed to any other multicast address, it will discard the packet; it's not interested in the contents.

### 20.5.5 Anycast addresses

We have covered unicast (one-to-one), broadcast (one-to-all), and multicast (one-to-multiple), but anycast (one-to-one-of-multiple) is a new concept. Anycast was already present in IPv4, but with IPv6, the concept was more formally defined and integrated. Figure 20.11 visually illustrates the concepts of unicast, broadcast, multicast, and anycast.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-439_322_1096_500_327.jpg)
Figure 20.11 Different routing/addressing methodologies. Unicast is one-to-one, broadcast is one-toall, multicast is one-to-multiple, and anycast is one-to-one-of-multiple.

When using anycast, the same IP address is shared by multiple hosts. The address itself is indistinguishable from a unicast address (i.e., a global unicast address); there is no reserved range for anycast addresses.

Normally, unicast addresses should be unique-not shared by multiple hosts. Anycast is an exception to that rule; the same address is configured on multiple hosts. Packets destined for the anycast address will be delivered to one of the hosts configured with the address. Which of those hosts the packets will be delivered to depends on the routers in the path; each router will forward the packets according to the best route in its routing table. As a result, the packets should reach the destination host closest to the source.

Anycast is useful when providing services over the internet. By configuring the same IP address on servers in different global locations, users can access the server closest to their location; this can result in lower latency. Content Delivery Network (CDN) services such as Cloudflare use anycast for this purpose.

Because anycast addresses are identical to unicast addresses, they can be configured like any other unicast address with the ipv6 address command. However, if you add the anycast keyword to the end of the command, the address will be marked as such in the output of some show commands. This doesn't affect the behavior of the router (and is therefore not necessary), but it is helpful for making the intent of the configuration clear for others (and your future self). In the following example, I configure an anycast address and verify with a show command:

```
R1(config)# interface g0/0
R1(config-if) # ipv6 address 2001:db8:0:100::1/64 anycast
R1(config-if) # show ipv6 interface g0/0
GigabitEthernet0/0 is up, line protocol is up
IPv6 is enabled, link-local address is FE80::260:2FFF:FE62:B801
```

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-439_143_316_1975_1264.jpg)

```
No Virtual link-local address(es):
Global unicast address(es):
2001:DB8::1, subnet is 2001:DB8::/64
2001:DB8:0:100::1, subnet is 2001:DB8:0:100::/64 [ANY]
```

20.5.6 Other reserved addresses

Both IPv4 and IPv6 have many other reserved addresses and address ranges. For the CCNA exam, there are two more reserved IPv6 addresses you should be aware of: the unspecified address and the loopback address.

The unspecified IPv6 address is the all-zeros address: 0000:0000:0000:0000:0000:0000 :0000:0000. When abbreviated, it is simply written as "::". This address can be used as a source address when a device doesn't yet know its IPv6 address. Also, IPv6 default routes are configured to destination ::/0-more on that in chapter 21. The IPv4 equivalent of this address is also all zeros: 0.0.0.0.

The loopback IPv6 address is the address immediately after the unspecified address; it's usually written as ::1. Messages sent to this address are looped back to the local device; they are not sent out of any interfaces. This can be used to test the networking software on the local device. The IPv4 equivalent is the reserved 127.0.0.0/8 range.

## Summary

- IPv4 address exhaustion-running out of IPv4 addresses-is a major problem in our era of unprecedented connectivity; IPv6 is the long-term solution.
- IPv4 addresses are 32 bits in length, providing $2{ }^{32}$ (over 4 billion) addresses, but IPv6 addresses are 128 bits in length, providing $2^{128}$ (over 340 undecillion) addresses.
- To convert a binary number to hexadecimal, (1) split the number into 4-bit groups, (2) convert each 4-bit group to decimal, and (3) convert each decimal number to hexadecimal.
- To convert a hexadecimal number to binary, (1) split up the hexadecimal digits, (2) convert each hexadecimal digit to decimal, and (3) convert each decimal digit to binary.
- The IPv6 header is 40 bytes in length and consists of eight fields: Version, Traffic Class, Flow Label, Payload Length, Next Header, Hop Limit, Source, and Destination.
- IPv6 addresses are divided into eight groups of 16 bits (usually called hextets) and written in hexadecimal, with a colon between each group.
- For simplicity, IPv6 addresses typically use /64 prefix lengths-the first 64 bits are the network portion, and the last 64 bits are the host portion.
- IPv6 addresses can be abbreviated by removing leading zeros from each hextet and by abbreviating two or more consecutive all-zero hextets with a double colon (::).

- A network prefix is a combination of a network address and prefix length. If a host has IPv4 address 192.168.1.10/24, the prefix is 192.168.1.0/24. If a host has IPv6 address 2001:db8:1:2:a:b:c:d/64, the prefix is 2001:db8:1:2::/64.
- Many IPv6 configuration and verification commands are similar to IPv4 equivalents: ipv6 address, ipv6 route, show ipv6 interface brief, show ipv6 route.
- When configuring IPv6 on a Cisco router, you should first enable IPv6 routing with the ipv6 unicast-routing command. IPv6 routing is not enabled by default.
- You can manually assign an IPv6 address to an interface with ipv6 address address/prefix-length. Unlike IPv4, IPv6 prefix lengths are configured with slash notation, not dotted-decimal netmasks.
- Even if you configure a full (or partially abbreviated) IPv6 address, IOS will display the fully abbreviated address in the output of show commands.
- If you configure an IPv4 address on an interface that already has an IPv4 address, the new address will overwrite the old one. If you do the same in IPv6, the new address will not overwrite the old one; the interface will have multiple addresses.
- Modified Extended Unique Identifier 64 (Modified EUI-64) is a method of automatically generating a 64-bit interface identifier (host portion of the address) by adding 16 bits (0xfffe) to the interface's 48-bit MAC address.
- Use ipv6 address prefix eui-64 to configure an IPv6 with Modified EUI-64.
- To convert a MAC address to a Modified EUI-64 interface ID, (1) divide the MAC address in half, (2) insert fffe in the middle, and (3) invert the seventh bit.
- IPv6 global unicast addresses are globally unique addresses used for communication over the public internet. The global unicast range was originally defined as $2000:: / 3$, but now includes any address not specifically reserved for other purposes.
- Global unicast addresses consist of a 48-bit global routing prefix (assigned by an RIR or ISP), a 16-bit subnet identifier (used to make subnets), and a 64-bit interface identifier (the host portion of the address).
- The IPv4 equivalent of a global unicast address is a public IPv4 address. Public IPv4 addresses include all addresses that are not part of the reserved private or other special-purpose ranges.
- IPv6 unique local addresses (ULAs) are private addresses; they do not have to be globally unique, and enterprises are free to use them in their internal networks. However, they cannot be used for communication over the internet.
- The ULA range is defined as fc00::/7 but is divided into two / 8 ranges: fc00::/8 (reserved) and $\mathrm{fd} 00: / 8$ (the active ranges for ULAs).

- After the fd prefix, a ULA has a 40-bit global ID, which should be randomly generated to avoid overlap with other enterprises. The next 16 bits are the subnet identifier, and the last 64 bits are the interface identifier.
- ULAs are equivalent to IPv4 private addresses, in the ranges 10.0.0.0/8, 172.16.0.0/12, and 192.168.0.0/16.
- IPv6 link-local addresses (LLAs) are automatically generated on IPv6-enabled interfaces using Modified EUI-64. The LLA range is fe80::/10, but the following 54 bits should be set to 0, resulting in the fe80::/64 prefix.
- The IPv4 equivalent is the 169.254.0.0/16 range. IPv6-enabled interfaces require an IPv6 LLA, but IPv4-enabled interfaces don't require an IPv4 LLA.
- You can enable IPv6 on an interface without configuring an IPv6 address with ipv6 enable. In that case, the interface will have only the automatically generated LLA.
- You can manually configure an interface's LLA with ipv6 address address link-local.
- LLAs can only be used for communication with devices on the same segment.
- IPv6 multicast addresses are from the ff00::/8 range. They are used to provide communication from one host to multiple others. The IPv4 equivalent is the class D range (224.0.0.0-239.255.255.255).
- Several scopes define how far the multicast packets should travel: interface-local (ff01), link-local (ff02), site-local (ff05), organization-local (ff08), and global (ff0e).
- Some common link-local IPv6 and IPv4 multicast addresses are all nodes (ff02::1, 224.0.0.1), all routers (ff02::2, 224.0.0.2), all OSPF routers (ff02::5, 224.0.0.5), all OSPF DRs/BDRs (ff02::6, 224.0.0.6), all RIP routers (ff02::9, 224.0.0.9), and all EIGRP routers (ff02::a, 224.0.0.10).
- Unicast communication is one-to-one, broadcast is one-to-all, multicast is one-to-multiple, and anycast is one-to-one-of-multiple.
- An anycast address is a unicast address configured on multiple hosts. Packets destined for the address will be delivered to the closest host with the address, as determined by routers' routing decisions.
- You can specify an IPv6 address as anycast with the ipv6 address address/ prefix-length anycast command.
- The unspecified IPv6 address is all zeros, usually written as ::. It is used in default routes or when the device doesn't know its IPv6 address yet. The IPv4 equivalent is 0.0.0.0.
- The loopback IPv6 address is ::1. Messages sent to this address are looped back to the local device without being sent out of an interface. This can be used to test the device's network software. The IPv4 equivalent is the 127.0.0.0/8 range.

## IPv6 routing

## This chapter covers

- How IPv6 uses Neighbor Discovery Protocol for address resolution (and more)
- The IPv6 routing table
- How routers forward IPv6 packets
- How to configure IPv6 static routes

In chapter 20, we covered IPv6 addressing, including how to configure IPv6 addresses on Cisco routers and the various IPv6 address types. In this chapter, we'll build upon that knowledge and look at IPv6 routing, including how routers forward IPv6 packets and how to configure IPv6 static routes. Specifically, we will cover the following exam topics:


- 3.1 Interpret the components of routing table
- 3.2 Determine how a router makes a forwarding decision by default
- 3.3 Configure and verify IPv4 and IPv6 static routing

We have already covered these exam topics from an IPv4 perspective, particularly in chapter 9. The good news is that the fundamentals of IPv6 routing are largely identical to those of IPv4 routing, so many of the concepts in this chapter will not be new to you. One point worth mentioning is that IPv6 dynamic routing is not included in the CCNA exam-only static routing; the exam topics list mentions OSPFv2 (which only supports IPv4), not OSPFv3 (which supports both IPv4 and IPv6).

### 21.1 Neighbor Discovery Protocol

When an end host sends an IPv4 packet, or when a router forwards an IPv4 packet, it must encapsulate the packet in an Ethernet frame destined for the packet's next hop; that might be the packet's final destination host, or it might be the next router in the path to the final destination. And how does it learn the MAC address of the next hop? The answer is ARP.

A host's ARP table maps Layer 3 (IPv4) addresses to Layer 2 (MAC) addresses. To encapsulate a packet in a frame with the proper destination MAC address, a host will look in its ARP table for an entry matching the next hop's IPv4 address and use the mapped MAC address as the frame's destination. If no such entry exists, the host will broadcast an ARP request to learn the next hop's MAC address.

NOTE The term host as I use it in this book includes both end hosts, such as servers and PCs, and network infrastructure devices, such as routers.

End hosts sending IPv6 packets and routers forwarding IPv6 packets also need to encapsulate those packets in frames addressed to the next hop. But there's a major difference between IPv4 and IPv6: IPv6 doesn't use ARP! Instead, IPv6 uses a protocol called Neighbor Discovery Protocol (NDP), which plays a similar role to ARP, mapping Layer 3 (IPv6) addresses to Layer 2 (MAC) addresses. However, NDP also has some additional functions, two of which we'll cover in this section: router discovery and duplicate address detection.

### 21.1.1 Solicited-node multicast

Some NDP functions use a particular kind of multicast address called a solicited-node multicast address, which is generated from a host's unicast address (global unicast, unique local, or link-local). To generate a solicited-node multicast address, prepend ff02:0000:0000:0000:0000:0001:ff (abbreviated to ff02::1:ff) to the last six hexadecimal digits of the unicast address. Figure 21.1 shows how to generate a solicited-node multicast address.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-444_293_1286_1553_358.jpg)
Figure 21.1 How to generate a solicited-node multicast address. Prepend ff02:0000:0000:0000:0000: 0001:ff (abbreviated to ff02::1:ff) to the last six hexadecimal digits of the unicast address.

NOTE The scope of a solicited-node multicast address is link-local, as indicated by ff02.

For reference, table 21.1 lists some unicast addresses and their equivalent solicitednode multicast addresses. In sections 21.1.2 and 21.1.4, we'll see some examples of this message type in action.

Table 21.1 Solicited-node multicast addresses
| Unicast address | Solicited-node multicast address |
| :--- | :--- |
| 2001:db8:123::1 | ff02::1:ff00:1 |
| fd12:3456:78:1df:ac80:0:3ab:fff1 | ff02::1:ffab:fff1 |
| fe80::99ff:fe12:1234 | ff02::1:ff12:1234 |


In chapter 20, I showed the following output when demonstrating multicast IPv6 addresses. In addition to joining the ff02::1 and ff02::2 multicast groups, note that R1 has joined two additional multicast groups for the solicited-node multicast address of each of its unicast addresses (global unicast and link-local):

```
R1# show ipv6 interface g0/0
GigabitEthernet0/0 is up, line protocol is up
IPv6 is enabled, link-local address is FE80::2D0:97FF:FE92:B401
No Virtual link-local address(es):
```

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-445_74_301_967_1317.jpg)

```
Global unicast address(es):
2001:DB8:1:1::1, subnet is 2001:DB8:1:1::/64
Joined group address(es):
FF02::1
FF02::2
FF02::1:FF00:1
FF02::1:FF92:B401
```

NOTE Multicast packets, such as those destined for a solicited-node multicast address, are encapsulated in frames destined for an equivalent multicast MAC address. The details of these MAC addresses are beyond the scope of the CCNA exam.

### 21.1.2 Address resolution with NDP

One function of NDP is address resolution-mapping a known Layer 3 address to an unknown Layer 2 address. This is the ARP-like aspect of NDP. Whereas IPv4's ARP uses ARP request and ARP reply messages, NDP uses the following two ICMPv6 messages:

- Neighbor Solicitation (NS)-ICMPv6 type 135
- Neighbor Advertisement (NA)-ICMPv6 type 136

## ICMP and ICMPv6 message types

NDP can be considered a component of ICMPv6-ICMP for IPv6. ICMP and ICMPv6 perform various functions in IPv4 and IPv6 networks, respectively, and they define various message types (each identified by a number) that they use to perform those functions.

Two example ICMP messages that we've covered are Echo Request (type 8) and Echo Reply (type 0), which are used for IPv4 ping messages. ICMPv6 uses its own Echo Request (type 128) and Echo Reply (type 129) messages for the same purpose.

NDP uses five different ICMPv6 messages: Router Solicitation (type 133), Router Advertisement (type 134), Neighbor Solicitation (type 135), Neighbor Advertisement (type 136), and Redirect (type 137). We'll cover the first four of those messages in this section.

Figure 21.2 shows the NDP address resolution process, consisting of an NS message from the requester and an NA message in response.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-446_339_1073_868_350.jpg)
Figure 21.2 An NDP exchange between two routers. R1 sends an NS message to learn R2 G0/0's MAC address, and R2 sends an NA message in response.

ARP request messages are broadcast to all hosts on the local segment. However, as we covered in chapter 20, the concept of "broadcast" doesn't exist in IPv6; there is no broadcast address. This is where the solicited-node multicast address comes into play.

To learn a neighbor's MAC address, a host will send a Neighbor Solicitation (NS) message (equivalent to an ARP request) to the neighbor's solicited-node multicast address. The target of the NS message will then reply with a Neighbor Advertisement (NA) message (equivalent to an ARP reply), informing the requester of its MAC address. Unlike the NS message, the NA message is a simple unicast message from the responder to the requester.

## Overlapping solicited-node multicast addresses

Although very unlikely, it's possible for two hosts to have different IPv6 addresses but identical solicited-node multicast addresses. For example:

- Host $A-2001: d b 8: 1: 1:: 1100: 1234 / 64$
- Host B-2001:db8:1:1::2200:1234/64

(continued)
Although the two addresses are unique, both result in the same solicited-node multicast IPv6 address: ff02::1:ff00:1234. So how can Host A and Host B differentiate between an NS destined for Host A and an NS destined for Host B?

The answer lies within the NS message body itself-the payload of the IPv6 packet. NS messages include a Target Address field in which the unicast IPv6 address of the message's target (i.e., 2001:db8:1:1::1100:1234 for Host A) is specified. By examining this field, the hosts can identify which NS messages are meant for them and which should be ignored.

Just as a host using IPv4 stores L3-L2 address mappings in its ARP table, a host using IPv6 stores L3-L2 address mappings in its IPv6 neighbor table, which you can check with the command show ipv6 neighbors. In the following example, I send a ping from R1 to R2 and then check R1's neighbor table. Notice that in addition to an entry for R2's global unicast address, R1 also has an entry for R2's link-local address, which R1 created automatically (without me pinging that address):

```
        Pings R2’s global unicast address
Entry for R2's link-local address
```

Entry for
Entry for
Entry for
R2's global
R2's global
R2's global
unicast
unicast
unicast
unicast
address
address
address
address

The IPv6 Address column lists each neighbor's IPv6 address, and the Age column indicates how many minutes have passed since each entry was created or refreshed. The Link-Layer Addr column lists each neighbor's MAC address, and the State columns indicate the state of each neighbor; the IPv6 neighbor states are beyond the scope of the CCNA exam (REACH means the neighbor is reachable-that's what you want to see). Finally, the Interface column lists the interface where each neighbor is connected.

### 21.1.3 Router discovery with NDP

In addition to its ARP-like address resolution functionality, NDP also provides router discovery, allowing hosts to automatically discover routers connected to the local network, as well as other characteristics of the local network (such as the network prefix). The following two message types are used for this purpose:

- Router Solicitation (RS)-ICMPv6 type 133
- Router Advertisement (RA)-ICMPv6 type 134

Router Solicitation (RS) messages are used to ask all routers on the local network to identify themselves; these messages are sent to the "all routers" link-local multicast address ff02::2. Router Advertisement (RA) messages are sent in response to RS messages and are also periodically sent by all routers (even without receiving an RS); RA messages are typically sent to the "all nodes" link-local multicast address ff02::1.

NDP's router discovery functionality is used to facilitate Stateless Address Autoconfiguration (SLAAC-pronounced "slack"). SLAAC allows a host to automatically generate its own IPv6 address and also learn other information such as the default gateway.

NOTE SLAAC is stateless because there is no central server keeping track of information such as which host has which address. This is in contrast to Dynamic Host Configuration Protocol (DHCP), in which a server manages address assignments; this is stateful. DHCP is the topic of chapter 4 of volume 2.

Figure 21.3 shows a Cisco router (R2) using SLAAC to generate an IPv6 address for its G0/0 interface; you can configure this with the ipv6 address autoconfig command. R2 then sends an RS message, and R1 replies with an RA message. From this RA message, R2 learns the network prefix of the link (2001:db8::/64) and uses Modified EUI-64 to generate the host portion of its IPv6 address.

NOTE Instead of Modified EUI-64, some operating systems randomly generate the host portion of the address. Cisco IOS, however, uses Modified EUI-64.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-448_630_1300_1305_348.jpg)
Figure 21.3 R2 uses SLAAC to generate an IPv6 address. (1) Issue the ipv6 address autoconfig command. (2) R2 sends an RS message. (3) R1 responds with an RA message. (4) R2 combines the learned prefix with a Modified EUI-64 interface ID to make its IPv6 address.

The ipv6 address prefix eui-64 command (covered in chapter 20) and the ipv6 address autoconfig commands both use Modified EUI-64 to generate the host portion of the address. However, the former command requires you to manually specify the prefix, whereas the latter automatically learns the prefix via NDP RS/RA messages.

NOTE If you use the ipv6 address autoconfig default command on a Cisco router, in addition to generating an IPv6 address, it will insert a default route into its routing table using the link-local address of the responding router as the next hop. Using the example of figure 21.3, R2 would insert a default route with R1 as the next hop.

### 21.1.4 Duplicate Address Detection

NDP's Duplicate Address Detection (DAD) is a feature that checks if an Ipv6 address is unique on the network before a host uses it. Whenever an interface is configured with an IPv6 address, whether it's automatically assigned or manually entered, it uses DAD. Also, any time a host's interface initializes (enters an up/up state), DAD checks all of its IPv6 addresses to make sure none of them are duplicates on the network.

NOTE An IPv6 address is considered tentative until the DAD process completes; it cannot be used for communication until it has been proven unique on the network.

DAD is performed using two NDP messages that we covered previously: Neighbor Solicitation (NS) and Neighbor Advertisement (NA). Figure 21.4 shows how a Cisco router (R2) uses DAD to determine whether its newly configured IPv6 address is unique.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-449_407_1300_1317_318.jpg)
Figure 21.4 R2 uses DAD to verify that its Ipv6 address is unique. (1) Configure R2's address. (2) R2 sends an NS message to its own solicited-node multicast address. (3) R2 waits for a response. (4) R2 doesn't receive a response, so the address is unique.

To perform DAD, a host will send an NS message to its own solicited-node multicast address. If it receives no response after waiting a certain period of time, the host can safely declare that the address is unique; it can now use the address for communication. However, if it receives an NA message in response, it means another device on the
network is already using the same IPv6 address; the address will be marked as a duplicate, and the host will not be able to use it to communicate over the network.

The following example shows what happens if, instead of a unique address, I configure the same IPv6 address as R1 on R2. A warning message is displayed, and the address is marked as [DUP] (duplicate) in the output of show ipv6 interface; R2 cannot use this address:
![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-450_514_1393_447_221.jpg)

The solution for a duplicate address is simply to fix the configuration error: use no to remove the duplicate address, and reconfigure the interface with a unique address.

NOTE DAD is not a standardized feature in IPv4, although some mechanisms can be used to detect duplicate addresses. IPv6 made DAD mandatory as a component of NDP.

### 21.2 The IPv6 routing table

Routers using IPv6 build a routing table-a database of the best route(s) to each destination known by the router. A router can learn routes in multiple ways, such as manual configuration (static routing) and dynamic routing. Additionally, a router automatically learns connected and local routes when you configure IP addresses on its interfaces.

All of this should sound familiar; it's the same as in IPv4. In this section, we will look at the routing table and review some fundamental routing concepts from an IPv6 perspective. Although the concepts are not new to you, understanding them in the context of IPv6 is crucial.

### 21.2.1 Connected and local routes

A connected route is a route to the network an interface is connected to; these routes are automatically added to the routing table for each interface that has an IP address and is in an up/up state. These routes tell the router that if a packet is destined for an IP address in this network, the router should forward the packet directly to the destination host.

A local route is a route to the exact IP address configured on the router's interface; the router automatically adds one of these routes to its routing table for each IP address
that it has. These routes tell the router that if a packet is destined for this IP address, the router should receive the packet for itself-continue to de-encapsulate the message and examine the contents.

To demonstrate IPv6 connected and local routes, let's use the simple network shown in figure 21.5: one router with two connected networks.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-451_383_1231_437_314.jpg)
Figure 21.5 A router with two connected IPv6 networks

NOTE As I noted in chapter 20, don't forget ipv6 unicast-routing! Forgetting this command is a very common mistake when configuring IPv6.

The following example shows the output of show ipv6 interface brief on R1. Notice that in addition to the global unicast address configured on each interface, R1 has two automatically generated (using Modified EUI-64) link-local addresses:

```
R1# show ipv6 interface brief
GigabitEthernet0/0 [up/up]
    FE80::5054:FF:FE06:1B6F
    2001:DB8::1
GigabitEthernet0/1 [up/up]
    FE80::5054:FF:FE09:A180
    2001:DB8:1::1
. . .
```

Now let's take a look at R1's IPv6 routing table with show ipv6 route. The output is very similar to what you're used to from the IPv4 routing table:

```
R1# show ipv6 route
IPv6 Routing Table - default - 5 entries
Codes: C - Connected, L - Local, S - Static, U - Per-user Static route
. . .
C 2001:DB8::/64 [0/0]
    via GigabitEthernet0/0, directly connected
L 2001:DB8::1/128 [0/0]
    via GigabitEthernet0/0, receive
C 2001:DB8:1::/64 [0/0]
    via GigabitEthernet0/1, directly connected
L 2001:DB8:1::1/128 [0/0]
    via GigabitEthernet0/1, receive
```

```
L FFOO::/8 [0/0]
    via NullO, receive
```

A route that discards multicast traffic

R1 added a connected route and a local route for each address configured on its interfaces. A local route is an example of a host route-a route to a single destination IP address (R1's own IP address, in this case). IPv6 host routes use a /128 prefix length. A connected route is an example of a network route-a route to more than one destination IP address. Any IPv6 route with a prefix length of /127 or shorter is a network route.

EXAM TIP Network and host routes are exam topics 3.3.b and 3.3.c, respectively. Make sure you can identify and configure them in both IPv4 and IPv6. We will cover how to configure IPv6 static routes in section 21.3.

However, R1 did not add any connected or local routes for its link-local addresses-this is expected. Traffic to and from link-local addresses doesn't need to be routed in the traditional sense because it never leaves the local link; there's no need for a routing table entry. A link-local address can be used as the next-hop address of a route (as we'll cover in section 21.3.2), but never as the destination of a route.

The final route in R1's routing table is a route to ff00::/8-the multicast address range. This route is inserted automatically by the router. The route specifies Null0, a virtual interface; packets sent to this interface are dropped. Cisco routers don't forward multicast packets by default, so this route ensures that multicast packets are dropped.

NOTE The ff00::/8 route doesn't prevent the router from sending or receiving multicast packets, such as those used in NDP exchanges with its neighbors. It prevents the router from forwarding multicast packets.

### 21.2.2 Route selection

When a router receives a packet, it looks up the packet's destination IP address in its routing table and selects the best route; this is called route selection. The router will forward the packet according to the most specific matching route-the matching route with the longest prefix length. Once again, this is identical to IPv4.

NOTE As covered in chapter 17, the term route selection can also refer to the process of selecting which routes enter the router's routing table (using metric and administrative distance).

Figure 21.6 shows a route selection decision by R1; a packet arrives on one of its interfaces, so R1 has to make a decision about what to do with the packet. Two of R1's routes match the packet's destination IP address, and R1 selects the more specific of the two-the route with the longer prefix length (/128 vs. /64).

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-453_428_1366_183_190.jpg)
Figure 21.6 R1 makes a route selection decision. 1) R1 receives a packet destined for 2001:db8:1::1. 2) R1 performs a routing table lookup and selects the most specific matching route-the local route to 2001:db8:1::1/128. 3) R1 receives the packet for itself.

What happens if there is no route in the routing table that matches the packet's destination IP address? The answer is the same as in IPv4: the router drops the packet.

### 21.3 IPv6 static routing

Connected routes allow a router to forward packets destined for hosts in its connected networks, and local routes allow the router to receive packets destined for itself. However, most routers also need to be able to forward packets to remote destinations- those that aren't directly connected to the router itself. To achieve that, we need to either manually configure routes to those destinations (static routing) or enable a dynamic routing protocol and allow the router to automatically share routing information with other routers.

Although IPv4 dynamic routing is included in the CCNA exam topics, IPv6 dynamic routing is not. IPv6 static routing, however, is a CCNA exam topic, so let's look at how to configure IPv6 static routes on Cisco routers. Figure 21.7 shows the network we'll use for this section; this is the same topology we used when covering IPv4 static routing in chapter 9.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-453_196_1412_1553_194.jpg)
Figure 21.7 An IPv6 network consisting of three routers. Which static routes must be configured to enable communication between PC1 and PC3?

For the goal of enabling communication between PC1 and PC3, each router needs a route to PC1's network (2001:db8:1::/64) and a route to PC3's network (2001:db8:3::/64). R1 has a connected route to 2001:db8:1::/64, and R3 has one to 2001:db8:3::/64, leaving four static routes that we must configure-table 21.2 lists those routes.

Table 21.2 IPv6 static routes required to enable communication between PC1 and PC3
| Router | Required routes | Next hop |
| :--- | :--- | :--- |
| R1 | 2001:db8:3::/64 | 2001:db8:12::2 (R2 GO/0) |
| R2 | 2001:db8:1::/64 | 2001:db8:12::1 (R1 GO/0) |
|  | 2001:db8:3::/64 | 2001:db8:23::2 (R3 GO/0) |
| R3 | 2001:db8:1::/64 | 2001:db8:23::1 (R2 G0/1) |


### 21.3.1 Configuring IPv6 static routes

The command to configure an IPv6 static route is ipv6 route. After specifying the destination network prefix, you can specify the next-hop IP address, the exit interface, or both. In this section, we'll examine each of those configuration methods:

- ipv6 route destination-prefix next-hop
- ipv6 route destination-prefix exit-interface
- ipv6 route destination-prefix exit-interface next-hop

Recursive static routes
A static route that specifies only the next-hop IP address is called a recursive static route. Figure 21.8 shows how to configure four recursive static routes to enable communication between PC1 and PC3.

```
R1(config)# ipv6 route 2001:db8:3::/64 2001:db8:12::2
```

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-454_377_1347_1264_232.jpg)
Figure 21.8 Configuring recursive static routes on R1, R2, and R3 to enable PC1-PC3 communication. Each route's next-hop IP address is specified.

NOTE Like configuring IPv6 addresses on interfaces, IPv6 static routes use slash notation (i.e. /64), not dotted-decimal netmasks.

In the following example, I configure the necessary routes on R2 and check its routing table with show ipv6 route static (to display only static routes):

```
R2(config)# ipv6 route 2001:db8:1::/64 2001:db8:12::1
R2(config)# ipv6 route 2001:db8:3::/64 2001:db8:23::2
```

```
R2(config)# do show ipv6 route static
. . .
S 2001:DB8:1::/64 [1/0]
    via 2001:DB8:12::1
S 2001:DB8:3::/64 [1/0]
    via 2001:DB8:23::2
```

This type of route is called recursive because it requires recursive (repeated) routing table lookups:

1 A lookup to find the IP address of the next hop
2 A lookup to find which interface the next hop is connected to

If R2 receives a packet destined for PC3, the second static route we configured tells R2 which next hop to forward the packet to (2001:db8:23::2). R2 would then need to do a second routing table lookup to determine which interface 2001:db8:23::2 is connected to. In the following example, I use show ipv6 route connected to show R2's connected routes:

```
R2# show ipv6 route connected
. . .
C 2001:DB8:12::/64 [0/0]
    via GigabitEthernet0/0, directly connected
C 2001:DB8:23::/64 [0/0]
    via GigabitEthernet0/1, directly connected
```

R2 has a connected route to 2001:db8:23::/64 on its G0/1 interface; this route matches next hop 2001:db8:23::2. R2 now knows how to forward the PC3-destined packet: it should forward it to next hop 2001:db8:23::2, which is connected to G0/1.

## Directly connected static routes

A static route that specifies only the exit interface-the interface packets should be forwarded out of-is called a directly connected static route. The reason for the name is that this kind of route makes the router believe that it is directly connected to the destination specified in the route. Figure 21.9 shows the same routes we configured previously-this time only specifying each route's exit interface.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-455_379_1417_1597_192.jpg)
Figure 21.9 Configuring fully specified static routes on R1, R2, and R3. Each route's exit interface is specified.

There is one major problem with these routes: they don't work! The routers will accept the commands, and the routes will appear in the routers' routing tables, but communication via these routes will fail.

IPv4 directly connected static routes rely on a mechanism called proxy ARP, in which a router responds to ARP requests on behalf of another host. Cisco routers don't support an equivalent proxy NDP, so these routes won't work as intended. In the following example, I configure R1's route to 2001:db8:3::/64 and check its routing table; at first glance, there doesn't seem to be anything wrong with the route:

```
R1(config)# ipv6 route 2001:db8:3::/64 g0/0
R1(config)# do show ipv6 route static
. . .
```

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-456_90_439_560_1196.jpg)

```
S 2001:DB8:3::/64 [1/0]
    via GigabitEthernet0/0, directly connected
```

The route appears as directly connected.

After looking at R1's routing table, you might assume that it's ready to forward packets between PC1 and PC3; if this were an IPv4 network, it would be. However, in this case, a ping from PC1 to PC3 fails, and R1's IPv6 neighbor table shows why:

```
R1# show ipv6 neighbors
IPv6 Address Age Link-layer Addr State Interface
2001:DB8:3::13 0 - INCMP Gi0/0
. . .
```

PC3's entry is incomplete.

R1's neighbor table shows an INCMP (incomplete) entry for PC3 (2001:db8:3::13). This entry means that R1 sent a neighbor solicitation message out of G0/0 to try to learn PC3's MAC address, but it failed to resolve the address; R2 didn't reply on behalf of PC3. As a result, R1 was unable to encapsulate PC1's pings to PC3 in properly addressed Ethernet frames; R1 had no choice but to drop them.

EXAM TIP If an exam question requires you to configure, verify, or troubleshoot IPv6 static routes, remember that directly connected static routes don't work on Ethernet interfaces, even though they appear as valid routes in the routing table-a potential trick question!

## Serial interfaces

Directly connected IPv6 static routes don't work on Ethernet interfaces (including Fast Ethernet, Gigabit Ethernet, etc.), but they do work on serial interfaces. Serial interfaces are a legacy technology used for point-to-point connections between two devices. Due to their exclusively point-to-point nature, they don't need any Layer 2 addressing to indicate which device frames are destined for; a frame sent by one device must be destined for the other device.
(continued)
As a result, serial interfaces don't use MAC addresses and don't require ARP/NDP address resolution, so directly connected IPv6 (and IPv4) routes work on these connections. Serial interfaces used to be common for Wide Area Network (WAN) connections and were on previous versions of the CCNA exam but were removed from the exam topics list in 2020.

## Fully specified static routes

A fully specified static route specifies both the exit interface and the next hop. Figure 21.10 shows the same four routes we configured in the previous two examples-this time specifying each route's exit interface and next-hop IP address.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-457_435_1411_804_193.jpg)
Figure 21.10 Configuring fully specified static routes on R1, R2, and R3 to enable PC1-PC3 communication. Each route's exit interface and next-hop IP address are specified.

In the following example, I show the configuration of R3's route to 2001:db8:1::/64 and then check its routing table:
![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-457_242_1292_1493_314.jpg)

In practice, you can configure either recursive or fully specified static routes; there is no significant difference in performance between them. Just remember that IPv6 directly connected static routes don't work on Ethernet connections.

### 21.3.2 Link-local next hops

Packets sourced from or destined for link-local addresses are not routable; a router will not forward them. However, a link-local address can be used as the next hop of a route. This may seem counterintuitive-how can an unroutable address be used in a
route? Link-local next hops work because the link-local address is only used to direct the packet to the next immediate destination on the local link, not to route it across multiple hops or network segments.

Figure 21.11 shows the same example network as before, with the same four routes configured to enable communication between PC1 and PC3. This time, I used linklocal next-hop addresses.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-458_438_1406_499_232.jpg)
Figure 21.11 Configuring static routes using link-local next hops on R1, R2, and R3 to enable PC1-PC3 communication

You may have noticed that global unicast addresses are not shown for the R1-R2 and R2-R3 links in figure 21.11. These connections are examples of transit links-links that only serve to carry traffic between different parts of a network. Because these links don't serve as destinations for data, there's often no need to be able to communicate with them from remote networks; link-local addressing is sufficient for them to serve their purpose.

Although all IPv6-enabled interfaces automatically generate a link-local address using Modified EUI-64 rules, I manually configured the routers' link-local addresses to simplify them. In the following example, I configure R2's link-local addresses as fe80::2:

```
R2(config) # interface range g0/0-1
R2(config-if-range) # ipv6 address fe80::2 link-local
```

If configuring the same IP address on two different interfaces seems strange to you, you're right! Because a router's role is to connect different networks, no two interfaces should be in the same subnet, let alone have the exact same IP address. However, link-local addresses are an exception to that rule.

Because link-local addresses are significant and must be unique only within the context of a single network link, the same link-local address can be used on different interfaces as long as they are on separate links. This allows for configurations like the previous example: identical link-local addresses on multiple interfaces of the same router.

All of the static routes shown in figure 21.11 are fully specified; that's for a good reason. All link-local addresses share the same fe80::/64 prefix, and it's possible for the
same IP address to exist on multiple links. For that reason, a route specifying a link-local next-hop IP address without specifying the exit interface is ambiguous; the router has no way to know which interface the correct next hop is connected to. In fact, IOS won't let you configure such a route, as shown in the following example:
![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-459_193_1241_381_316.jpg)

However, fully specified routes with link-local next hops are accepted. In the next example, I configure R2's routes and check its routing table:

```
R2(config)# ipv6 route 2001:db8:1::/64 g0/0 fe80::1
R2(config)# ipv6 route 2001:db8:3::/64 g0/1 fe80::3
```

Fully specified routes with link-local next hops

The routes are accepted and inserted into the routing table.

To summarize, here are three takeaways regarding link-local IP addresses:

- Link-local addresses alone can be sufficient on transit links. Global unicast/ unique local addresses might be unnecessary.
- Link-local addresses have to be unique only in the context of the local link.
- Routes with link-local next hops must be fully specified.

### 21.3.3 Configuring a default route

An IPv6 default route is a route to ::/0, which matches every possible IPv6 address-all $340,282,366,920,938,463,463,374,607,431,768,211,456$ of them. This is equivalent to 0.0.0.0/0 in IPv4. By being the least specific route possible, a default route is used as a catch-all, only selected to forward a packet if no other routes match the packet's destination IP address.

Default routes are most commonly used to provide a route to the internet. The logic is that packets destined for hosts in the enterprise's internal network should have a more specific route in the routing table; if the router receives a packet that doesn't match any internal destination, it's probably destined for the internet. Figure 21.12 shows an example of a default route to the internet.

EXAM TIP Default static routes are exam topic 3.3.a, and you should know how to configure them in both IPv4 and IPv6.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-460_337_1020_186_354.jpg)
Figure 21.12 Configuring a default route to the internet

### 21.3.4 Floating static routes

As in IPv4, specifying an administrative distance (AD) value at the end of an IPv6 static route allows you to create a floating static route-a route that is made less preferable by using a non-default AD value. Figure 21.13 shows an example of a floating static route. Continuing from the example we saw in figure 21.12, R1 is now connected to a second ISP (ISP-B). A floating static route using ISP-B as the next hop provides a backup route to the internet.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-460_379_1069_1045_354.jpg)
Figure 21.13 Configuring default routes to the internet. The route via ISP-A serves as the primary route, and the floating route via ISP-B serves as a backup.

Let's examine the effect of this configuration. In the following example, I configure the two routes on R1 and check its routing table:
![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-460_274_1302_1696_346.jpg)

Static routes have an AD value of 1 by default. By configuring the ISP-B route with an AD value of 2, I made it less preferable than the ISP-A route. As a result, R1 inserts only
the ISP-A route into its routing table. The floating static route via ISP-B should only enter the routing table if the ISP-A route is removed. To test the floating static route, in the following example, I disable R1's G0/0 interface, simulating a hardware failure on the connection:

```
R1(config) # interface g0/0
R1(config) # shutdown
R1(config)# do show ipv6 route static
. . .
S ::/0 [2/0]
via 2001:DB8:1::2
```

As expected, the floating static route entered the routing table only after the ISP-A route was removed. Floating static routes are used as backup routes, whether that is a backup to another static route (as in this example) or another kind of route.

EXAM TIP Floating static routes are exam topic 3.3.d, so make sure you know how to configure them in both IPv4 and IPv6.

## Exam scenarios

Just like IPv4 routing, IPv6 routing is a critical part of the CCNA exam, although when it comes to IPv6, only static routing is covered. Here are a couple of examples of questions that test your understanding of IPv6 static routing:

1 (multiple choice, multiple answers)
You issue the command ipv6 route 2001:db8:1:1::1/128 GigabitEthernet0/1 fe80::2. Which of the following statements are true about the route created by the command? (Select two.)
    A It is a network route.
    B It is a host route.
    c It is a recursive route.
    D It is a directly connected route.
    E It is a fully specified route.

This question tests your understanding of the different IPv6 route types we covered in this chapter. The destination of the route uses a /128 prefix length; it is a route to a single destination IPv6 address, making it a host route. So B is correct. The route specifies both an exit interface and a next-hop IP address, so it is a fully specified route. Therefore, E is the second correct answer.

2 (lab simulation)
A lab simulation might require you to configure IPv6 static routes to enable connectivity between hosts in a network. You could be given a network diagram and be required to configure the appropriate routes on one or more routersperhaps with a floating static route or two thrown in for an additional challenge! Remember that the principles of IPv4 and IPv6 static routes are the same; the only major differences are the first word of the command (ip vs ipv6) and the format of the addresses.

## Summary

- IPv6 uses Neighbor Discovery Protocol for various essential functions like ARP-esque Layer 2 address resolution, router discovery, and duplicate address detection.
- Some NDP functions, such as address resolution and duplicate address detection, use a solicited-node multicast address, which is generated from a unicast address.
- To generate a solicited-node multicast address, prepend ff02:0000:0000:0000:00 00:0001:ff (ff02::1:ff) to the last six hexadecimal digits of the unicast address. For example, 2001:db8::12:3456 results in ff02::1:ff12:3456.
- NDP address resolution uses two ICMPv6 messages: Neighbor Solicitation (NS) and Neighbor Advertisement (NA). These are equivalent to ARP Request and ARP Reply.
- To learn a neighbor's MAC address, a host will send an NS message to the neighbor's solicited-node multicast address. The neighbor will reply with a unicast NA message, informing the sender of its MAC address.
- An IPv6 host stores its L3-L2 address mappings in the IPv6 neighbor table, which can be viewed with show ipv6 neighbors.
- NDP's router discovery function allows hosts to automatically discover routers connected to the local network, as well as other characteristics of the local network (such as the prefix).
- Router discovery uses two message types: Router Solicitation (RS) and Router Advertisement (RA).
- RS messages are sent to the "all routers" multicast address (ff02::2) to ask all routers on the local link to identify themselves. Routers typically send RA messages to the "all nodes" multicast address (ff02::1) in reply.
- NDP router discovery facilitates Stateless Address Autoconfiguration (SLAAC), which allows a host to automatically generate its own IPv6 address.
- A SLAAC-enabled host will send an RS message to discover routers on the local link, and routers will reply with RA messages.
- The host will combine the prefix information learned from the RA with an EUI-64 interface identifier to automatically generate its IPv6 address.
- You can configure a Cisco router to learn its IPv6 address via SLAAC with ipv6 address autoconfig.
- NDP's Duplicate Address Detection (DAD) is a feature that checks if an IPv6 address is unique on the network before a host uses it.
- Whenever an interface is configured with an IPv6 address, the host uses DAD. Likewise, when an IPv6-enabled interface initializes, it uses DAD.
- To perform DAD, a host sends an NS message to its own solicited-node multicast address. If there is no response, the address is determined to be unique. If the host receives an NA message in response, the address is a duplicate.
- If DAD detects a duplicate IPv6 address, the address cannot be used.

- IPv6-enabled routers build a routing table in which they store the best route(s) to each known destination. You can view the IPv6 routing table with show ipv6 route.
- A router will insert a connected route to each network its interfaces are connected to and a local route to the exact IP address of each of its interfaces.
- A local route is an example of a host route-a route to a single destination IP address. IPv6 host routes use a /128 prefix length.
- A connected route is an example of a network route-a route to more than one destination IP address, with a /127 or shorter prefix length.
- Routers forward IPv6 packets according to the most specific matching route in the routing table-the same as IPv4 packets.
- A static route that specifies only the next-hop IP address is called a recursive static route because it requires recursive (repeated) routing table lookups to forward a packet. The command syntax is ipv6 route destination-prefix next-hop.
- A static route that specifies only the exit interface is called a directly connected static route because it makes the router believe that it is directly connected to the destination network. The syntax is ipv6 route destination-prefix exit-interface.
- Directly connected IPv6 static routes don't work on Ethernet interfaces. Directly connected IPv4 static routes rely on proxy ARP to work, but Cisco routers don't support an equivalent proxy NDP.
- A static route that specifies both the exit interface and the next-hop IP address is called a fully specified static route. The syntax is ipv6 route destination-prefix exit-interface next-hop.
- Although packets sourced from and destined for link-local addresses are not routable, link-local addresses can be used as next-hop addresses of routes.
- Transit links are links that only serve to carry traffic between different parts of a network-they often do not need global unicast/unique local addresses. Linklocal addresses are often sufficient.
- Link-local addresses must be unique only within the context of a single link, so multiple interfaces on the same router can share the same link-local address.
- Static routes with link-local next hops must be fully specified; IOS will reject the command otherwise.
- An IPv6 default route is a route to ::/0, which matches every possible IPv6 address. This is equivalent to 0.0.0.0/0 in IPv4.
- Default routes are most commonly used to provide a route to the internet. Packets that don't match any internal destinations are forwarded to the internet.

- An administrative distance (AD) value can be specified at the end of the ipv6 route command to configure a floating static route-a route that is made less preferable by configuring it with a non-default AD.
- Floating static routes act as backup routes, only entering the routing table if a more desirable route (i.e., another static route with the default AD value) is removed.

Licensed to Luke Chen [mic215fa@gmail.com](mailto:mic215fa@gmail.com)

## Part 6

## Layer 4 and IP access control lists

Having covered several key Layer 2 and Layer 3 concepts in previous parts of this book, in part 6, we now move up to Layer 4: the Transport Layer. Whereas Layers 1, 2, and 3 are focused on carrying messages between hosts with the help of the switches and routers that form the network infrastructure, Layer 4 runs on top of those lower layers and is responsible for ensuring that messages are delivered to the correct application in an efficient and reliable manner. Chapter 22 of this book delves into the two primary protocols that operate at this layer-TCP and UDP-comparing and contrasting their features, benefits, and drawbacks.

Chapters 23 and 24 move our focus to IP access control lists (ACLs), an essential tool for network security, controlling the flow of traffic by selectively permitting and denying packets. Standard ACLs, discussed in chapter 23, enable traffic filtering based on source IP addresses, providing a basic level of security and traffic control. Chapter 24 covers extended ACLs, providing a chance to apply the Layer 4 knowledge acquired in chapter 22; extended ACLs enable traffic filtering based not only on source and destination IP addresses but also Layer 4 TCP/UDP port numbers and other parameters.

Part 6 is a critical section that bridges the gap between the lower-level networking functions of Layers 1, 2, and 3 and the higher-level applications they support. TCP, UDP, and standard/extended ACLs have wide applications in networking and are critical CCNA exam topics; they will come up several times in volume 2 of this book. So let's get started!

Licensed to Luke Chen [mic215fa@gmail.com](mailto:mic215fa@gmail.com)

## Transmission Control Protocol and User Datagram Protocol

## This chapter covers

- How the Transmission Control Protocol and User Datagram Protocol provide Layer 4 addressing and session multiplexing
- How TCP provides features like reliable communication and flow control
- Comparing TCP and UDP, and the situations in which each is preferred

In chapter 3 of this book, we covered physical cables, connectors, and ports-Layer 1 of the TCP/IP model. In other chapters, we covered the Data Link Layer (Layer 2): MAC addresses, frame switching, VLANs, STP, and other related topics. We have also spent many pages on the Network Layer (Layer 3): IPv4 and IPv6 addressing, subnetting, packet forwarding, static and dynamic routing, first-hop redundancy protocols, etc. In this chapter, we will venture beyond those first three layers and take a look at Layer 4 of the TCP/IP model: the Transport Layer. Specifically, we will examine the two major Layer 4 protocols: Transmission Control Protocol (TCP) and User Datagram Protocol (UDP), which are exam topic 1.5: Compare TCP to UDP.

### 22.1 The role of Layer 4

In chapter 4 of this book, we took a high-level look at the role of each layer of the TCP/ IP model. For review, figure 22.1 summarizes the role of each layer.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-469_563_1372_367_192.jpg)
Figure 22.1 A web browser on PC1 uses a Layer-7 protocol (HTTPS) to request a web page from the web server on SRV1. Layers 2, 3, and 4 work together to deliver the message to the appropriate application on SRV1. Layer 1 provides the medium over which the communication occurs.

NOTE Hypertext Transfer Protocol Secure (HTTPS) is an Application Layer protocol often used for transmitting web pages over a network, such as the internet.

Layer 1 encompasses the physical components required for communication: the cables connecting the devices and the signals that travel over them. Layer 2 deals with hop-to-hop communication between intermediate nodes in the path to the destination. Switches use Layer 2 information (MAC addresses) to make forwarding decisions, and end hosts and routers must encapsulate packets in frames destined for the MAC address of the next hop.

Layer 3 provides end-to-end addressing from the source host to the destination host, and routers use Layer 3 information (IP addresses) to make forwarding decisions, ensuring packets reach their correct destinations. However, it's not enough for the data to reach the correct destination host; the data has to reach the correct application process on the destination host, and that's one of the major roles of Layer 4. However, Layer 4 can also provide many other services that we will examine when looking at the specifics of TCP and UDP in section 22.2.

In this chapter, we will focus primarily on Layer 4 communication between hosts, but don't forget that Layer 4 operates on top of Layers 1, 2, and 3; the lower layers are necessary to deliver Layer 4 segments between the communicating hosts. Likewise, keep in mind that Layer 4's purpose is to provide services to the Application Layer protocols that run above Layer 4, although we won't focus on the specifics of those protocols in this chapter.

### 22.1.1 Port numbers

Like Ethernet and IP, Layer 4 protocols such as TCP and UDP use their own system of addressing, called ports. The term port, in this case, does not refer to a physical port on a device; a Layer 4 port is a number ranging from 0 to 65535 that is used to address a message to a specific application process on the destination host.

Figure 22.2 shows how PC1 addresses data to a specific application service on SRV1. Data prepared by the Application Layer is encapsulated in a TCP header, making a TCP segment. The segment is sourced from a random port (more on this later) and destined for port 443; this addresses the message to the HTTPS service running on SRV1. This segment is encapsulated in an IP header destined for SRV1's IP address. This packet is then encapsulated in a new Ethernet header/trailer at each hop in the path from PC1 to SRV1-a process we've covered a few times in this book.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-470_556_1412_773_225.jpg)
Figure 22.2 PC1 sends a TCP segment to SRV1. The segment is sourced from port 50000 on PC1 and destined for port 443 on SRV1. SRV1's reply reverses the ports: it is sourced from port 443 on SRV1 and destined for port 50000 on PC1.

When SRV1 replies to PC1, SRV1 reverses the port numbers; SRV1's TCP segment to PC1 is sourced from port 443 and destined for port 50000. This is similar to how the IP addresses are reversed; the reply is sourced from SRV1's IP address and destined for PC1's IP address.

Some Application Layer protocols use TCP as their Layer 4 protocol, and some use UDP; as we'll cover in section 22.2, TCP and UDP have different characteristics that make them suitable for different kinds of Application Layer protocols. In addition to using a specific Layer 4 protocol, each Application Layer protocol also uses a different port number. As we saw in figure 22.2, HTTPS uses TCP as its Layer 4 protocol, with its port number being 443. To address a message to the web server running on SRV1 (which uses HTTPS), PC1 must send the message in a TCP segment destined for port 443.

NOTE Although TCP and UDP are the most common Layer 4 protocols, they are not the only ones. However, for the purpose of the CCNA exam, you just need to know TCP and UDP.

A server running a particular application service is said to "listen on" the relevant port, meaning it waits for incoming messages addressed to the specific TCP or UDP port number. For example, a web server using HTTPS listens on TCP port 443. Table 22.1 lists the Layer 4 protocols and port numbers of some common Application Layer protocols. Note that some protocols can use both TCP and UDP, depending on what kind of communication is required. For example, Domain Name System (DNS)-used to translate names like google.com to IP addresses-uses either TCP or UDP; we'll cover DNS in chapter 3 of volume 2 of this book.

Table 22.1 Port numbers of common Application Layer protocols
| TCP |  | UDP |  |
| :--- | :--- | :--- | :--- |
| Application Layer Protocol | Port | Application Layer Protocol | Port |
| FTP (File Transfer Protocol) data | 20 | DNS (Domain Name System) | 53 |
| FTP control | 21 | DHCP (Dynamic Host Configuration Protocol) server | 67 |
| SSH (Secure Shell) | 22 | DHCP client | 68 |
| Telnet | 23 | TFTP (Trivial File Transfer Protocol) | 69 |
| SMTP (Simple Mail Transfer Protocol) | 25 | NTP (Network Time Protocol) | 123 |
| DNS (Domain Name System) | 53 | SNMP (Simple Network Management Protocol) agent | 161 |
| HTTP (Hypertext Transfer Protocol) | 80 | SNMP manager | 162 |
| POP3 (Post Office Protocol version 3) | 110 | Syslog | 514 |
| IMAP (Internet Message Access Protocol) | 143 |  |  |
| HTTPS (HTTP Secure) | 443 |  |  |


EXAM TIP Make sure you know the Layer 4 protocol (TCP/UDP) and port number of each Application Layer protocol we cover in this book. Table 22.1 would be a good reference for flashcards to help you memorize the port numbers for each protocol. Several chapters in volume 2 of this book will also give you a chance to review most of these protocols as we cover them in more detail.

Port numbers are assigned by an organization called the Internet Assigned Numbers Authority (IANA, pronounced as "eye-AN-uh"). IANA divides port numbers into three ranges, each with its own purpose. Well-known ports (0-1023) are reserved for the most common protocols; all of the port numbers in table 22.1 are from this range. Registered ports (1024-49151) are not as strictly controlled as well-known ports, but an enterprise
may register its protocol with IANA to use a port number in this range to avoid conflicts; different protocols should not use the same port number.

The remaining ports are ephemeral ports (49152-65535). These ports are not controlled by IANA, so registration is not required to use them. They are most commonly used by client devices as the source ports for connections. For example, in the example we saw in figure 22.2, a client (PC1) used port 50000 (an ephemeral port) as the source port of its message to a server (SRV1). The following is a summary of the three port ranges:

- Well-known ports-0-1023
    - Reserved for use by the most common protocols.
    - Controlled and assigned by IANA.
    - Also called system ports.
- Registered ports-1024-49151
    - Protocols may be registered with IANA to use a port number in this range.
    - Registration is done to avoid conflicts, so that two different protocols don't use the same port number.
    - Also called user ports.
- Ephemeral ports-49152-65535
    - Not controlled or assigned by IANA.
    - Dynamically selected by client devices as the source port for connections.
    - Also called dynamic or private ports.

### 22.1.2 Session multiplexing

Imagine you have three web browsers open on your PC: Chrome, Firefox, and Edge. In Chrome, you visit wikipedia.org and click around, viewing a few articles. Then you do the same on Firefox and Edge. Each browser on your PC is a client using HTTPS to interact with a Wikipedia web server, and each of these interactions is called a session- an exchange between two communicating devices. How does your PC keep track of these sessions? Why does a link clicked in Chrome not open in Firefox?

When a client, like one of your web browsers, initiates communication with a server, it selects a random number from the ephemeral port range (49152-65535) to use as the source port for that particular session. This port is used to uniquely identify the session for the duration of its communication with the server. This process of keeping track of each communication session is called session multiplexing.

This is why when you click a link in Chrome, the resulting page doesn't appear in Firefox-each browser selects a unique port associated with its connection to the server. Figure 22.3 shows how these ephemeral ports are used to keep track of each session between your PC and the Wikipedia server.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-473_628_1410_181_192.jpg)
Figure 22.3 Session multiplexing is achieved through port numbers. PC1 selects a unique source port for each communication session with SRV1, which addresses each response to the same port from which the corresponding request originated.

## Sockets and sessions

The combination of an IP address, a port number, and Layer 4 protocol is called a socket. A web server with IP address 203.0.113.1 that listens on TCP port 443 has the following socket: 203.0.113.1, TCP port 443. When a client wants to connect to this server, it opens its own socket. Let's say a client with IP address 192.168.1.1 uses port 50000 for this purpose-its socket is 192.168.1.1, TCP port 50000.

A session can be thought of as a combination of the two sockets. In our example, that means the following two sockets:

- Client socket-192.168.1.1, TCP port 50000
- Server socket-203.0.113.1, TCP port 443

This pair of sockets is sometimes called a five-tuple, because it consists of five elements: the client IP, the client port number, the server IP, the server port number, and the Layer 4 protocol (TCP or UDP). You may or may not encounter the terms socket and five-tuple on the CCNA exam, but they are terms commonly used in networking.

### 22.2 TCP and UDP

As mentioned previously, there are two main Layer 4 protocols in use today: TCP and UDP. Because exam topic 1.5 states that you must be able to "compare TCP to UDP," it's important that you understand the characteristics of these two protocols.

### 22.2.1 Transmission Control Protocol

Transmission Control Protocol (TCP) is a Layer 4 protocol that, in addition to providing addressing and session multiplexing, offers a variety of benefits, such as connection-oriented communication, data sequencing, reliable communication via acknowledgment and retransmission, and flow control; we will cover these benefits in this section.

Figure 22.4 shows the format of the TCP header. Although I will refer to some of these fields as we cover TCP's features, don't worry about memorizing the header; I include it here only for reference.

|  | Byte | 0 |  |  |  |  |  |  |  |  | 1 |  |  |  |  |  |  |  |  | 2 |  |  |  |  |  |  |  |  | 3 |  |  |  |  |  |  |  |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Byte | Bit | 0 | 1 | 2 | 3 | 4 | 5 | 6 |  | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 |  | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 |  | 24 | 25 | 26 | 27 | 28 | 29 | 30 | 31 |
| 0 | 0 | Source Port |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | Destination Port |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| 4 | 32 | Sequence Number |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| 8 | 64 | Acknowledgment Number |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| 12 | 96 | Data Offset |  |  |  | Reserved |  |  |  |  | Flags (ie., SYN, ACK, FIN) |  |  |  |  |  |  |  |  | Window Size |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| 16 | 128 | Checksum |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | Urgent Point |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| 20 | 160 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| ⋮ | ⋮ |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| 56 | 448 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |

Figure 22.4 The format of the TCP header. Like the IPv4 header, it is a minimum of 20 bytes in size but can be up to $\mathbf{6 0}$ bytes if Options are included.

NOTE The Source Port and Destination Port fields are each 16 bits in length. That's why there are 65,536 port numbers (from 0-65535) in total: $2^{16}=65,536$.

## TCP connections

TCP is a connection-oriented protocol. That means that, before exchanging data, hosts must first establish a connection. Furthermore, after the necessary data has been exchanged, the hosts will terminate the connection. Therefore, exchanging data via TCP is a three-step process:

1 Connection establishment
2 Data exchange
3 Connection termination

The Flags field of the TCP header is 8 bits in length, meaning there are eight different flags, each with a different function. A flag is an individual bit that indicates or controls a particular behavior in a protocol. To activate ("set") a flag, the sending host will set
the flag's binary value to 1. TCP connections are established using two flags in the Flags field of the TCP header: SYN (synchronize) and ACK (acknowledge). The process is called the three-way handshake, because it involves three messages, as shown in figure 22.5.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-475_206_1273_394_318.jpg)
Figure 22.5 The TCP three-way handshake. PC1 initiates the connection by sending a TCP segment with the SYN flag, SRV1 responds with a segment with the SYN and ACK flags, and finally, PC1 sends a segment with the ACK flag.

NOTE The three segments sent during TCP connection establishment are empty-there is no encapsulated data. Their only purpose is to establish the connection, not to exchange data.

After the TCP connection has been established, the data exchange can take place. For example, perhaps the client requests a file from the server, which the server then transfers to the client. After each device is done sending data, it will send a TCP segment with the FIN (finish) bit set, and the hosts will acknowledge each other's FIN segments by sending a segment with the ACK bit set. This usually (but not always) occurs in four separate messages and thus is often called the four-way handshake (in contrast with the three-way handshake used for connection establishment). Figure 22.6 shows the TCP connection termination process after connection establishment and data exchange.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-475_339_1284_1382_318.jpg)
Figure 22.6 TCP connection termination. After the connection establishment and data exchange, PC1 sends a segment with the FIN flag to terminate the connection. SRV1 replies with an ACK segment and then sends its own FIN segment, to which PC1 replies with an ACK segment.

EXAM TIP Remember the TCP connection establishment (SYN, SYN-ACK, ACK) and termination (FIN, ACK, FIN, ACK) sequences for the exam.

Unlike TCP connection establishment, which always occurs in three sequential messages, connection termination doesn't always occur as stated previously. For example, in some cases, the server might send a single segment with both the ACK and FIN flags set, resulting in the sequence FIN, ACK-FIN, ACK. However, the details of this are beyond the scope of the CCNA exam, so I recommend learning the typical four-way handshake for now.

## Data sequencing and acknowledgment

Another feature of TCP is data sequencing, meaning that even if segments arrive at their destination out of order, TCP provides the means to rearrange them in the correct order. This is achieved using the Sequence Number field of the TCP header. Furthermore, each segment is acknowledged by the receiver using the Acknowledgment Number field, providing reliable communication; TCP confirms that each segment is received. Figure 22.7 shows this process.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-476_323_1302_814_350.jpg)
Figure 22.7 TCP data sequencing and acknowledgment. During connection establishment, each host sets a random initial sequence number. Each segment is acknowledged by the receiver by indicating the sequence number of the next segment it expects to receive.

During the TCP connection establishment process, each host sets a random initial sequence number that it increments as it sends data to the other host. In figure 22.7's example, PC1 selected 10 as its initial sequence number, and SRV1 selected 50.

To acknowledge receipt of a particular segment, the receiving host will set its next segment's acknowledgment number to the sequence number of the next segment it expects to receive (not the sequence number of the segment it just received). For example, after SRV1 receives PC1's segment with sequence number 10, SRV1's reply uses acknowledgment number 11. Likewise, after PC1 receives SRV1's segment with sequence number 50, PC1 acknowledges its receipt by using acknowledgment number 51.

The exchange in figure 22.7 shows PC1 and SRV1 sending and receiving segments in turn, one segment at a time. Before sending a new segment, each host waits for its previously sent segment to be acknowledged. However, for the sake of efficiency, it's possible for multiple segments to be acknowledged with a single segment. Figure 22.8 shows an example: PC1 and SRV1 send each other two segments at a time before waiting for acknowledgment.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-477_244_1273_183_318.jpg)
Figure 22.8 PC1 and SRV1 send each other two segments at a time before waiting for acknowledgment. Segments sent consecutively without receiving a new segment from the other host have identical acknowledgment numbers.

TCP connections operate bidirectionally, with each host maintaining its own sequence and acknowledgment numbers. The incrementation of these numbers is determined by the communication dynamics between the hosts. In scenarios where one host predominantly sends data while the other predominantly receives-such as a file transfer from one host to the other-only the sender's sequence number and the receiver's acknowledgment number will consistently increment. This reflects the flow of data from the sender to the receiver and the receiver's acknowledgment of that data.

NOTE In these examples, I use simple sequence and acknowledgment numbers, but real TCP sequence and acknowledgment numbers can be very large (on the scale of billions). Also, note that sequence and acknowledgment numbers are actually measures of bytes sent and received, not values that are incremented by 1 with each segment. For the purpose of the CCNA, just understand the concepts-don't worry about the exact numbers.

## Retransmission

We just covered how TCP acknowledges receipt of each segment, but how does this provide the "reliable communication" that I mentioned previously? What happens if the sender does not receive acknowledgment for a segment that it sent? If the sender of a segment does not receive an acknowledgment within a certain period of time (called the retransmission timeout), it will retransmit the segment, as shown in figure 22.9.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-477_219_1271_1591_320.jpg)
Figure 22.9 TCP retransmission. PC1's first segment with sequence number 26 does not reach SRV1. Because PC1 doesn't receive an acknowledgment for the segment, it retransmits the segment, which is then received and acknowledged by SRV1.

NOTE For simplicity's sake, figure 22.9 focuses on the data transfer from PC1 to SRV1, showing only PC1's sequence numbers and SRV1's acknowledgment numbers.

Another event that could trigger a retransmission is a host receiving a segment with a faulty checksum. Similar to Ethernet's Frame Check Sequence field, which checks for errors in a frame, and IPv4's Header Checksum field, which checks for errors in the IPv4 header, TCP uses its Checksum field to check for errors in segments.

When a device constructs a TCP segment, it uses an algorithm to calculate a checksum, which is included in the TCP header's Checksum field. The host that receives the segment will calculate its own checksum value from the segment. If the two values don't match, it means that the data has been corrupted in transit. As a result, the receiver discards the segment and does not send an acknowledgment, which triggers a retransmission from the sender.

## Flow control

The final TCP feature we will cover is flow control, a mechanism that prevents a sender from overwhelming a receiver by sending too much data too quickly. It ensures that the sender only sends as much data as the receiver is able to handle at a given time. This is critical in situations where the sender can transmit data faster than the receiver can process it.

The appropriate data transfer rate depends on multiple things, such as the speed of the receiver's network connection, its processing capability, and the current load on both. If the sender sends data too quickly, either the receiver or the network it is connected to could be overloaded, resulting in dropped segments (and lots of retransmissions-bad for performance!). Note that the appropriate rate is not fixed; it can vary within a session, and TCP provides the ability to adjust depending on the current conditions.

Flow control is implemented in TCP primarily through the Window Size field of the TCP header, through which a receiver tells a sender how much data the sender should send before waiting for an acknowledgment. Figure 22.10 demonstrates the concept: PC1 (the receiver) uses the Window Size field of its segments to tell SRV1 how much data to send before waiting for the next acknowledgment.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-478_240_1271_1656_350.jpg)
Figure 22.10 TCP flow control is achieved through the Window Size field of the TCP header. In its segments to SRV1, PC1 specifies how much data SRV1 should send before waiting for the next acknowledgment. This can be adjusted dynamically to find an appropriate rate.

NOTE Figure 22.10 focuses only on the data transfer from SRV1 to PC1. Keep in mind that each host specifies its own window size, although SRV1's specified window size isn't shown. Each host specifies its window size in every single TCP segment it sends.

In the first message from PC1 to SRV1 shown in figure 22.10, PC1 specifies a window size of 100 bytes. SRV1 then sends a segment with 100 bytes of encapsulated data and waits for acknowledgment from PC1. In its segment acknowledging receipt of SRV1's segment, PC1 specifies a greater window size: 200 bytes. SRV1 then sends 200 bytes of data (in two TCP segments) before waiting for PC1's acknowledgment. In the final message shown, PC1 specifies a window size of 300 bytes.

NOTE Because TCP is able to dynamically adjust the window size to find an appropriate rate, it is said that TCP uses a sliding window.

### 22.2.2 User Datagram Protocol

Just like TCP, the User Datagram Protocol (UDP) provides Layer 4 addressing and session multiplexing via port numbers. However, when compared to TCP, the defining characteristics of UDP stem more from what it doesn't provide rather than what it does:

- UDP is not connection oriented. Hosts do not establish a connection before communication; the sending host simply sends the data.
- UDP does not provide reliable communication. UDP does not provide a mechanism for acknowledging received messages or retransmitting lost messages.
- UDP does not provide data sequencing. There is no sequence number in the UDP header. If messages arrive out of order, UDP provides no mechanism to put them back in order.
- UDP does not provide flow control; it has no mechanism like TCP's window size to control the flow of data. UDP simply sends data as quickly as it can.

Figure 22.11 shows the UDP header. Whereas the TCP header is $20-60$ bytes in size, the UDP header is very simple and lightweight-only 8 bytes in size.

|  | Byte | 0 |  |  |  |  |  |  |  |  | 1 |  |  |  |  |  |  |  | 2 |  |  |  |  |  |  |  |  | 3 |  |  |  |  |  |  |  |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Byte | Bit | 0 | 1 | 2 | 3 | 4 | 5 | 6 |  | 7 | 8 | 9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 | 21 | 22 | 23 |  | 24 | 25 | 26 | 27 | 28 |  | 30 | 31 |
| 0 | 0 |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | Destination Port |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |
| 4 | 32 | Length |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  | Checksum |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |  |

Figure 22.11 The UDP header. Compared to the TCP header, the UDP header is very lightweight, being only 8 bytes in size.

NOTE Because UDP has a checksum field, it is able to provide error detection. However, unlike TCP, there is no mechanism for retransmission, so messages with errors are discarded and forgotten by UDP.

Defining UDP by what it doesn't provide may make UDP seem inferior to TCP, but that is not the case. There are many instances where UDP is preferred, and we will cover them in section 22.2.3.

## Datagrams and data streams

A datagram (the D in UDP) is a basic unit of transfer that is self-contained and independent of other messages. In the case of UDP, "datagram" is in the name because UDP uses a datagram model for transmission, which means it sends data in discrete chunks (datagrams) that are independent of each other.

This is in contrast to TCP, which treats data as a continuous stream. It breaks this stream up into manageable chunks (segments) for transmission, but the boundaries of these segments have no correlation with the structure of the data itself.

As an analogy, you can think of the data that must be sent as a chapter of a book. UDP might send the data one sentence (or paragraph-whatever fits into a single datagram) at a time; each is a coherent unit. TCP, on the other hand, treats the chapter (the data) as a continuous stream, and the boundary between segments might be in the middle of a paragraph, a sentence, or even a word. Although a half sentence doesn't make sense on its own, TCP provides the mechanisms to ensure that all of the data is received and is in the correct order.

### 22.2.3 Comparing TCP and UDP

Although all of TCP's features may seem great, they come at the cost of additional overhead-the additional data and processing required to manage and ensure the transmission of the actual user data inside each segment. TCP's overhead comes in three main forms:

- Data overhead-The extra bytes of the TCP header (20-60 bytes versus UDP's 8) means that the ratio of encapsulated data to headers is reduced. Headers are necessary for the protocols to function but do not carry actual user data.
- Processing overhead-This refers to the additional computing resources required to implement the protocol. TCP, with its features such as connection establishment, reliable communication, and flow control, requires more processing power and memory than UDP, which provides none of these features.
- Time overhead-Establishing TCP connections introduces additional latency; unlike UDP, in which a host immediately sends data, hosts using TCP must first perform the three-way handshake to establish a connection. Furthermore, the need to wait for acknowledgments of transmitted data can slow down transmission.

EXAM TIP The material in this section is especially important for the CCNA exam since it directly addresses exam topic 1.5: Compare TCP to UDP.

Table 22.2 summarizes the differences between TCP and UDP. We will examine these differences in greater detail as we cover the situations in which each protocol is preferred.

Table 22.2 Comparing TCP and UDP
| TCP | UDP |
| :--- | :--- |
| Connection-oriented | Not connection-oriented |
| Data sequencing | No data sequencing |
| Reliable | Unreliable |
| Flow control | No flow control |
| More overhead | Less overhead |
| Preferred in situations where data integrity is critical and delivery of all segments is required | Preferred in situations where speed or efficiency is a priority and some packet loss is acceptable |


NOTE Although we are discussing Layer 4, where the term segment (TCP) or datagram (UDP) is more accurate, the term packet (which includes the Layer 3 header) is often used more broadly. For example, packet loss refers to the loss or discard of messages in a network during transmission.

## QUIC

Although not a CCNA exam topic, there is a third Layer 4 protocol that is gaining in popularity: QUIC, which was originally developed by engineers at Google. QUIC is officially not an acronym; it's just the protocol's name (although it originally stood for Quick UDP Internet Connections). QUIC is built on top of UDP but provides TCP-like functionality with reduced latency and built-in encryption for secure connections. QUIC is now used for the majority of connections from Chrome to Google web servers and is currently used by roughly 7\% of all websites.

## Scenarios where TCP is preferred

TCP, with its various features, is preferred in situations where data integrity is critical and the delivery of all segments is required. For example, TCP is usually the preferred choice for file transfers. When downloading a file, minor latency due to connection establishment, retransmissions, and other TCP features is acceptable; it's more important that the entire file arrives intact, with all of its bytes in the correct order.

Web browsing is another scenario where TCP is typically preferred. When a user accesses a website, the underlying HTTP or HTTPS protocols use TCP to ensure that all of the web content (text, images, etc.) is reliably delivered and displayed in the correct
order. Similarly, email protocols like SMTP, POP3, and IMAP use TCP to ensure that emails are not lost in transmission and that they arrive correctly at their destinations.

Scenarios where UDP is preferred
UDP, simple and lightweight, is preferred in several situations, such as these:

- Real-time applications
- Simple query/response protocols
- When reliability is provided by other means

Real-time applications like video streaming, online gaming, and voice/video calling (i.e., Zoom) typically use UDP. These applications need fast data transmission and can tolerate some loss of data; the most recent data is the most valuable, and lost data quickly becomes irrelevant. For example, in a video call, if some packets are lost, it's usually better to have minor temporary visual or audio degradation rather than a delay while waiting for the missing packets to be retransmitted.

UDP is also often preferred for simple query-response protocols, such as DNS. Figure 22.12 shows an example: PC1 uses DNS to learn the IP address of google.com before it is able to use HTTPS to access the website.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-482_552_1250_1022_348.jpg)
Figure 22.12 Simple DNS exchanges like the one in steps (1) and (2) use UDP. Having learned google .com's IP address, in step 3, PC1 uses HTTPS to access the website.

A protocol like DNS, which usually involves short requests (queries) and responses, does not require the various features of TCP. The overhead of setting up a TCP connection isn't worth it for a single, small request and response; it would only delay the exchange by introducing latency.

NOTE As mentioned previously, DNS uses TCP in some cases, although we will not cover those cases in this chapter.

A third situation where UDP may be preferred is when reliability is provided by other means. Although UDP itself doesn't provide reliability, the underlying Application Layer protocol might; Trivial File Transfer Protocol (TFTP) is one example. Although I previously stated that TCP is usually preferred for file transfers, the TFTP protocol-which, as the name suggests, is used for transferring files-has its own built-in reliability mechanism. When using TFTP, the receiver must acknowledge each TFTP message it receives. For that reason, TFTP does not need to rely on TCP to provide reliable delivery of messages; it uses UDP instead. We will cover TFTP in chapter 8 of volume 2 of this book.

Although voice/video calling applications fall into the category of "real-time applications," you could say that they have their own built-in reliability too. If you are on a call and packet loss causes a short disruption, you can ask the speaker to repeat what they said. This is reliability at Layer 8-the User Layer!

## Layer 8

Layer 8 isn't an officially recognized layer of the OSI or TCP/IP models. It's just a term used humorously or ironically in networking and other IT fields to acknowledge that, beyond the technical issues handled within the layers of the OSI and TCP/IP models, there are often factors like user error, organizational politics, and other human factors that can impact network performance, security, and other aspects of a system.

## Summary

- Layer 4 (Transport) operates on top of Layers 1, 2, and 3, which are responsible for delivering Layer 4 segments between communicating hosts. Likewise, Layer 4 provides services to the Application Layer protocols that run above it.
- The most common Layer 4 protocols are Transmission Control Protocol (TCP) and User Datagram Protocol (UDP).
- TCP and UDP use their own system of addressing called ports. A port is a number ranging from 0 to 65535 that is used to address a message to a specific application process on the destination host.
- Application Layer protocols run on top of either TCP or UDP (or both in some cases). Each also listens on a specific port number. For example, HTTPS listens on TCP port 443.
- Port numbers are assigned by the Internet Assigned Numbers Authority (IANA). IANA divides port numbers into three ranges: well-known, registered, and ephemeral.
- Well-known ports (0-1023) are reserved for the most common protocols and are strictly controlled and assigned by IANA.
- Registered ports (1024-49151) are not as strictly controlled as well-known ports but can be registered for use with a specific protocol.

- Ephemeral ports (49152-65535) are not controlled by IANA, and registration is not required to use them. They are most commonly used by client devices as the source ports for connections.
- Port numbers also allow for session multiplexing, which allows hosts to keep track of communication sessions by associating them with their unique source and destination IP addresses and port numbers.
- In addition to addressing and session multiplexing, TCP provides benefits like connection-oriented communication, data sequencing, reliable communication, and flow control.
- Exchanging data via TCP is a three-step process: (1) connection establishment, (2) data exchange, (3) connection termination.
- TCP connection establishment uses two flags in the TCP header: SYN and ACK. It involves three messages (SYN, SYN-ACK, ACK) and is often called the three-way handshake.
- TCP connection termination uses FIN and ACK flags, usually in a four-message exchange that is sometimes called the four-way handshake: FIN, ACK, FIN, ACK.
- TCP provides data sequencing and reliable communication using sequence and acknowledgment numbers. Each TCP segment that a host sends must be acknowledged by the receiver.
- Each host randomly selects an initial sequence number in the three-way handshake.
- A host acknowledges receipt of one or more segments by sending a segment with the acknowledgment number set to the sequence number of the next segment it expects to receive.
- If the sender of a segment does not receive an acknowledgment within a certain period of time (the retransmission timeout), it will retransmit the segment. This provides reliable delivery of segments.
- Flow control is a mechanism that prevents a sender from overwhelming a receiver by sending too much data too fast. It is implemented in TCP through the Window Size field, which specifies how many bytes the sender should send before waiting for an acknowledgment.
- Each host in a TCP exchange specifies its own window size, which it adjusts throughout the exchange. For this reason, it is said that TCP uses a sliding window.
- Like TCP, UDP provides Layer 4 addressing and session multiplexing via port numbers. However, UDP is not connection oriented and does not provide reliable communication, data sequencing, or flow control.
- Although TCP provides more features than UDP, they come at the cost of additional overhead-the additional data and processing required to manage and ensure the transmission of the actual user data in each segment.
- TCP is preferred in situations where data integrity is critical and the delivery of all segments is required, such as file transfers, web browsing, and email.

- For example, in file transfers, minor latency due to connection establishment, retransmissions, and other TCP features is acceptable. It's more important that the entire file arrives intact, with all of its bytes in the correct order.
- UDP is preferred for real-time applications, for simple query-response protocols, and when reliability is provided by other means.
- Real-time applications use UDP because the most recent data is the most valuable, and lost data quickly becomes irrelevant. If packet loss occurs in a voice/ video call, it's better to have minor temporary visual or audio degradation than a delay while waiting for missing packets.
- Simple query-response protocols (i.e., DNS, which usually uses UDP) prefer UDP because the overhead of setting up a TCP connection isn't worth it for a single, small query and response.
- UDP is also preferred when reliability is provided by other means, such as the Application Layer protocol. For example, Trivial File Transfer Protocol (TFTP) provides basic reliability by requiring acknowledgment of each message.

## Standard access control lists

## This chapter covers

- How access control lists filter packets by matching and acting on them
- Configuring standard numbered and named ACLs
- Applying ACLs to interfaces to filter inbound or outbound packets

By default, a Cisco router forwards any packet that has a matching route in its routing table. However, this default behavior may not align with an organization's security needs. In many cases, access to specific resources-such as servers containing sensitive information-should be restricted to authorized individuals or devices.

In a networking role, it's typically not your responsibility to define the security requirements of your organization-most organizations above a certain size will have a dedicated security team. However, it is your responsibility to build and maintain a network that meets your organization's security requirements, and access control lists (ACLs) are an essential tool to help you achieve that goal. In this chapter, we'll examine ACLs from the perspective of a network engineer who must fulfill such requirements: users in department A shouldn't be able to access resources on server B, users in departments X and Y shouldn't be able to communicate with each other over the network, etc.

ACLs function as packet filters, examining each packet as it enters or exits a router's interface and then determining if the packet should be allowed or blocked based on a set of predefined rules. ACLs are CCNA exam topic 5.6: Configure and verify access control lists. In this chapter, we will cover standard ACLs, which filter packets based on their source IP address. In the next chapter, we will cover extended ACLs, which enable the router to filter packets based on various other parameters such as Layer 4 protocol and port numbers.

### 23.1 How ACLs work

An ACL is an ordered list of rules that filters packets as they are received by or forwarded out of an interface. If you're familiar with programming concepts, you may find ACLs fairly simple to grasp; they are very similar to if-then-else conditionals, which evaluate conditions and take different actions based on the results. ACLs examine certain criteria within a packet (such as the source IP address) and then take specific actions. For example:

1 If the packet matches rule 1, then take the corresponding action.
2 Otherwise, if the packet matches rule 2, then take the corresponding action.
3 Otherwise, discard the packet (if no rules are matched).

If you don't have any programming experience-I didn't when I got my CCNA-don't worry: we'll walk through the logic of ACLs step by step in this section. We'll explore the mechanics of how ACLs operate: the process of matching packets, the role of the implicit deny, how ACLs are applied to interfaces, and the various types of ACLs available.

NOTE Don't worry about Cisco IOS command syntax in this section; I will represent ACLs with simple English sentences. The goal is to understand ACL concepts before learning how to configure them.

### 23.1.1 Matching and acting on packets

Each ACL consists of a series of rules called access control entries (ACEs). Each ACE specifies a matching condition (an IP address or range of IP addresses) and an action (permit or deny). Packets that are permitted are forwarded, and packets that are denied are discarded.

Imagine that your organization uses the 192.168.0.0/16 address range for its internal network, from which you create various subnets to be used by different departments; perhaps 192.168.1.0/24 is reserved for users in the engineering department. The requirements from the security team state that users in the engineering department must not be able to access a particular group of servers containing customer information, but users in other departments should be able to. Here's an example of an ACL, consisting of two ACEs, that fulfills this requirement:

1 If the source IP address matches 192.168.1.0/24, then deny the packet.
2 If the source IP address matches 192.168.0.0/16, then permit the packet.

A router evaluating a packet against this ACL will process the ACEs in order, from top to bottom. Figure 23.1 shows a flow chart demonstrating how the router would evaluate a packet against this ACL.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-488_379_1212_474_350.jpg)
Figure 23.1 How a router evaluates a packet against an ACL. If the source IP matches 192.168.1.0/24, then deny the packet. Otherwise, if the source IP matches 192.168.0.0/16, then permit the packet. Otherwise, deny the packet.

NOTE The final deny action is not defined in either of the ACEs. It is known as the implicit deny; we will cover it in section 23.1.2.

Packets are evaluated against each ACE in order, from top to bottom. If a packet matches an ACE's condition, the router takes the action specified in the ACE-permit or deny-and doesn't process the rest of the ACL; the remaining ACEs will be ignored. For this reason, the order of the ACEs is very important; it influences the overall effect of the ACL.

So what's the effect of our example ACL? Packets from IP addresses in the 192.168.1.0/24 range (the engineering department) will be denied-the router will discard them. Then, packets from other IP addresses in the 192.168.0.0/16 range (the rest of the internal network) will be permitted-the router will forward them. Finally, packets from all other source IP addresses (hosts outside of the internal network) will be denied.

NOTE Even though the 192.168.1.0/24 range is included in the 192.168.0.0/16 range, packets from IP addresses in 192.168.1.0/24 will not be permitted by the second ACE; they will be denied by the first ACE before the router processes the second ACE.

Let's reverse the two ACEs in our example ACL to see how this changes the effect of the ACL. Here is the ACL now:

1 If the source IP address matches 192.168.0.0/16, then permit the packet.
2 If the source IP address matches 192.168.1.0/24, then deny the packet.

Figure 23.2 shows how a router would evaluate a packet from 192.168.1.1 against this ACL; the effect of this ACL would be quite different from the first.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-489_512_1212_428_318.jpg)
Figure 23.2 A router evaluates a packet from 192.168.1.1 against the ACL. The packet is permitted by the first ACE. It is never evaluated against the second ACE, which would deny the packet.

Even though the second ACE specifies that packets with a source IP in 192.168.1.0/24 (such as 192.168.1.1) should be denied, the packet from 192.168.1.1 is permitted because it matches the first ACE. The overall effect of this ACL is that all packets sourced from IP addresses in 192.168.0.0/16 (including those in 192.168.1.0/24) will be permitted, and all other packets will be denied.

NOTE The second ACE is an example of a shadowed rule-a rule (ACE) that will never be acted upon because it is preceded by a less specific rule covering its matching condition (192.168.0.0/16 includes 192.168.1.0/24). This shouldn't occur in a properly configured ACL; you should always configure more specific rules first.

### 23.1.2 The implicit deny

As mentioned at the beginning of this chapter, a Cisco router will forward any packet with a valid route by default. However, when using ACLs, the behavior changes: any packet not explicitly permitted by the ACL is denied by default.

At the end of every ACL, there is a hidden rule that denies any packets not previously matched by the ACL's configured ACEs; this is called the implicit deny. If a packet doesn't match any explicitly defined conditions, it will be denied by this hidden rule. The implicit deny ensures a secure stance, where only the traffic explicitly permitted by the ACL will be forwarded and everything else will be automatically discarded. This is the example ACL from the previous section, with the implicit deny explicitly stated:

1 If the source IP address matches 192.168.1.0/24, then deny the packet.
2 If the source IP address matches 192.168.0.0/16, then permit the packet.
3 If the source IP address doesn't match any preceding entry, deny the packet.

Figure 23.3 shows an example of a packet being denied by the implicit deny. The packet's source IP is 172.16.1.1, which is outside of the range used for the internal networks of our fictional organization (192.168.0.0/16). Although the ACL doesn't explicitly deny packets from this IP address, the packet is discarded thanks to the ACL's implicit deny rule.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-490_514_1210_615_350.jpg)
Figure 23.3 A packet is denied by the implicit deny. The packet's source IP (172.16.1.1) doesn't match the first ACE (192.168.1.0/24) or the second ACE (192.168.0.0/16), so it is denied by the implicit deny; the router will discard it.

### 23.1.3 Applying ACLs

The act of creating an ACL doesn't affect the router's behavior on its own; the ACL must be applied to one (or more) of the router's interfaces to take effect. You can apply ACLs in either the inbound or outbound direction (or both), depending on the desired result.

An inbound ACL evaluates packets as they enter the interface the ACL is applied to; every time the router receives a packet on the interface, the router will evaluate the packet against the ACL. If the ACL permits the packet, the router will then continue to process the packet. If the ACL denies the packet, the router will discard it.

Outbound ACLs evaluate packets as they exit the interface; if the router determines that a packet should be forwarded out of the interface, the router will first evaluate the packet against the ACL. If the ACL permits the packet, the router will forward the packet, but if the ACL denies the packet, the router will discard it.

NOTE You may encounter the terms ingress and egress instead of inbound and outbound, respectively-they mean the same thing.

Figure 23.4 demonstrates an outbound ACL filtering packets destined for the 192.168.3.0/24 LAN. The ACL is applied outbound on R1's G0/2 interface and, therefore, filters packets as they are forwarded out of G0/2-as they exit the interface. The ACL does not filter packets that are received by G0/2-packets that enter the interface.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-491_546_1191_390_316.jpg)
Figure 23.4 An outbound ACL on GO/2 filters packets as they exit the interface. Packet 1 from 192.168.1.2 is denied by ACE 1. Packet 2 from 192.168.2.99 is permitted by ACE 2. Packet 3 from 192.168.3.10 isn't evaluated against the ACL because the packet enters GO/2; the ACL is applied outbound, not inbound.

Figure 23.5 shows the same ACL applied inbound on R1 G0/2. Because the ACL is applied inbound, it is not used to filter packets sent out of G0/2. As a result, the first two packets are forwarded without being evaluated against ACL 1. The third packet, from 192.168.3.10 to 192.168.2.99, is permitted by ACE 2.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-491_539_1302_1368_316.jpg)
Figure 23.5 An inbound ACL on GO/2 filters packets as they enter the interface. Packets 1 and 2 are forwarded out of G0/2 without being evaluated against ACL 1. Packet 3 is evaluated as it enters G0/2 and is permitted by ACE 2.

The ideal location and direction to apply an ACL depends on the desired result; applying ACL 1 inbound on G0/2, as in figure 23.5, doesn't make sense. The effect of ACL 1 is to deny packets sourced from the 192.168.1.0/24 LAN, but packets sourced from that LAN will never be received by G0/2, to which the 192.168.3.0/24 LAN is connected. In effect, ACL 1 in figure 23.5 is useless.

However, applying ACL 1 outbound on G0/2, as we saw in figure 23.4, does make sense: it blocks packets sourced from 192.168.1.0.24 from reaching destinations in G0/2's connected LAN (192.168.3.0/24); they will be discarded before being forwarded out of the interface.

Standard ACLs should usually be applied outbound on the interface connected to the destination LAN you want to protect; this serves to filter packets destined for that LAN. However, we will see an example in which applying an ACL inbound is useful in section 23.2, allowing you to block unwanted traffic from entering a router and therefore from accessing any of the router's connected LANs.

NOTE Each interface can have a maximum of one ACL applied in each direction: one inbound and one outbound. The same ACL can be applied to multiple interfaces.

### 23.1.4 ACL types

ACLs can be categorized based on two characteristics-their matching parameters and their identification method:

- Matching parameters-Standard ACLs, extended ACLs
- Identification method-Numbered ACLs, named ACLs

Standard ACLs, the topic of this chapter, match packets based on a single parameter: source IP address. Extended ACLs, the topic of the next chapter, allow for more granular packet-filtering; they match packets based on additional parameters, such as source/ destination IP addresses, source/destination port numbers, and others.

Numbered ACLs are identified by a number, and named ACLs are identified by a name. By combining the two methods of categorization, we can identify the four ACL types you should know for the CCNA exam, as shown in table 23.1.

Table 23.1 ACL types
|  | Numbered ACL | Named ACL |
| :--- | :--- | :--- |
| Standard ACL | Standard numbered ACL | Standard named ACL |
| Extended ACL | Extended numbered ACL | Extended named ACL |


EXAM TIP Only IP ACLs-ACLs that filter IP packets-are covered in the CCNA exam. The four ACL types in table 23.1 are all examples of IP ACLs. Specifically, you are expected to know IPv4 ACLs; IPv6 ACLs are not covered in the CCNA exam.

In section 23.2, we'll see how to configure and apply standard numbered and named ACLs in Cisco IOS. Then, in the next chapter, we'll do the same for extended ACLs.

### 23.2 Configuring standard ACLs

Configuring ACLs, whether standard or extended, numbered or named, consists of two steps:

1 Creating the ACLs
2 Applying the ACLs

In this section, we will examine how to create standard numbered and named ACLs and how to apply them to interfaces to filter packets. Figure 23.6 shows the network we will use for this section, as well as the commands we will use to configure and apply the ACLs:

- ACL 1, applied outbound on R2 G0/0, blocks the engineering department (192.168.1.0/24) from accessing Server LAN A, which includes SRV1.
- ACL BLOCK_MARTHA_BOB, applied inbound on R2 G0/2, prevents two users from accessing either of R2's connected LANs.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-493_761_1404_1273_192.jpg)
Figure 23.6 Configuring and applying standard numbered and named ACLs to control traffic in the network

EXAM TIP The exam topics list states that you must be able to configure ACLs and verify them with show commands. For practice, I recommend recreating the network shown in figure 23.6 in a lab (i.e., in Cisco Packet Tracer) and following along as you read this section. You can also experiment with configuring and testing other ACLs using the same network.

### 23.2.1 Numbered ACLs

Numbered ACLs, as the name implies, are identified with numbers. However, you can't just pick any number to identify an ACL; there are reserved ranges that can only be used for specific kinds of ACLs. For example:

- Standard IP ACLs-1-99 and 1300-1999
- Extended IP ACLs-100-199 and 2000-2699

The original ranges for standard and extended ACLs are 1-99 and 100-199, respectively. However, those ranges were later expanded to allow for a greater number of ACLs, giving us the 1300-1999 and 2000-2699 ranges. Because we are covering standard ACLs in this chapter, we must pick our ACL numbers from the appropriate ranges.

EXAM TIP Make sure you know the ranges for standard and extended ACLs. As always, I recommend flashcards to help with memorization.

## Creating the ACL

In our example network, R1 and R2 are connected by a point-to-point link, and each router is connected to two LANs: R1 is connected to 192.168.1.0/24 (the engineering department) and 192.168.2.0/24 (the accounting department), and R2 is connected to 192.168.3.0/24 (Server LAN A) and 192.168.4.0/24 (Server LAN B), which contain servers used by the organization. To demonstrate numbered ACL configuration, we will create an ACL that limits access to Server LAN A, blocking the engineering department from accessing the LAN.

When configuring a numbered ACL, you must configure each ACE sequentially, from top to bottom. The order in which you configure the ACEs is very important, as the router will process the ACEs in the order they were configured in when evaluating a packet against the ACL.

The primary command to configure an ACE as part of a standard numbered ACL is access-list number \{permit | deny\} source-ip wildcard-mask. The number argument is the number used to identify the ACL-make sure it's in one of the correct ranges (1-99 or 1300-1999). In the following example, I configure ACL 1 on R2, consisting of two ACEs (and an optional remark, identifying the ACL's purpose):
![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-495_244_1408_181_190.jpg)

NOTE The remark is optional but can be useful for indicating the ACL's purpose to others who look at the config (or to your future self).

ACLs use wildcard masks, which we covered in chapter 17. For review, wildcard masks indicate bits that must match with a 0 and bits that don't have to match with a 1. In practice, you can generally think of them as inverse netmasks: 0.0.0.255 is the wildcard mask equivalent of 255.255.255.0, a /24 netmask.

NOTE A shortcut to calculating a netmask's equivalent wildcard mask is to subtract each octet of the netmask from 255. The first three octets of a /24 netmask are 255 and $255-255=0$. The final octet is 0 and $255-0=255$. That gives us 0.0.0.255.

After configuring an ACL, you can verify it with either show access-lists (which shows all ACLs) or show ip access-lists (which shows only IP ACLs); since we are configuring only IP ACLs, the output of both commands should be the same. In the following example, I verify ACL 1 on R2:

```
R2# show access-lists
Standard IP access list 1
    10 deny 192.168.1.0, wildcard bits 0.0.0.255
    20 permit 192.168.0.0, wildcard bits 0.0.255.255
```

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-495_202_477_1185_1146.jpg)

NOTE The remark doesn't appear in show access-lists, only in the config.
Notice the sequence numbers that were automatically added to the beginning of each ACE: 10 and 20. When configuring ACLs, the first ACE is given sequence number 10, and the default increment is 10 . This leaves plenty of room between each ACE, which enables you to configure new ACEs in between existing ACEs if needed. However, that is only possible in named ACL config mode, which we will cover in section 23.2.2.

Applying the ACL
For an ACL to take effect, it must be applied to an interface. The command to do so is ip access-group number $\{$ in $\mid$ out $\}$. In the following example, I apply ACL 1 outbound on R2's G0/0 interface and verify with show ip interface g0/0:

```
R2(config) # interface g0/0
R2(config-if)# ip access-group 1 out
R2(config-if) # do show ip interface g0/0
```

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-495_116_399_1970_1113.jpg)

```
GigabitEthernet0/0 is up, line protocol is up (connected)
Internet address is 192.168.3.1/24
. . .
Outgoing access list is 1
Inbound access list is not set
```

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-496_78_461_291_872.jpg)

Figure 23.7 shows the effect of this configuration: packets are filtered as R2 forwards them out of G0/0, controlling access to Server LAN A.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-496_788_1418_529_223.jpg)
Figure 23.7 Packets filtered by R2's ACL 1, applied outbound on G0/0. Packet 1, from the engineering department, is denied by ACE 10. Packets 2 and 3 are permitted by ACE 20.

The general rule when applying standard ACLs is to apply them as close to the destination as possible; the destination is the LAN you want to protect with the ACL. In this case, the destination is Server LAN A-we want to filter traffic destined for that LAN. By placing the standard ACL outbound on R2 G0/0, you ensure that only traffic destined for G0/0's connected LAN is filtered by the ACL; other traffic is not affected.

### 23.2.2 Named ACLs

Numbered and named ACLs differ not only in how they identify ACLs (with a number or a name) but also in how they are configured. Whereas numbered ACLs are configured entirely from global config mode, named ACLs are first created in global config mode, but each ACE is configured in a separate config mode. You can enter standard named ACL config mode and configure each ACE with the following commands:

- Enter standard named ACL config mode: ip access-list standard name
- Configure ACEs: [seq-num] \{permit | deny\} source-ip wildcard-mask

NOTE In named ACL config mode, you can optionally specify a seq-num (sequence number) to control where the ACE is inserted in the ACL. By default, ACEs will start at sequence 10 and increment by 10 for each new ACE, but this option is useful for inserting new ACEs in between existing ACEs (for example, at sequence number 15).

In our example scenario, we'll block two users from accessing Server LAN A and Server LAN B: Martha from engineering (who uses PC1) and Bob from accounting (who uses PC2). To do so, let's configure three ACEs: one to deny Martha (PC1), one to deny Bob (PC2), and one to allow all other traffic.

The command to configure an ACE is quite flexible when matching a single IP address. For example, here are three ways to configure an ACE denying 8.8.8.8:

- deny 8.8.8.8
- deny 8.8.8.8 0.0.0.0 (a /32 wildcard mask)
- deny host 8.8.8.8

All three of these commands have the same effect, so you can use whichever you prefer-I'll use the first option (replacing 8.8.8.8 with the appropriate addresses). Note that all three methods work for both numbered and named ACLs. In the following example, I configure the ACL on R2 and verify with show access-lists, specifying BLOCK_MARTHA_BOB to view only that ACL:
![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-497_384_1184_1302_316.jpg)

NOTE The any keyword used in the third ACE is a handy way to match all IP addresses; alternatively, you could configure permit 0.0.0.0 255. 255. 255. 255. You can use it in both numbered and named ACLs.

Applying a named ACL is identical to applying a numbered ACL: you can apply it inbound on R2's G0/2 interface with ip access-group BLOCK_MARTHA_BOB in. By
applying the ACL inbound on G0/2, packets are filtered as R2 receives them from R1. This filters packets destined for both destinations that we want to protect: Server LAN A and Server LAN B. Figure 23.8 shows the result of applying our new ACL. Note that ACL 1, which we configured and applied previously, is still in effect.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-498_768_1417_389_222.jpg)
Figure 23.8 Packets filtered by R2's ACLs. Packet 1 is denied by BLOCK_MARTHA_BOB's ACE 10 as it enters R2 G0/2. Packet 2 is permitted by BLOCK_MARTHA_BOB's ACE 30 as it enters R2 G0/2 but denied by ACL 1's ACE 10 as it exits R2 G0/0. Packet 3 is permitted by BLOCK_MARTHA_BOB's ACE 30 as it enters R2 GO/2 and not evaluated against an ACL as it exits R2 G0/1-no ACL is applied to that interface.

In modern versions of Cisco IOS, you can also configure numbered ACLs in named ACL config mode by specifying a number instead of a name after ip access-list standard. In the following example, I configure the previous ACL, using a number instead of a name:

```
R2(config)# ip access-list standard 99
R2(config-std-nacl)# deny 192.168.1.11
R2(config-std-nacl) # deny 192.168.2.17
R2(config-std-nacl)# permit any
```

However, after configuring the ACL, it will appear in the running config as if it was configured using the traditional method. The following example shows ACL 99 in R2's config:

```
R2# show running-config | include access-list 99
access-list 99 deny 192.168.1.11
access-list 99 deny 192.168.2.17
access-list 99 permit any
```

Named ACL config mode provides some benefits, such as the ability to specify sequence numbers and easier ACL editing-we'll cover that in the next chapter. However, keep in mind that ACLs configured using both methods function identically after they have been configured; both are standard ACLs, filtering packets based on source IP addresses.

If you just need to configure a simple ACL with a few ACEs, a numbered ACL configured in global config mode is an easy method. However, in some cases, you may need to take advantage of named ACL config mode's benefits, such as when editing ACLs.

### 23.3 Example scenario

It takes some practice to become comfortable with ACLs. In this section, let's practice by going through a scenario using the same network as in previous examples. We'll start from a blank slate, with no ACLs configured. Figure 23.9 shows the network and the requirements we must fulfill by configuring ACLs.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-499_537_1302_845_190.jpg)
Figure 23.9 Requirements to be fulfilled by configuring standard ACLs

NOTE You can fulfill these requirements using numbered or named ACLs; the destination is more important than how you get there. For demonstration purposes, I will configure both types.

The first requirement states that "Hosts in the accounting department can't access the servers in Server LAN B, but all other hosts can." To fulfill that requirement, I will configure an ACL on R2 that denies the accounting subnet (192.168.2.0/24) but permits all other IP addresses. I will then apply it as close to the destination as possible: outbound on R2's G0/1 interface. In the following example, I configure and apply the ACL:

```
Denies packets sourced from the
accounting department
R2(config) # access-list 10 deny 192.168.2.0 0.0.0.255
R2(config) # access-list 10 permit any
R2(config) # interface g0/1
R2(config-if) # ip access-group 10 out
```

Applies the ACL outbound on G0/1

That ACL fulfills the first requirement. The second requirement states that "Hosts in the engineering and accounting departments can't communicate with each other." This can be achieved by configuring two ACLs on R1:

1 An ACL that denies 192.168.1.0/24 but permits all other IP addresses
2 An ACL that denies 192.168.2.0/24 but permits all other IP addresses

By applying the first ACL outbound on R1 G0/1, packets from hosts in the engineering department will be blocked from reaching destinations in the accounting department. In the following example, I configure and apply the ACL on R1:

```
R1(config)# ip access-list standard BLOCK_ENGINEERING
R1(config-std-nacl) # deny 192.168.1.0 0.0.0.255
R1(config-std-nacl)# permit any
R1(config-std-nacl) # interface g0/1
R1(config-if)# ip access-group BLOCK_ENGINEERING out
```

Applies the ACL outbound on G0/1

Likewise, by applying the second ACL outbound on R1 G0/0, packets sourced from hosts in the accounting department will be blocked from reaching hosts in the engineering department. I configure and apply the second ACL in the following example:

```
R1(config)# ip access-list standard BLOCK_ACCOUNTING
R1(config-std-nacl) # deny 192.168.2.0 0.0.0.255
R1(config-std-nacl)# permit any
R1(config-std-nacl) # interface g0/0
R1(config-if)# ip access-group BLOCK_ACCOUNTING out
```

We have now fulfilled the requirements! For review, figure 23.10 shows the requirements and the ACLs we configured to fulfill those requirements.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-501_687_1395_179_194.jpg)
Figure 23.10 Three ACLs configured on R1 and R2 fulfill the requirements. ACL 10, configured on R2, prevents hosts in 192.168.2.0/24 from accessing 192.168.4.0/24. The two named ACLs on R1 prevent the engineering and accounting departments from communicating with each other.

## Summary

- An access control list (ACL) is an ordered list of rules that filters packets as they are received by or forwarded out of an interface.
- ACLs function like if-then-else conditionals in programming, evaluating conditions and taking actions based on the results.
- Each ACL consists of a series of rules called access control entries (ACEs). Each ACE specifies a matching condition and an action (permit or deny).
- Packets that are permitted are forwarded, and packets that are denied are discarded.
- A router evaluating a packet against an ACL will process the ACEs sequentially from top to bottom. Once a matching ACE is found, the appropriate action is taken, and the router doesn't process any remaining ACEs.
- An ACE that will never be acted upon because it is preceded by a less specific rule covering its matching condition is an example of a shadowed rule.
- If a packet doesn't match any of an ACL's explicitly defined conditions, it will be denied by a hidden rule called the implicit deny.
- The act of creating an ACL doesn't affect the router's behavior on its own; the ACL must be applied to one (or more) interfaces to take effect.
- ACLs can be applied to an interface inbound and/or outbound. An inbound ACL filters packets as they are received by the interface. An outbound ACL filters packets as they are forwarded out of the interface.

- Each interface can have a maximum of one ACL applied in each direction: one inbound and one outbound.
- ACLs can be categorized based on their matching parameters: standard ACLs match packets based only on their source IP address. Extended ACLs can match packets based on source/destination IP addresses, source/destination ports, and others.
- ACLs can also be categorized by how they are identified. Numbered ACLs are identified by a number. Named ACLs are identified by a name.
- You should know four ACL types for the CCNA: standard numbered, standard named, extended numbered, and extended named.
- Numbered ACLs are identified by a number that indicates whether the ACL is standard or extended. Standard ACLs use ranges 1-99 and 1300-1999, and extended ACLs use ranges 100-199 and 2000-2699.
- Standard numbered ACLs are configured one ACE at a time in global config mode with the access-list number \{permit | deny\} source-ip wildcard -mask command.
- The order in which you configure ACEs is important. The ACEs will be processed in the order you configure them.
- You can configure an optional descriptive remark with access-list number remark remark. This is useful to clarify the purpose of the ACL.
- You can view all ACLs with show access-lists and all IP ACLs with show ip access-lists.
- Each ACE is automatically assigned a sequence number, starting from 10 and incrementing by 10 for each ACE.
- You can apply an ACL to an interface with ip access-group \{number | name\} \{in | out\}.
- Named ACLs are created in global config mode, but ACEs are configured in a separate config mode.
- You can create a standard named ACL with ip access-group standard name.
- You can configure ACEs in standard named ACL config mode with [seq-num] \{permit | deny\} source-ip wildcard-mask.
- You can match a single IP address in three ways:
    - \{permit | deny\} 8.8.8.8
    - \{permit | deny\} 8.8.8.8 0.0.0.0
    - \{permit | deny\} host 8.8.8.8
- The any keyword can be used to match all IP addresses, such as permit any.
- Numbered ACLs can also be configured in named ACL config mode.
- Named ACL config mode provides some benefits, such as the ability to specify sequence numbers and easier ACL editing. However, both types function identically once they have been configured.

## Extended access control lists

## This chapter covers

- The various parameters extended access control lists use to match packets
- Configuring extended numbered and named ACLs
- Editing ACLs by deleting and resequencing ACEs

In the previous chapter, we covered standard ACLs, which filter packets based on a single parameter: the source IP address. Although standard ACLs have their uses, they are a blunt instrument; they don't provide precise control over exactly which kinds of traffic are permitted and denied. Extended ACLs, the topic of this chapter, are a more precise tool: they allow you to filter packets based on many more parameters, providing more granular control over traffic.

Although extended ACLs can be more complex than standard ACLs, the good news is that the fundamentals of how ACLs work, as we covered in the previous chapter, remain the same. Like standard ACLs, the access control entries (ACEs) of an extended ACL are processed in order from top to bottom. Extended ACLs include an implicit deny that discards all traffic that isn't matched by an explicitly configured ACE. Extended ACLs also need to be applied to an interface in the inbound and/or outbound directions to take effect.

Given these similarities, in this chapter we will jump right into looking at how to configure extended ACLs-how to configure ACEs that match packets based on their protocol, source/destination IP addresses, and source/destination ports. As in the previous chapter, we will cover CCNA exam topic 5.6: Configure and verify access control lists.

### 24.1 Configuring extended ACLs

Extended ACLs can filter packets based on a variety of different parameters, such as

- The protocol of the packet's payload (TCP, UDP, ICMP, etc.)
- Source and/or destination IP addresses
- Source and/or destination TCP/UDP ports

Configuring extended ACLs is very similar to configuring standard ACLs: you can configure numbered ACLs in global config mode with the access-list command or create and configure ACLs in named ACL config mode with the ip access-list command. However, due to the additional parameters that extended ACLs can use to match packets, there are additional keywords and arguments to familiarize yourself with.

### 24.1.1 Matching protocol, source, and destination

Let's begin by configuring ACLs that match packets based on the encapsulated protocol, source IP address, and destination IP address. The following example shows the command syntax for how to configure a numbered ACL with these parameters:

```
R1(config)# access-list number {permit | deny} protocol source destination
```

NOTE Extended numbered ACLs must use a number from one of the appropriate ranges: 100-199 or 2000-2699.

The following example shows how to configure an extended ACL in named ACL config mode. As with standard ACLs, modern versions of Cisco IOS allow you to configure both extended numbered and extended named ACLs in named ACL config mode:

```
R1(config)# ip access-list extended {name | number}
R1(config-ext-nacl)# [seq-num] {permit | deny} protocol source destination
```

The protocol, source, and destination arguments in these commands are the matching parameters; this is where you specify which packets should match-and be acted upon by-the ACE. For a packet to match an ACE, it must match all of the specified values: the specified protocol, source, and destination. Partial matches don't count! Figure 24.1 gives more detail about which values you can configure for these arguments.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-505_263_950_185_318.jpg)
Figure 24.1 Possible matching parameters. The protocol argument specifies the protocol of the packet's payload. The source and destination arguments specify the packet's source and destination IP addresses, respectively.

The protocol argument warrants some additional explanation. You can specify a keyword like icmp, tcp, udp, or ospf to match packets that carry an ICMP, TCP, UDP, or OSPF payload, respectively. Another common keyword is ip, which matches all IPv4 packets; use this if you don't care what the protocol of the encapsulated message is and you want to match only based on source/destination IP addresses.

You should be familiar with the options for the source and destination arguments: we covered them in the previous chapter. By providing an IP address and wildcard mask (ip-addr wildcard-mask), you can specify a range of IP addresses to match. The any keyword matches all possible IP addresses-equivalent to 0.0.0.0 255.255.255.255 (that's a /0 wildcard mask, not a /32 netmask). host ip-addr allows you to specify a single IP address to match-equivalent to an IP address and a /32 wildcard mask ( 0.0 .0 .0 ).

NOTE When configuring a standard ACL, you can match a single IP address by simply specifying the IP address, without the host keyword or a wildcard mask. However, that doesn't work in extended ACLs. To match a single IP address in an extended ACL, for example 8.8.8.8, use host 8.8.8.8 or 8.8.8.8 0.0.0.0.

With these three parameters, you can fulfill more precise security requirements than with standard ACLs-for example, "block all TCP traffic from host A to LAN B" or "permit only ICMP traffic from LAN X to LAN Y." Table 24.1 lists and explains some example ACEs. Compare each ACE and its explanation to familiarize yourself with the logic of ACEs that match packets based on protocol, source, and destination.

Table 24.1 Example ACEs for extended ACLs
| ACE | Explanation |
| :--- | :--- |
| permit ip any any | Permits all IPv4 packets from any source to any destination |
| deny udp 10.0.0.0 0.0.0.255 192.168.1.0 0.0.0.255 | Prevents 10.0.0.0/24 from sending UDP messages to 192.168.1.0/24 |
| deny icmp any 203.0.113.0 0.0.0.255 | Prevents all hosts from sending ICMP messages (i.e., ping) to 203.0.113.0/24 |
| permit ip host 172.16.1.1 any | Permits all IPv4 packets from 172.16.1.1 to any destination |


Now let's examine an entire extended ACL and consider its effect on traffic. Figure 24.2 shows an example network-the same one we looked at in the previous chapter- and an extended named ACL, applied outbound on R1's G0/2 interface.

```
R1(config)# ip access-list extended TEST1
R1(config-ext-nacl)# permit icmp host 192.168.1.11 192.168.3.0 0.0.0.255
R1(config-ext-nacl)# deny icmp 192.168.1.0 0.0.0.255 any
R1(config-ext-nacl)# deny icmp 192.168.2.0 0.0.0.255 any
R1(config-ext-nacl) # deny udp any any
R1(config-ext-nacl) # permit ip any any
R1(config-ext-nacl) # interface g0/2
R1(config-if)# ip access-group TEST1 out
```

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-506_376_1417_654_220.jpg)
Figure 24.2 Extended ACL TEST1 is applied outbound on R1 G0/2. TEST1 permits ICMP traffic from PC1 to 192.168.3.0/24, denies ICMP traffic from 192.168.1.0/24 and 192.168.2.0/24, denies all UDP traffic, and permits all other IPv4 packets.

TEST1's first ACE permits ICMP traffic from PC1 (192.168.1.11) to hosts in Server LAN A, such as SRV1; this would allow PC1 to test connectivity by pinging SRV1, for example. The second and third ACEs, however, deny all ICMP traffic from hosts in the engineering and accounting subnets (except for PC1's ICMP traffic that is matched by the first ACE).

The fourth ACE denies all UDP traffic, and the final ACE permits all other IPv4 packets. The any any keywords in both ACEs mean they will match packets from any source IP address to any destination IP address. Keep in mind that the ACL is applied outbound on R1's G0/2 interface, so it only filters packets that are to be forwarded out of that interface, toward R2 and its connected LANs.

As covered in chapter 23, standard ACLs should be applied as close to the destination as possible. Standard ACLs don't provide granular control of which packets are filtered, so if you apply them too close to their source, you could inadvertently block legitimate traffic from the source.

Extended ACLs, however, provide more control over exactly which types of packets are permitted and which should be denied. For that reason, it's more efficient to apply extended ACLs as close to the source as possible. A properly configured extended ACL will only block the intended traffic, and applying the ACL close to the source is more
efficient, because it prevents packets from traveling all the way across the network just to be discarded right before reaching their destination.

In this example, we are filtering packets from the engineering and accounting departments' LANs to Server LAN A and Server LAN B. Applying TEST1 outbound on R1 G0/2 filters packets from both source LANs early in their path to either of the destination LANs.

NOTE We will practice configuring and applying extended ACLs from a set of requirements in section 24.2. For now, our focus is on understanding how they work.

### 24.1.2 Matching TCP/UDP port numbers

Matching packets based on protocol, source, and destination provides much more granular control than standard ACLs provide. However, by adding TCP and UDP port numbers to the mix, we can achieve even more control over which packets are permitted or denied. Figure 24.3 shows how to configure an ACE that includes ports in its matching parameters; pay particular attention to the keyword values that you can provide for the operator arguments.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-507_274_1290_1035_320.jpg)
Figure 24.3 Configuring an ACE that matches packets based on protocol (TCP/UDP), source/ destination IP addresses, and source/destination ports

Here is an example and explanation of each of the keywords you can provide for the operator arguments:

- eq 80 = Equal to 80
- gt 80 = Greater than 80 (but not including 80)
- It 80 = Less than 80 (but not including 80)
- neq 80 = Not equal to 80 (anything other than 80)
- range 80100 = From 80 to 100 (including 80 and 100)

Table 24.2 lists some example ACEs that include the source and/or destination port numbers in their list of matching parameters. Once again, compare each ACE and its explanation to familiarize yourself with their logic. Make sure you can identify each part of each ACE: the protocol, source IP addresses, source ports, destination IP
addresses, and destination ports. Some ACEs specify both source and destination port numbers, but others specify only one. If you don't specify a source port number, all source port numbers will count as a match, and the same applies for the destination port number.

Table 24.2 Example ACEs including port numbers
| ACE | Explanation |
| :--- | :--- |
| permit tcp 10.0.0.0 0.0.0.255 any eq 443 | Allows hosts in 10.0.0.0/24 to access all web servers using HTTPS (port 443) |
| deny udp any gt 50000 host 203.0.113.1 | Prevents all hosts with a UDP source port greater than 50000 from accessing 203.0.113.1 |
| permit tcp 10.0.0.0 0.0.0.255 gt 9999 host 203.0.113.1 neq 23 | Allows hosts in 10.0.0.0/24 with a TCP source port greater than 9,999 to access all TCP ports on 203.0.113.1 except port 23 |
| deny tcp any lt 1024 any gt 1023 | Denies all TCP messages with a source port lower than 1,024 and a destination port greater than 1,023 |


NOTE Remember that a packet must match all parameters in the ACE to be considered a match. If the ACE specifies the protocol, source IP, source port, destination IP, and destination port, all five parameters must match.

Figure 24.4 shows each of the ACEs from table 24.1 and identifies each part: protocol, source IP, source port, destination IP, and destination port.

1) permit $\frac{\mathrm{tcp}}{\text { protocol }} \frac{10.0 .0 .00 .0 .0 .255}{\text { src. IP }} \frac{\text { any }}{\text { dst. IP }} \frac{\mathrm{eq} 443}{\text { dst. port }}$
2) deny $\frac{\text { udp }}{\text { protocol }} \frac{\text { any }}{\text { src. IP }} \frac{\text { gt } 50000}{\text { src. port }} \frac{\text { host 203.0.113.1 }}{\text { dst. IP }}$
3) permit $\underset{\text { protocol }}{\frac{\mathrm{tcp}}{10.0 .0 .0 ~ 0.0 .0 .255}} \underset{\text { src. IP }}{\underline{\mathrm{gt} 9999}} \frac{\text { host 203.0.113.1 }}{\text { src. port }} \frac{\text { neq } 23}{\text { dst. port }}$
4) deny $\frac{\mathrm{tcp}}{\text { protocol }} \frac{\text { any }}{\text { src. IP }} \frac{\mathrm{lt} 1024}{\text { src. port }} \frac{\text { any }}{\text { dst. IP }} \frac{\mathrm{gt} 1023}{\text { dst. port }}$

Figure 24.4 ACEs that match packets based on protocol, source IP, source port, destination IP, and destination port. Not all ACEs specify both the source and destination ports.

Now let's examine an extended ACL that uses ACEs like these, filtering packets based on source and/or destination ports. Examine the ACL shown in figure 24.5-applied outbound on R1 G0/2-and consider which traffic is allowed and which isn't.

```
R1(config)# ip access-list extended TEST2
R1(config-ext-nacl)# permit tcp 192.168.1.0 0.0.0.255 192.168.3.0 0.0.0.255 eq 443
R1(config-ext-nacl) # deny tcp any any eq 443
R1(config-ext-nacl) # permit udp any host 192.168.4.19 eq 123
R1(config-ext-nacl) # deny udp any any eq 123
R1(config-ext-nacl) # permit ip any any
R1(config-ext-nacl) # interface g0/2
R1(config-if)# ip access-group TEST2 out
```

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-509_376_1417_495_190.jpg)
Figure 24.5 Extended ACL TEST2 is applied outbound on R1 G0/2. TEST2 permits HTTPS traffic from the engineering department to Server LAN A, denies all other HTTPS traffic, permits all NTP traffic to SRV2, denies all other NTP traffic, and permits all other IPv4 packets.

In the following example, I configure the ACL shown in figure 24.5 and confirm with show access-lists. Notice that UDP port 123, as specified in the third and fourth ACEs, is replaced by the keyword ntp, because NTP uses UDP port 123. You can configure some well-known port numbers by specifying either the port number itself or the equivalent keyword. Whichever you configure, the keyword (i.e., ntp) will appear in the output of show commands. However, as the example also demonstrates, not all common protocols have such a keyword; TCP port 443 isn't replaced by https:

```
R1(config)# ip access-list extended TEST2
R1(config-ext-nacl) # permit tcp 192.168.1.0 0.0.0.255 192.168.3.0 0.0.0.255 eq 443
R1(config-ext-nacl) # deny tcp any any eq 443
R1(config-ext-nacl) # permit udp any host 192.168.4.19 eq 123
R1(config-ext-nacl) # deny udp any any eq 123
R1(config-ext-nacl) # permit ip any any
R1(config-ext-nacl) # do show access-lists
Extended IP access list TEST2
    10 permit tcp 192.168.1.0 0.0.0.255 192.168.3.0 0.0.0.255 eq 443
    20 deny tcp 192.168.2.0 0.0.0.255 any eq 443
    30 permit udp any host 192.168.4.19 eq ntp
    40 deny udp any any eq ntp
```

TCP port 443, used by HTTPS, is not replaced by the keyword https.

UDP port 123, used by NTP, is replaced by the keyword ntp.

```
    50 permit ip any any
```

Because ACLs in CCNA exam questions may use keywords instead of port numbers, I recommend getting familiar with a few common ones, such as the following:

- TCP 20 (FTP data) = ftp-data
- TCP 21 (FTP control) = ftp

- TCP 23 (Telnet) = telnet
- TCP/UDP 53 (DNS) = domain
- UDP 67 (DHCP server) = bootps
- UDP 68 (DHCP client) = bootpc
- UDP 69 (TFTP) = tftp
- TCP 80 (HTTP) = www

EXAM TIP Make sure you know the Layer 4 protocol and port numbers of common protocols, such as those listed here. I listed them in chapter 22 and will mention them as we cover each protocol in volume 2 of this book, but it's worth emphasizing the importance of knowing them.

### 24.2 Example security requirements

Now that we've examined extended ACLs and their components, let's practice configuring them to fulfill a set of requirements as defined by the security team of our fictional organization. Extended ACLs add a few layers of complexity on top of standard ACLs, so this practice is especially important. Figure 24.6 shows the scenario, with three requirements we must fulfill by configuring extended ACLs.

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-510_519_1300_1098_348.jpg)
Figure 24.6 Requirements to be fulfilled by configuring extended ACLs

The first requirement states that "Only ICMP is permitted between Server LAN A and Server LAN B." I will accomplish this with two ACLs:

- An ACL that permits ICMP traffic from Server LAN A to Server LAN B and denies other traffic between them but allows all traffic to other destinations
- An ACL that permits ICMP traffic from Server LAN B to Server LAN A and denies other traffic between them but allows all traffic to other destinations

In the following example, I configure the first ACL and apply it inbound on R2 G0/0; this filters packets sent from hosts in Server LAN A:
![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-511_388_1307_287_314.jpg)
That fulfills one half of the first requirement. In the next example, I configure the second ACL, this time applying it inbound on R2 G0/1:
![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-511_390_1304_801_314.jpg)
The effect of these two ACLs is that hosts in Server LAN A and Server LAN B will only be able to communicate with each other using ICMP-ping, for example. However, other traffic is permitted by the permit ip any any ACE at the end of both ACLs.

The second requirement in our scenario states that "Hosts in the engineering department can't use HTTP to access Server LAN A or Server LAN B." This can be accomplished with one ACL applied inbound on R1's G0/0 interface. The ACL should deny TCP messages sourced from hosts in Server LAN A and destined for port 80 on hosts in Server LAN A or Server LAN B. In the following example, I configure that ACL:
![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-511_379_1286_1619_314.jpg)

NOTE When a host uses HTTP to access resources on another host (a server), it uses port 80 as the destination port, not the source port. Therefore, to filter HTTP traffic from a client to a server, make sure to match based on the destination port-not the source port. This applies to other protocols too; the following example filters TFTP traffic by matching destination port 69.

The final requirement states, "Deny all TFTP traffic from hosts in the accounting department." We can fulfill this requirement with a simple two-ACE ACL: one ACE to deny TFTP traffic and one to permit all other traffic. In the following example, I configure that ACL and apply it inbound on R1 G0/1. For demonstration purposes, I'll configure this one as a numbered ACL from global config mode:
![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-512_272_1406_683_223.jpg)

We have now fulfilled all three requirements. For review, figure 24.7 shows the requirements again and the four ACLs we configured to fulfill them.,

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-512_679_1399_1119_232.jpg)
Figure 24.7 Four ACLs configured on R1 and R2 fulfill the requirements. ACLs ICMP_1 and ICMP_2 fulfill requirement 1, ACL NO_HTTP fulfills requirement 2, and ACL 100 fulfills requirement 3.

### 24.3 Editing ACLs

The order in which you configure an ACL's ACEs is very important, because it determines the order the router will process them when evaluating a packet against the ACL. However, even if you configure the ACEs in the proper order, in some cases you may have to edit the ACL later. For example, you may have to delete an ACE or insert a new ACE in between existing ones. In this section, we'll examine how to do that.

NOTE Everything in this section applies to both standard and extended ACLs.
To keep the ACLs simple, I'll use standard ACLs.

### 24.3.1 Deleting ACEs

Negating a command in Cisco IOS is as simple as inserting the no keyword in front of it. However, let's see what happens when we use that method to delete an ACE from a numbered ACL:

```
R1(config)# do show access-lists
Standard IP access list 1
    10 deny 192.168.1.1
    20 deny 192.168.2.1
    30 deny 192.168.3.0, wildcard bits 0.0.0.255
    40 permit any
R1(config)# no access-list 1 deny 192.168.3.0 0.0.0.255
R1(config)# do show access-lists
R1(config) #
```

No output is shown.
![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-513_134_276_961_1343.jpg)

After using no to negate the command for ACE 30, ACL 1 is no longer shown in the output of show access-lists-the entire ACL was deleted! When editing a numbered ACL from global config mode, you can't delete individual ACEs; you can only delete the entire ACL. Fortunately, named ACL mode fixes this limitation.

Deleting an ACE in named ACL config mode is easy: just use no followed by the sequence number. This works for both numbered and named ACLs; just remember to do it from named ACL config mode. In the following example, I delete ACE 30 from the same ACL as before, without deleting the entire ACL this time:

```
R1(config)# do show access-lists
Standard IP access list 1
    10 deny 192.168.1.1
    20 deny 192.168.2.1
    30 deny 192.168.3.0, wildcard bits 0.0.0.255
    40 permit any
```

R1(config)\# ip access-list standard 1
R1(config-std-nacl)\# no 30
R1(config-std-nacl)\# do show access-lists
Standard IP access list 1

Enters named ACL config mode and deletes ACE 30
| 10 | deny | 192.168.1.1 |
| :--- | :--- | :--- |
| 20 | deny | 192.168.2.1 |
| 40 | permit | any |


ACE 30 was removed from ACL1.

Named ACL config mode also provides the benefit of allowing you to insert new ACEs in between existing ones by specifying a sequence number at the beginning of the configuration command. In the following example, I insert a new ACE 30 to the same ACL as before in place of the ACE 30 that I just deleted:

```
R1(config-std-nacl) # 30 deny 192.168.4.0 0.0.0.255
R1(config-std-nacl)# do show access-lists
Standard IP access list 1
    10 deny 192.168.1.1
    20 deny 192.168.2.1
    30 deny 192.168.4.0, wildcard bits 0.0.0.255
    40 permit any
```


### 24.3.2 Resequencing ACEs

Named ACL config mode allows you to configure new ACEs between existing ones by specifying each ACE's sequence number, but what if there is no space between ACEs? By default, sequence numbers begin at 10 and increment by 10, so this problem is rare. However, if you do run out of space between ACEs, Cisco IOS provides a handy command that will automatically adjust the sequence numbers. The syntax of the command is ip access-list resequence \{name|number\} starting-seq -num increment. Figure 24.8 shows an example command and explains the final two arguments (starting-seq-num and increment):

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-514_231_1413_1128_222.jpg)
Figure 24.8 The ip access-list resequence command. The starting-seq-num argument specifies the new sequence number of the ACL's first ACE, and the increment argument specifies the increment for each subsequent ACE.

In the following example, I view an example ACL (ACL 2) that has four ACEs with sequence numbers 10, 18, 19, and 20. I then use ip access-list resequence to adjust the sequence numbers. After issuing the command, there is room between the ACEs to insert new ones as needed:

```
R1(config)# do show access-lists
Standard IP access list 2
    10 deny 192.168.1.0 0.0.0.255
    18 deny 192.168.3.0 0.0.0.255
    19 deny 192.168.4.0 0.0.0.255
    20 permit any
R1(config)# ip access-list resequence 2 10 10
R1(config) # do show access-lists
Standard IP access list 2
```

![](./images/eda999fc-7a9e-45a9-9a8f-e340098c9d94-514_137_405_1847_1225.jpg)

```
10 deny 192.168.1.0 0.0.0.255
20 deny 192.168.3.0 0.0.0.255
30 deny 192.168.4.0 0.0.0.255
40 permit any
```


## Summary

- Extended ACLs can filter packets based on parameters such as the protocol of the packet's payload (TCP, UDP, ICMP, OSPF, etc.), source/destination IP addresses, and source/destination ports.
- Extended ACL configuration is similar to standard ACL configuration. You can configure extended ACLs in global config mode with the access-list command (numbered only) or in named ACL config mode with ip access-list.
- You can configure an extended numbered ACL with access-list number \{permit | deny\} protocol source destination. Extended numbered ACLs must use a number from ranges 100-199 or 2000-2699.
- You can configure an extended ACL in named ACL config mode with ip access-list extended \{name | number\}. From there, configure each ACE with [seq-num] \{permit | deny\} protocol source destination.
- For the protocol argument, you can specify a keyword like icmp, tcp, udp, or ospf to match packets that carry the specified protocol in its payload. Or you can specify ip to match all IPv4 packets, regardless of the encapsulated protocol.
- The options for the source and destination arguments are any (to match any IP address), host ip-addr (to match only the specified IP address), and ip-addr wildcard-mask (to match the specified range of IP addresses).
- Whereas standard ACLs should be applied as close to the destination as possible, extended ACLs should be applied as close to the source as possible.
- When tcp or udp are specified for the protocol argument, you can also specify TCP/UDP port number(s) as matching conditions.
- The command syntax to configure an ACE that specifies a port number is \{permit|deny\} \{tcp|udp\} src-ip [operator src-port] dst-ip [operator dst-port].
- The options for the operator argument are eq (equal to X), gt (greater than X), lt (lower than X), neq (not equal to X), and range (from X to Y). Some examples are
    - eq 80 = Equal to 80
    - gt 80 = Greater than 80 (but not including 80)
    - It 80 = Less than 80 (but not including 80)
    - neq 80 = Not equal to 80 (anything other than 80)
    - range 80100 = From 80 to 100 (including 80 and 100)

- An ACE can specify only the source port, only the destination port, both, or neither. If you don't specify the source/destination ports as matching conditions, all source/destination ports will match.
- A packet must match all of an ACE's conditions to be considered a match. If the ACE specifies the protocol, source IP, source port, destination IP, and destination port, all five parameters must match.
- When specifying port numbers in an ACE, some common protocols have keywords that can be used instead of numbers. Some examples are
    - TCP 20 (FTP data) = ftp-data
    - TCP 21 (FTP control) = ftp
    - TCP 23 (Telnet) = telnet
    - TCP/UDP 53 (DNS) = domain
    - UDP 67 (DHCP server) = bootps
    - UDP 68 (DHCP client) = bootpc
    - UDP 69 (TFTP) = tftp
    - TCP 80 (HTTP) = www
- When a host uses a protocol to access resources on another host (a server), it uses the protocol's port number as the destination port, not the source port. To filter that traffic, make sure to filter based on the destination port, not the source port.
- When editing a numbered ACL from global config mode, you can't delete individual ACEs; you can only delete the entire ACL.
- Deleting an ACE in named ACL config mode is easy: use no followed by the sequence number, such as no 30 to delete the ACE with sequence number 30.
- Named ACL config mode also allows you to insert new ACEs in between existing ones by specifying a sequence number at the beginning of the command.
- You can renumber an ACL's ACEs with the ip access-list resequence \{name | number\} starting-seq-num increment command. starting-seq -num specifies the sequence number of the first ACE, and increment specifies the increment for each additional ACE. For example, ip access-list resequence 155 will set ACL 1's first ACE to sequence number 5, the second to 10, the third to 15, etc.

## Exam topics reference table

The following table lists the CCNA exam topics and the chapters in this book that cover each of them. However, keep in mind that Cisco occasionally (but rarely) makes minor, unannounced changes to the exam topics, so I recommend going straight to the source to verify the official list: https://learningnetwork.cisco .com/s/ccna-exam-topics.

Furthermore, Cisco publishes its Certification Roadmaps at https://learningnetwork.cisco.com/s/cisco-certification-roadmaps. I recommend bookmarking that page; it will give you information about Cisco's yearly certification review process and any scheduled changes coming to the CCNA exam (and Cisco's other exams).

The first time you read this book, I recommend following the chapters in order. However, it's important to carefully examine the CCNA exam topics and make sure you know all of them before taking the CCNA exam. This resource should be helpful in that process; if there are any exam topics that you don't feel confident about, refer to the table to know which chapters you should review.

