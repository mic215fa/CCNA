## The Cisco IOS CLI

## This chapter covers

- The interfaces used to configure network devices
- How to connect to the CLI of a Cisco device via the console port
- Navigating between modes of the Cisco IOS command hierarchy
- Viewing and saving a device's configuration files
- Password-protecting a Cisco IOS device

This chapter is a break from the networking theory of the previous chapter; it's time to get hands-on with Cisco routers and switches. Understanding the theory of networking is absolutely essential, but networking is also a skill that must be practiced, and that means configuring network devices.

> [!translation] 逐句繁體中文翻譯
> 本章會暫時離開上一章的網路理論；現在該開始實際操作 Cisco routers 與 switches 了。  
> 理解網路理論絕對必要，但網路也是一項需要練習的技能，而這代表你必須設定 network devices。

In the CCNA exam topics list, you will find a few different verbs, such as explain $X$, describe $Y$, and identify $Z$, indicating that Cisco expects you to have a theoretical understanding of the listed concepts and how they work. However, there are also many exam topics that state configure X or configure and verify Y. For these topics, in addition to having a theoretical understanding of their concepts, you must be able to configure them on Cisco network devices and verify their operations.

> [!translation] 逐句繁體中文翻譯
> 在 CCNA exam topics list 中，你會看到幾種不同動詞，例如 explain $X$、describe $Y$、identify $Z$，這表示 Cisco 期待你對列出的概念及其運作方式具備理論理解。  
> 然而，也有許多 exam topics 會寫 configure X，或 configure and verify Y。  
> 對這些主題來說，除了理解其概念之外，你還必須能在 Cisco network devices 上設定它們，並驗證其運作。

As an introduction to making configuration changes to a Cisco device and saving those changes, in this chapter, we will touch on exam topic 5.3: Configure and verify device access control using local passwords. However, this chapter is not specifically aimed at one of the CCNA exam topics but rather lays a necessary foundation for all of the exam topics that require you to configure and verify various protocols.

> [!translation] 逐句繁體中文翻譯
> 作為修改 Cisco device 設定並儲存這些變更的入門，本章會稍微碰到 exam topic 5.3：使用 local passwords 設定並驗證 device access control。  
> 不過，本章並不是特別針對某一個 CCNA exam topic；它更像是為所有需要你 configure and verify 各種 protocols 的考試主題打下必要基礎。

### 5.1 Shells: GUI and CLI

A shell is a computer program that allows a user to interact with the computer. It's the interface between the computer and the user, and it's called a shell because it's the outer layer of the operating system. To configure a Cisco router or switch, you use a shell to give commands to the device. In this section, we will look at the two types of shells we will use in this book.

> [!translation] 逐句繁體中文翻譯
> Shell 是一種 computer program，讓 user 能與 computer 互動。  
> 它是 computer 與 user 之間的 interface；之所以稱為 shell，是因為它是 operating system 的外層。  
> 若要設定 Cisco router 或 switch，你會使用 shell 對 device 下 commands。  
> 在本節中，我們會看本書會使用的兩種 shells。

### 5.1.1 GUI and CLI

There are two main kinds of shells: graphical user interface (GUI, pronounced "G-U-I" or "gooey") and command-line interface (CLI). Let's examine these two types.

> [!translation] 逐句繁體中文翻譯
> Shells 主要有兩種類型：graphical user interface（GUI，發音可唸作「G-U-I」或「gooey」）以及 command-line interface（CLI）。  
> 讓我們來檢視這兩種類型。

Graphical user interfaces
A GUI allows a user to manipulate the computer via a graphical interface. Regardless of your degree of experience or inexperience with computers, I'm certain you've used a GUI before. If you have a Windows PC, the GUI is what you're interacting with when you open, close, and move windows, or when you open the Start menu to search for a program, etc. This is the Windows shell. If you have a smartphone, you use a GUI to interact with the phone and its apps.

> [!translation] 逐句繁體中文翻譯
> Graphical user interfaces。  
> GUI 讓 user 能透過 graphical interface 操作 computer。  
> 無論你對 computers 的經驗多或少，我很確定你以前都用過 GUI。  
> 如果你有 Windows PC，當你開啟、關閉、移動 windows，或打開 Start menu 搜尋 program 等等時，你互動的就是 GUI。  
> 這就是 Windows shell。  
> 如果你有 smartphone，你也是使用 GUI 與 phone 及其 apps 互動。

Although most of the CCNA exam does not focus on GUIs, you are expected to be familiar with one GUI for the exam: the Cisco wireless LAN controller (WLC) GUI. We will cover wireless LANs and how to configure a WLC via the GUI in part 4 of volume 2 of this book. Figure 5.1 shows a screenshot of the GUI of a Cisco WLC.

> [!translation] 逐句繁體中文翻譯
> 雖然 CCNA exam 大多數內容不聚焦於 GUIs，但考試仍期待你熟悉其中一種 GUI：Cisco wireless LAN controller（WLC）GUI。  
> 我們會在本書 Volume 2 Part 4 涵蓋 wireless LANs，以及如何透過 GUI 設定 WLC。  
> Figure 5.1 顯示 Cisco WLC GUI 的 screenshot。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-077_590_866_1448_320.jpg)
Figure 5.1 The GUI of a Cisco wireless LAN controller, accessed via a web browser

> [!translation] 逐句繁體中文翻譯
> Figure 5.1：Cisco wireless LAN controller 的 GUI，透過 web browser 存取。

## Command-line interfaces

A CLI is a text-based interface that allows you to control and interact with a device by entering commands, which are lines of text. A famous CLI you might have seen before is the Windows Command Prompt, as pictured in figure 5.2. Although the vast majority of users use the GUI exclusively (or almost exclusively), the Command Prompt CLI provides an alternative way to interact with the PC.

> [!translation] 逐句繁體中文翻譯
> CLI 是 text-based interface，讓你透過輸入 commands，也就是一行行文字，來控制 device 並與 device 互動。  
> 你以前可能看過的一個知名 CLI 是 Windows Command Prompt，如 Figure 5.2 所示。  
> 雖然絕大多數 users 只使用，或幾乎只使用 GUI，但 Command Prompt CLI 提供了另一種與 PC 互動的方式。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-078_556_976_491_352.jpg)
Figure 5.2 The Command Prompt CLI of a Windows PC, accessed from within the Windows shell GUI

> [!translation] 逐句繁體中文翻譯
> Figure 5.2：Windows PC 的 Command Prompt CLI，從 Windows shell GUI 內存取。

For the CCNA exam, you must be familiar with the CLI of Cisco routers and switches running Cisco IOS. For those with no prior experience with a CLI (such as myself when I started my CCNA studies in 2018), this can seem intimidating. However, by the end of this chapter, I hope you will see that navigating around the Cisco IOS CLI isn't so complicated.

> [!translation] 逐句繁體中文翻譯
> 對 CCNA exam 來說，你必須熟悉執行 Cisco IOS 的 Cisco routers 與 switches 的 CLI。  
> 對於沒有 CLI 經驗的人，例如我在 2018 年開始學 CCNA 時，這可能看起來很嚇人。  
> 不過，到本章結束時，我希望你會發現操作 Cisco IOS CLI 並沒有那麼複雜。

EXAM TIP Throughout the two volumes of this book, I will introduce various CLI commands to configure the protocols you must know for the CCNA exam. Hands-on practice with these commands-for example, using Cisco Packet Tracer-is an essential part of preparing for the CCNA exam.

