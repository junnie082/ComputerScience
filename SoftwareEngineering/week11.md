# week11 Software Evolution

#### 2024.06.06.(목)

### Topics covered

- Evolution processes

- Legacy systems

- Software maintenance

### Software change

- Software change is inevitable.

  - New requirements emerge when the software is used;
  - The business environment changes;
  - Errors must be repaired;
  - New computers and equipment is added to the system;
  - The performance or reliability of the system may have to be improved.

- A key problem for all organizations is implementing and managing change to their existing software systems.

### Importance of evolution

- Organisations have huge investments in their software systems - they are critical business assets.

- To maintain the value of these assets to the business, they must be changed and updated.

- The majority of the software budget is devoted to changing and evolving existing software rather than developing new software.
  - Software debugging alone takes significant portion of developers' time.
  - US developers spent 35% of their time on software debugging.
  - Another study shows that debugging takes about 50% of the time, and software bugs cost $312 billion per year.

### Evolution and servicing

Software development -> Software evolution -> Software servicing -> Software retirement

### A spiral model of development and evolution

`Specification`, `Implementation`, `Validation`, `Operation`

## Evolution processes

Change requests -> Impact analysis -> Release planning -> Change implementation -> System release

Release planning -> Fault repair, Platform adaptation, System enchancement

### Change implementation

- Iteration of the development process where the revisions to the system are designed, implemented and tested.

- A critical difference is that the first stage of change implementation may involve program understanding,

  - especially if the original system developers are not responsible for the change implementation.

- During the program understanding phase,

* you have to understand how the program is structured,
* how it delivers functionality, and
* how the proposed change might affect the program.

### Agile methods and evolution

- Agile methods are based on incremental development so the transition from development to evolution is a seamless one.

  - Evolution is simply a continuation of the development process based on frequent system releases.

- Automated regression testing during continuous integration is particularly valuable when changes are made to a system.

- Changes may be expressed as additional user stories.

## Legacy Systems

- Legacy systems are older systems that rely on languages and technology that are no longer used for new systems development.

  - e.g.) Y2K issues and COBOL, Dismissal of ActiveX, Flash.

- Legacy software may be dependent on older hardware, such as mainframe computers and may have associated legacy processes and procedures.

- Legacy systems are not just software systems but are broader socio-technical systems that include hardware, software, libraries and other supporting software and business processes.

- `System hardware`: Legacy systems may have been written for hardware that is no longer available.

- `Support software`: The legacy system may rely on a range of support software, which may be obsolute or unsupported.

- `Application software`: The application system that provides the business services is usually made up of a number of application programs.

- `Application data`: These are data that are processed by the application system. They may be inconsistent, duplicated or held in different databases.

### Legacy system components

- `Business processes`: These are processes that are used in the business to achieve some business objective.

  - Business processes may be designed around a legacy system and constrained by the functionality that it provides.

- `Business policies and rules`: These are definitions of how the business should be carried out and constraints on the business.
  - Use of the legacy application system may be embedded in these policies and rules.

### Legacy system replacement

- Legacy system replacement is `risky` and `expensive` so business continue to use these systems.

- System replacement is risky for a number of reasons.
  - Lack of complete system specification
  - Tight integration of system and business processes
  - Undocumented business rules embedded in the legacy system
  - New software development may be late and/or over budget

### Legacy system change

- Legacy systems are expensive to change for a number of reasons:
  - Inconsistent programming style.
  - Use of obsolute programming languages with few people available with these language skills.
  - Inadequate system documentation.
  - System structure degradation.
  - Program optimizations may make them hard to understand.
  - Data errors, duplication and inconsistency.

### Legacy system management

- Organisations that rely on legacy systems must choose a strategy for evolving these systems.

  - `Scrap` the system completely and modify business processes so that it is no longer required;
  - Continue `maintaining` the system;
  - `Transform` the system by re-engineering to improve its maintainability;
  - `Replace` the system with a new system.

- The strategy chosen should depend on the `system quality` and its `business value`.

### Legacy system categories

