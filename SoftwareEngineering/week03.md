# Software Engineering

# Week 03

#### 2024.04.19.(금)

### The software process

- A structured set of activities to develop a software system.

- Many different software processes but all involve:

  - `Specification (Requirements)` - defining what the system should do;
  - `Design and implementation` - defining the organization of the system and implementing the system;
  - `Validation` - checking that it does what the customer wants;
  - `Evolution`- changing the system in response to changing customer needs.

- A software process model is an abstract representation of a process. It presents a description of a process from some particular perspective.

### Software process descriptions

- Process descriptions: `Activities + Orders`

- Process descriptions may also include:

* `Products`, which are the outcomes of a process activity;
* `Roles`, which reflect the responsibilities of the people involved in the process;
* `Pre- and post-conditions`, which are statements that are true before and after a process activity has been enacted or a product produced.

### Plan-driven and agile processes

- `Plan-driven` processes
  - All of the process activities are planned in advance and progress is measured against this plan.
- `Agile` processes

* Planning is incremental and it is easier to change the process to reflect changing customer requirements.

- In practice, most practical processes include elements of both plan-driven and agile approaches.
- There are `no right or wrong software processes.`

## Software process models

### Software process models

- `The waterfall model - plan-driven`
  - Separate and distinct phases of specification and development.
- `Incremental development - plan-driven or agile`
  - Specification, development and validation are interleaved.
- `Integration and configuration - plan-driven or agile`
  - The system is assembled from existing configurable components.
- In practice, most large systems are developed using a process that incorporates elements from all of these models.

### The waterfall model

- In principle, a phase has to be complete before moving onto the next phase.

- Each phase produces outputs such as documents which will be used as inputs for the next phase.

![3](/assets/images/2024-04-19/3.png)

### Waterfall model problems

- Inflexible partitioning of the project into distinct stages makes it difficult to respond to changing customer requirements.
  - Therefore, this model is only appropriate when the requirements are well-understood and changes will be fairly limited during the design process.
  - Few business systems have stable requirements.
- The waterfall model is mostly used for large systems engineering projects where a system is developed at several sites.
  - In those circumstances, the plan-driven nature of the waterfall model helps coordinate the work.

### Incremental development

![4](/assets/images/2024-04-19/4.png)

### Incremental development benefits

- Reduced cost for accommodating customer requirements changes.
  - The amount of analysis and documentation that has to be redone is much less than that is required with the waterfall model.
- Easy to get customer feedback.
  - Customers can comment on demonstrations of the software and see how much has been implemented.
- More rapid delivery and deployment to the customer.
  - Customers are able to use and gain value from the software earlier than is possible with a waterfall process.

### Incremental development problems

- The process is not visible.
  - Managers need regular deliverables to measure progress.
  - If systems are developed quickly, it is not cost-effective to produce documents that reflect every version of the system.
- System structure tends to degrade as new increments are added.
  - Unless time and money is spent on refactoring to improve the software, regular change tends to corrupt its structure.
  - Incorporating further software changes becomes increasingly difficult and costly.

### Integration and configuration

- Based on software reuse where systems are integrated from existing components or application systems.

* These existing components are sometimes called COTS(Commercial-off-the-shelf) systems.

- Reused elements may be configured to adapt their behaviour and functionality to a user's requirements.

- `Reuse is now the standard approach` for building many types of business system.

### Types of reusable software

- `Stand-alone application systems(COTS)` that are configured for use in a particular environment.

- `Collections of objects` that are developed as a package to be integrated with a component framework such as PyGame.

- `Web services` that are developed according to service standards and which are available for remote invocation.

- Even `entire systems` can be considered as reusable software.
  - Cloud Services like AWS, Containers supported by Docker, etc.

### Reuse-oriented software engineering

- Key Process Stages

![5](/assets/images/2024-04-19/5.png)

### Advantages and disadvantages

- Advantages

* Reduced costs and risks, as less software is developed from scratch.
* Faster delivery and deployment of system.

- Disadvantages

* Requirements compromises are inevitable so system may not meet real needs of users.
* Loss of control over evolution of reused system elements.
