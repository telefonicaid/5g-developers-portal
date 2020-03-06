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