- Low quality, Low business value

  - These systems should be scrapped.

- Low-quality, High-business value

  - These make an important business contribution but are expensive to maintain.
  - Should be re-engineered or replaced if a suitable system is available.

- High-quality, Low-business value

  - Replace with COTS, scrap completely or maintain.

- High-quality, High business value
  - Continue in operation using normal system maintenance.

### Business value assessment

- The use of the system

* If systems are only used occasionally or by a small number of people, they may have a low business value.

- The business processes that are supported

* A system may have a low business value if it forces the use of inefficient business processes.

- System dependability

* If a system is not dependable and the problems directly affect business customers, the system has a low business value.

- The system outputs

* If the business depends on system outputs, then the system has a high business value.

### System quality assessment

- Business process assessment

* How well does the business process support the current goals of the business?

- Environment assessment

* How effective is the system's environment and how expensive is it to maintain?

- Application assessment

* What is the quality of the application software system?

## Software maintenance

- Modifying a program after it has been put into use.

- The term is mostly used for changing custom software.

  - Generic software products are said to evolve to create new versions.

- Maintenance does not normally involve major changes to the system's architecture

- Changes are implemented by modifying existing components and adding new components to the system.

### Types of maintenance

- Fault repairs.

  - Changing a system to fix bugs/vulnerabilities and correct deficiencies in the way meets its requirements.

- Environmental adaption
  - Maintenance to adapt software to a different operating environment
  - Changing a system so that it operates in a different environment (computer, OS, etc.) from its initial implementation.

### Maintenance effort distribution

Functionality addition or modification (58%), Fault repair (24%), Environmental adaptation (19%)

### Maintenance costs

- Usually greater than development costs (2x to 100x depending on the application).

- Affected by both technical and non-technical factors.

- Increases as software is maintained.

  - Maintenance corrupts the software structure so makes further maintenance more difficult.

- Aging software can have high support costs (e.g. old languages, compilers etc.).

### Software reengineering

- Restructuring or rewriting part or all of a legacy system without changing its functionality.

- Applicable where some but not all sub-systems of a larger system require frequent maintenance.

- Reengineering involves adding effort to make them easier to maintain.
  - The system may be re-structured and re-documented.

### Advantages of reengineering

- Reduced risk

  - There is a high risk in new software development.
  - There may be development problems, staffing problems and specification problems.

- Reduced cost
  - The cost of re-engineering is often significantly less than the costs of developing new software.

### Reengineering process activities

- Source code translation

  - Convert code to a new language.

- Reverse engineering

  - Analyse the program to understand it;

- Program structure improvement

  - Restructure automatically for understandability;

- Program modularisation

  - Reorganise the program structure;

- Data engineering
  - Clean-up and restructure system data.

### Refactoring

- Refactoring is the process of making improvements to a program to slow down degradation through change.

- You can think of refactoring as '`preventative maintenance`' that reduces the problems of future change.

- Refactoring involves modifying a program to improve its structure, reduce its complexity or make it easier to understand.

- When you refactor a program, you should not add functionality but rather concentrate on program improvement.
  - Root Canal vs Floss Refactoring.

### Refactoring and reengineering

- Re-engineering takes place after a system has been maintained for some time and maintenance costs are increasing.

  - You use automated tools to process and re-engineer a legacy system to create a new system that is more maintainable.

- Refactoring is a continuous process of improvement throughout the development and evolution process.
  - It is intended to avoid the structure and code degradation that increases the costs and difficulties of maintaining a system.

### Refactoring

- Martin Fowler is one of the most famous figure in this matter.

- He and Kent Beck are famous figures in Agile development, who signed the Agile Manifesto.

### 'Bad smells' in program code

- Duplicate code (a.k.a Code Clones)

  - The same or very similar code may be included at different places in a program. This can be removed and implemented as a single method or function that is called as required.

- Long methods

  - If a method is too long, it should be redesigned as a number of shorter methods.

- Switch (case) statements

  - These often involve duplication, where the switch depends on the type of a value.
  - The switch statements may be scattered around a program. In object-oriented languages, you can often use polymorphism to achieve the same thing.

