# Software Engineering

# Week 02

#### 2024.04.19.(금)

## Project Management

### Software project management

- Concerned with activities involved in ensuring that

* Software is delivered on time and on schedule,
* and in accordance with the requirements of the organisations developing and procuring the software.

- Project management is needed because software development is always subject to budget and schedule constraints that are set by the organisation developing the software.

### Success criteria

- Deliver the software to the customer at the agreed time.

- Keep overall costs within budget.

- Deliver software that meets the customer's expectations.

- Maintain a coherent and well-functioning development team.

### Software management distinctions

- The product is intangible.

  - Software cannot be seen or touched.
  - Software project managers cannot see progress by simply looking at the artifact that is being constructed.

- Many software projects are 'one-off' projects.

  - Large software projects are usually different in some ways from previous projects.
  - Even managers who have lots of previous experience may find it difficult to anticipate problems.

- Software processes are variable and organization specific.
  - We still cannot reliably predict when a particular software process is likely to lead to development problems.

### Universal management activities

- People management

  - Project managers have to choose people for their team and establish ways of working that leads to effective team performance.

- Risk management

  - Project managers assess the risks that may affect a project, monitor these risks and take action when problems arise.

- Project planing

* Project managers are responsible for planning.
* Estimating and scheduling project development and assigning people to tasks.

## People Management

### Managing people

- > > People are an organisation's most important assets.

- The tasks of a manager are essetially people-oriented.

  - Unless there is some understanding of people, management will be unsuccessful.

- An important role of a manager is to motivate the people working on a project.

- Poor people management is an important contributor to project failure.

### Teamwork

- Most software engineering is a group activity.

  - The development schedule for most non-trivial software projects is such that they cannot be completed by one person working alone.

- A good group is cohesive and has a team spirit.

  - The people involved are motivated by the success of the group as well as by their own personal goals.

- Group interaction is a key determinant of group performance.

- Flexibility in group composition is limited.
  - Managers must do the best they can with available people.

### Group cohesiveness

- The advantages of a cohesive group are:
  - Group quality standards can be developed by the group members.
  - Team members learn from each other and get to know each other's work.
  - Inhibitions caused by ignorance are reduced.
  - Knowledge is shared.
  - Continuity can be maintained if a group member leaves.
  - Refactoring and continual improvement is encouraged.
  - Group members work collectively to deliver high quality results and fix problems, irrespective of the individuals who originally created the design or program.

### Selecting group members

- A manager or team leader's job is to create a cohesive group and organize their group so that they can work together effectively.

- This involves creating a group with the right balance of technical skills and personalities, and organizing that group so that the members work together effectively.

### Assembling a team

- May not be possible to appoint the ideal people to work on a project
  - Project budget may not allow for the use of highly-paid staff;
  - Staff with the appropriate experience may not be available;
  - An organisation may wish to develop employee skills on a software project
- Managers have to work within these constraints especially when there are shortages of trained staff.

### Group organization

- Small software engineering groups are usually organised informally without a rigid structure.

- For large projects, there may be a hierarchical structure where different groups are responsible for different sub-projects.

### Informal groups

- The group acts as a whole and comes to a consensus on decisions affecting the system.

- The group leader serves as the external interface of the group but does not allocate specific work items.

- Rather, work is discussed by the group as a whole and tasks are allocated according to ability and experience.

- This approach is successful for groups where all members are experienced and competent.

### Group communications

- Good communications are essential for effective group working.

- Information must be exchanged on the status of work, design decisions and changes to previous decisions.

- Good communications also strengthens group cohesion as it promotes understanding.

## Risk management

#### Risk management

- Risk management is concerned with identifying risks and drawing up plans to minimise their effect on a project.

- Software risk management is important because of the inherent uncertainties in software development, and these uncertainties stem from

  - loosely defined requirements,
  - requirements changes due to changes in customer needs,
  - difficulties in estimating the time and resources required for software development,
  - and differences in individual skills.

- You have to anticipate risks, understand the impact of these risks on the project, the product and the business, and take steps to avoid these risks.

### Risk classification

- There are two dimensions of risk classification
  - The type of risk (technical, organizational, ..)
  - What is affected by the risk:
- `Project risks` affect schedule or resources;
- `Product risks` affect the quality of performance of the software being developed;
- `Business risks` affect the organisation developing or procuring the software.

