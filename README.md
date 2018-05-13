# RSF
Robot Security Framework (RSF) is a standardized methodology to perform security assessments in robotics.

Based on the work of _Shyvakov, O. (2017). Developing a security framework for robots (Master's thesis, University of Twente)_.

## Differences from _Shyvakov Robot Security Framework_:
- `Component` becomes `aspect`
- Content within `evaluation criteria` has been moved into further sub-sections.

## Bibliography
- [1] Shyvakov, O. (2017). Developing a security framework for robots (Master's thesis, University of Twente)

## The `Robot Security Framework`

_Text description dumping the table from [Shyvakov_MA_EEMCS (1).pdf](https://github.com/aliasrobotics/management/files/1997960/Shyvakov_MA_EEMCS.1.pdf) and reasoning about it._

## 1. `Layer`: Physical
### 1.1 `Aspect`: External ports
#### 1.1.1 `Criteria`: Presence of external communication ports

- `Objective`: identify presence of unprotected external ports
- `Rationale`: Unprotected external ports can let attackers in physical proximity to perform a variety of attacks and serve as an entry point for them
- `Method`:
   - Inspect documentation / consult developers / inspect robot’s body and look for accessible ports (e.g. Ethernet, USB)
   - Open all doors, which are not protected by locks and look for ports inside
   - Investigate ventilation holes and see if they are wide enough to access internal communication ports

#### 1.1.2 `Criteria`:  Security of external communication ports
- `Objective`: verify if attackers can sniff or modify any critical data during communication with a docking station or by connecting to the ports.
- `Rationale`: Unprotected external ports can let attackers in physical proximity to perform a variety of attacks and serve as an entry point for them
- `Method`: ...
   - Connect to the identified communication ports
   - Is authentication required to use them (e.g. Network access control for Ethernet) and do accounts meet requirements from **section 4.1?** (_review this_)
   - Try communicating with them, attempt fizzing to discover if robot’s state can be affected.
   - If a robot connects to a docking station to transfer some data, try to use sniffers to see how data exchange is being done (verify if some sensitive, configuration or control data is transferred in clear text)

### 1.2 `Aspect`: Internal components
#### 1.2.1 `Criteria`:  Availability of internal components from outside

- `Objective`: identify internal hardware that is accessible from outside without a need
- `Rationale`: Directly accessible internal components can be physically damaged, stolen, tampered or completely disabled
- `Method`:
   - Inspect robots body and look for accessible components (e.g. HDD, embedded devices)
   - Open all doors which are not protected by locks and look for accessible components inside
- `Notes`:  *All cables should also remain inside of the robot. Some components require to be partially outside of the body frame (e.g. range finding systems, WI-FI/LTE antennas) in such a case only the required part should stick out, but not the whole component*.

#### 1.2.2 `Criteria`: Monitoring and alert capabilities

- `Objective`: identify whether rogue access to the internal hardware of the robot can be detected.
- `Rationale`: Having no verification whether the internals of the robot were accessed or not means that attackers can easily tamper with any internal components or install a hardware Trojan unnoticed.
- `Method`: ...
   - Identify all parts of the frame that can be opened or removed to get access to the internal components
   - Check whether there is an active (tamper switches) or passive (tamper evident screws and seals) monitoring
capability present
   - In case of active monitoring capability, verify that operator receives a real-time alert and the incident is
being logged and acted upon by reviewing procedures
- `Notes`: *Passive monitoring provides information upon inspection whether internals were accessed or not. However, there is still a time window between inspections when exploited robots can be abused*.

## 2. `Layer`: Network
### 2.1 `Aspect`: Internal network
#### 2.1.1 `Criteria`: Monitoring and alert capabilities

- `Objective`: identify whether internal network activity is monitored and alerts are issued based on known signatures or anomalies
- `Rationale`: Proper security controls on the internal network might be quite hard and sometimes even impossible to implement due to hardware limitations or performance requirements. If all other security measures from this document are implemented properly, unauthorized access to the internal network is very unlikely. Therefore monitoring capability should be a sufficient security control.
- `Method`: ...
   - Enumerate internal network and find entry points (e.g. switch)
   - Connect to the network and attempt to perform network based attacks (e.g. ARP poisoning, denial of service on a particular node) and verify whether an operator receives a real time alert and incidents are being logged and acted upon by reviewing procedures.
- `Notes`: *If it is not possible to implement full network monitoring due to hardware limitations. At least there should a capability to
detect new unauthorized devices on the network. In general thresholds on IDS of the internal network should be lower than on the external network. Because normal user is usually not supposed to connect to the internal network*.



## x. `Layer`: ...
### x.y `Aspect`: ...
#### x.y.z `Criteria`: ...

- `Objective`: ...
- `Rationale`: ...
- `Method`: ...


## Simplified template to execute the assessment

| Layer | Aspect | Criteria | Objective | Rationale | Method | Assessment |
| ----- | --------| --------|------------ |------------- | --------| ---------|
| Physical | External ports | Presence of external communication ports |  identify presence of unprotected external ports | Unprotected external ports can let attackers in physical proximity to perform a variety of attacks and serve as an entry point for them  | **How to** a) Inspect documentation / consult developers / inspect robot’s body and look for accessible ports (e.g. Ethernet, USB) b) Open all doors, which are not protected by locks and look for ports inside c) Investigate ventilation holes and see if they are wide enough to access internal communication ports | |
| Physical | External ports | Security of external communication ports |  verify if attackers can sniff or modify any critical data during communication with a docking station or by connecting to the ports. | Unprotected external ports can let attackers in physical proximity to perform a variety of attacks and serve as an entry point for them   | How to a) Connect to the identified communication ports b) Is authentication required to use them (e.g. Network access control for Ethernet) and do accounts meet requirements from section 4.1? c) Try communicating with them, attempt fizzing to discover if robot’s state can be affected. d) If a robot connects to a docking station to transfer some data, try to use sniffers to see how data exchange is being done (verify if some sensitive, configuration or control data is transferred in clear text) | |
| ... | | | | | | |
