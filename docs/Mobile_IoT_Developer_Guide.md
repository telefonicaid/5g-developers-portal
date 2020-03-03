# Developer Guide

## Introduction

### About this Document

This document gives device and firmware developers an approach of LTE-M and NBIoT and its benefits in comparison with previous Cellular IoT technologies.

### General strategy
To attend requirements for IOT specific use cases, Telefonica has clearly betted to evolve its networks to licensed LPWA technologies, 
it means Telefonica is going to develop NB-IOT, and LTEM.

We include these new technologies as part of our value proposition including our current offer managed connectivity, security, QoS, private networks, etc.
Besides, Telefonica is going to develop end to end solutions; device, connectivity, data management, analytics, 
etc for some specific use cases to try to accelerate implementation of the customer service.

## Technological background

### Mobile IoT Technologies
Since the beginning of cellular communications, not only people have been connected using mobile phones, also machines have been connected to each other, 
what we call M2M, Machine-To-Machine communications. Currently, the term M2M has been replaced by IoT, 
to make clear the evolution that will have this type of technologies in the coming years, miniaturization of connected devices, 
massive increase in the number of connected equipment, increased coverage will be what will lead to the M2M a step further to the IoT. 

Typically the cheapest and simplest solution has been to use second generation 2G mobile technologies (GSM, GPRS, EDGE),
but when the throughput requirements were higher and the cost requirements were less demanding, 3G and 4G technologies were used too.


It is necessary to include cellular technologies in the use cases:
- Utilities: gas and water metering,
- Medical assisted living, remote monitoring
- Agriculture: Stationary tracking, environmental monitoring
- Industrial: machinery control, safety monitoring
- Consumer: wearables, pet tracking.
- Smart City: parking sensors, waste management

These use cases have specific requirements that force the emergence of new cellular technologies focused on the IoT:
- Wide area coverage: Covered by all mobile technologies.
- No access to the mains power: The devices will be battery powered in most cases.
- Low cost: The number of devices and applications will require a cost per device inside the current one.
- Strong propagation: Go one step further in coverage to enable IoT access to basements, rural environments, etc.

These four requirements are not met by current mobile technologies, 
which forced 3GPP to define two new standards based on the existing 4G infrastructure: NBIoT and LTE-M

Now launched commercially, the Mobile Internet of Things (Mobile IoT) is expanding rapidly with over 40 Commercial networks available globally, 
a combination of LTE-M and NB-IoT. Operating in licensed spectrum, low power wide area networks can provide low cost, yet secure, 
connectivity to battery-powered devices in both rural and urban locations. Following successful pilots involving a wide variety of use cases, 
Mobile IoT connectivity has now been deployed across Europe, North America and East Asia. 

### Selecting a technology: LTE-M vs NBIoT

![pick](pictures/ltem_LPWA.png)

LTE-M extends LTE with features for improved support for Machine-Type Communications (MTC) and IoT.
The objective of LTE-M was to bring down the LTE device cost substantially to make LTE attractive for low end MTC applications that have so far been
adequately handled by GPRS. The cost reduction techniques were: reduced peak rate (1Mbps), single receive antenna, half duplex operation,
reduced bandwidth (1,4Mhz) and TX power (20dBm). 

NBIoT is designed for ultra-low-cost massive Machine-Type Communications, aiming to support a massive number of devices in a cell.
Low device complexity is one of the main design objectives, enabling module cost. Furthermore, it is designed to offer substantial coverage
improvements over GPRS as well as for enabling long battery lifetime. NBIoT has been designed to give maximal deployment flexibility.


|| LTE-M | NBIOT |
|:------ | ----- | ----- |
| BANDWIDTH | 1,4MHz | 200kHz |
| DL PEAK RATE | 300kbps HD | 50kbps |
|   | 1Mbps FD |  |
| UP PEAK RATE | 375kbps HD | 20kbps single tone |
|   | 1Mbps FD | 50 kbps multi tone |
| DUPLEX | HD (type B) or FD | HD (type B) or FD |
| LATENCY | 10-15ms | 1,4-10s |
| ANTENNAE | 1 | 1 |
| TRANSMIT POWER | 20dBm | 23dBm |
| MOBILITY  | Handover | Cell-Resellection |
| VOICE | Yes | No |



