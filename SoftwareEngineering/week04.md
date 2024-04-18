# Software Engineering

# Week 04

#### 2024.04.19.(금)

### Rapid software development

- Rapid development and delivery is now often the most important requirement for software systems
  - Business operate in a fast-changing requirement and it is practically impossible to produce a set of stable software requirements.
  - Software has to evolve quickly to reflect changing business needs.
- Plan-driven development is essential for some types of system, but does not meet these business needs.

- Agile development methods emerged in the late 1990s whose aim was to radically reduce the delivery time for working software systems.

### Agile development

- Program specification, design, implementation and testing are inter-leaved.

- The system is developed as a series of versions or increments with stakeholders involved in version specification and evaluation.

- Frequent delivery of new versions for evaluation.

- Extensive tool support (e.g. automated testing tools) used to support development.

- Minimal documentation - focus on working code.

### Plan-driven and agile development

![9](/assets/images/2024-04-19/9.png)

### Plan-driven and agile development

- Plan-driven development

  - A plan-driven approach to software engineering is based around separate development stages with the outputs to be produced at each of these stages planned in advance.
  - Not necessarily waterfall model - plan-driven, incremental development is possible.
  - Iteration occurs within activities.

- Agile development
  - Specification, design, implementation and testing are inter-leaved and the outputs from the development process are decided through a process or negotiation during the software development process.

## Agile methods

### Agile methods

- Dissatisfaction with the overheads involved in software design methods of the 1980s and 1990s led to the creation of agile methods. These methods:

  - focus on the code rather than the design.
  - are based on an iterative approach to software development.
  - are intended to deliver working software quickly and evolve this quickly to meet changing requirements.

- The aim of agile methods is to reduce overheads in the software process (e.g. by limiting documentation) and to be able to respond quickly to changing requirements without excessive rework.

### Agile manifesto

- We are uncovering better ways of developing software by doing it and helping others do it.

- Through this work we have come to value:

  - `Individuals and interactions` over processes and tools.
  - `Working software` over comprehensive documentation.
  - `Customer collaboration` over contract negotiation.
  - `Responding to change` over following a plan.

- That is, while there is value in the items on the right, we value the items on the left more.

### The principles of agile methods

| Principle | Description |
| Customer involvement | Customers' role is provide and prioritize new system requirements and to evaluate the iterations of the system. |
| Incremental delivery | The software is developed in increments with the customer specifying the requirements to be included in each increment. |
| People not process | The skills of the development team should be recognized and exploited. Team members should be left to develop their own ways of working without prescriptive processes. |
| Embrace change | Expect the system requirements to change and so design the system to accommodate these changes. |
| Maintain simplicity | Focus on simplicity in both the software being developed and in the development process. |

### Agile method applicability

- Product development where a software company is developing a small or medium-sized product for sale.

  - Virtuallu all software products and apps are now developed using an agile approach.

- Custom system development within an organization, where there is a clear commitment from the customer to become involved in the development process and where there are few external rules and regulations that affect the software.

## Agile development techniques

### Extreme programming

- A very influential agile method, developed in the late 1990s, that introduced a range of agile development techniques.

- Extreme Programming (XP) takes an 'extreme' approach to iterative development.
  - New versions may be built several times per day;
  - Increments are delivered to customers every 2 weeks;
  - All tests must be run for every build and the build is only accepted if tests run successfully.

### The extreme programming release cycle

![10](/assets/images/2024-04-19/10.png)

### Extreme programming practices

| principle or practice | Description |
| Incremental planning | Requirements are recorded on story cards and the stories to be included in a release are determined by the time available and their relative priority. The developers break these stories into development 'Tasks'. |
| Small releases | The minimal useful set of functionality that provides business value is developed first. Releases of the system are frequent and incrementally add functionality to the first release. |
| Simple design | Enough design is carried out to meet the current requirements and no more. |
| Test-first development | An automated unit test framework is used to write tests for a new piece of functionality before that functionality itself is implemented. |
| Refactoring | All developers are expected to refactor the code continuously, and keep the code simple and maintainable. |