- Data clumping

  - Data clumps occur when the same group of data items (fields in classes, parameters in methods) re-occur in several places in a program.
  - These can often be replaced with an object that encapsulates all of the data.

- Speculative generality
  - This occurs when developers include generality in a program in case it is required in the future.
  - This can often simply be removed.

### Topics covered

- Dependability properties

- Sociotechnical systems

- Redundancy and diversity

- Dependable processes

### System dependability

- For many computer-based systems, the most important system property is the dependability of the system.

- The dependability of a system reflects the user's degree of trust in that system.

* It reflects the extent of the user's confidence that it will operate as users expect and that it will not 'fail' in normal use.

- Dependability covers the related systems attributes of reliability, availability and security.

* These are all inter-dependent.

### Importance of dependability

- System failures may have widespread effects with large numbers of people affected by the failure.

- Systems that are not dependable and are unreliable, unsafe of insecure may be rejected by their users.

- The costs of system failure may be very high if the failure leads to economic losses or physical damage.

- Undependable systems may cause information loss with a high consequent recovery cost.

## Dependability properties

### The principal dependability properties

Dependability

- Availability -> The ability of the system to deliver services when requested
- Reliability -> The ability of the system to deliver services as specified
- Safety -> The ability of the system to operate without catastrophic failure
- Security -> The ability of the system to protect itself against deliberate or accidental intrusion
- Resilience -> The ability of the system to resist and recover from damaging events

### Other dependability properties

- Repairability

* Reflects the extent to which the system can be repaired in the event of a failure.

- Maintainability

* Reflects the extent to which the system can be adapted to new requirements.

- Error tolerance

* Reflects the extent to which user input errors can be avoided and tolerated.

### Dependability attribute dependencies

- Safe system operation depends on the system being available and operating reliably.

- A system may be unreliable because its data has been corrupted by an external attack.

* Denial of service attacks on a system are intended to make it unavailable.
* Also, if a system is infected with a virus, you cannot be confident in its reliablity or safety.
* Security -> Reliablity, Availability, Safety.

### Dependability achievement

- Avoid the introduction of accidental errors when developing the system.
- Design V & V processes that are effective in discovering residual errors in the system.
- Design systems to be fault tolerant so that they can continue in operation when faults occur.
- Design protection mechanisms that guard against external attacks.
- Configure the system correctly for its operating environment.
- Include system capabilities to recognise and resist cyberattacks.
- Include recovery mechanisms to help restore normal system service after a failure.

### Dependability costs

- Dependability costs tend to increase exponentially as increasing levels of dependability are required.

- There are two reasons for this:

* The use of more expensive development techniques and hardware that are required to achieve the higher levels of dependability.
* The increased testing and system validation that is required to convince the system client and regulators that the required levels of dependability have been achieved.

### Dependability economics

- Because of very high costs of dependability achievement, it may be more cost effective to accept trustworthy systems ad pay for failure costs.

- However, this depends on social and political factors.

* A reputation for products that can't be trusted may lose future business.

- Depends on system type.

* For business systems in particular, modest levels of dependability may be adequate.

## Sociotechnical systems

### Systems and software

- Software engineering is not an isolated activity but is part of a broader systems engineering process.

- Software systems are therefore not isolated systems but are essential components of broader systems that have a human, social or organizational purpose.

### The Socio-Technical System(STS) stack

`Society`, `Organization`, `Business processes`, `Application system`, `Communications and data management`, `Operating system`, `Equipment`

Systems engineering: Organization, Business processes, Application system, Communications and data management, Operating system, Equipment
Software engineering: Applicatioin system, Communications and data management, Operating system

### Holistic system design

- There are interactions and dependencies between the layers in a system and changes at one level ripple through the other levels.

- For dependability, a system's perspective is essential.

* Contain software filures within the enclosing layers of the STS stack.
* Understand how faults and failures in adjacent layers may affect the software in a system.
* e.g.) Apps for delivery, mobility, contents recommendation, reviews, etc.