NBIoT is recommended when devices are massive and static. But when dealing with moving devices it is recommended to use a multi-technology 
NBIoT/LTE-M solution to avoid coverage issues and to guarantee the services.

## Functionalities for limited power consumption

There are a set of techniques that can be used in order to optimize the power consumption of the devices when using NBIoT and LTE-M. Some of them were added in previous 3GPP releases. Due to these functionalities the modules can save so much energy that some use cases are enabled. 

These functionalities are directly related with the status of the RRC link. AS you can see in the following figure there are four different status. RRC Connected, RRC Idle, PSM and Power Off.

![pick](pictures/limited_power_consumption.png)

A UE is in RRC_CONNECTED when an RRC connection is established. If no RRC connection is established the UE is in RRC_IDLE mode. When the device is in IDLE state, it is only listening to control channel broadcasts, such as paging notifications of inbound traffic, or connected, in which case the network has an established context and resource assignment for the client. When in RRC_IDLE, the device cannot send or receive any data. To do so, it must first synchronize itself to the network by listening to the network broadcasts and then issue a request to the RRC to be moved to the RRC_CONNECTED.
Once in a RRC_CONNECTED, a network context is established between the radio tower and the device, and data can be transferred. However, once either side completes the intended data transfer after a certain time of inactivity the device goes back to RRC_IDLE.

### RRC_IDLE
The radio is inactive but IP address is assigned and tracked by the network. UE is known in EPC but not known to eNB (LTE base station). The term "IDLE" gives an impression that UE is doing nothing in this state, but actually UE is quite busy in this state. Mobility is controlled by UE.
Device radio is in a low-power state and only listening to control traffic. No radio resources are assigned to the client within the carrier network.
Major procedures defined in IDLE mode:
- PLMN Selection: Detect PLMN of cells and identify the cell to clamp on.
- Cell Selection and Reselection : Performs neighboring cell measurement and do reselection 
- CSG cell selection and reselection
- Cell Reservations and Access Restrictions
- Tracking Area Registration
- Broadcast message reception: Acquire MIB and SIB
- Paging : Monitors the paging channel
- DRX reception

### RRC_CONNECTED
The radio is active and UE is known to both EPC and eNB. Device radio is in a high-power state while it either transmits data or waits for data, 
and dedicated radio resources are allocated by the radio network. Mobility is controlled by Network.
Major procedures defined in RRC_CONNECTED mode:
- Control Plane
  - eNB context and RRC connection
  - Network can transmit and/or receive data to/from UE
  - Neighbor cell measurement
- User Plane
  - UE can transmit and/or receive data to/from network
  - Monitors control signaling channel
  - Reports CQI and feedback information to eNB.
  - Connected Mode DRX

### DRX
The LTE modem supports the LTE DRX (3GPP LTE release 8) feature to enable short sleeps between data transmissions, and thus to minimize the UE power consumption. 
Two DRX modes are defined:
- DRX in LTE RRC_IDLE state: I-DRX
- DRX in LTE RRC_CONNECTED: C-DRX

### I-DRX
UE is said to be in RRC_IDLE mode when it is not connected to eNB, still the network keep track of the UE by paging mechanism. 
After the initial power-on sequence, the UE will perform limited functionality in IDLE mode hence saves lot of battery power. 
UE is paged for DL traffic but for UL traffic the UE will no longer be in IDLE state, it will move to the connected state.

![pick](pictures/limited_power_consumption_idrx.png)

