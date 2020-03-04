## Annex A	GSM/UMTS Cause Code 

### #2 IMSI unknown in HLR/HSS
  - **Message:** LOCATION UPDATING REJECT
  - **Technology:** 2G/3G
  - **Network:** HPLMN/VPLMN
  - **Expected Result:**
According to 3GPP  TS 24.008 (2G/3G) after receiving a  LOCATION UPDATING REJECT  with cause #2 (IMSI unknown in HLR/HSS):
    - The DuT shall consider the SIM/USIM invalid for non-GPRS services until a reboot occurs. 
The DuT could keep connected to GPRS services.

In addition to 3GPP standards, Telefonica requirements and guidelines also have to be followed:
    - Reboots are required to recover the full connectivity of the DuT (CS+PS) and shall not be more frequent than once every 1 hour. It is recommend to follow a randomised and exponential delayed on time reboot scheme based on hours

### #7 GPRS/EPS Service Not Allowed

### Message
DETACH REQUEST FROM THE NETWORK

### Technology
4G

### Network
VPLMN

### Expected Result

According to 3GPP  TS 24.008 (2G/3G) and TS 24.301 (LTE) after receiving a DETACH REQUEST FROM THE NETWORK with cause #7 (EPS services not allowed) in VPLMN: 

- The DuT shall consider SIM/USIM invalid for EPS services, shall retry once to register in CS selecting the 2G/3G available network and NO ATTACH REQUEST or TRACKING AREA UPDATE REQUEST or SERVICE REQUEST shall be done until a reboot occurs.

The DuT can keep attached to CS services. 

In addition to 3GPP standards, Telefonica requirements and guidelines also have to be followed:

- Reboots are required to recover the full connectivity of the DuT (CS+PS) and shall not be more frequent than once every 1 hour. It is recommend to follow a randomised and exponential delayed on time reboot scheme based on hours.