### Examples of project, product, and business risks

| Risk | Affects | Description |
| Staff turnover | Project | Experienced staff will leave the project before it is finished |
| Requirements change | Project and product | There will be a larger number of changes to the requirements than anticipated. |
| Size underestimate | Project and project | The size of the system has been underestimated. |
| Technology change | Business | The underlying technology on which the system is built is superseded by new technology. |
| Product competition | Business | A competitive product is marketed before the system is completed. |

### The risk management process

- Risk identification
  - Identify project, product and business risks;
- Risk analysis
  - Assess the likelihood and consequences of these risks;
- Risk planning
  - Draw up plans to avoid or minimise the effects of the risk;
- Risk monitoring
  - Monitor the risks throughout the project;

### Risk Identification and Risk Types

- There are various types of risk such as estimation, organizational, people, requirements, technology and tools.

- e.g.)
  - Underestimate required time to develop the software.
  - Organizational finance issues force the project budget reduction.
  - Key staff are ill, or it's impossible to recruit a required staff.
  - Customers fail to understand the impact of requirement changes.
  - Reusable software component has defects.

### Risk Analysis

- Analyze identified risks, and assess their probability and effects.

- Assess likelihood of a risk from low to high.

- Categorize effects of the risk based on their significance.

* Catastrophic, Serious, Tolerable, Insignificant.

- e.g.) Impossible to recruit a staff with required skills.

  - Probability is high: in case every company is hiring.
  - Effects are catastrophic: without this staff, we cannot proceed the project.

- Customers fail to understand the impact of requirement changes?

### Risk planning

- Consider each risk and develop a strategy to manage that risk.

- `Avoidance strategies`

* The probability that the risk will arise is reduced;
* e.g.) Schedule regular maintenance to avoid hardware failure.

- `Minimization strategies`

* The impact of the risk on the project or product will be reduced;
  - e.g.) Maximize information hiding to reduce change impact.

- `Contingency plans`
  - If the risk arises, contingency plans are plans to deal with that risk;
  - e.g.) When there is a fire, route traffics to a different data center.

### Risk monitoring

- Assess each identified risks regularly to decide whether or not it is becoming less or more probable.

- Also assess whether the effects of the risk have changed.

- Each key risk should be discussed at management progress meetings.

## Project Planning

### Project planning

- Project planning involves

* breaking down the work into parts and assign these to project team members,
* anticipate problems that might arise and prepare tentative solutions to those problems.

- The project plan, which is created at the start of a project, is used to communicate
  - how the work will be done to the project team and customers,
  - and to help assess progress on the project.

### Planning stages

- At the `proposal stage`, when you are bidding for a contract to develop or provide a software system.

- During the `project startup phase`, when you have to plan who will work on the project,

  - how the project will be broken down into increments, how resouces will be allocated across your company, etc.

- `Periodically throughout the project (development planning)` when you modify your plan in the light of experience gained and information from monitoring the progress of the work.

### Project scheduling

- Project scheduling is the process of deciding how the work in a project will be organized as separate tasks, and when and how these tasks will be executed.

- You estimate the calendar time needed to complete each task, the effort required and who will work on the tasks that have been identified.

- You also have to estimate the resources needed to complete each task, such as the disk space required on a server, the time required on specialized hardware, such as a simulator, and what the travel budget will be.

### The project scheduling process

![1](/assets/images/2024-04-19/1.png)

### Scheduling problems

- Estimating the difficulty of problems and hence the cost of developing a solution is hard.

- Productivity is not proportional to the number of people working on a task.

- Adding people to a late project makes it later because of communication overheads.

- The unexpected always happens. Always allow contingency in planning.

### Project activities

- Project activities(tasks) are the basic planning element. Each activity has:
  - a `duration` in calendar days or months,
  - an `effort estimate`, which shows the number of person-days or person-months to complete the work,
  - a `deadline` by which the activity should be complete,
  - a `defined end-point`, which might be a document, the holding of a review meeting, the successful execution of all tests, etc.

### Milestones and deliverables

- Milestones are points in the schedule against which you can assess progress.

  - e.g.) the handover of the system for testing.

- Deliverables are work products that are delivered to the customer.
  - e.g.) a requirements document for the system.

![2](/assets/images/2024-04-19/2.png)