In LTE, just as in most other modern wireless standards, there are shared uplink and downlink radio channels, the access to which is controlled by the RRC. 
When in a RRC_CONNECTED state, the RRC tells each and every device which timeslots are assigned to whom, which transmit power must be used, modulation, 
plus a dozen other variables.
If the mobile device does not have an assignment for these resources by the RRC, then it cannot transmit or receive any user data. 
Consequently, when in a DRX state, the device is synchronized to the RRC, but no uplink or downlink resources are allocated to it: the device is “half awake.”

![pick](pictures/limited_power_consumption_idrx_t.png)

### C-DRX
This feature allow the UE to optimize its power consumption while active wait of incoming messages is being performed. 
It works in a similar way than I-DRX, but during the RRC_CONNECTED status, as showed in the following figures:

![pick](pictures/limited_power_consumption_cdrx.png)

In addition to I-DRX, LTE had RRC mode DRX as well also known as Connected Mode DRX (C-DRX), the purpose of both the DRX is same to conserve the battery power. 
During RRC connected state when there is no data transmission in either direction (UL/DL) UE goes into the DRX mode. 
It starts monitoring the PDCCH channel discontinuously in other words UE is in sleep and wake cycle.

![pick](pictures/limited_power_consumption_cdrx_t.png)

Without the DRX the UE needs to monitor PDCCH in every subframe to check if there is downlink data available which drains the battery fast.

![pick](pictures/limited_power_consumption_cdrx_t2.png)

There are also three sub status when in RRC_CONNECTED:
- **Continuous reception** Highest power state, established network context, allocated network resources.
- **Short Discontinuous Reception (Short DRX)** Established network context, no allocated network resources.
- **Long Discontinuous Reception (Long DRX)** Established network context, no allocated network resources.

In the high-power state, the RRC creates a reservation for the device to receive and transmit data over the wireless interface and 
notifies the device for what these time-slots are, the transmit power that must be used, the modulation scheme, and a dozen other variables. 
Then, if the device has been idle for a configured period of time, it is transitioned to a Short DRX power state, where the network context is still maintained, 
but no specific radio resources are assigned. When in Short DRX state, the device only listens to periodic broadcasts from the network, 
which allows it to preserve the battery. This functionality is enabled by the network and it is automatically used by the UE, 
without any need of additional configuration.

### eDRX
There are largely two types of DRX in LTE. One is Idle mode DRX. This idle mode DRX is more commonly known as Paging Cycle which is explained in detail in Paging page. The other type is called C-DRX (Connected Mode DRX) which was described before.

eDRX is a mechanism that can extend the cycle (sleeping duration) of these two DRX (Idle mode DRX and C-DRX). The concept of eDRX can be illustrated as shown below.
This is intended to save energy (battery consumption). So eDRX would be mostly used in the application of IoT(Internet of Things) operating in energy saving mode.

![pick](pictures/limited_power_consumption_edrx.png)

The basic concept of eDRX would sound simple. Just extension of sleeping duration. So it can be implemented by adding a couple of large values Paging Cycle and DRX Cycle in RRC message. Both UE and eNB require a very accurate timer (clock) and the clock should be synchronized on both side. As you know, SFN (System Frame Number) and Subframe is the timer being used in current LTE for most of the synchronized operation. The maximum duration 
of SFN is 1024 radio frame meaning 10240 radio frame (around 10). However, eDRX cycle is designed to be much longer than this reaching several hours in case of Idle mode DRX. So we got to need another kind of timer (clock) that can measure much longer time duration. 

Extended Discontinuous Reception (eDRX) is an extension of an existing LTE feature, which can be used by IoT devices to reduce power consumption. eDRX can be used without PSM or in conjunction with PSM to obtain additional power savings.

Today, many smartphones use discontinuous reception (DRX) to extend battery life. By momentarily switching off the receive section of the radio module for a fraction of a second, the smartphone is able to save power. The smartphone cannot be contacted by the network whilst it is not listening, but if the period of time is kept to a brief moment, the smartphone user will not experience a noticeable degradation of service. For example, if called, the smartphone might simply ring a fraction of a second later than if DRX was not enabled.