> [!translation] 逐句繁體中文翻譯
> 考試提示：在本書兩冊中，我會介紹各種 CLI commands，用來設定 CCNA exam 必須知道的 protocols。  
> 實際動手練習這些 commands，例如使用 Cisco Packet Tracer，是準備 CCNA exam 的必要部分。

### 5.1.2 Accessing the CLI of a Cisco device

To configure Cisco devices, you first have to connect your computer to the device to access the CLI. There are two main methods to do so:

> [!translation] 逐句繁體中文翻譯
> 若要設定 Cisco devices，你首先必須將 computer 連接到 device，以存取 CLI。  
> 主要有兩種方法可以做到這件事：

- Connect a PC/laptop to the console port of the device with a console cable.
- Connect to the device over the network using a protocol like Telnet or Secure Shell (SSH).

We will cover Telnet and SSH in chapter 5, volume 2. Until then, we will focus on connections via the console port of the device. The console port is a physical port that
allows you to connect a computer directly to the device (as opposed to connecting via the network infrastructure). In order to do so, you must be physically near the device; a console cable is typically only a few feet in length.

> [!translation] 逐句繁體中文翻譯
> 我們會在 Volume 2 Chapter 5 涵蓋 Telnet 與 SSH。  
> 在那之前，我們會聚焦於透過 device 的 console port 連線。  
> Console port 是一個 physical port，讓你可以將 computer 直接連接到 device，而不是透過 network infrastructure 連線。  
> 若要這樣做，你必須實際靠近該 device；console cable 通常只有幾英尺長。

NOTE Console ports cannot be used to communicate over the network. They are dedicated to configuring the device via the CLI.

> [!translation] 逐句繁體中文翻譯
> 注意：Console ports 不能用來透過 network 通訊。  
> 它們專門用於透過 CLI 設定 device。

Figure 5.3 shows two console ports on a Cisco switch: USB Mini-B and RJ45. The exact type of console ports available depends on the model of the device, but USB Mini-B and RJ45 are common across many different Cisco router and switch models. You can connect to either port but not both; only one console connection is supported at a time.

> [!translation] 逐句繁體中文翻譯
> Figure 5.3 顯示 Cisco switch 上的兩個 console ports：USB Mini-B 與 RJ45。  
> 可用的 console port 確切類型取決於 device model，但 USB Mini-B 與 RJ45 在許多不同 Cisco router 與 switch models 上都很常見。  
> 你可以連接其中任一個 port，但不能同時連接兩個；一次只支援一個 console connection。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-079_381_757_723_320.jpg)
Figure 5.3 Two console ports on a Cisco switch: USB Mini-B (left) and RJ45 (right)

> [!translation] 逐句繁體中文翻譯
> Figure 5.3：Cisco switch 上的兩個 console ports：USB Mini-B（左）與 RJ45（右）。

Console cables come in a variety of types with a variety of different connectors. The type used depends on the ports available on the device itself-the PC connecting to it. Perhaps the simplest option is to use a standard USB cable to connect your PC to the device's USB console port (make sure the cable has the correct USB connector types for your PC and the device you want to connect to).

> [!translation] 逐句繁體中文翻譯
> Console cables 有多種類型，也有多種不同 connectors。  
> 使用哪種類型取決於 device 本身與連接它的 PC 上有哪些 ports。  
> 最簡單的選項可能是使用標準 USB cable，將你的 PC 連接到 device 的 USB console port；請確認 cable 具有適合你的 PC 與目標 device 的正確 USB connector types。

To connect to the RJ45 console port, you must use a rollover cable. This is a different pattern than the straight-through and crossover cables we covered in chapter 3; rollover cables are wired as follows:

> [!translation] 逐句繁體中文翻譯
> 若要連接 RJ45 console port，你必須使用 rollover cable。  
> 這種接線 pattern 與 Chapter 3 中涵蓋的 straight-through 與 crossover cables 不同；rollover cables 的接線如下：

- Pin 1 to pin 8
- Pin 2 to pin 7
- Pin 3 to pin 6
- Pin 4 to pin 5
- Pin 5 to pin 4
- Pin 6 to pin 3
- Pin 7 to pin 2
- Pin 8 to pin 1

The wiring of a rollover cable is illustrated in figure 5.4.

> [!translation] 逐句繁體中文翻譯
> Rollover cable 的接線方式如 Figure 5.4 所示。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-080_316_759_266_360.jpg)
Figure 5.4 The wiring of a rollover cable, used to connect a PC to the RJ45 console port of a network device. Pin 1 on one end connects to pin 8 on the other end, pin 2 to pin 7, pin 3 to pin 6, pin 4 to pin 5, pin 5 to pin 4, pin 6 to pin 3, pin 7 to pin 2, pin 8 to pin 1.

> [!translation] 逐句繁體中文翻譯
> Figure 5.4：Rollover cable 的接線，用於將 PC 連接到 network device 的 RJ45 console port。  
> 一端的 Pin 1 連到另一端的 Pin 8，Pin 2 連到 Pin 7，Pin 3 連到 Pin 6，Pin 4 連到 Pin 5，Pin 5 連到 Pin 4，Pin 6 連到 Pin 3，Pin 7 連到 Pin 2，Pin 8 連到 Pin 1。

After physically connecting your PC to the device's console port, you then need to use a type of application called a terminal emulator to access the CLI. A terminal emulator is a software application that replicates the functions of a computer terminal-an old hardware device consisting of a monitor and keyboard that was used to input data into (and receive and display data from) a computer. A popular (and free) terminal emulator on Windows is PuTTY (www.putty.org), but there are many options available for a variety of platforms.

> [!translation] 逐句繁體中文翻譯
> 將 PC 實體連接到 device 的 console port 之後，你還需要使用一種稱為 terminal emulator 的 application 來存取 CLI。  
> Terminal emulator 是一種 software application，用來模擬 computer terminal 的功能；computer terminal 是一種舊式硬體設備，由 monitor 與 keyboard 組成，用來向 computer 輸入 data，以及從 computer 接收並顯示 data。  
> Windows 上一個常見且免費的 terminal emulator 是 PuTTY（www.putty.org），但不同 platforms 上也有許多其他選項。

When using a terminal emulator to connect from a PC to a device's console port, there are a few settings you will have to configure. Those are

> [!translation] 逐句繁體中文翻譯
> 使用 terminal emulator 從 PC 連到 device 的 console port 時，你需要設定幾個項目。  
> 這些項目是：

- Speed-The rate at which data is sent
- Data bits-The number of bits of information used for each character of text sent to the device
- Stop bits-Sent after every character to allow the receiving device to detect the end of the character
- Parity-An extra bit sent with each character to be used for error detection
- Flow control-Provides support for circumstances where a device sends data faster than the receiver can handle

The appropriate value for each setting depends on the device you are configuring; to learn the appropriate settings for a particular device, you will have to check the manufacturer's documentation for that device. Figure 5.5 shows how to initiate a console connection to a Cisco device in PuTTY.

> [!translation] 逐句繁體中文翻譯
> 每個設定的適當值取決於你正在設定的 device。  
> 若要知道特定 device 的正確設定，你必須查看該 device 製造商的 documentation。  
> Figure 5.5 顯示如何在 PuTTY 中啟動到 Cisco device 的 console connection。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-081_698_830_185_320.jpg)
Figure 5.5 How to use PuTTY to access a Cisco device's CLI via the console port. From the Serial tab, configure the following settings, and then click Open: (1) Speed (baud): 9600 bits per second, (2) Data bits: 8, (3) Stop bits: 1, (4) Parity: None, (5) Flow control: None.

