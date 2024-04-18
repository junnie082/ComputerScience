# Software Engineering

# Week 05

#### 2024.04.19.(금)

## Quality management

### Software quality management

- Concerned with ensuring that the required level of quality is achieved in a software product.

- Three principal concerns:
  - Organizational level
    - QM is concerned with establishing a framework of organizational processes and standards that will lead to high-quality software.
  - Project level
    - QM involves the application of specific quality processes and checking that these planned processes have been followed.
    - QM is also concerned with establishing a quality plan for a project.

### Quality management activities

- Quality management provides an independent check on the software development process.

- The quality management process checks the project deliverables to ensure that they are consistent with organizational standards and goals.

- The quality team should be independent from the development team so that they can take an objective view of the software.
  - This allows team to report on software quality without being influenced by software development issues.

## Software quality

- Quality means that a product should meet its specification.

- This is problematical for software systems

  - There is a tension between customer quality requirements (efficiency, reliability, etc.) and developer quality requirements (maintainability, reusability, etc);
  - Some quality requirements are difficult to specify in an unambiguous way;
  - Software specifications are usually incomplete and often inconsistent.

- The focus may be 'fitness for purpose' rather than 'specification conformance'.

### Non-functional characteristics

- The subjective quality of a software system is largely based on its non-functional characteristics.

- This reflects practical user experience.

  - If the software's functionality is not what is expected, then users will often just work around this and find other ways to do what they want to do.

- However, if the software is unreliable or too slow, then it is practically impossible for them to acheive their goals.

### Quality conficts

- It is not possible for any system to be optimized for all of the non-functional attributes.

  - For example, improving robustness may lead to loss of performance.
  - Security vs. Usability - Strong passwords, 2FA, etc.

- The quality plan should therefore define the most important quality attributes for the software that is being developed.

- The plan should also include
  - a definition of the quality assessment process,
  - an agreed way of assessing whether some quality, such as maintainability or robustness, is present in the product.

### Process and product quality

- The quality of a developed product is influenced by the quality of the production process.

- This is important in software development as some product quality attributes are hard to assess.

- However, there is a very complex and poorly understood relationship between software processes and product quality.
  - The application of individual skills and experience is particularly important in software development;
  - External factors may decrease product quality.
    - e.g.) accelerated development schedule.

### Quality culture

- Quality managers should aim to develop a `quality culture` where everyone responsible for software development is committed to achieving a high level of product quality.

- They should encourage teams to take responsibility for the quality of their work and to develop new approaches to quality improvement.

- They should support people who are interested in the intangible aspects of quality and encourage professional behavior in all team members.

## Reviews and inspections

### Reviews and inspections

- A group examines part or all of a process or system and its documentation to find potential problems.

- There are different types of review with different objectives:
  - Inspections for defect removal (product);
  - Reviews for progress assessment (product and process);
  - Quality reviews (product and standards).

### Program inspections

- These are peer reviews where engineers examine the source of a system with the aim of discovering anomalies and defects.

- Inspections do not require execution of a system so may be used before implementation.

- They may be applied to any representation of the system (requirements, design, configuration data, test data, etc).

- They have been shown to be an effective technique for discovering program errors.

### Inspection checklists

- Checklist of common errors should be used to drive the inspection.

- Error checklists are programming language dependent and reflect the characteristic errors that are likely to arise in the language.

- In general, the 'weaker' the type checking, the larger the checklist.

  - Example: Intialisation, Constant naming, loop termination, array bounds, etc.

- Static analysis tools automatically perform such inspection and provide a report for such checklist.
  - e.g.) FindBugs (Java). IDE errors/warnings, Lint/Linters, etc.

![13](/assets/images/2024-04-19/13.png)

### Quality management and agile development

- Quality management in agile development is informal rather than document-based.

- It relies on establishing a quality culture, where all team members feel responsible for software quality and take actions to ensure that quality is maintained.

- The agile community is fundamentally opposed to what it sees as the bureaucratic overheads of standards-based approaches and quality processes as embodied in ISO 9001.

### Shared good practice

- Check before check-in (commit)

  - Programmers are responsible for organizing their own code reviews with other team members before the code is checked in to the build system.