eDRX allows the time interval during which a device is not listening to the network to be greatly extended. For an IoT application, it might be quite acceptable for the device to not be reachable for a few seconds or longer.

Whilst not providing the same levels of power reduction as PSM, for some applications eDRX may provide a good compromise between device reachability and power consumption. The maximum values are: 2621,44 (43min) seconds for LTE-M and 10485,76 seconds (174min) for NBIoT.

eDRX mode is controlled by three timers:
- **T3324 active timer:** It determines the duration during which the device remains reachable for mobile terminated transaction on transition from connected to idle mode. The device starts the active timer when it moves from connected to idle mode and when the active timer expires, the device moves to Power Saving Mode. This timer is controlled by the UE

![pick](pictures/limited_power_consumption_edrx_t1.png)

- Paging window (PW): 
During this time the UE is listenting periodically for incoming packets. The length of this stage is controlled by the network. 
- eDRX cycle: 
This timer controls the duty cycle of the eDRX mode. This time can be configured by the UE, should be sized taking into account latency and reachability restrictions. This cycle is directly controlled by the UE.

Please refer to the “Timer Sumary” for further details.

![pick](pictures/limited_power_consumption_edrx_t2.png)

In the following figure the DRX mode is disabled, which means, that the UE does not enter PSM but it do perform TAU procedure periodically. 
A UE using PSM is available for mobile terminating services only for the period of an Active Time after a mobile originated event like data transfer or 
signaling for example after a periodic TAU/RAU procedure. The MME allows a value of '0' for the T3324 timer. In this case the UE enters 
the Power Saving Mode immediately.

### PSM
Power Saving Mode (PSM) is designed to help IoT devices conserve battery power and potentially achieve a 10-year battery life. When in PSM the device is not reachable, so it cannot send or receive any message from the network.
It is an especially kind of UE status that can minimize the energy consumption that is supposed to be even lower than normal idle mode energy consumption. 
This is newly added feature in Release 12 and is specified in 3GPP 24.301-5.3.11 Power saving mode and 23.682-4.5.4 UE Power Saving Mode. Similar to power-off, 
but the UE remains registered with the network. No need to re-attach or re-establish PDN connections. 
A UE in PSM is not immediately reachable for mobile terminating services.

![pick](pictures/limited_power_consumption_psm.png)

Whilst it has always been possible for a device’s application to turn its radio module off to save battery power, the device would subsequently have to reattach to the network when the radio module was turned back on. The reattach procedure consumes a small amount of energy, but the cumulative energy consumption of reattaches can become significant over the lifetime of a device. Therefore, battery life could be extended if this procedure could be avoided.

When a device initiates PSM with the network, it provides two preferred timers (T3324 and T3412); PSM time is the difference between these timers (T3412-T3324). The UE asks the network with certain timer values, the network may accept these values or set different ones, in the case those values were out of bound. The network then retains state information and the device remains registered with the network. If a device awakes and sends data before the expiration of the time interval it agreed with the network, a reattach procedure is not required.

![pick](pictures/limited_power_consumption_psm_t.png)

For example, for a monitoring application, the radio module in a device might be configured by an application to enable PSM, negotiate a 24-hour time interval with the network and provide a daily status update to a centralised monitoring point. If the device’s monitoring application were to detect an alarm condition, irrespective of any agreed sleep interval, the application could wake the radio module instantly and send vital information to the centralised monitoring point without the need to execute a reattach procedure.

However, in a similar manner to a radio the network cannot contact module that has been powered off, a radio module in PSM whilst it is asleep. The inability to be contacted whilst asleep may preclude the use of PSM for some applications.

Whilst the device is asleep, an operator might store incoming packets or SMS (if supported) to be forwarded to the device once it awakens. 
As is clear from the figure above, the value of the PSM is limited by the utilised tracking area update (TAU). During the attachment procedure, the device can also request a periodic TAU, also by providing a T3412 value.


## Release Assistance

The purpose of the Release Assistance indication is to inform the network whether:
-	No futher uplink or downlink data transmission is expected
-	Only a single downlink data transmission and no further uplink data transmission subsequent to the uplink data transmission is expected.