> [!translation] 逐句繁體中文翻譯
> Figure 5.5：如何使用 PuTTY 透過 console port 存取 Cisco device 的 CLI。  
> 從 Serial tab 設定以下項目，然後按 Open：（1）Speed（baud）：每秒 9600 bits，（2）Data bits：8，（3）Stop bits：1，（4）Parity：None，（5）Flow control：None。

NOTE You won't be tested on how to use PuTTY or another terminal emulator to connect to a device's console port in the CCNA exam, but I am including this information just in case you have physical hardware to practice on. To get hands-on lab practice for the CCNA, I recommend Cisco Packet Tracer, in which you can simply click on a device's icon to access the CLI.

> [!translation] 逐句繁體中文翻譯
> 注意：CCNA exam 不會考你如何使用 PuTTY 或其他 terminal emulator 連到 device 的 console port，但我仍納入這些資訊，以防你有實體硬體可以練習。  
> 若要進行 CCNA hands-on lab practice，我推薦 Cisco Packet Tracer；在其中你只要點擊 device icon，就可以存取 CLI。

### 5.2 Navigating the Cisco IOS CLI

Now we will finally get hands-on in the Cisco IOS CLI, navigating through different modes and giving commands to a Cisco device. I want to emphasize once again that networking is not just theory but also a practical skill. It will be difficult to absorb this information without putting it into practice yourself, so I highly recommend following along in Packet Tracer (or the CLI of a real Cisco router or switch) as you read and trying out the different commands and shortcuts we cover.

> [!translation] 逐句繁體中文翻譯
> 現在，我們終於要在 Cisco IOS CLI 中實際操作，切換不同 modes，並向 Cisco device 下 commands。  
> 我想再次強調，networking 不只是理論，也是一項實作技能。  
> 如果不親自實作，這些資訊會很難吸收，所以我強烈建議你閱讀時跟著在 Packet Tracer 中操作，或使用真實 Cisco router / switch 的 CLI，並嘗試本章涵蓋的不同 commands 與 shortcuts。

When you first access the CLI of a new Cisco device, you are given the option to configure the device using the system configuration dialog, as shown in the following example:

> [!translation] 逐句繁體中文翻譯
> 當你第一次存取新 Cisco device 的 CLI 時，系統會提供一個選項，讓你使用 system configuration dialog 設定 device，如下例所示：

```
--- System Configuration Dialog ---
Would you like to enter the initial configuration dialog? [yes/no]: no
```

NOTE In the CLI output shown in this book, bold text indicates commands typed by the user. Normal text indicates the output shown by the device.

> [!translation] 逐句繁體中文翻譯
> 注意：本書顯示的 CLI output 中，粗體文字表示 user 輸入的 commands。  
> 一般文字表示 device 顯示的 output。

The system configuration dialog is a step-by-step configuration wizard that allows you to do a simple setup of the device without having to know Cisco IOS CLI commands. This feature is typically not used, and it's not something you need to know for the CCNA, so I recommend skipping it by typing no and pressing the Enter key (the options [yes/ no] are shown in square brackets).

> [!translation] 逐句繁體中文翻譯
> System configuration dialog 是逐步式 configuration wizard，讓你不必知道 Cisco IOS CLI commands，也能對 device 做簡單 setup。  
> 這個功能通常不會使用，也不是 CCNA 需要知道的內容，所以我建議輸入 no 並按 Enter 來跳過它；方括號中的 [yes/no] 表示可選項目。

### 5.2.1 The EXEC modes

After skipping the system configuration dialog, you are shown a prompt like the following, where you can type commands and press Enter to send them to the device. The format of the prompt is the hostname (in this case, Router, the default hostname of Cisco routers) followed by a greater-than sign. This indicates that you are in user EXEC mode:

> [!translation] 逐句繁體中文翻譯
> 跳過 system configuration dialog 後，你會看到如下 prompt，可以在其中輸入 commands 並按 Enter 將它們送給 device。  
> Prompt 的格式是 hostname 後面接一個 greater-than sign；在這個例子中，Router 是 Cisco routers 的預設 hostname。  
> 這表示你目前位於 user EXEC mode：

```
The hostname followed by a greater-than
sign indicates user EXEC mode.
```

NOTE All of the commands we cover in this chapter apply to both Cisco routers and switches. They both run the same operating system: Cisco IOS.

> [!translation] 逐句繁體中文翻譯
> 注意：本章涵蓋的所有 commands 都同時適用於 Cisco routers 與 switches。  
> 它們都執行相同的 operating system：Cisco IOS。

User EXEC mode is the least-privileged mode in the Cisco IOS command hierarchy; it allows you to enter some basic commands to view information about the device's configuration and status. However, it does not allow you to do anything intrusive like make any changes to the device's configuration, restart the device, etc. To demonstrate a simple command that you can use in user EXEC mode, I type show clock and press Enter. The router then displays the current time of its clock:

> [!translation] 逐句繁體中文翻譯
> User EXEC mode 是 Cisco IOS command hierarchy 中權限最低的 mode；它允許你輸入一些基本 commands，查看 device configuration 與 status 的資訊。  
> 然而，它不允許你執行具侵入性的動作，例如變更 device configuration、重新啟動 device 等等。  
> 為了示範 user EXEC mode 中可用的一個簡單 command，我輸入 show clock 並按 Enter。  
> Router 接著會顯示其 clock 的目前時間：

```
Router> show clock
*02:21:03.832 UTC Fri Feb 10 2023
```

Views the time of the device's clock

> [!translation] 逐句繁體中文翻譯
> 查看 device clock 的時間。

EXAM TIP There are a variety of show commands that you will become familiar with throughout this book. Learning the available show commands and how to interpret their output is a major part of studying for the CCNA.

> [!translation] 逐句繁體中文翻譯
> 考試提示：本書中你會逐漸熟悉各種 show commands。  
> 學會可用的 show commands，以及如何解讀其 output，是準備 CCNA 的重要部分。

Checking the time is clearly not intrusive, so the show clock command is available in user EXEC mode. However, a more intrusive command like reload, which restarts the device, does not work in user EXEC mode, as shown in the following example. The router displays an error message instead (a percent sign indicates a message from IOS):

> [!translation] 逐句繁體中文翻譯
> 查看時間顯然不是侵入性操作，所以 show clock command 可在 user EXEC mode 中使用。  
> 然而，像 reload 這種會重新啟動 device 的較侵入性 command，不能在 user EXEC mode 中執行，如下例所示。  
> Router 會改為顯示 error message；percent sign 表示來自 IOS 的 message：

```
Router> reload
% Unknown command or computer name, or unable to find computer address
```

To access more powerful commands, you must enter the next mode in the IOS command hierarchy: privileged EXEC mode. To access privileged EXEC mode, use the enable command. From privileged EXEC mode, the reload command now works:
![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-083_308_1305_365_316.jpg)

> [!translation] 逐句繁體中文翻譯
> 若要存取更強大的 commands，你必須進入 IOS command hierarchy 中的下一個 mode：privileged EXEC mode。  
> 若要進入 privileged EXEC mode，請使用 enable command。  
> 在 privileged EXEC mode 中，reload command 現在可以執行：

