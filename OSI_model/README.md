要開始看這些傳輸協定之前，必須先瞭解我們到底在講一個communication中的哪一段，不然很容易失焦，像是你不會想問TCP/IP用的是哪一種型號的接頭，或是拿著DP9的RS-232接頭問說這條線IP是多少，而透過OSI 7 Layer Model，一個「概念模型」，可以幫助了解和分類各個通訊協定所在的分工位置多上層多底層或是他到底是一個多大scale的東西橫跨幾層。

# Open System Interconnection Model (OSI Model)
開放式系統互聯模型（Open System Interconnection Model, OSI）是一種概念模型，由國際標準化組織提出，一個試圖使各種電腦在世界範圍內互連為網路的標準框架。  
The Open Systems Interconnection model (OSI model) is a conceptual model that characterises and standardises the communication functions of a telecommunication or computing system without regard to its underlying internal structure and technology. Its goal is the interoperability of diverse communication systems with standard communication protocols.  
The model partitions the flow of data in a communication system into seven abstraction layers, from the physical implementation of transmitting bits across a communications medium to the highest-level representation of data of a distributed application. Each intermediate layer serves a class of functionality to the layer above it and is served by the layer below it. Classes of functionality are realized in software by standardized communication protocols. [1][2]

|      |   Layer  |  Protocol data unit (PDU)  |   Function   | Examples |
| ---- | -------- | -------------------------- | ------------ | -------- |
| Host layers |	7.Application | Data |	High-level APIs, including resource sharing, remote file access | DNS, FTP, HTTP, SMTP, Telnet, DHCP |
| Host layers | 6.Presentation| Data |  Translation of data between a networking service and an application; including character encoding, data compression and encryption/decryption | MIME, XDR, ASN.1, ASCII, PGP |
| Host layers | 5.Session 	  | Data |  Managing communication sessions, i.e., continuous exchange of information in the form of multiple back-and-forth transmissions between two nodes | Real-time Transport Protocol (RTP) |
| Host layers | 4.Transport 	|Segment, Datagram| Reliable transmission of data segments between points on a network, including segmentation, acknowledgement and multiplexing | TCP, UDP | 
| Media layers |3.Network 	| Packet | 	Structuring and managing a multi-node network, including addressing, routing and traffic control | IPv4, IPv6 |
| Media layers |2.Data link |	Frame  |	Reliable transmission of data frames between two nodes connected by a physical layer | IEEE 802.3(Ethernet), IEEE 802.11(WIFI), MAC, ISO 11898-1(CAN) |
| Media layers |1.Physical 	| Bit, Symbol |	Transmission and reception of raw bit streams over a physical medium | IEEE 802.3(Ethernet), IEEE 802.11(WIFI), USB, Bluetooth, RS-232, ISO 11898-2(CAN)|

<img src="https://raw.githubusercontent.com/shannon112/Notes/main/OSI_model/OSI_data_pyramid.png" width=400>

# Common Protocol in Robotics Application: Overview
|           |  I2C | SPI |  RS-232 RS-485 (UART based)|  CAN          | Ethernet    |  CANOpen | EtherCAT | Modbus |
| --------  | ---- | --- | -------------------------- | ------------- | ----------  | -------- | -------- | ------ |
| 1.Physical|  I2C |  SPI  |   RS-232 RS-485          |  ISO 11898-2  | IEEE 802.3  |  CAN     | Ethernet |
| 2.Data-Link| I2C |  SPI  |    UART                  |  ISO 11898-1  | IEEE 802.3  |  CAN     | Ethernet MAC, Mailbox/Buffer Handling, Process Data Mapping, Extreme Fast Auto-Forwarder |
| 3.Network|
| 4.Transport|
| 5.Session|
| 6.Presentation|
| 7.Application|
| * Speed | 100/400 kbps |	a few mbps | 9.6/19.2/38.4/57.6/115.2 kbps | 