So after one of these two cases the device enters directly into RRC_IDLE avoiding to stay in RRC_CONNECTED status during the Inactivity Timer interval. 
The amount of energy that can be saved using this optimization can enable even more battery constrained use cases.

When a UE transmits data in uplink, the NAS signaling message encapsulating the data packet can include a Release Assistance Information (RAI) field. 
This RAI field allows the UE to notify the MME if no further uplink or downlink data transmissions are expected, 
or only a single downlink data transmission subsequent to this uplink data transmission is expected. In such case, 
the MME can immediately trigger the S1 Release procedure (unless user plane bearers between eNB and Serving Gateway (SGW) are established). 
Hence, the RAI field enables the MME to reduce the period the UE is in DRX waiting for possible additional transmissions.

![pick](pictures/release_assistance.png)

In such a way the module can optimize its power consumption avoiding to lose time and energy waiting for incoming packets, during the inactivity timer.

![pick](pictures/release_assistance_t.png)


## Timers summary

![pick](pictures/timers_summary.png)

- **T3412:**
The module perform a TAU (Tracking Area Update) procedure. This value is proposed by the UE to the network, which is usually accepted but when this value is out of EPC limits the network will set a default value.

- **T3324:**
The module stays in RRC Idle (DRX or eDRX) waiting for an incoming packet. This value is controlled by the UE.

- **T3412 - T3324:**
The module is in PSM mode, its current consumption is in the microamperes scale and the module is not reachable. 

- **eDRX Cycle:**
Time between paging windows the following table shows the available values in seconds. This value is negotiated between the module and the network.

![pick](pictures/table_eDRX_Cycle.png)

- **Paging Window:**
The module is reachable during this time while in eDRX mode. These are the available values in seconds. This time in controller by the network.

![pick](pictures/table_Paging_Window.png)

- **Inactivity Timer:**
This timer is reset each the UE has an incoming or outcoming packet to/from the network. The module should stay in RRC Connected during this time after the last communication before entering RRC Idle. This timer is controlled by the network.

- **DRX period:**
Time between pagings in NBIoT (2,56s) and LTE-M (1,28s). This timer is controlled by the network.


## Coverage enhancement

Some IoT applications require devices to be positioned in areas that are not readily accessible by radio coverage, 
such as underground parking garages and in ground pits. The 3GPP Enhanced Coverage feature is an integral characteristic of NB-IoT and LTE-M, 
as it increases the deptxh of radio coverage to enable IoT devices to operate in locations that would otherwise not be possible.

In NBIoT there are up to three different Coverage Levels signalled via SIB2-NB:

![pick](pictures/table_Coverage_Enhancement_nb.png)

The coverage level selected determines the NPRACH resources to use (subset of subcarriers, PRACH Repetitions, Max number of attempts, etc…) UE derives the Coverage Level based on NRSRP measured.

In LTE-M two Coverage Enhance modes are enabled: Mode A and Mode B. The Operation mode is determined by eNB (informed to UE via RRC message). Thus, the Level within each Mode is determined by UE. (based on the reference signal power (RSRP) it measure and inform it to eNB by PRACH resource (frequency,time, preamble) it uses.)
As per agreement, the connected UE can opérate in Mode A and mode B. These modes are configured by the eNB when entering RRC_CONNECTED, 
there are two levels in eacn CE Mode:

![pick](pictures/table_Coverage_Enhancement_lte.png)

- CE Mode A: No repetition or small number of repetition. It has an equivalent coverage that of UE Cat 1.
- CE Mode B: Large number of repetition. It has coverage enhancement up to 15dB in comparisons with UE Cat 1.

## Connection architectures

This part is intended to clarify the connection planes. As LTE-M and NBIoT have evolved from LTE they inherit the user & control plane. But in order to improve its battery lifetime, latency, etc. some optimizations has been made.
First of all, let’s clarify the two plane main functions then the optimizations.