- Never break the build

  - Team members should not check in code that causes the system to fail.
  - Developers have to test their code changes against the whole system and be confident that these work as expected.

- Fix problems when you see them
  - If a programmer discovers problems or obscurities in code developed by someone else, they can fix these directly rather than referring them back to the original developer.

### Reviews and agile methods

- The review process in agile software development is usually informal.
  - Relies on quality culture rather than a specific formal process.
- In Scrum, there is a review meeting after each iteration of the software has been completed (a sprint review), where quality issues and problems may be discussed.

- In Extreme Programming, pair programming ensures that code is constantly being examined and reviewed by another team member.

## Configuration management

### Configuration management

- Software systems are constantly changing during development and use.

- `Configuration management (CM)` is concerned with the policies, processes and tools for managing changing software systems.

- You need CM because it is easy to lose track of what changes and component versions have been incorporated into each system version.

- CM is essential for team projects to control changes made by different developers.

### CM activities

- Version management

  - Keeping track of the multiple versions of system components and ensuring that changes made to components by different developers do not interfere with each other.

- System building

  - The process of assembling program components, data and libraries, then compiling these to create an executable system.

- Change management

  - Keeping track of requests for changes to the software from customers and developers, working out the costs and impact of changes, and deciding the changes should be implemented.

- Release management
  - Preparing software for external release and keeping track of the system versions that have been release for customer use.

### Agile development and CM

- Agile development, where components are systems are changed several times per day, is impossible without using CM tools.

- The definitive versions of components are held in a shared project repository and developers copy these into their own workspace.

- They make changes to the code then use system building tools to create a new system on their own computer for testing.
  - Once they are happy with the changes made, they return the modified components to the project repository.

### Multi-version systems

- For large systems, there is never just one 'working' version of a system.

- There are always several versions of the system at different stages of development.

- There may be several teams involved in the development of different system versions.

### Multi-version system development

![14](/assets/images/2024-04-19/14.png)

![15](/assets/images/2024-04-19/15.png)

## Version management

### Version management

- `Version management(VM)` is the process of keeping track of different versions of software components or configuration items and the system in which these components are used.

- It also involves ensuring that changes made by different developers to these versions do not interfere with each other.

- Therefore version management can be thought of as the process of managing codelines and baselines.

- Version management activities are supported by `Version Control System (VCS)` and IDE.
  - e.g.) Git, GitHub and their integration with IDE.

### Codelines and baselines

![16](/assets/images/2024-04-19/16.png)

### Version control systems

- `Version Control Systems (VCS)` identify, store and control access to the different versions of components. There are two types of modern version control system.
  - `Centralized systems`, where there is a single main repository that maintains all versions of the software components that are being developed.
    - Subversion is a widely used example of a centralized VCS.
  - `Distributed systems`, where multiple versions of the component repository exist at the same time.
    - Git is a widely-used example of a distributed VSC.

### Public repository and private workspaces

- To support independent development without interference, VSCs use the concept of a project repository and a private workspace.

  - In git, it is often divided by "remote" and "local" repositories.

- The project repository maintains the 'main' version of all components.

  - It is used to create baselines for system building.

- When modifying components, developers copy (check-out) these from the repository into their workspace and work on these copies.

- When they have finished their changes, the changed components are returned (checked-in) to the repository.

### Repository Check-in/Check-out

- Centralized Version Control
  ![17](/assets/images/2024-04-19/17.png)

### Distributed version control

- A 'main' repository is created on a server that maintains the code produced by the development team.

- Instead of checkout out the files that they need, a developer creates a clone of the project repository that is downloaded and installed on their computer.

- Developers work on the files required and maintain the new versions on their private repository on their own computer.

- When changes are done, they 'commit' these changes and update their private repository.
  - They may then 'push' these changes to the project repository.

### Repository cloning

- Distributed Version Control

![18](/assets/images/2024-04-19/18.png)

### Storage management

- When version control systems were first developed, storage management was one of their most important functions.

- Disk space was expensive and it was important to minimize the disk space used by the different copies of components.

- Instead of keeping a complete copy of each version, the system stores a list of differences (deltas) between one version and another.
  - By applying these to a main version (usually the most recent version), a target version can be recreated.

### Storage management in Git

- As disk storage is now relatively cheap, Git uses an alternative, faster approach.

