# The `Robot Security Framework` (RSF)
Robot Security Framework (RSF) is a standardized methodology to perform security assessments in robotics.

**Version**: 0.2

## How to cite our work
TODO

## Simplified `Markdown` template to execute the assessment

TODO: other formats.

| Layer | Aspect | Criteria | Objective | Rationale | Method | Assessment |
| ----- | --------| --------|------------ |------------- | --------| ---------|
| Physical | External ports | Presence of external communication ports |  identify presence of unprotected external ports | Unprotected external ports can let attackers in physical proximity to perform a variety of attacks and serve as an entry point for them  | **How to** a) Inspect documentation / consult developers / inspect robot’s body and look for accessible ports (e.g. Ethernet, USB) b) Open all doors, which are not protected by locks and look for ports inside c) Investigate ventilation holes and see if they are wide enough to access internal communication ports | |
| Physical | External ports | Security of external communication ports |  verify if attackers can sniff or modify any critical data during communication with a docking station or by connecting to the ports. | Unprotected external ports can let attackers in physical proximity to perform a variety of attacks and serve as an entry point for them   | How to a) Connect to the identified communication ports b) Is authentication required to use them (e.g. Network access control for Ethernet) and do accounts meet requirements from section 4.1? c) Try communicating with them, attempt fizzing to discover if robot’s state can be affected. d) If a robot connects to a docking station to transfer some data, try to use sniffers to see how data exchange is being done (verify if some sensitive, configuration or control data is transferred in clear text) | |
| ... | | | | | | |

## License
GPLv3.

## Glossary
- **component**: a part of something that is discrete and identifiable with respect to combining with other parts to produce something larger (*source: ISO/IEC 24765*).
  - *Note 1 to entry*: Component can be either software or hardware. Even a component that is mainly software or hardware can be referred to as a software or hardware component respectively.
- **module**: component with special characteristics to facilitate system design, integration, interoperability, re-use.  
- **interoperability**: the capability to communicate and transfer data among modules and combine modules physically in a manner that requires the user to have little or no knowledge of the unique characteristics of those modules.
- **firmware**: (in the context of robotics) software that is embedded in robots.