### Connection Plane
The radio protocol architecture for LTE can be separated into control plane architecture and user plane architecture as shown below:

![pick](pictures/diagram_architecture_connection_plane.png)

At user plane side, the application creates data packets that are processed by protocols such as TCP, UDP and IP, while in the control plane, the radio resource control (RRC) protocol writes the signalling messages that are exchanged between the base station and the mobile. 
In both cases, the information is processed by the packet data convergence protocol (PDCP), the radio link control (RLC) protocol and the medium access control (MAC) protocol, before being passed to the physical layer for transmission.

### User Plane
The user plane protocol stack between the eNodeB and UE consists of the following sub-layers:
- PDCP (Packet Data Convergence Protocol)
- RLC (radio Link Control)
- Medium Access Control (MAC)

On the user plane, packets in the core network (EPC) are encapsulated in a specific EPC protocol and tunneled between the P-GW and the eNodeB. Different tunneling protocols are used depending on the interface. GPRS Tunneling Protocol (GTP) is used on the S1 interface between the eNodeB and S-GW and on the S5/S8 interface between the S-GW and P-GW.

![pick](pictures/diagram_architecture_user_plane.png)

Packets received by a layer are called Service Data Unit (SDU) while the packet output of a layer is referred to by Protocol Data Unit (PDU) and IP packets at user plane flow from top to bottom layers.

### Control Plane
The control plane includes additionally the Radio Resource Control layer (RRC) which is responsible for configuring the lower layers.
The control plane handles radio-specific functionality that depends on the state of the user equipment that includes two states: idle or connected.

- **Idle:**
The user equipment camps on a cell after a cell selection or reselection process where factors like radio link quality, cell status and radio access technology are considered. The UE also monitors a paging channel to detect incoming calls and acquire system information. In this mode, control plane protocols include cell selection and reselection procedures.
- **Connected:**
The UE supplies the E-UTRAN with downlink channel quality and neighbour cell information to enable the E-UTRAN to select the most suitable cell for the UE. In this case, control plane protocol includes the Radio Link Control (RRC) protocol.

The protocol stack for the control plane between the UE and MME is shown below. The grey region of the stack indicates the access stratum (AS) protocols. The lower layers perform the same functions as for the user plane with the exception that there is no header compression function for the control plane.

![pick](pictures/diagram_architecture_control_plane.png)

### User Plane optimization
The alternative data transmission procedure optimization is UP. It requires an initial RRC connection establishment that configures the radio bearers and the AS security context in the network and UE. After this, UP enables the RRC connection to be suspended and resumed by means of two new control procedures: Connection Suspend and Resume. The support of this optimization is optional for NB-IoT UEs. 

![pick](pictures/diagram_architecture_user_plane_optimization.png)

When the UE transits to RRC Idle state, the Connection Suspend procedure enables to retain UE’s context at the UE, eNB, and MME. Later, when there is new traffic, the UE can resume the connection. To resume the RRC connection, the UE provides a Resume ID to be used by the eNB to access the stored context. By means of preserving the UE context instead of release it, the UE avoids AS security setup and RRC reconfiguration in each data transfer, compared to conventional SR procedure. As the UP optimization utilizes the usual user plane connectivity, subsequent data packets can be transferred through the data paths. Therefore, UP is suitable for short or large data transactions.

### Control Plane optimization
Today in LTE when the UE needs to send uplink data while in EMM-IDLE state, it has to send a Service Request to the core network, which then triggers radio bearer establishment and setup of S1U GTPU tunnel between eNB and SGW (which involves a Modify Bearer Request signaling from MME to SGW). 

For battery constrained IoT devices that need to send uplink data once in a week or a month and that too a small payload, such elaborate signaling procedures are a waste of unnecessary signaling and energy (battery drain). Instead its much simpler and efficient to send the small data payload over the control plane signaling and let MME that receives the small data over NAS (control plane) send the data to SGW.

![pick](pictures/diagram_architecture_control_plane_optimization.png)