### Extreme programming practices

| Principle or practice | Description |
| Pair programming | Developers work in pairs, checking each other's work and providing the support to always do a good job. |
| Collective ownership | The pairs of developers work on all areas of the system, so that no islands of expertise develop and all the developers take responsibility for all of the code. |
| Continuous integration | As soon as the work on a task is complete, it is integrated into the whole system. |
| Sustainable pace | Large amounts of overtime are not considered acceptable as the net effect is often to reduce code quality and medium term productivity |
| On-site customer | In an extreme programming process, the customer is a member of the development team and is responsible for bringing system requirements to the team for implementation. |

### XP and agile principles

- Incremental development is supported through small, frequent system releases.

- Customer involvement means full-time customer engagement with the team.

- People not process through pair programming, collective ownership and a process that avoids long working hours.

- Change supported through regular system releases.

- Maintaining simplicity through constant refactoring of code.

### Influential XP practices

- Extreme programming has a technical focus and is not easy to integrate with management practice in most organizations.

- Consequently, while agile development uses practices from XP, the method as originally defined is not widely used.

- Key practices
  - User stories for specification
  - Refactoring
  - Test-first development
  - Pair programming

### User stories for requirements

- In XP, a customer or user is part of the XP team and is responsible for making decisions on requirements.

- User requirements are expressed as user stories or scenarios.

- These are written on cards and the development team break them down into implementation tasks.

  - These tasks are the basis of schedule and cost estimates.

- The customer chooses the stories for inclusion in the next release based on their priorities and the schedule estimates.

### A Story Example

- A Simple To-Do App Example.

- There are seven user stories describing what's necessary for the app.

- Pick up one of the stories, then identify tasks to be done.

  - Can we use LLMs for this?

- Each task may have sub-tasks - i.e., it's hierarchical.

  - One task can be divided into smaller tasks to be done.

- Review and Discuss you identified tasks properly.

### Refactoring

- Conventional wisdom in software engineering is to design for change.

  - It is worth spending time and effort anticipating changes as this reduces costs later in the life cycle.

- XP, however, maintains that this is not worthwhile as changes cannot be reliably anticipated.

- Rather, it proposes constant code improvement (refactoring) to make changes easier when they have to be implemented.

- `Refactoring` is an activity which `improves software without modifying its original functionalities.`

- Programming teams look for possible software improvements and make these improvements even where there is no immediate need for them.

- This improves the understandability of the software and so reduces the need for documentation.

- Changes are easier to make because the code is well-structured and clear.

- However, some changes require architecture refactoring and this is much more expensive.

### Examples of refactoring

- Re-organization of a class hierarchy to remove duplicate code.

- Tidying up and renaming attributes and methods to make them easier to understand.

  - Increase readability and maintainability of code.

- The replacement of inline code with calls to methods that have been included in a program library.
  - Extract Method Refactoring.

### Test-first development

- Testing is central to XP and XP has developed an approach where the program is tested after every change has been made.

- XP testing features:

  - Test-first development.
  - Incremental test development from scenarios.
  - User involvement in test development and validation.
  - Automated test harnesses are used to run all component tests each time that a new release is built.

- Writing tests before code clarifies the requirements to be implemented.

- Tests are written as programs rather than data so that they can be executed automatically.

  - The test includes a check that it has executed correctly.
  - Usually relies on a testing framework such as JUnit, PyTest, or Jest.

- All previous and new tests are run automatically when new functionality is added, thus checking that the new functionality has no introduced errors.

### Customer involvement

- The role of the customer in the testing process is to help develop acceptance tests for the stories that are to be implemented in the next release of the system.

- The customer who is part of the team writes tests as development proceeds.

  - All new code is therefore validated to ensure that it is what the customer needs.

- However, people adopting the customer role have limited time available and so cannot work full-time with the development team.
  - They may feel that providing the requirements was enough of a contribution and so may be reluctant to get involved in the testing process.

### Test automation

