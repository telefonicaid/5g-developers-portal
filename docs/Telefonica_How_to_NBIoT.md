# Telefónica IoT Connectivity

## Network parameters:
- PLMN “21407”
- LTE Band 20 - FDD 800MHz - EARFCN 6390

## PDP Connection:
The PDP Connection will be IPv4 and when configuring the APN name you have two choices:
- Set an empty APN: When attaching, the network will provide automatically the APN.
e.g. AT+CGDCONT=1,”IP”
- Manually set the APN, e.g. AT+CGDCONT=1,”IP”, “sm2ms.movistar.es”: 
  - PDN Type: IPv4
  - PDN Name: “sm2ms.movistar.es”, 
  - PDN User: empty
  - PDN Password: empty

## Internet access:
The SIMs use an APN whose access to the internet is via Proxy NAT, the IP address assigned to your device will belong to a private network. 
In this way, the connection should always be initiated by the device. 
The possible central management server can only be connected to the device if it starts the activity. 
After the first packet sent, if no further communication is done, the server will be able to reach the device for a maximum of 25 minutes.
Your server will need to allow Telefonica Public IP addresses in your firewall.


&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; 176.83.208.0/21&nbsp; &nbsp; &nbsp; &nbsp; 176.83.144.0/21&nbsp; &nbsp; &nbsp; &nbsp; 176.83.136.0/21&nbsp; &nbsp; &nbsp; &nbsp; 176.83.152.0/21

&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; 176.83.216.0/23&nbsp; &nbsp; &nbsp; &nbsp; 176.83.164.0/23&nbsp; &nbsp; &nbsp; &nbsp; 176.83.162.0/23&nbsp; &nbsp; &nbsp; &nbsp; 176.83.166.0/23

## NBIOT
These are the NBIOT Network specific parameters:
- Attach type: EPS Attach, Combined Attach
- Inactivity timer: EC0 10s, EC1 15s, EC2 20s
- PSM timer (T3412): Range: 1 -320h. Default 1h
- Active timer (T3324): Range: 0 – 11160s. Default 180s.
- eDRX: Not available in Demo Kits. Disable eDRX e.g. AT+CEDRXS=0,5
- SMS: Not available in Demo Kits.

## LTE-M
- Attach type: EPS Attach, Combined Attach.
- Inactivity Timer: 10s
- PSM timer (T3412): Range: 1 -320h. Default 1h
- Active timer (T3324): Range: 0 – 11160s. Default 180s.
- eDRX: Not available in Demo Kits. Disable eDRX e.g. AT+CEDRXS=0,4
- SMS: Available when using “Combined Attach”.

# KITE Platform
You should have received an email to activate your account in the KITE Platform. Please check your inbox. Using KITE Platform you will manage your SIMs.