# RS-232
Overview: https://www.youtube.com/watch?v=eo9dbnrpspM
- Recommended Standard 232 (RS-232) is a standard originally introduced in 1960 for **serial communication** transmission of data. 
- It formally defines signals connecting between a **DTE (data terminal equipment) such as a computer terminal, and a DCE (data circuit-terminating equipment or data communication equipment), such as a modem**. 
- The standard defines the electrical characteristics and timing of signals, the meaning of signals, and the physical size and pinout of connectors. **OSI Layer 1 Physical Layer**. EIA standard RS-232-C[3] as of 1969 defines:
  - Electrical signal characteristics such as voltage levels, signaling rate, timing, and slew-rate of signals, voltage withstand level, short-circuit behavior, and maximum load capacitance.
    > The voltage levels that correspond to logical one and logical zero levels for the data transmission and the control signal lines. Valid signals are either in the range of +3 to +15 volts(0,space) or the range −3 to −15 volts(1,mark) with respect to the "Common Ground" (GND) pin; consequently, the range between −3 to +3 volts is not a valid RS-232 level. For data transmission lines (TxD, RxD, and their secondary channel equivalents), logic one and zero is represented. Control signals have the opposite polarity: the asserted or active state is positive voltage and the de-asserted or inactive state is negative voltage. Examples of control lines include request to send (RTS), clear to send (CTS), data terminal ready (DTR), and data set ready (DSR). 
  - Interface mechanical characteristics, pluggable connectors and pin identification.
    > 1.According to the standard, **male connectors have DTE pin functions, and female connectors have DCE pin functions**. Other devices may have any combination of connector gender and pin definitions. Many terminals were manufactured with female connectors but were sold with a cable with male connectors at each end; the terminal with its cable satisfied the recommendations in the standard.  
    > 2.The standard recommends the D-subminiature 25-pin(D-sub 25, DB25). Most devices only implement a few of the twenty signals specified in the standard, so connectors and cables with fewer pins are sufficient for most connections, more compact, and less expensive. Personal computer manufacturers replaced the DB-25M connector with the smaller DE-9M(DB-9) connector (see Serial port).  
    > 3.Presence of a 25-pin D-sub connector does not necessarily indicate an RS-232-C compliant interface.  
    > 4.The standard does not define a maximum cable length, but instead defines the maximum capacitance that a compliant drive circuit must tolerate. A widely used rule of thumb indicates that cables more than 15 m (50 ft) long will have too much capacitance, unless special cables are used. By using low-capacitance cables, communication can be maintained over larger distances up to about 300 m (1,000 ft).For longer distances, other signal standards, such as RS-422, are better suited for higher speeds.   
  - Functions of each circuit in the interface connector.
  - Standard subsets of interface circuits for selected telecom applications.
    > A minimal "3-wire" RS-232 connection consisting only of TX, RX, and GND, is commonly used when the full facilities of RS-232 are not required. Even a "2-wire" connection (data and ground) can be used if the data flow is one way (for example, a digital postal scale that periodically sends a weight reading, or a GPS receiver that periodically sends position, if no configuration via RS-232 is necessary). When only hardware flow control is required in addition to two-way data, the RTS and CTS lines are added in a "5-wire" version. 
  - The standard does not define such elements as the character encoding (i.e. ASCII, EBCDIC, or others), the framing of characters (start or stop bits, etc.), transmission order of bits, or error detection protocols. It is **OSI Layer 2**. The character format and transmission bit rate are set by the serial port hardware, typically a **UART**, which may also contain circuits to convert the internal logic levels to RS-232 compatible signal levels. The standard does not define bit rates for transmission, except that it says it is intended for bit rates lower than 20,000 bits per second.
- The current version of the standard is **TIA-232-F** Interface Between Data Terminal Equipment and Data Circuit-Terminating Equipment Employing Serial Binary Data Interchange, issued in 1997. Electronic Industries Association (EIA). The RS-232 standard had been commonly used in computer serial ports and is still widely used in **industrial communication devices such as Programmable Logic Controller(PLC), Human-Machine Interface (HMI), motor driver**.
- A serial port complying with the RS-232 standard was once a standard feature of many types of computers. Personal computers used them for connections not only to **modems**, but also to **printers, computer mice, data storage, uninterruptible power supplies, and other peripheral devices**.
- RS-232, when compared to later interfaces such as RS-422, RS-485 and Ethernet, has **lower transmission speed, short maximum cable length, large voltage swing, large standard connectors, no multipoint capability and limited multidrop capability**.
- In modern personal computers, USB has displaced RS-232 from most of its peripheral interface roles. Few computers come equipped with RS-232 ports today, so one must use either an external **USB-to-RS-232 converter** or an internal expansion card with one or more serial ports to connect to RS-232 peripherals. 
- Nevertheless, thanks to their simplicity and past ubiquity, RS-232 interfaces are still used—particularly in industrial machines, networking equipment, and scientific instruments where a **short-range, point-to-point, low-speed, inexpensive** wired data connection is fully adequate. 
- Hardware Interrupt:
  - Ring Indicator (RI) is a signal sent from the DCE to the DTE device. It indicates to the terminal device that the phone line is ringing. In many computer serial ports, a **hardware interrupt** is generated when the RI signal changes state. Having support for this hardware interrupt means that a program or operating system can be informed of a change in state of the RI pin, without requiring the software to constantly "poll" the state of the pin.