--This optimization uses the control plane to forward the UE’s data packets.-- 
To do that, the data packets are sent encapsulated in Non Access Stratum (NAS) signaling messages to the MME (messages 5 and 6 of Figure 3). For NB-IoT UEs, the support of this procedure is mandatory. Since CP uses control plane to forward data packets, the transmission or reception of messages is sent as NAS signaling messages between the UE and MME. Compared to conventional SR procedure, the UE avoids AS security setup and user plane bearers establishment required in each data transfer. Hence, it is more suitable for short data transactions. When a UE transmits data in uplink, the NAS signaling message encapsulating the data packet can include a Release Assistance Information (RAI) field. This RAI field allows the UE to notify the MME if no further uplink or downlink data transmissions are expected, or only a single downlink data transmission subsequent to this uplink data transmission is expected. In such case, the MME can immediately trigger the S1 Release procedure (unless user plane bearers between eNB and Serving Gateway (SGW) are established). Hence, the RAI field enables the MME to reduce the period the UE is in DRX waiting for possible additional transmissions. Unfortunately, CP does not currently allow the application servers to notify the MME if no further data transmissions are expected.


## Communication Protocols: Network Protocols

### IP
As LTE-M and NBIoT have been developed as an evolution of LTE, but taking into account the requirements of IoT cellular communication. It’s an ALL IP solution thus all protocols an data transmission are performed over IP network layer.

### Non-IP
The standard defines Non IP Access. The support of Non-IP Data Delivery (NIDD) is part of the CIoT EPS optimizations. As you can see in the figure below there are two ways to reach an IoT Platform using NonIP communication, via PGW or via SCEF.

![pick](pictures/communication_protocols_network_protocols.png)

- Black line represents IP/NonIP data delivery using User Plane. 
- Red line represents IP/NonIP data delivery using Control Plane CIoT Optimization
- Orange line represents NonIP data delivery vía SCEF.

SCEF provides:
SMALL DATA TRANSFERS INTERFACE. SCEF is the interface for small data transfers and control messaging between Enterprises and the Operators Core Network.
EXPOSE NETWORK CAPABILITIES VIA API. SCEF can communicate monitoring events to the customer AS:
- UE reachability
- UE loss of connectivity
- Location of the UE
- Change location…

Telefonica IoT networks already have NIDD vía PGW available. SCEF deployment is in the roadmap, but it is not available.

## Communication Protocols: Transport Protocols

### TCP
The transport layer in the TCP/IP architecture provides congestion control and reliable delivery, both of which are implemented by TCP, the dominant transport layer protocol on the Internet. TCP has been engineered for many years to efficiently deliver a large bulk of data over a long-lived point-to-point connection without stringent latency requirement. It models the communication as a byte stream between sender and receiver, and enforces reliable in-order delivery of every single byte in the stream.
However, IoT applications usually face a variety of communication patterns which TCP cannot support efficiently:
- Due to the energy constraints, devices may frequently go into sleep mode, thus it is infeasible to maintain a long-lived connection in IoT applications.
- A lot of IoT communication involves only a small amount of data, making the overhead of establishing a connection unacceptable.
- Some applications (e.g., device actuation) may have low-latency requirement, which may not tolerate the delay caused by TCP handshaking.

### UDP
Because there is no connection setup, UDP is faster than TCP and results in less network traffic. In addition it doesn’t consume resources on the receiving machine as it doesn’t hold a connection open.
UDP it’s recommended for IoT aplications in comparision with TCP because the TCP’s packet acknowledgment and retransmission features are useless overhead for those applications. If a piece of does not arrive at its destination in time, there is no point in retransmitting the packet as it would arrive out of sequence and garble the message.
Also as the size of the messages are usually quite little, tenths to two hundred bytes, there is no point in stablishing a persistent connection, like TCP does.

### SMS
SMS are also available in NBIoT and LTE-M. The combined attach procedure allows the UE to be able to receive and send SMS using the existing 3G infrastructure.


## Remote eUICC provisioning