- Git does not use deltas but applies a standard compression algorithm to stored files and their associated meta-information.

- It does not store duplicate copies of files.

  - Retrieving a file simply involves decompressing it, with no need apply a chain of operations.

- Git also uses the notin of packfiles where several smaller files are combined into an indexed single file.

## System building

### System building

- System building is the process of creating a complete, executable system by compiling and linking the system components, external libraries, configuration files, etc.

- System building tools and version management tools must communicate as the build process involves checking out component versions from the repository managed by the version management system.

- The configuration description used to identify a baseline is also used by the system building tool.

### Build platforms

- `The development system`, which includes development tools such as compilers, source code editors, etc.

  - Developers check out code from the version management system into a private workspace before making changes to the system.

- `The build server`, which is used to build definitive, executable versions of the system.

  - Developers check-in code to the version management system before it is built.
  - The system build may rely on external libraries that are not included in the version management system.

- `The target environment`, which is the platform on which the system executes.
  - For real-time and embedded systems, the target environment is often smaller and simpler than the development environment (e.g. a cell phone).

### Development, build, and target platforms

![19](/assets/images/2024-04-19/19.png)

### Continuous Integration

- Agile Building

![20](/assets/images/2024-04-19/20.png)

### Agile building

- Check out the mainline system from the version management system into the developer's private workspace.

- Build the system and run automated tests to ensure that the built system passes all tests.

  - If not, the build is broken and you should inform whoever checked in the last baseline system. They are responsible for repairing the problem.

- Make the changes to the system components.

- Build the system in the private workspace and re-run system tests.

  - If the tests fail, continue editing.
  - This can be supported by build system such as Maven, Gradle.

- Once the system has passed its tests, check it into the build system but do not commit it as a new system baseline.

- Build the ssytem on the build server and run the tests.

  - You need to do this in case others have modified components since you checked out the system.
  - If this is the case, check out the components that have failed and edit these so that tests pass on your private workspace.

- If the system passes its tests on the build system, then commit the changes you have made as a new baseline in the system mainline.

- Nowadays, Continuous Integration and Delivery (CI/CD) is getting more and more important to evolve software faster.

- CI/CD activities can be supported by various tools.
  - e.g.) Jenkins (Java), Docker, GitHub Actions, GitLab, etc.

### Pros and cons of continuous integration

- Pros

  - The advantages of continuous integration is that it allows problems caused by the interactions between different developers to be discovered and repaired as soon as possible.
  - The most recent system in the mainline is the definitive working system.

- Cons
  - If the system is very large, it may take a long time to build and test, especially if integration with other application systems is involved.
    - Modularization is crucial to avoid this issue - e.g.) micro services.
  - If the development platform is different from the target platform, it may not be possible to run system tests in the developer's private workspace.
    - Container & Cloud instances.

## Release management

### Release management

- A system release is a versioin of a software system that is distributed to customers.

- For mass market software, it is usually possible to identify two types of release:
  - major releases which deliver significant new functioinality,
  - minor releases, which repair bugs and fix customer problems that have been reported.
  - Usually this is denoted by version numbering convention such as <major>, <minor>, <patch>
    - e.g.) 1.4.2 - the first major version with four minor changes including two patches.

### Release components

- As well as the executable code of the system, a release may also include:

  - configuration files defining how the release should be configured for particular installations;

  - data files, such as files of error messages, that are needed for successful system operation;

  - an installation program that is used to help install the system on target hardware;

  - electronic and paper documentation describing the system;

  - packaging and associated publicity that have been designed for that release.

### Online Delivery

- Recent advance in software eco-system change the delivery of releases significantly.

- Now, releases are mostly download and updated via so-called 'app market'.

- For stand-alone software which is distributed by its own channel, automatic delivery of releases is quite common.

  - e.g.) Game clients, App Stores, OS, etc.

- In terms of development process, tool supports in building system make release creation much easier.
  - You can build and 'package' necessary materials based on configuration.

### Software as a service

- Delivering software as a service (SaaS) reduces the problems of release management.

- It simplifies both release management and system installation for customers.

- The software developer is responsible for replacing the existing release of a system with a new release and this is made available to all customers at the same time.

- However, providing reliable software services is another concern.