- Flow control (data) § Hardware flow control:
  - The **Request to Send (RTS) and Clear to Send (CTS)** signals were originally defined for use with half-duplex (one direction at a time) modems such as the Bell 202. These modems disable their transmitters when not required and must transmit a synchronization preamble to the receiver when they are re-enabled. **The DTE asserts RTS to indicate a desire to transmit to the DCE, and in response the DCE asserts CTS to grant permission**, once synchronization with the DCE at the far end is achieved. Such modems are no longer in common use. There is no corresponding signal that the DTE could use to temporarily halt incoming data from the DCE. Thus RS-232's use of the RTS and CTS signals, per the older versions of the standard, is asymmetric. 
  - There is a need for symmetric, bidirectional flow control. They redefined the RTS signal and defined a new signal **"RTR (Ready to Receive)"**, shares the same pin as RTS. In this scheme, commonly called **"RTS/CTS flow control" or "RTS/CTS handshaking"** (though the technically correct name would be "RTR/CTS"), the DTE asserts RTR whenever it is ready to receive data from the DCE, and the DCE asserts CTS whenever it is ready to receive data from the DTE. Unlike the original use of RTS and CTS with half-duplex modems, these two signals operate independently from one another. This is an example of hardware flow control.
- Limitations:
  - The large **voltage swings** and requirement for positive and negative supplies increases **power consumption** of the interface and complicates power supply design. The voltage swing requirement also **limits the upper speed** of a compatible interface.
  - Single-ended signaling referred to a **common signal ground** limits the **noise immunity and transmission distance**.
  - **Multi-drop** connection among more than two devices is not defined. While multi-drop "work-arounds" have been devised, they have limitations in speed and compatibility.
  - The standard does not address the possibility of connecting **a DTE directly to a DTE, or a DCE to a DCE**. **Null modem cables** can be used to achieve these connections, but these are not defined by the standard, and some such cables use different connections than others.
  - The definitions of the **two ends of the link are asymmetric**. This makes the assignment of the role of a newly developed device problematic; the designer must decide on either a DTE-like or DCE-like interface and which connector pin assignments to use.
  - **The handshaking and control lines** of the interface are intended for the setup and takedown of a dial-up communication circuit; in particular, the use of handshake lines for **flow control** is not reliably implemented in many devices.
  - No method is specified for **sending power to a device**. While a small amount of current can be extracted from the DTR and RTS lines, this is only suitable for low-power devices such as mice.
  - The DB25 connector recommended in the standard is large compared to current practice.

# RS-485
https://www.youtube.com/watch?v=3wgKcUDlHuM
serial communication, younger and faster than RS-232, multiple device connected(32), 4000feet, no standard but 9pin DB9 cable is widely used, cable with shield to prevent noise

# Ethernet
https://www.youtube.com/watch?v=HLziLmaYsO0 
1983 IEEE 802.3 ethernet defined physical layer and data link layer, connecting Local Area Network(LAN) devices, coaxial cable->twisted pair cable(CAT5/5e CAT6 CAT6a CAT7)with RJ-45connector->Fiber optic cable(glass or plastic)with SFP and SC connector, physical layer: cabling and devices(bridge and gateway), full-duplex or half-dulplex, data link layer: media access control(MAC) logical link control(LLC), MAC each network interface card (NIC) have one, CSMA/CD to data transmission, star topology and bus topology connection, then connect several LAN by Internet to a wide area network (WAN)

# EtherCAT
https://www.youtube.com/watch?v=tYAl2jkaB8Q
ethercat is a ethernet based communication protocol, ethernet for control automation technology(CAT), with several mechanism benefit to automation application

# Modbus
https://www.youtube.com/watch?v=txi2p5_OjKU

# CAN
https://www.youtube.com/watch?v=FqLDpHsxvf8
The Controller Area Network system, electronic control systems (ECS) are connected by CAN bus. In automative system ECUs can be engine control unit, airbags,  audio system, ...etc(more than 70 ECUs in a morden car). 1.Low cost 2.Centralized 3.Robust 4.Efficient 5.Flexible, Bosch invent it. CAN msg format 8 key parts. Related protocol: SAE J1939, OBD-II, CANopen. It is a data link layer(ISO 11898-1) and physical layer(ISO 11898-2).

# CANOpen
https://www.youtube.com/watch?v=DlbkWryzJqg
CANOpen is a CAN based communication protocol, but 6 new important concept. https://www.csselectronics.com/screen/page/canopen-tutorial-simple-intro

# I2C, SPI, UART
Read the details in:
- Comparison: https://shannon112.blogspot.com/2021/01/embedded-system-serial-communication.html
- SPI: https://shannon112.blogspot.com/2021/01/embedded-system-serial-peripheral.html
- I2C: https://shannon112.blogspot.com/2021/01/embedded-system-inter-integrated.html
- UART: https://shannon112.blogspot.com/2021/01/embedded-system-universal-asynchronous.html

# Reference
[1] https://en.wikipedia.org/wiki/OSI_model  
[2] http://linux.vbird.org/linux_server/0110network_basic.php#whatisnetwork_what  
[3] https://en.wikipedia.org/wiki/RS-232
[4] https://en.wikipedia.org/wiki/D-subminiature