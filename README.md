# The `Robot Security Framework` (RSF)
Robot Security Framework (RSF) is a standardized methodology to perform security assessments in robotics.

**Version**: 0.2

## How to cite our work
TODO

## Simplified `Markdown` template to execute the assessment

TODO: review new format.


<!-- Layer 1 -->
<table>
  <tr>
    <th colspan="3"> 

### Physical layer
   </th>
  </tr>
  
  <!-- Aspect 1 -->
  <tr>
   <td>Aspect: <b>Ports</b></td>
   <td>
     
   <!-- Criteria 1 -->
   <table>
    <tr>
      <th colspan="2">Criteria: <b>Presence of external communication ports</b></th>
    </tr>
    <tr>
      <td>Objective</td>
      <td>Identify presence of unprotected external ports</td>
    </tr>
    <tr>
      <td>Rationale</td>
      <td>Unprotected external ports can let attackers in physical proximity to perform
        a variety of attacks and serve as an entry point for them</td>
    </tr>
    <tr>
      <td>Method</td>
      <td>
        
- Inspect documentation, consult developers and inspect robot’s body and components.
Look for accessible ports (e.g. Ethernet, USB, CAN, etc.)
- Open all doors, which are not protected by locks and look for ports inside</td>
    </tr>
   </table>

   <!-- Criteria 2 -->
   <table>
    <tr>
      <th colspan="2">Criteria: <b>Presence of internal communication ports</b></th>
    </tr>
    <tr>
      <td>Objective</td>
      <td>Identify presence of unprotected internal ports that typically correspond
with sensors, user interfaces, power or other robot-related components</td>
    </tr>
    <tr>
      <td>Rationale</td>
      <td>Unplugging robot components can potentially lead to the exposure of internal
communication ports. Often, these internal communication ports are typically
not protected in robots. This may allow attackers in physical proximity to
perform a variety of attacks and serve as an entry point</td>
    </tr>
    <tr>
      <td>Method</td>
      <td>
        
- Open all doors, which are not protected by locks, even those protected, and look
for robot components and their buses
- Investigate ventilation holes and see if they are wide enough to access internal
communication ports</td>
    </tr>
   </table>

   <!-- Criteria 3 -->
   <table>
    <tr>
      <th colspan="2">Criteria: <b>Security of external and internal communication ports</b></th>
    </tr>
    <tr>
      <td>Objective</td>
      <td>Verify if attackers can sniff or modify any critical data during communication
with a docking station or by connecting to the ports</td>
    </tr>
    <tr>
      <td>Rationale</td>
      <td>Unprotected external and internal ports can let attackers in physical proximity
to perform a variety of attacks and serve as an entry point for them</td>
    </tr>
    <tr>
      <td>Method</td>
      <td>
        
- Try to connect to the identified communication ports:
  - Determine if authentication is required(e.g. Network access control for Ethernet)?
  - Assess whether the communication is encripted
  - Try communicating with them, attempt fizzing to discover if robot’s state can
be affected.
- If a robot connects to a docking station to transfer some data, try to use sniffers
to see how data exchange is being done (verify if some sensitive, configuration or
control data is transferred in clear text)</td>
    </tr>
   </table>

   </td>
  </tr>
  
  <!-- Aspect 2 -->
  <tr>
   <td>Aspect: <b>Components</b></td>
   <td>
     
   <!-- Criteria 1 -->
   <table>
    <tr>
      <th colspan="2">Criteria: <b>Availability of components from outside</b></th>
    </tr>
    <tr>
      <td>Objective</td>
      <td>Identify internal hardware that is accessible from outside without a need</td>
    </tr>
    <tr>
      <td>Rationale</td>
      <td>Directly accessible components can be physically damaged, stolen, tampered,
removed or completely disabled causing the robot to misbehave. The most
obvious example is the removal of critical sensors for the behavior of the robot</td>
    </tr>
    <tr>
      <td>Method</td>
      <td>
        
- Inspect robots body and look for accessible components (e.g. sensors, actuators,
computation units, user interfaces, power components, etc.)
- Open all doors which are not protected by locks and look for accessible components
inside.</td>
    </tr>
    <tr>
      <td>Notes</td>
      <td>All cables should also remain inside of the robot. Some components require
to be partially outside of the body frame (e.g. certain sensors such as range finders, or
the antennas of certain wireless communication components) in such a case only the
        required part should stick out, but not the whole component</td>
    </tr>
   </table>

   <!-- Criteria 2 -->
   <table>
    <tr>
      <th colspan="2">Criteria: <b>Monitoring and alerting capabilities</b></th>
    </tr>
    <tr>
      <td>Objective</td>
      <td>Identify whether rogue access to the internal hardware of the robot can be
detected</td>
    </tr>
    <tr>
      <td>Rationale</td>
      <td>Having no verification whether the internals of the robot were accessed or
not means that attackers can easily tamper with any components or install a hardware
*trojan* unnoticed</td>
    </tr>
    <tr>
      <td>Method</td>
      <td>
        
- Identify all parts of the frame that can be opened or removed to get access to the
components or modules.
- Check whether there is an active (tamper switches) or passive (tamper evident
screws and seals) monitoring capability present.
- In case of active monitoring capability, verify that operator receives a real-time
alert and the incident is being logged and acted upon by reviewing procedures.</td>
    </tr>
    <tr>
      <td>Notes</td>
      <td>Passive monitoring provides information upon inspection whether internals
were accessed or not. However, there is still a time window between inspections when
        exploited robots can be abused</td>
    </tr>
   </table>

   <!-- Criteria 3 -->
   <table>
    <tr>
      <th colspan="2">Criteria: <b>Review logs of physical changes in the robot</b></th>
    </tr>
    <tr>
      <td>Objective</td>
      <td>Verify the logs of the robot and look for tampering actions. Log examples
include powering on/off events, connection/disconnection of physical components,
sensor values or actuator actions. Detect potential tampering based on this information</td>
    </tr>
    <tr>
      <td>Rationale</td>
      <td>Most robots register logs of a variety of events going from powering on/off
the robot to each individual component data. Specially, some robots detect physical changes on their components and register it. Such changes could lead to an undetected tampering of the system. Reviewing the logs could lead to discovering physical
tampering of the robot</td>
    </tr>
    <tr>
      <td>Method</td>
      <td>
        
- Review the logs of powering on and off routines of the robot.
- Review the logs of physical changes in the robot.
- Review the logs of each individual component and look for anomalies.</td>
    </tr>
   </table>

   </td>
  </tr>
</table>







## License
GPLv3.

## Glossary
- **component**: a part of something that is discrete and identifiable with respect to combining with other parts to produce something larger (*source: ISO/IEC 24765*).
  - *Note 1 to entry*: Component can be either software or hardware. Even a component that is mainly software or hardware can be referred to as a software or hardware component respectively.
- **module**: component with special characteristics to facilitate system design, integration, interoperability, re-use.  
- **interoperability**: the capability to communicate and transfer data among modules and combine modules physically in a manner that requires the user to have little or no knowledge of the unique characteristics of those modules.
- **firmware**: (in the context of robotics) software that is embedded in robots.
