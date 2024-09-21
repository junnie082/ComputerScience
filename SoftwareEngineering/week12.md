# Week12 - Reliability Engineering, Safety Engineering

#### 2024.06.07.(금)

### Topics covered

- Reliability Engineering

* Availability and reliability
* Reliability requirements
* Programming for reliability

- Safety Engineering

* Sefety-critical systems
* Safety requirements
* Safety engineering process

### Software reliability

- In general, software customers expect all software to be dependable.

* However, for non-critical applications, they may be willing to accept some system filures.

- Some applications (critical systems) have very high reliability requirements and special software engineering techniques may be used to achieve this.

* Medical systems
* Telecommunications and power systems
* Aerospace systems

### Faults, errors and failures

| Term | Description |
| Human mistake | Human behavior that results in the introduction of faults into a system. |
| System fault | A characteristic of a software system that can lead to a system error. |
| System error | An erroneous system state that can lead to system behavior that is unexpected by system users. |
| System failure | An event that occurs at some point in time when the system does not deliver a service as expected by its users. |

### Faults and failures

- Failures are a usually a result of system errors that are derived from faults in the system

- However, faults do not necessarily result in system errors.

* The erroneous system state resulting from the fault may be transient and 'corrected' before an error arises.
* The faulty code may never be executed.

- Errors do not necessarily lead to system failures.

* The error can be corrected by built-in error detection and recovery
* The failure can be protected against by built-in protection facilities.

### Fault management and Reliability Achievement

- Fault avoidance

* The system is developed in such a way that human error is avoided and thus system faults are minimised.
* The development process is organised so that faults in the system are detected and repaired before delivery to the customer.

- Fault detection

* Verification and validation techniques are used to discover and remove faults in a system before it is deployed.

- Fault tolerance

* The system is designed so that faults in the delivered software do not result in system failure.

## Availability and reliability

- Reliability

* The probability of failure-free system opeartion over a specified time in a given environment for a given purpose.

- Availability

* The probability that a system, at a point in time, will be operational and able to deliver the requested services

- Both of these attributes can be expressed quantitatively.

* e.g.) Availability of 0.999 means that the system is up and running for 99.9% of the time.

### Reliability and specifications

- Reliability can only be defined formally with respect to a system specification.

* i.e.) a failure is a deviation from a specification.

- However, many specifications are incomplete or incorrect - hence, a system that conforms to its specification may 'fail' from the perspective of system users.

- Furthermore, users don't read specifications so don't know how the system is supposed to behave.

- Therefore perceived reliability is more important in practice.

### Perceptions of reliability

- The formal definition of reliability does not always reflect the user's perception of a system's reliability.

- The assumptions that are made about the environment where a system will be used may be incorrect.

* Usage of a system in crowded transportation provides quite different environment compared to use the same system at home.

- The consequences of system failures affects the perception of reliability.

* Failures that have serious consequences (such as an engine breakdown in a car) are given greater weight by users than failures that are inconvenient.

### Availability perception

- Availability is usually