### Regulation and compliance

- The general model of economic organization that is now almost universal in the world is that.

  - privately owned companies offer goods and services and make a profit on these.

- To ensure the safety of their citizens, most governments regulate (limit the freedom of) privately owned companies,

* so that they must follow certain standards to ensure that their products are safe and secure.

### Regulated systems

- Many critical systems are regulated systems, which means that their use must be approved by an external regulator before the systems go into service.

* Nuclear systems
* Air traffic control systems
* Medical devices

- A safety and dependability case has to be approved by the regulator.

* Therefore, critical systems development has to create the evidence to convince a regulator that the system is dependable, safe and secure.

### Safety regulation

- Regulation and compliance (following the rules) applies to the sociotechnical system as a whole and not simply the software element of that system.

- Safety-related systems may have to be certified as safe by the regulator.

- To achieve certification, companies that are developing safety-critical systems have to produce an extensive safety case that shows that rules and regulations have been followed.

- It can be as expensive develop the documentation for certification as it is to develop the system itself.

## Redundancy and diversity

- Redundancy

* Keep more than a single version of critical components so that if one fails then a backup is available.

- Diversity

* Provide the same functionality in different ways in different components so that they will not fail in the same way.

- Redundant and diverse components should be independent so that they will not suffer from 'common-mode' failures

* For example, components implemented in different programming languages means that a compiler fault will not affect all of them.

### Process diversity and redundancy

- Process activities, such as validation, should not depend on a single approach, such as testing, to validate the system.

- Redundant and diverse process activities are important especially for verification and validation.

- Multiple, different process activities the complement each other and allow for cross-checking help to avoid process errors, which may lead to errors in the software.

### Problems with redundancy and diversity

- Adding diversity and redundancy to a system increases the system complexity.

- This can increase the chances of error because of unanticipated interactions and dependencies between the redundant system components.

- Some engineers therefore advocate simplicity and extensive V & V as a more effective route to software dependability.

- e.g.) Airbus FCS architecture is redundant/diverse; Boeing 777 FCS architecture has no software diversity.

## Dependable processes

- To ensure a minimal number of software faults, it is important to have a well-defined, repeatable software process.

- A well-defined repeatable process is one that does not depend entirely on individual skills; rather can be enacted by different people.

- Regulators use information about the process to check if good software engineering practice has been used.

- For fault detection, it is clear that the process activities should include significant effort devoted to verification and validation.

### Dependable process characteristics

- Explicitly defined

* A process that has a defined process model that is used to drive the software production process.
* Data must be collected during the process that proves that the development team has followed the process as defined in the process model.

- Repeatable

* A process that does not rely on individual interpretation and judgment.
* The process can be repeated across projects and with different team members, irrespective of who is involved in the development.

### Dependable process activities

- Requirements reviews to check that the requirements are, as far as possible, complete and consistent.

- Requirements management to ensure that changes to the requirements are controlled and that the impact of proposed requirements changes is understood.

- `Formal specification`, where a mathematical model of the software is created and analyzed.

- `System modeling`, where the software design is explicitly documented as a set of graphical models, and the links between the requirements and these models are documented.

### Dependable process activities

- Design and program inspections, where the different descriptions of the system are inspected and checked by different people.

- `Static analysis`, where automated checks are carried out on the source code of the program.

- `Test planning and management`, where a comprehensive set of system tests is designed.

* The testing process has to be carefully managed to demonstrate that these tests provide coverage of the system requirements and have been correctly applied in the testing process.

### Dependable processes and agility

- Dependable software often rquires certification so both process and product documentation has to be produced.

- Up-front requirements analysis is also essential to discover requirements and requirements conflicts that may compromise the safety and security of the system

- These conflict with the general approach in agile development of co-development of the requirements and the system and minimizing documentation.

- An agile process may be defined that incorporates techniques such as iterative development, test-first development and user involvement in the development team.

- So long as the team follows that process and documents their actions, agile methods can be used.

- However, additional documentation and planning is essential so 'pure agile' is impractical for dependable systems engineering.
