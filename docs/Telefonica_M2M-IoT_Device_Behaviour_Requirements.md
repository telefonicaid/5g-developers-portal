

## DOCUMENT REFERENCES
1. [3GPP specifications](www.3gpp.org)

Including but not restricted to:
- [3GPP TS 24.008](http://www.3gpp.org/ftp/Specs/html-info/24008.htm)
- [3GPP TS 23.060](http://www.3gpp.org/ftp/Specs/html-info/23060.htm)
- [3GPP TS 23 401](http://www.3gpp.org/ftp/Specs/html-info/23401.htm)

2. [Embedded Mobile Guidelines, GSMA, www.gsma.org]
(http://www.gsma.com/connectedliving/sample-page/embedded-mobile-guidelines)

3. [TS.24 Operator Minimum Acceptance Values for Device Antenna Performance, GSMA, www.gsma.org]
(http://www.gsma.com/newsroom/official-document-ts-24-operator-minimum-acceptance-values-for-device-antenna-performance)

4. [Device Implementation Guidelines, SIM Alliance, www.simalliance.org]
(http://www.simalliance.org/en/resources/recommendations/simalliance-uicc-device-implementation-guidelines_hckkbowk.html?&highlight=1&keys=device+implementation+guidelines&lang=0)

5. Telefonica M2M/IoT Device Behaviour Guidelines

6. Telefonica M2M/IoT Device Behaviour Requirements

7- [IoT Device Connection Efficiency Guidelines Version 5.0 - 8 January 2018]
(https://www.gsma.com/newsroom/wp-content/uploads//TEF.IOT.DB_v5.0.pdf)


## 1. Introduction
In M2M/IoT scenarios device firmware and software play a significant part in determining the overall performance and behaviour of the service on the mobile network. With no human intervention to fall back on, mechanisms that manage recovery from failure need to be built into software. Poor design of device - network interactions which disregard the network and device status may result in inefficient use of network and device resources, affecting the M2M/IoT experience end-to-end.
Efficient device behaviour can be accomplished following some basic guidelines on network resource management for outstanding performance in M2M/IoT scenarios.

### 1.1 Goal of this document
The purpose of this document is to enable Customers of Telefonica to achieve the best performance from their M2M or IoT Devices providing guidance to manage network resources efficiently.
Following these guidelines, our customers will deliver the best quality M2M/IoT solutions and optimize the performance of all the elements part of the solution: SIM card, device, network and service.

### 1.2 Requirements origin and type
These requirements are based but not restricted to relevant 3GPP specifications and GSMA and SIM Alliances best practices. On top of them
Along the document some Requirements will be marked as Recommendations and some others as Mandatory. 
Mandatory requirements will be tested along Telefónica Device Onboarding Process. Devices fulfilling all Mandatory Requirements will pass successfully said process.

### 1.3 Document content
Sections 2 and 3 describe Telefonica requirements [6] and additional best practices to achieve an efficient use of network resources.
In many cases developers will use IoT/M2M modules. Modules encapsulate many details of network connection.
But in some cases they don’t give to the developer the information needed to fulfill the previous requirements.
Section 4 gives some simple advices to fulfill Telefónica requirements when using a module
Annex A describes in detail how Telefónica and 3GPP requirements apply to each error response from the network.
This document is assuming expertise in the basic concepts of mobile network communication.
Please refer to [1] for further information on mobile network architecture and communication procedures.

### 1.4 Definition of Terms 
ADM	Access condition to an Elementary File (EF) which is under the control of the authority which creates this file
Back-off Timer
	The Back-off Timer is a dynamic timer which value is based on a unique value for the device (desirably the IMSI) and the number of consecutive failures (which points to different Back-off Base Intervals).
Communications Module	The communications component which provides wide area (2G, 3G, 4G) radio connectivity. Comprising of Communications Module Firmware, Radio Baseband Chipset and UICC
Communications Module Firmware	The functionality within the Communications Module that provides an API to the IoT Device Application and controls the Radio Baseband Chipset.
End Customer	Means the consumer of IoT Services provided by the IoT Service Provider. It is feasible that the End Customer and IoT Service Provider could be the same actor, for example a utility company.
Fast Dormancy	Device power saving mechanism. See GSMA TS.18 [14].
Global Certification Forum		An independent worldwide certification scheme for mobile phones and wireless devices that are based on 3GPP standards. The GCF provides the framework within which cellular GSM, UMTS and LTE mobile devices and Communication Modules obtain certification for use on GCF Mobile Network Operators’ networks. Obtaining GCF Certification on a mobile device ensures compliance with 3GPP network standards within the GCF Mobile Network Operators' networks. Consequently, GCF Mobile Network Operators may block devices from their network if they are not GCF certified. For more information, see http://www.globalcertificationforum.org 

Internet of Things	The Internet of Things describes the coordination of multiple machines, devices and appliances connected to the Internet through multiple networks. These devices include everyday objects such as tablets and consumer electronics, and other machines such as vehicles, monitors and sensors equipped with machine-to-machine (M2M) communications that allow them to send and receive data.
IoT Device	The combination of both the IoT Device Application and the Communications Module.
IoT Device Application	The application software component of the IoT Device that controls the Communications Module and interacts with an IoT Service Platform via the Communications Module.
IoT Device Host	The application specific environment containing the IoT Device e.g. vehicle, utility meter, security alarm etc.
IoT Server Application	An application software component that runs on a server and can exchange data and interact with the IoT Devices and the IoT Device Applications over the IoT Service Platform.
IoT Service	The IoT service provided by the IoT Service Provider.
IoT Service Platform	The service platform, hosted by the IoT Service Provider which communicates to an IoT Device to provide an IoT Service. The IoT Service Platform can exchange data with the IoT Device Application over the Mobile Network and through the Communication Module, using (among others) IP-based protocols over a packet-switched data channel. Also, the IoT Service Platform typically offers Device Management capabilities, acting as a so-called Device Management Server. Finally, the IoT Service Platform typically offers APIs for IoT Server Applications to exchange data and interact with the IoT Device Applications over the IoT Service Platform.
IoT Service Provider	The provider of IoT services working in partnership with a Mobile Network Operator to provide an IoT Service to an End Customer. The provider could also be a Mobile Network Operator.
Machine to Machine	Machine-to-Machine (M2M) is an integral part of the Internet of Things (IoT) and describes the use of applications that are enabled by the communication between two or more machines. M2M technology connects machines, devices and appliances together wirelessly via a variety of communications channels, including IP and SMS, to deliver services with limited direct human intervention turning these devices into intelligent assets that open up a range of possibilities for improving how businesses are run.
Mobile Network Operator	The mobile network operator(s) connecting the IoT Device Application to the IoT Service Platform.
PTCRB	The independent body established as the wireless device certification forum by North American Mobile Network Operators. The PTCRB provides the framework within which cellular GSM, UMTS and LTE mobile devices and Communication Modules obtain certification for use on PTCRB Mobile Network Operator networks. Obtaining PTCRB Certification on a mobile device ensures compliance with 3GPP network standards within the PTCRB Mobile Network Operators' networks. Consequently, PTCRB Mobile Network Operators may block devices from their network if they are not PTCRB certified. For more information, see http://ptcrb.com 

Radio Baseband Chipset	The functionality within the Communications Module that provides connectivity to the mobile network.
Requirements numbering:-
TS34_X.X_REQ_YYY	TS.34 = this PRD number.
X.X = the section number the requirement can be found in.
REQ = Requirement
YYY = the requirement number.
Subscriber Identity Module	Module provided by the Mobile Network Operator containing the International Mobile Subscriber Identity (IMSI) and the security parameters used to authenticate the (U)SIM with the Network. Seen as an authentication application contained in the Universal Integrated Circuit Card (UICC).
UICC	The smart card used by a mobile network to authenticate devices for connection to the mobile network and access to network services.

### 1.5 1.5	Abbreviations
3GPP	3rd Generation Project Partnership
API	Application Programming Interface
APN	Access Point Name
GCF	Global Certification Forum
GSM	Global System Mobile
GSMA	GSM Association 
IMEI	International Mobile station Equipment Identity
IMSI	International Mobile Subscriber Identity
IoT	Internet of Things
IP	Internet Protocol
LTE	Long Term Evolution
M2M	Machine to Machine
NAT	Network Address Translation
NFM	Network Friendly Mode – see section ¡Error! No se encuentra el origen de la referencia.

OTA	Over The Air
PDP	Packet Data Protocol
PTCRB	A pseudo-acronym, originally meaning PCS Type Certification Review Board, but no longer applicable.
RFC	Request for Comments – a document of the Internet Engineering Task Force
RPM	Radio Policy Manager – see section ¡Error! No se encuentra el origen de la referencia.

RRC	Radio Resource Control
SMS	Short Message Service
UMTS	Universal Mobile Telecommunications Service
(U)SIM	(Universal) Subscriber Identity Module
USB	Universal Serial Bus

### 1.6 References