- Test automation means that tests are written as executable components before the task is implemented.

  - These testing components should be stand-alone, should simulate the submission of input to be tested and should check that the result meets the output specification.
  - An automated test framework (e.g. JUnit) is a system that makes it easy to write executable tests and submit a set of tests for execution.

- As testing is automated, there is always a set of tests that can be quickly and easily executed.
  - Whenever any functionality is added to the system, the tests can be run and problems that the new code has introduced can be caught immediately.

### Problems with test-first development

- Programmers prefer programming to testing and sometimes they take short cuts when writing tests.
  - For example, they may write incomplete tests that do not check for all possible exceptions that may occur.
- Some tests can be very difficult to write incrementally.
  - For example, in a complex user interface, it is often difficult to write unit tests for the code that implements the 'disply logic' and workflow between screens.
- It is difficult to judge the completeness of a set of tests.
  - Although you may have a lot of system tests, your test set may not provide complete coverage.

### Pair programming

- Pair programming involves programmers working in pairs, developing code together.

- This helps develop common ownership of code and spreads knowledge across the team.

- It serves as an informal review process as each line of code is looked at by more than one person.

- It encourage refactoring as the whole team can benefit from improving the system code.

* In pair programming, programmers sit together at the same computer to develop the software.

* Pairs are created dynamically so that all team members work with each other during the development process.

* This sharing of knowledge that happens during pair programming is very important as it reduces the overall risks to a project when team members leave.

* Pair programming is no necessarily inefficient and there is some evidence that suggests that a pair working together is more efficient than two programmers working separately.

## Agile project management

### Agile project management

- The principal responsibility of software project manager is to manage the project so that the softwre is delivered on time and within the planned budget for the project.

- The standard approach to project management is plan-driven.

  - Managers draw up a plan for the project showing what should be delivered, when it should be delivered and who will work on the development of the project deiverables.

- Agile project management requires a different approach, which is adapted to incremental development and the practices used in agile methods.

### Scrum

- Scrum is an agile method that focuses on managing iterative development rather than specific agile pratices.

- There are three phases in Scrum.
  - `The initial phase` is an outline planning phase where you establish the `general objectives` for the project and design the `software architecture`.
  - This is followed by `a series of sprint cycles`, where each cycle `develops an increment` of the system.
  - `The project closure phase` wraps up the project,
    - completes required documentation such as system help frames and user manuals, and assesses the lessons learned from the project.

### Scrum terminology

| Scrum team | Definition |
| Development team | A self-organizing group of software developers, which should be no more than 7 people. |
| Potentially shippable product increment | The software increment that is delivered from a sprint. THe idea is that this should be 'potentailly shippable' |
| Product backlog | This is a list of 'to do' items which the Scrum team must tackle. |
| Product owner | An individual (or possibly a small group) whose job is to identify product featufes or requirements, prioritize these for development and continuously review the product backlog. |
| Scrum | A daily meeting of the Scrum team that reviews progress and prioritizes work to be done that day. |
| ScrumMaster | The ScrumMaster is responsible for ensuring that the Scrum process is followed and guides the team in the effective use of Scrum. |
| Sprint | A development iteration. Sprints are usually 2~4 weeks long. |
| Velocity | An estimate of how much product backlog effort that a team can cover in a single sprint. |

### The Scrum sprint cycle

- Sprints are fixed length, normally 2~4 weeks.

- The starting point for planning is the product backlog, which is the list of work to be done on the project.

- The selection phase involves all of the project team who work with the customer to select the features and functionality from the product backlog to be developed during the sprint.

![11](/assets/images/2024-04-19/11.png)

### The Spring cycle

- Once these are agreed, the team organize themselves to develop the software.

- During this stage the team is isolated from the customer and the organization, with all communications channelled through the so-called 'Scrum master'.

- The role of the Scrum master is to protect the development team from external distractions.

- At the end of the sprint, the work done is reviewed and presented to stakeholders.
  - The next sprint cycle then begins.

### Teamwork in Scrum

