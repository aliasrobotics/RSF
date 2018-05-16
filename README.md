# RSF
Robot Security Framework (RSF) is a standardized methodology to perform security assessments in robotics.

Based on the work of _Shyvakov, O. (2017). Developing a security framework for robots (Master's thesis, University of Twente)_.

## Differences from _Shyvakov Robot Security Framework_:
- `Component` becomes `aspect`
- Content within `evaluation criteria` has been moved into further sub-sections.
- Formalized `Firmware` layer, added `middleware` as a relevant aspect and elaborated it.

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
   - Is **authentication** required to use them (e.g. Network access control for Ethernet) and do accounts meet requirements from **section 4.1?** (_review this_)
   - Try **communicating** with them, attempt fizzing to discover if robot’s state can be affected.
   - If a robot connects to a docking station to transfer some data, try to use **sniffers** to see how data exchange is being done (verify if some sensitive, configuration or control data is transferred in clear text)

### 1.2 `Aspect`: Internal components
#### 1.2.1 `Criteria`:  Availability of internal components from outside

- `Objective`: identify internal hardware that is accessible from outside without a need
- `Rationale`: Directly accessible internal components can be physically damaged, stolen, tampered or completely disabled
- `Method`:
   - Inspect robots body and look for accessible components (e.g. HDD, embedded devices)
   - Open all doors which are not protected by **locks** and look for accessible components inside
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

#### 2.1.2 `Criteria`: Firewall

- `Objective`: identify whether internal network is separated from the external by the firewall.
- `Rationale`: Firewall can help to further protect internal nodes from the outside and ensure that they cannot accidentally leak data to the external network.
- `Method`:
  - Inspect documentation / consult developers / inspect node which is responsible for external communications and identify whether firewall if enabled.
  - Inspect firewall settings and verify that no internal nodes are allowed to communicate to the external network unless it is necessary.
  - If VPN is used verify that there are rules which allow internal nodes to communicate with the outside world only via the VPN tunnel.

### 2.2 `Aspect`: External network
#### 2.2.1 `Criteria`: Protocol security

- `Objective`: check if used protocol is up-to-date, secure and have no known vulnerabilities.
- `Rationale`: vulnerabilities in communication protocols can allow attackers to get unauthorized access to the external network of the robot and intercept or modify any transmitted data.
- `Method`: ...
How to
  - Identify all communication capabilities being present by inspecting documentation / consulting developers / manual analysis
  - Analyze if used protocol versions provide encryption and mutual authentication
  - Verify that used protocol is hardened according to industry standards. There is a suggested standard that can be used next to the protocol name.
    - WIFI:
      - If robot acts as an access point – SANS, Residential Wireless Network Audit Checklist https://www.sans.org/media/score/checklists/ResidentialWirelessNetworkAudit.doc
      - If robot acts as a client, it should be able to support strong encryption standards (WPA2-PSK, WPA2-EAP)

    - Zigbee – Homeland security, Recommended Practices Guide for Securing ZigBee Wireless Networks in Process Control System Environments, Section G, Security Best Practice Recommendations https://energy.gov/sites/prod/files/oeprod/DocumentsandMedia/Securing_ZigBee_Wireless_Networks.pdf

    - Bluetooth - NIST 800-121 Guide to Bluetooth Security, Table 4-2. Bluetooth Piconet Security Checklist http://csrc.nist.gov/publications/drafts/800-121/sp800_121_r2_draft.pdf

- `Note`: *Amount of suggestions from the documentation that should be implemented depends on the robot application and required security level. If providing encryption on the protocol level is not possible for some reasons, VPN or application level encryption should be used*.


#### 2.2.2 `Criteria`:  Network ports exposure

- `Objective`: identify whether only necessary network ports are exposed to the external network.
- `Rationale`: More open ports mean bigger attack surface and therefore their number should be as low as possible. Services that are exposed should have no known vulnerabilities due to the ease of their exploitation.
- `Method`: ...
  - Connect to the same network that is used by the robot for communication and scan all robot’s TCP and UDP ports, find the open ones. If possible verify with developers if their presence is required
  - Attempt to identify what service is running behind an open port and its version
  - Verify whether identified service is still receiving security updates and has no known vulnerabilities
- `Note`: UDP port scanning can be very slow and in case of time constraints it can be limited to the amount of most popular ports which can be scanned in a given amount of time.

#### 2.2.3 `Criteria`: Monitoring and alert capabilities

- `Objective`: identify whether external network activity is monitored and alerts are issued based on known signatures or anomalies.
- `Rationale`: Properly configured external network monitoring can spot network based attacks in their inception even if other security mechanisms are compromised.
- `Method`:
  - If external network is password protected attempt common password guessing. Verify whether an operator receives a real-time alert and the incident is being logged and acted upon by reviewing procedures.
  - Connect to the external network
  - Try to perform network based attacks (e.g. network scans, ARP poisoning, denial of service) and verify whether operator receives a real-time alert and the incident is being logged and acted upon by reviewing procedures.

## 3. `Layer`:  Firmware layer
Software that is embedded in robots.

### 3.1 `Aspect`: Operating System (OS)
#### 3.1.1 `Criteria`: Underlying OS updates

- `Objective`: verify that the used operating system is still supported by the manufacturer and there is a mechanism to perform system updates
- `Rationale`: Outdated operating systems can have security vulnerabilities
- `Method`:
  - Check if the underlying OS is still maintained and receive security patches
  - Check whether the latest security updates are applied
  - Check if there is an update mechanism present

### 3.2 `Aspect`: Middleware
#### 3.1.1 `Criteria`: Middleware updates

- `Objective`: verify that the used middleware is still maintained and supported by the manufacturer. Very the mechanisms to perform system updates in the middlware.
- `Rationale`: Outdated middlewares in robotics are subject to have security vulnerabilities. This is specially true with ROS, ROS 2 and other middlewares.
- `Method`:
  - Check if the underlying middleware is still maintained and receive security patches
  - Check whether the latest security updates are applied
  - Check if there is an update mechanism present

### 3.3 `Aspect`: Firmware
#### 3.3.1 `Criteria`: Firmware updates

- `Objective`: check if manufacturer firmware can be securely updated
- `Rationale`: If new vulnerabilities are discovered it is important to ensure that there is a way to provide updates to all the devices that are already sold to customers. However, update mechanism can be circumvented by an attacker to deliver malicious update. Therefore, it is important to verify the origin of the update prior to installation.
- `Method`:
  - Identify if there is a mechanism to deliver firmware updates
  - Verify that updates are cryptographically signed
  - Verify that the signature is verified prior to installation

#### 3.3.2 `Criteria`: Integrity check

- `Objective`: identify whether the system performs an integrity check of critical components and takes action if they are not present or modified.
- `Rationale`: Tampering with any of the critical components can make robot to cause physical to people and property
- `Method`:
  - Consult documentation / developers to find whether integrity check for critical components is being present
  - Try disabling or modifying critical components (e.g. safety sensors or range finding systems) of the robot and check if operator receives a real-time alert and the incident is being logged.
  - Check whether robot continues to function afterwards. Its operation should be stopped as soon as any critical component is disabled or modified. (e.g. if a proximity sensor is disabled the robot should not be able to move, because it will not be able to spot obstacles and can easily do some physical damage).
- `Note`: Critical components are components that can directly influence robot operations, functionality or safety.

## 4. `Layer`: Application
### 4.1 `Aspect`: Accounts
#### 4.1.1 `Criteria`: Default passwords

- `Objective`: identify presence of default passwords
- `Rationale`: Default passwords are easy to find on the internet and so far, remain the most popular and easy way to exploit internet connected devices.
- `Method`:
  - Review documentation / consult developers and identify whether default passwords are used
  - Attempt to login with commonly used passwords
  - If default passwords are used verify whether their change is enforced on the first use
  - If unique passwords are created on a per device basis, ensure that they are random and not in a sequential order
- `Note`: When trying commonly used passwords beware of account lockouts and verify that there is a recovery mechanism present.

#### 4.1.2 `Criteria`: Password complexity

- `Objective`: verify that password complexity is enforced.
- `Rationale`: Weak passwords take little time to guess.
- `Method`: Attempt to change password to a weak one and verify if change succeeded.
- `Note`: Password complexity requirements depend on the sensitivity of the application. In general the minimum requirements that should be in place are:
  - Password length at least 8 characters
  - Enforce usage of 3 of 4 categories (lower-case, upper-case, numbers, special characters)

#### 4.1.3 `Criteria`: Login Lockout

- `Objective`: identify whether the login lockout is present
- `Rationale`: Having strong and non-default passwords is not enough. And brute force attempts should be prevented by implementing a login lockout mechanism.
- `Method`: Attempt to login with incorrect credentials multiple times. Verify that the account has got locked out.
- `Note`: The lockout threshold depends on the sensitivity of the service. In general, it should be 5 login attempts or less. Prior to testing verify that lockout recovery mechanism is being present. Accounts can be either locked out for a specific duration of time and/or they can be recovered by physical interaction with the robot.

#### 4.1.4 `Criteria`: Hardcoded or backdoor accounts

- `Objective`: identify presence of hardcoded / backdoor accounts
- `Rationale`: Hardcoded / backdoor credentials pose same danger as default passwords. However, their identification is usually harder due to the need for reverse engineering or possession of the source code.
- `Method`: ...
  - Consult documentation and developers to identify whether hardcoded / backdoor credentials are used
  - Analyze the source code for hardcoded / backdoor credentials

#### 4.1.5 `Criteria`: Cleartext passwords

- `Objective`: identify whether passwords are stored in cleartext.
- `Rationale`: Cleartext passwords can be leveraged by an attacker for privilege escalation or lateral movement.
- `Method`:
  - Review the source code and documentation / consult developers and identify whether passwords are stored in a cleartext
- `Note`:
Lockout threshold depends on the sensitivity of the service. In general, it should be 5 login attempts or less.

### 4.2 `Aspect`: Authorization
#### 4.2.1 `Criteria`: Authorization
- `Objective`: verify that resources are accessible only to authorized users or services
- `Rationale`: Access to the restricted functions by anonymous users or users with lower access control rights diminishes all the benefits of access control.
- `Method`:  
  - Login with authorized credentials and attempt to perform different actions, record the requests that are being made
  - Log out and attempt to send same requests as an unauthenticated user. Verify whether it is successful
  - Log out and login again as a user with lower access rights. Attempt to send same requests again. Verify whether it is successful

### 4.3 `Aspect`: Communication
#### 4.3.1 `Criteria`: Encryption
- `Objective`: ensure that all sensitive data is transmitted over an encrypted channel
- `Rationale`: If data is transmitted in a cleartext attackers can easily gather sensitive information (e.g. credentials, audio and video streams, private data)
- `Method`:  
  - Intercept connection between a robot and a control center application / cloud server
  - Use protocol analyzer to verify that transmitted data is encrypted

#### 4.3.2 `Criteria`: Replay protection
- `Objective`: ensure that transmitted data cannot be replayed
- `Rationale`: If replay protection is absent attackers can record legitimate packets and then arbitrary replay them to achieve desired actions
- `Method`:  
  - Intercept connection between robot and control center application / cloud server
  - Record control or configuration packets sent to the robot
  - Attempt to replay them and verify whether the desired action is executed

### 4.4 `Aspect`: 3rd party libraries and components
#### 4.4.1 `Criteria`: Vulnerabilities
- `Objective`: verify that 3 rd party software components do not have known vulnerabilities.
- `Rationale`: It is quite common to blindly rely on 3 rd party components. However they can easily introduce a vulnerability into the product where they are used.
- `Method`:  
  - Identify which 3rd party libraries and components are used and what are their versions
  - Look for known vulnerabilities in the current version
  - Verify whether identified component is still receiving security updates and has no known vulnerabilities
  - Verify that the latest security updates are installed  

### 4.5 `Aspect`: Privacy
#### 4.5.1 `Criteria`: Privacy
- `Objective`: identify whether the robot is compliant to the laws and regulations that apply
- `Rationale`: Not complying with regulations could result in financial consequences
- `Method`:  
  - Verify that minimum Personally identifiable information (PII) is collected and transmitted over the internet
  - Verify that if PII is collected users are made aware of it. (e.g. in case of a video recording people can be warned by stickers or signs on the robot)
  - Verify that all PII is stored and transmitted in a secure manner
- `Note`: It is not relevant to the security of the robot itself. However not complying with privacy regulations can result in financial consequences and therefore should be taken into consideration.

### 4.6 `Aspect`: Control center application
#### 4.6.1 `Criteria`: Web application
- `Objective`: perform a security assessment of the web application
- `Rationale`: Robot can be indirectly compromised if attacker exploits a web control center application
- `Method`:  
  - Identify web interface that is being used (hosted on the robot itself or a cloud server)
  - Use OWASP methodology to test web application against OWASP Top 10 Web application vulnerabilities

#### 4.6.2 `Criteria`: Mobile phone application
- `Objective`: perform a security assessment of the mobile application
- `Rationale`: Robot can be indirectly compromised if attacker exploits a mobile phone control center application
- `Method`:  
  - Identify whether robot has a mobile app that can be used to control or interact with it.
  - Test the application against OWASP Mobile Top 10




## Simplified template to execute the assessment

| Layer | Aspect | Criteria | Objective | Rationale | Method | Assessment |
| ----- | --------| --------|------------ |------------- | --------| ---------|
| Physical | External ports | Presence of external communication ports |  identify presence of unprotected external ports | Unprotected external ports can let attackers in physical proximity to perform a variety of attacks and serve as an entry point for them  | **How to** a) Inspect documentation / consult developers / inspect robot’s body and look for accessible ports (e.g. Ethernet, USB) b) Open all doors, which are not protected by locks and look for ports inside c) Investigate ventilation holes and see if they are wide enough to access internal communication ports | |
| Physical | External ports | Security of external communication ports |  verify if attackers can sniff or modify any critical data during communication with a docking station or by connecting to the ports. | Unprotected external ports can let attackers in physical proximity to perform a variety of attacks and serve as an entry point for them   | How to a) Connect to the identified communication ports b) Is authentication required to use them (e.g. Network access control for Ethernet) and do accounts meet requirements from section 4.1? c) Try communicating with them, attempt fizzing to discover if robot’s state can be affected. d) If a robot connects to a docking station to transfer some data, try to use sniffers to see how data exchange is being done (verify if some sensitive, configuration or control data is transferred in clear text) | |
| ... | | | | | | |