### Requirements

The modules & devices must accomplish some requirements to support remote eUICC provisionng. This information is in the Annex G of the GMSA Specification:

Remote Provisioning Architecture for Embedded UICC 
Technical Specification

V3.1:
[SGP.02 V3.1](https://www.gsma.com/iot/wp-content/uploads/2016/07/SGP.02_v3.1.pdf) annex G (Device Requirements)

V3.2:
[SGP.02 V3.2](https://www.gsma.com/newsroom/wp-content/uploads/SGP.02_v3.2_updated.pdf) annex G (Device Requirements)

--Please, contact your module provider to check if it supports this requirements.--

### Recomendations

In order to perform SWAP procedure we recommend you to implement an “SWAP Mode”. In this mode the device does not enter in PSM (e.g. AT+CPSMS=0) and after the inactivity timer it enter IDLE mode and stays in it forever. This is necessary because in order to start the swap the module must receive a SMS in a short period of time. 
In order to avoid malfunctions during/after the SWAP procedure we recommend not to configure APN neither PLMN:
- Automatic APN selection: AT+CGDCONT=1,”IP”
- Automatic PLMN selection: AT+COPS=0
- Combined attach is mandatory.

## Telefonica LPWA connectivity

### Radio Access Network

![pick](pictures/table_Telefonica_LPWA_RadioAccessNetwork.png)

### Core Network

![pick](pictures/table_Telefonica_LPWA_CoreNetwork.png)

## Recipes

### Smart meter pattern

#### Pattern
- Transmit 50 bytes every 24 hours
- No other interaction

#### Recommendation
- Technology: NB-IoT
- Behaviour: 
  - Early Release+PSM 
    - T3412 > 24 hours
    - If device management is not necessary: T3324 = 0
    - If device management is mandatory: T3324 != 0
  - Switch On/Off module if early release is not available
- Protocols:
 - UDP messages via CP

#### Projection (without the proposed DRX Inactivity Timer optimization)
Assuming the perfect use of three Alkaline AA battery of 2,9Wh each:
- 24 years

#### Projection (with the proposed DRX Inactivity Timer optimization)
Assuming the perfect use of one Alkaline AA battery of 2,9Wh each:
- 26 years

### Always on pattern

#### Pattern
The device must listen and receive short commands from the network. The device is available 24x7 but the commands can accept a 30’’ delay before executing.

#### Recommendation
- Technlogy: NBIoT/LTE-M
- Behaviour:
  - Set T3324 equal to T3412, and both to the maximum possible value (in order to minimize TAU updates). 
  - Set eDRX timer depending on the max latency of your application
- Protocols
  - UDP/TCP when using LTE-M
  - UDP when using NBIoT
  
#### Projection
Assuming:
- the perfect use of three Alkaline AA battery of 2,9Wh each
- and one short command per day

We’d expect a duration of:
- 410 days

The implementation of our recommendation on DRX Inactivity Timer does not affect this projection.

### Massive data transfer pattern

#### Pattern
A certain device must transmit 1MB per day. The device can group data as it needs.

#### Recommendation
- Technology: Use LTE-M
- Behaviour: 
  - Move to PSM between connections.
  - T3412 > 24 hours
  - T3324 = 0
- Protocols: Transmit all 1MB in the same connection.

#### Projection
Assuming:
- the perfect use of three Alkaline AA battery of 2,9Wh each

We’d expect a duration of:
- 6,5 years

The implementation of our recommendation on DRX Inactivity Timer would move this projection to 10 years


### Movable devices

#### Pattern

A certain device is moving continuously sending its position plus further data.

#### Recommendation
- Technology: Use LTE-M
- Behaviour: 
  - Move to PSM between connections.
  - T3412 > 1 hour
  - T3324 = 30s
- Protocols: 
  - UDP

### Remote setup

#### Pattern
A certain device should be configured, and it is not reachable.
Recommendation
- Technology: Use LTE-M/NBIoT
- Protocols: 
  - SMS-MT