- The 'Scrum Master' is a facilitator who arranges daily meetings, tracks the backlog of work to be done, records decisions, measures progress against the backlog and communicates with customers and management outside of the team.

- The whole team attends short daily meetings (Scrums) where all team members share information, describe their progress since the last meeting, problems that have arisen and what is planned for the following day.
  - This means that everyone on the team knows what is going on and, if problems arise, can re-plan short-term work to cope with them.

### Scrum benefits

- The product is broken down into a set of manageable and understandable chunks.

- Unstable requirements do not hold up progress.

- The whole team have visibility of everything and consequently team communication is improved.

- Customers see on-time delivery of increments and gain feedback on how the product works.

- Trust between customers and developers is established and a positive culture is created in which eveyone expects the project to succeed.

## Agile planning

### Agile planning

- Agile methods of software development are iterative approaches where the software is developed and delivered to customers in increments.

- Unlike plan-driven approaches, the functionality of these increments is not planned in advance but is decided during the development.

  - The decision on what to include in an increment depends on progress and on the customer's priorities.

- The customer's priorities and requirements change so it makes sense to have a flexible plan that can accommodate these changes.

### Agile planning stages

- `Release planning`, which looks ahead for several months and decides on the features that should be included in a release of a system.

- `Iteration planning`, which has a shorter term outlook, and focuses on planning the next increment of a system. This is typically 2-4 weeks of work for the team.

### Approaches to agile planning

- Planning in Scrum.

- Based on managing a project backlog (things to be done) with daily reviews of progress and problems.

- The planning game:
  - Developed originally as part of Extreme Programming (XP).
  - Dependent on user stories as a measure of progress in the project.

### Story-based planning

- The planning game is based on user stories that reflect the features that should be included in the system.

- The project team read and discuss the stories and rank them in order of the amount of time they think it will take to implement the story.

- Stories are assigned 'effort points' reflecting their size and difficulty of implementation.

- The number of effort points implemented per day is measured giving an estimate of the team's 'velocity'

- This allows the total effort required to implement the system to be estimated.

![12](/assets/images/2024-04-19/12.png)

### Release and iteration planning

- Release planning involves selecting and refining the stories that will reflect the features to be implemented in a release of a system and the order in which the stories should be implemented.

- Stories to be implemented in each iteration are chose, whith the number of stories reflecting the time to deliver an iteration (usually 2 or 3 weeks).

- The team's velocity is used to guide the choice of stories so that they can be delivered within an iteration.

### Task allocation

- During the task planning stage, the developers break down stories into developmen tasks.

  - A development task should take 4-16 hours.
  - All of the tasks that must be completed to implement all of the stories in that iteration are listed.
  - The individual developers then sign up for the specific tasks that they will implement.

- Benefits of this approach:
  - The whole team gets an overview of the tasks to be completed in an iteration.
  - Developers have a sense of ownership in these tasks and this is likely to motivate them to complete the task.

### Software delivery

- A software increment is always delivered at the end of each project iteration.

- If the features to be included in the increment cannot be completed in the time allowed, the scope of the work is reduced.

- The delivery schedule is never extended.

### Agile planning difficulties

- Agile planning is reliant on customer involvement and availability.

- This can be difficult to arrange, as customer representatives sometimes have to prioritize other work and are not available for the planning game.

- Furthermore, some customers may be more familiar with traditional project plans and may find it difficult to engage in an agile planning process.

### Agile planning applicability

- Agile planning works well with small, stable development teams that can get together and discuss the stories to be implemented.

- However, where teams are large and/or geographically distributed, or when team membership changes frequently,
  - It is practically impossible for everyone to be involved in the collaborative planning that is essential for agile project management.

### Agile Planning

- You already identified tasks for the to-do app.
- Based on the identified tasks, plan your development.
- Some tasks may require other tasks - from other user stories - are already done before they start.
  - Try to identify such cases and also plan the overall release and iterations.
- Is your task identification good enouth to assign them to team members?
- Which stories/tasks should be done first?