NOTE The greater-than sign (>) in the prompt changes to a hash (\#) when in privileged EXEC mode.

> [!translation] 逐句繁體中文翻譯
> 注意：進入 privileged EXEC mode 後，prompt 中的大於符號（>）會變成井字號（\#）。

Privileged EXEC mode gives unlimited access to the available show commands as well as many other commands to control various features of the device. To return to user EXEC mode from privileged EXEC mode, you can use the disable command. However, disable is rarely used because there are no commands in user EXEC mode that you can't use in privileged EXEC mode; there's rarely a need to return to user EXEC mode.

> [!translation] 逐句繁體中文翻譯
> Privileged EXEC mode 可讓你不受限制地使用可用的 show commands，也能使用許多其他 commands 來控制 device 的各種功能。  
> 若要從 privileged EXEC mode 回到 user EXEC mode，可以使用 disable command。  
> 不過，disable 很少被使用，因為 user EXEC mode 中沒有任何 command 是 privileged EXEC mode 不能使用的；通常很少需要回到 user EXEC mode。

Although privileged EXEC mode is more powerful than user EXEC mode, both modes are limited in that they do not allow you to make changes to the device's configuration. The EXEC modes only allow you to view the device's status and configuration, as well as execute operational commands to perform actions like restart the device, save the configuration, move and delete files, etc.

> [!translation] 逐句繁體中文翻譯
> 雖然 privileged EXEC mode 比 user EXEC mode 更強大，但兩者都有一個限制：它們都不允許你修改 device 的 configuration。  
> EXEC modes 只允許你查看 device 的 status 與 configuration，並執行 operational commands，例如重新啟動 device、儲存 configuration、移動與刪除 files 等。

### 5.2.2 Global configuration mode

To make changes to the configuration of the device, we must leave the EXEC modes and proceed to the next mode in the IOS command hierarchy: global configuration mode. To do so, use the configure terminal command from privileged EXEC mode:
![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-083_285_1229_1583_316.jpg)
Although there are only two EXEC modes in the Cisco CLI (user EXEC mode and privileged EXEC mode), there are several configuration modes that we will examine throughout this book. In this chapter, we will only look at the first one: global configuration mode. From global configuration mode, you can configure various features like the
device's hostname and passwords. From this mode, you can also access the other configuration modes that we will look at in later chapters of this book's two volumes.

> [!translation] 逐句繁體中文翻譯
> 若要修改 device 的 configuration，我們必須離開 EXEC modes，並前往 IOS command hierarchy 中的下一個 mode：global configuration mode。  
> 若要這麼做，請在 privileged EXEC mode 中使用 configure terminal command：  
> 雖然 Cisco CLI 中只有兩種 EXEC modes（user EXEC mode 與 privileged EXEC mode），但還有好幾種 configuration modes，本書後續都會介紹。  
> 在本章中，我們只先看第一種：global configuration mode。  
> 在 global configuration mode 中，你可以設定多種功能，例如 device 的 hostname 與 passwords。  
> 從這個 mode，你也可以進入本書兩冊後續章節會介紹的其他 configuration modes。

One configuration that you can make from global configuration mode is to change the hostname of the device with the hostname command, as shown in the following example. Notice that after executing the command, the prompt changes from Router to R1, indicating that the hostname has changed. The command takes effect immediately. Configuring a unique hostname on each device in the network is essential to make them easy to identify. For the purpose of this book, we will use simple numerical identifiers (R1, R2, etc.). In a real enterprise network, other information, such as the device's location, is often included in the hostname (i.e., Office1_R1):
![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-084_202_999_666_348.jpg)

> [!translation] 逐句繁體中文翻譯
> 你可以在 global configuration mode 中做的一項設定，是使用 hostname command 變更 device 的 hostname，如下例所示。  
> 請注意，執行 command 之後，prompt 會從 Router 變成 R1，表示 hostname 已經改變。  
> 這個 command 會立即生效。  
> 為網路中的每台 device 設定唯一 hostname 很重要，這樣它們才容易辨識。  
> 在本書中，我們會使用簡單的數字識別名稱（R1、R2 等）。  
> 在真實的企業網路中，hostname 通常也會包含其他資訊，例如 device 的位置（例如 Office1_R1）：

NOTE If you want to undo a configuration command, you can use no in front of the command. For example, after the hostname R1 command, no hostname R1 would remove the command and revert the device's hostname to the default of Router.

> [!translation] 逐句繁體中文翻譯
> 注意：如果你想取消某個 configuration command，可以在 command 前面加上 no。  
> 例如，在 hostname R1 command 之後，no hostname R1 會移除該 command，並將 device 的 hostname 還原為預設的 Router。

To return from global configuration mode to privileged EXEC mode, there are a few options. The end command, the Ctrl-C keyboard shortcut, and the Ctrl-Z keyboard shortcut will return you to privileged EXEC mode from global configuration mode or any other configuration mode. The exit command will return you to privileged EXEC mode from global configuration mode. However, if you're in another configuration mode, it will return you to global configuration mode. Figure 5.6 shows how to navigate between user EXEC mode, privileged EXEC mode, and global configuration mode.

> [!translation] 逐句繁體中文翻譯
> 若要從 global configuration mode 回到 privileged EXEC mode，有幾種選項。  
> end command、Ctrl-C keyboard shortcut，以及 Ctrl-Z keyboard shortcut，都可以讓你從 global configuration mode 或任何其他 configuration mode 回到 privileged EXEC mode。  
> exit command 會讓你從 global configuration mode 回到 privileged EXEC mode。  
> 不過，如果你在另一種 configuration mode 中，exit 會讓你回到 global configuration mode。  
> Figure 5.6 顯示如何在 user EXEC mode、privileged EXEC mode 與 global configuration mode 之間切換。

NOTE If you use the Ctrl-Z shortcut in the middle of typing a command, the device will execute the typed command before returning to privileged EXEC mode; it's equivalent to pressing Enter and then issuing end. Be careful! Ctrl-C does not do this; it will just return you to privileged EXEC mode.

> [!translation] 逐句繁體中文翻譯
> 注意：如果你在輸入 command 的途中使用 Ctrl-Z shortcut，device 會先執行已輸入的 command，然後才回到 privileged EXEC mode；這等同於按下 Enter 後再輸入 end。  
> 請小心！  
> Ctrl-C 不會這麼做；它只會讓你回到 privileged EXEC mode。

![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-084_257_1208_1751_350.jpg)
Figure 5.6 How to navigate between user EXEC mode, privileged EXEC mode, and global configuration mode in the Cisco IOS command hierarchy

> [!translation] 逐句繁體中文翻譯
> Figure 5.6：如何在 Cisco IOS command hierarchy 中的 user EXEC mode、privileged EXEC mode 與 global configuration mode 之間切換。

Configuration modes such as global configuration mode allow you to configure the device, but EXEC mode commands like show do not work. However, the do command allows you to use EXEC mode commands from a configuration mode, so you don't have to return to privileged EXEC mode. This can speed up your workflow when you are configuring a device but also want to use show commands to check its status. The following example demonstrates this; the show clock command results in an error message, but the do show clock command displays the time of the device's clock:

> [!translation] 逐句繁體中文翻譯
> 像 global configuration mode 這類 configuration modes 可讓你設定 device，但像 show 這類 EXEC mode commands 在其中無法運作。  
> 不過，do command 可讓你在 configuration mode 中使用 EXEC mode commands，因此不需要先回到 privileged EXEC mode。  
> 當你正在設定 device、但也想使用 show commands 檢查 status 時，這可以加快工作流程。  
> 下列範例示範這件事；show clock command 會產生 error message，但 do show clock command 會顯示 device clock 的時間：

```
R1(config)# show clock
        ^
```

Show commands don't work

> [!translation] 逐句繁體中文翻譯
> Show commands 無法運作。

```
% Invalid input detected at '^' marker.
```

in global configuration mode.

> [!translation] 逐句繁體中文翻譯
> 在 global configuration mode 中。

```
R1(config)# do show clock
*03:06:22.892 UTC Fri Feb 10 2023
```


### 5.2.3 Keyboard shortcuts

There are several keyboard shortcuts that can help you more smoothly navigate through the CLI and enter commands. We covered two in the previous section; Ctrl-C and Ctrl-Z can be used to return to privileged EXEC mode from any configuration mode. There are many others, and we will look at a few of them next.

> [!translation] 逐句繁體中文翻譯
> 有幾個 keyboard shortcuts 可以幫助你更順暢地在 CLI 中移動並輸入 commands。  
> 前一節已經介紹兩個；Ctrl-C 與 Ctrl-Z 可用來從任何 configuration mode 回到 privileged EXEC mode。  
> 還有許多其他 shortcuts，接下來我們會看其中幾個。

When typing commands in the CLI, there is a cursor indicating where the next character will be inserted when typed. By default, this will be after the previous character, as you would probably expect. You can also move the cursor, for example, to fix an error in a previously typed word. The following are some keyboard shortcuts that can be used to move the cursor and edit the current command you are typing:

> [!translation] 逐句繁體中文翻譯
> 在 CLI 中輸入 commands 時，cursor 會指出下一個輸入字元會被插入的位置。  
> 預設情況下，它會位於前一個字元之後，這應該符合你的預期。  
> 你也可以移動 cursor，例如修正先前輸入的單字錯誤。  
> 以下是一些可用來移動 cursor 與編輯目前正在輸入 command 的 keyboard shortcuts：

- Left arrow-Moves the cursor left
- Right arrow-Moves the cursor right
- Backspace-Moves the cursor left and deletes the previous character
- Ctrl-A-Moves the cursor to the beginning of the command you are typing
- Ctrl-E-Moves the cursor to the end of the command you are typing
- Ctrl-U-Deletes all characters to the left of the cursor

You can also use the keyboard to view previously executed commands, which Cisco IOS stores in a memory buffer. This is useful if you made a mistake in a previous command and want to correct it without typing out the entire command again; you can return to the previous command, fix the error, and then execute the command again. You can use the following shortcuts to scroll through the buffer:

> [!translation] 逐句繁體中文翻譯
> 你也可以使用 keyboard 查看先前執行過的 commands，Cisco IOS 會把它們儲存在 memory buffer 中。  
> 如果你在前一個 command 中打錯，並想在不重新輸入整個 command 的情況下修正，這會很有用；你可以回到前一個 command、修正錯誤，然後再次執行。  
> 你可以使用下列 shortcuts 在 buffer 中捲動：

- Up arrow-Previous command
- Down arrow-Next command

### 5.2.4 Context-sensitive help

You will have to learn many different commands to prepare for the CCNA, and those commands are only a fraction of all of the available commands in Cisco IOS. For the purpose of the CCNA exam, it is important to practice and become familiar with the various commands we will look at in this book. However, Cisco IOS has a feature called context-sensitive help that can help you if you have forgotten a command.

> [!translation] 逐句繁體中文翻譯
> 為了準備 CCNA，你必須學會許多不同 commands，而這些 commands 只佔 Cisco IOS 所有可用 commands 的一小部分。  
> 以 CCNA exam 為目的，練習並熟悉本書會介紹的各種 commands 很重要。  
> 不過，如果你忘記某個 command，Cisco IOS 有一項稱為 context-sensitive help 的功能可以幫助你。

Viewing the available commands
A question mark (?) can be used for help in the Cisco IOS CLI in a few ways:

> [!translation] 逐句繁體中文翻譯
> 查看可用 commands。  
> 在 Cisco IOS CLI 中，question mark（?）可以用幾種方式提供 help：

- To list the available commands in the current EXEC or configuration mode
- To list the keywords available for a command
- To list the possible completions of a partially typed command or keyword

In the first use case, the question mark is used to list the commands available in the current mode of the CLI hierarchy, along with a brief description of each command. Note that you don't have to press Enter; the list of commands is shown immediately after typing the question mark. The first few commands available in user EXEC mode are as follows:
![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-086_347_1279_1008_347.jpg)

> [!translation] 逐句繁體中文翻譯
> 在第一種用法中，question mark 用來列出目前 CLI hierarchy mode 中可用的 commands，並附上每個 command 的簡短說明。  
> 請注意，你不需要按 Enter；輸入 question mark 後，commands 清單會立即顯示。  
> user EXEC mode 中前幾個可用 commands 如下：

Few Cisco IOS commands are a single word; most commands include one or more keywords, which are further parameters typed after the initial command. The show command we looked at previously is an example of this; show on its own is not a valid command, but show clock is. In the second use case of the question mark, you can use it after a command to view the available keywords. The following example demonstrates this:

> [!translation] 逐句繁體中文翻譯
> 很少 Cisco IOS commands 只有單一個字；多數 commands 會包含一個或多個 keywords，也就是在初始 command 後面輸入的進一步參數。  
> 我們先前看過的 show command 就是例子；單獨輸入 show 並不是有效 command，但 show clock 是。  
> 在 question mark 的第二種用法中，你可以把它放在 command 後面，以查看可用的 keywords。  
> 下列範例示範這種用法：

```
            show is not a valid command on its own.
R1> show
% Type "show ?" for a list of subcommands
R1> show ?
    aaa Show AAA values
    arp ARP table
    auto Show Automation Template
    call-home Show command for call home
    capability Capability Information
. . .
```

You can also use the question mark in this manner after a keyword to display any further keywords. For example, show clock ? lists the keyword detail, which can be used to view more information about the device's clock. This is shown in the following example:

> [!translation] 逐句繁體中文翻譯
> 你也可以用同樣方式把 question mark 放在 keyword 後面，以顯示後續可用的 keywords。  
> 例如，show clock ? 會列出 keyword detail，可用來查看 device clock 的更多資訊。  
> 這如下例所示：

```
R1> show clock ?
    detail Display detailed information
    | Output modifiers
    <cr> <cr>
```

Views the available options after show clock

> [!translation] 逐句繁體中文翻譯
> 查看 show clock 後方可用的 options。

The other two options displayed are also worth mentioning:

> [!translation] 逐句繁體中文翻譯
> 顯示出來的另外兩個 options 也值得一提：

- The pipe (|) can be used to filter the output of a show command. I will show an example of this later in this chapter.
- "cr"means carriage return, which refers to the Enter key. This means that you can simply press Enter to execute the command. Although a keyword (detail) is available, show clock on its own is a valid command.

The third use case for the question mark is to display the possible completions of a partially typed command or keyword. In this case, the question mark should be typed immediately after the partially typed command, without a space. For example, typing e? in user EXEC mode will list multiple commands that begin with e. Typing en?, on the other hand, will show that enable is the only command that begins with en, as shown in the following example:

> [!translation] 逐句繁體中文翻譯
> question mark 的第三種用法，是顯示部分輸入 command 或 keyword 的可能補完。  
> 在這種情況下，question mark 應該緊接在部分輸入的 command 後面輸入，中間不要有空格。  
> 例如，在 user EXEC mode 中輸入 e? 會列出多個以 e 開頭的 commands。  
> 相對地，輸入 en? 會顯示 enable 是唯一以 en 開頭的 command，如下例所示：

```
R1> e?
```

In user EXEC mode, three

> [!translation] 逐句繁體中文翻譯
> 在 user EXEC mode 中，有三個。

```
enable ethernet exit
```

commands begin with e.

> [!translation] 逐句繁體中文翻譯
> commands 以 e 開頭。

```
R1> en?
enable
```


## Auto-completing commands

Typing various commands can be tedious when manually configuring a device. Fortunately, Cisco IOS does not require you to type full commands; it only requires you to type enough characters so that there is only one possible command that begins with those characters.

> [!translation] 逐句繁體中文翻譯
> 手動設定 device 時，輸入各種 commands 可能會很繁瑣。  
> 幸好，Cisco IOS 不要求你輸入完整 commands；它只要求你輸入足夠字元，使得只有一個可能的 command 以這些字元開頭。

If you type enough characters so that there is only one possible command beginning with those characters and then press the Tab key, IOS will automatically complete the command for you. For example, typing en and then pressing Tab will automatically complete the command to enable. Then you can simply press Enter to execute the command. However, if you don't type enough characters and there are multiple possible commands beginning with the character(s) you have typed, the command won't work; it will simply print the character(s) again on a new line. This is shown in the following example. Note that "Tab" indicates where I pressed the Tab key:
![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-088_232_1020_188_346.jpg)

> [!translation] 逐句繁體中文翻譯
> 如果你輸入足夠字元，使得只有一個可能的 command 以這些字元開頭，然後按下 Tab key，IOS 會自動幫你補完整個 command。  
> 例如，輸入 en 後按下 Tab，會自動將 command 補完為 enable。  
> 接著你只要按 Enter 就能執行 command。  
> 不過，如果你輸入的字元不夠，且有多個可能的 commands 以你輸入的字元開頭，command 不會成功；它只會在新的一行再次印出這些字元。  
> 下列範例顯示這件事。  
> 請注意，「Tab」表示我按下 Tab key 的位置：

But wait, there's more: you don't even have to use Tab to complete the command. Using the previous example of the enable command, if you type e and then press Enter to execute the command, the terminal will display an error message stating that e is an ambiguous command. That is because there are multiple possible commands beginning with e. However, if you type en and press Enter, the command is accepted as enable, and you are brought to privileged EXEC mode:
![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-088_196_978_748_346.jpg)

> [!translation] 逐句繁體中文翻譯
> 但還不只如此：你甚至不一定要用 Tab 來補完整個 command。  
> 以前面的 enable command 為例，如果你輸入 e 後按 Enter 執行 command，terminal 會顯示 error message，指出 e 是 ambiguous command。  
> 這是因為有多個可能的 commands 以 e 開頭。  
> 不過，如果你輸入 en 後按 Enter，command 會被接受為 enable，並把你帶到 privileged EXEC mode：

NOTE Auto-completion with Tab and executing partial commands both apply to a command's keywords too. For example, conf t can be used instead of configure terminal to enter global configuration mode.

> [!translation] 逐句繁體中文翻譯
> 注意：使用 Tab 自動補完，以及執行部分 command，兩者也都適用於 command 的 keywords。  
> 例如，可以用 conf t 取代 configure terminal，以進入 global configuration mode。

Table 5.1 summarizes these context-sensitive help features. Spend some time experimenting with them in the CLI; once you get used to them, you will probably find yourself using them quite often as you practice configuring and verifying the various IOS features you need to know for the CCNA.

> [!translation] 逐句繁體中文翻譯
> Table 5.1 摘要整理這些 context-sensitive help features。  
> 花一些時間在 CLI 中實驗它們；一旦習慣之後，你在練習設定與驗證 CCNA 所需各種 IOS features 時，可能會發現自己常常使用它們。

Table 5.1 Cisco IOS context-sensitive help features

> [!translation] 逐句繁體中文翻譯
> Table 5.1：Cisco IOS context-sensitive help features。
| Command | Description |
| :--- | :--- |
| ? | Lists the available commands in the current mode |
| command ? | Lists the available keywords for the command |
| partial-command ? | Lists the possible commands beginning with the currently typed characters |
| partial-command"Tab" | Automatically completes the command if there is only one option beginning with the currently typed characters |
| partial-command"Enter" | Executes the command if there is only one option beginning with the currently typed characters |


### 5.3 IOS configuration files

Cisco IOS devices make use of two different text files that store the device's configurations: running-config and startup-config. The two files are each stored in different hardware memory and serve different purposes. You can view each configuration file with the show running-config and show startup-config commands.

> [!translation] 逐句繁體中文翻譯
> Cisco IOS devices 使用兩個不同的文字檔來儲存 device 的 configurations：running-config 與 startup-config。  
> 這兩個檔案分別儲存在不同的 hardware memory 中，並有不同用途。  
> 你可以使用 show running-config 與 show startup-config commands 查看各自的 configuration file。

NOTE The output of show running-config and show startup-config can be quite long. When the output of a command is beyond a certain length, only partial output will be shown with a prompt that says --More-- at the bottom. Use the Enter key to scroll through the output one line at a time or the spacebar to scroll through the output one screen at a time.

> [!translation] 逐句繁體中文翻譯
> 注意：show running-config 與 show startup-config 的 output 可能相當長。  
> 當某個 command 的 output 超過一定長度時，只會顯示部分 output，底部會出現 --More-- prompt。  
> 使用 Enter key 可以一次捲動一行 output，使用 spacebar 可以一次捲動一整個畫面。

The configurations in the running-config file determine the current operations of the device. When you enter a configuration command in the CLI, you are modifying the running-config file. Changes take effect instantly; as shown previously, after the hostname command is executed, the hostname of the device changes immediately.

> [!translation] 逐句繁體中文翻譯
> running-config file 中的 configurations 決定 device 目前的運作方式。  
> 當你在 CLI 中輸入 configuration command 時，你正在修改 running-config file。  
> 變更會立即生效；如先前所示，hostname command 執行後，device 的 hostname 會立刻改變。

The running config file is stored in random-access memory (RAM). It is important to note that the contents of RAM are lost when the device is powered off or restarted; therefore, changes to running-config are lost in either event. To save configuration changes so they persist even if the device is powered off or restarted, the startup-config file is used.

> [!translation] 逐句繁體中文翻譯
> running config file 儲存在 random-access memory（RAM）中。  
> 必須注意的是，device 關機或重新啟動時，RAM 的內容會遺失；因此，running-config 的變更在這兩種情況下都會消失。  
> 若要儲存 configuration changes，使它們即使在 device 關機或重新啟動後仍保留，就要使用 startup-config file。

The configurations in startup-config do not determine the current operations of the device. Rather, the startup-config is the configuration file that is loaded by the device when it boots up-for example, after being powered on or restarted. The contents of the startup-config file are copied to the running-config file in RAM when the device boots up.

> [!translation] 逐句繁體中文翻譯
> startup-config 中的 configurations 不決定 device 目前的運作。  
> 相反地，startup-config 是 device 開機時載入的 configuration file，例如在通電或重新啟動後。  
> device 開機時，startup-config file 的內容會被複製到 RAM 中的 running-config file。

The startup-config file is stored in a special type of RAM called nonvolatile RAM (NVRAM). The contents of NVRAM are kept even when the device is powered off or restarted, so to save changes made to the running-config file, the contents must be copied to startup-config. Otherwise, the device will have a factory-default configuration every time it boots up.

> [!translation] 逐句繁體中文翻譯
> startup-config file 儲存在一種特殊 RAM 中，稱為 nonvolatile RAM（NVRAM）。  
> 即使 device 關機或重新啟動，NVRAM 的內容也會保留；因此，若要儲存 running-config file 中的變更，必須將內容複製到 startup-config。  
> 否則，device 每次開機時都會使用 factory-default configuration。

DEFINITION Factory-default refers to the original state of the device as it is sent from the factory, before any configuration changes are made.

> [!translation] 逐句繁體中文翻譯
> 定義：Factory-default 指 device 從工廠出貨時的原始狀態，也就是尚未做任何 configuration changes 之前的狀態。

There are a few different commands (entered in privileged EXEC mode) that can be used to copy the contents of the running-config file to the startup-config file. The effect of each of these commands is the same, so it doesn't matter which one you use:

> [!translation] 逐句繁體中文翻譯
> 有幾個不同 commands（在 privileged EXEC mode 中輸入）可以用來將 running-config file 的內容複製到 startup-config file。  
> 這些 commands 的效果都相同，所以使用哪一個都可以：

- write
- write memory
- copy running-config startup-config

NOTE A new device that has booted up for the first time won't even have a startup-config file until you use one of these commands. If no startup-config file is present, the device uses the factory-default configuration.

> [!translation] 逐句繁體中文翻譯
> 注意：剛第一次開機的新 device，甚至還不會有 startup-config file，直到你使用其中一個 command。  
> 如果沒有 startup-config file，device 會使用 factory-default configuration。

If you want to return a device to its factory-default configuration, you can erase startupconfig and then restart the device with the reload command. Just as with saving the configuration, there are a few different commands you can use to delete startup-config:

> [!translation] 逐句繁體中文翻譯
> 如果你想讓 device 回到 factory-default configuration，可以清除 startup-config，然後使用 reload command 重新啟動 device。  
> 就像儲存 configuration 一樣，你也可以使用幾個不同 commands 來刪除 startup-config：

- write erase
- erase nvram:
- erase startup-config

### 5.4 Password-protecting privileged EXEC mode

Privileged EXEC mode not only allows a user to execute any of the available show commands to gather information about the device's configuration and status, but it also allows the user to access global configuration mode and make configuration changes to the device. Because of this, it's always a good idea to configure a password to prevent unauthorized users from accessing privileged EXEC mode. In this section, we will look at the enable password and its more secure version, the enable secret.

> [!translation] 逐句繁體中文翻譯
> Privileged EXEC mode 不只允許 user 執行任何可用的 show commands，以收集 device configuration 與 status 的資訊。  
> 它也允許 user 進入 global configuration mode，並對 device 進行 configuration changes。  
> 因此，設定 password 來防止未授權 users 進入 privileged EXEC mode，一直都是好做法。  
> 在本節中，我們會看 enable password 以及更安全的版本 enable secret。

### 5.4.1 Configuring the enable password

The enable password is a password that you must enter to access privileged EXEC mode. It's also the name of the command used to configure the password; you configure it with the enable password command in global configuration mode. After you configure the enable password, any time a user uses the enable command in user EXEC mode, the user will have to enter that password to access privileged EXEC mode.

> [!translation] 逐句繁體中文翻譯
> enable password 是進入 privileged EXEC mode 時必須輸入的 password。  
> 它也是用來設定該 password 的 command 名稱；你會在 global configuration mode 中使用 enable password command 進行設定。  
> 設定 enable password 後，任何 user 只要在 user EXEC mode 中使用 enable command，就必須輸入該 password 才能進入 privileged EXEC mode。

NOTE The enable password is case-sensitive: cisco and Cisco are two different passwords.

> [!translation] 逐句繁體中文翻譯
> 注意：enable password 區分大小寫；cisco 與 Cisco 是兩個不同 passwords。

In the following example, I configure an enable password of ccna, use exit to return to privileged EXEC mode, and use disable to return to user EXEC mode. When I then use enable to return to privileged EXEC mode again, I have to enter the configured enable password of ccna to gain access. Note that, for security purposes, passwords are not displayed as you type them in Cisco IOS:
![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-090_344_1189_1758_348.jpg)

> [!translation] 逐句繁體中文翻譯
> 在下列範例中，我設定 enable password 為 ccna，使用 exit 回到 privileged EXEC mode，並使用 disable 回到 user EXEC mode。  
> 接著當我再次使用 enable 回到 privileged EXEC mode 時，必須輸入已設定的 enable password：ccna，才能取得 access。  
> 請注意，基於安全考量，在 Cisco IOS 中輸入 password 時不會顯示出來：

There is a major problem with the enable password: it is stored in cleartext, meaning the exact password (ccna in this case) is stored in the configuration file as is. Anyone who can see running-config can read the password, and this is a major security concern. The following example demonstrates this: I use the command show running-config | include enable to view the enable password in running-config. The command is displayed exactly as I configured it, with the password in cleartext:
![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-091_226_1267_480_316.jpg)

> [!translation] 逐句繁體中文翻譯
> enable password 有一個重大問題：它會以 cleartext 儲存，也就是 exact password（此例為 ccna）原樣存放在 configuration file 中。  
> 任何能看到 running-config 的人都能讀到 password，這是重大的 security concern。  
> 下列範例示範這件事：我使用 command show running-config | include enable 來查看 running-config 中的 enable password。  
> 該 command 會完全照我設定的形式顯示，password 以 cleartext 呈現：

NOTE After a show command, a pipe (|) followed by the keyword include allows you to filter output to only show lines including the specified characters (enable, in this case).

> [!translation] 逐句繁體中文翻譯
> 注意：在 show command 後面加上 pipe（|）與 keyword include，可以過濾 output，只顯示包含指定字元的行（本例為 enable）。

To improve the security of enable password, you can use the service password -encryption command in global configuration mode. This encrypts all current passwords configured on the device, as well as passwords you configure in the future. The following example demonstrates this: after issuing the command and viewing running-config again, the original password is not shown. Instead, the password is stored as ciphertext (encrypted text):
![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-091_266_1264_1187_316.jpg)

> [!translation] 逐句繁體中文翻譯
> 若要改善 enable password 的安全性，可以在 global configuration mode 中使用 service password-encryption command。  
> 這會加密 device 上目前已設定的所有 passwords，以及你未來設定的 passwords。  
> 下列範例示範這件事：輸入 command 並再次查看 running-config 後，原始 password 不會顯示。  
> 相反地，password 會以 ciphertext（encrypted text）形式儲存：

NOTE The 7 before the ciphertext string 0307580507 indicates the encryption type.

> [!translation] 逐句繁體中文翻譯
> 注意：ciphertext string 0307580507 前面的 7 表示 encryption type。

The service password-encryption command encrypts passwords using type 7 encryption. It is a very weak form of encryption that is easily reversed with free tools available on the internet (a Google search for "cisco type 7 decrypt" will give many results). Although it does prevent someone from looking over your shoulder to read the password as you look at running-config, it does not provide sufficient protection. To provide improved security, you should use the enable secret instead.

> [!translation] 逐句繁體中文翻譯
> service password-encryption command 會使用 type 7 encryption 加密 passwords。  
> 這是一種非常弱的 encryption，很容易被網路上的免費工具還原（Google 搜尋「cisco type 7 decrypt」會出現許多結果）。  
> 雖然它可以防止別人在你查看 running-config 時從肩膀後方直接讀到 password，但它無法提供足夠保護。  
> 若要提升安全性，應該改用 enable secret。

NOTE If you use the no service password-encryption command to undo the encryption, currently encrypted passwords will not be decrypted. Future passwords, however, will not be encrypted.

> [!translation] 逐句繁體中文翻譯
> 注意：如果你使用 no service password-encryption command 取消 encryption，目前已加密的 passwords 不會被解密。  
> 不過，未來設定的 passwords 將不會被加密。

The enable password is an example of a legacy feature-something that has been replaced with a newer feature (the enable secret) but is still supported in Cisco IOS. The differences between the enable password and the enable secret are a potential exam question, but when configuring network devices, you should always use the enable secret.

> [!translation] 逐句繁體中文翻譯
> enable password 是 legacy feature 的例子，也就是已被較新功能（enable secret）取代，但 Cisco IOS 仍然支援的功能。  
> enable password 與 enable secret 的差異可能成為 exam question，但在設定 network devices 時，你應該一律使用 enable secret。

### 5.4.2 Configuring the enable secret

The enable secret is a more secure password that can be configured to protect access to privileged EXEC mode. It stores the password as a hash, rather than encrypted ciphertext. Hashing can be thought of as one-way encryption; it can't be reversed. The enable secret can be configured with the enable secret command in global configuration mode. In the following example, I configure an enable secret and view it in running-config:
![](./images/0f86a34f-6bf0-4f73-9c16-fc4f38df6c5c-092_266_1431_788_221.jpg)

> [!translation] 逐句繁體中文翻譯
> enable secret 是更安全的 password，可設定用來保護 privileged EXEC mode 的 access。  
> 它會將 password 儲存為 hash，而不是 encrypted ciphertext。  
> Hashing 可以視為 one-way encryption；它無法被反轉。  
> enable secret 可在 global configuration mode 中用 enable secret command 設定。  
> 在下列範例中，我設定 enable secret，並在 running-config 中查看它：

NOTE Notice that the enable password remains in the configuration. If both the enable password and enable secret are configured, only the enable secret can be used. The enable password command remains in the configuration but cannot be used to access privileged EXEC mode.

> [!translation] 逐句繁體中文翻譯
> 注意：請注意 enable password 仍保留在 configuration 中。  
> 如果同時設定 enable password 與 enable secret，只有 enable secret 可以使用。  
> enable password command 仍會留在 configuration 中，但不能用來進入 privileged EXEC mode。

The enable secret command hashes the specified password using the default hashing algorithm of the device. There are a few different hashing algorithms that can be used to hash the password, and the hashing algorithms available vary depending on the IOS version of the device. On the platform I am using for this demonstration, the algorithm type is scrypt (pronounced S-crypt), also known as type 9 (as indicated by the 9 before the hash in the previous example's output). On many older devices, the default algorithm is Message Digest 5 (MD5), also known as type 5. Type 5 is not as secure as type 9, so type 9 should be used when possible. In chapter 11, volume 2, we will examine the different hashing algorithms supported by Cisco IOS and how to configure secrets using specific hashing algorithms.

> [!translation] 逐句繁體中文翻譯
> enable secret command 會使用 device 的預設 hashing algorithm，對指定 password 進行 hash。  
> 有幾種不同 hashing algorithms 可用來 hash password，而且可用的 hashing algorithms 會依 device 的 IOS version 而不同。  
> 在我用於示範的平台上，algorithm type 是 scrypt（發音為 S-crypt），也稱為 type 9（如前一個範例 output 中 hash 前面的 9 所示）。  
> 在許多較舊 devices 上，預設 algorithm 是 Message Digest 5（MD5），也稱為 type 5。  
> Type 5 不如 type 9 安全，因此可行時應使用 type 9。  
> 在 volume 2 的 chapter 11 中，我們會檢視 Cisco IOS 支援的不同 hashing algorithms，以及如何使用特定 hashing algorithms 設定 secrets。

## Summary

- A shell is a computer program that allows a user to interact with the computer. A graphical user interface (GUI) is a shell with a graphical interface, and a command-line interface (CLI) is a shell with a text-based interface.
- For the CCNA exam, you must be able to use the Cisco IOS CLI to configure the protocols and features listed in the exam topics list.

- The CLI of a network device can be accessed by connecting a PC to the device's console port with a console (rollover) cable or by connecting over the network infrastructure using Telnet or Secure Shell (SSH).
- After physically connecting a PC to the device's console port, a terminal emulator application (such as PuTTY) is required to access the CLI.
- To give commands to a network device, you type commands in the CLI and press Enter.
- After connecting to a device's CLI, you will be in user EXEC mode, which only allows you to view basic information about the device but not perform anything intrusive. The format of the prompt is hostname>.
- To access more powerful commands, use the enable command to access privileged EXEC mode, which provides unlimited access to EXEC mode commands. For example, you can view information about the device, restart it, save the configuration, move and delete files, etc. The format of the prompt is hostname\#.
- Use the disable command to return to user EXEC mode from privileged EXEC mode.
- Use the reload command in privileged EXEC mode to restart the device.
- To make configuration changes to the device, use the configure terminal command in privileged EXEC mode to access global configuration mode. The prompt is hostname (config) \#.
- Global configuration mode allows you to make configuration changes to the device. It also allows you to access other configuration modes for specific features.
- To change the hostname of the device, use the hostname command in global configuration mode.
- To undo a command, use no in front of the command. For example, no hostname R1.
- Use the end command, the exit command, or the Ctrl-C/Ctrl-Z shortcuts to return to privileged EXEC mode from global configuration mode.
- When in a configuration mode, you can use do in front of a command to execute EXEC mode commands.
- Keyboard shortcuts can be used to move the cursor and scroll through previously executed commands.
- Context-sensitive help can be used for guidance within the CLI. It can list available commands and possible completions for partially written words.
- Cisco IOS devices use two configuration files: the running-config file and the startup-configfile.
- The running-config file is stored in RAM and determines the current operations of the device. Configuration commands change running-config and immediately take effect. The running-config file is lost when the device is powered off or restarted.

- The startup-config file is stored in nonvolatile RAM (NVRAM) and does not determine the current operations of the device. The contents of startup-config are copied to running-config when the device boots up.
- To save the running-config file to the startup-config file, use write, write memory, or copy running-config startup-config in privileged EXEC mode.
- To return the device to the factory-default configuration, delete startup-config with write erase, erase nvram:, or erase startup-config, and restart the device with reload.
- Privileged EXEC mode can be password-protected with an enable password or enable secret. If both the enable password and enable secret commands are configured, only the enable secret can be used to access privileged EXEC mode.
- The enable password can be configured with the enable password command in global configuration mode. It is stored in the configuration as cleartext by default but can be encrypted with the service password-encryption command (type 7).
- The enable password remains in Cisco IOS as a legacy feature, but on modern devices, the enable secret should be used instead.
- The enable secret can be configured with the enable secret command in global configuration mode. It is stored in the configuration as a hash, using one of multiple hashing algorithms. The hashing algorithms available vary depending on the IOS version.
- Message Digest 5 (MD5) is type 5 encryption, and scrypt is type 9. scrypt is more secure and should be used instead of MD5 if supported by the device.
