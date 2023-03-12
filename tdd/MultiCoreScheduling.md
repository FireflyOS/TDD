# Title

Authors: @V01D-NULL

Date: March 12 2023

### Introduction
The introduction of an SMP aware scheduler will allow for faster and concurrent execution of both kernel and user tasks. This scheduler will NOT focus on the algorithms, but rather the concurrency challenges that come with an SMP system.

### Terminology
MP = Multi Processing

SMP = Symmetric Multi Processing

#### GOALS
- Scheduling algorithms can be changed during compile time with little to no changes to the code.
- Spend as little time waiting for work as possible.
- Use as few spinlocks as possible

#### NON GOALS
- Sophisticated scheduling algorithms. We will purely focus on the smp aspect of it.
- Adding support for a single core scheduler.
- Anything related to userspace or ring level transitions.

### Requirements
- List the functional and non-functional requirements for the project, including any constraints or assumptions that were made during the planning process.

### Architecture
At it's core, this Symmetric Multi Processing scheduler will:
- Enforce a soft affinity policy
- Balance workloads using the pull migration strategy

TODO: ...further details unknown at the moment...

- Describe the overall architecture of the system, including the main components and how they interact with each other.
- Provide diagrams or other visual aids, if necessary, to help clarify the architecture.

## Phase 1:
  TODO: ...scheduling tasks on all cores (with their own local data and what not)
  ### Design
   - Detail the design of each component of the system, including any algorithms, data structures, or other implementation details.
   - Provide pseudocode or code snippets, if appropriate, to illustrate the design.
  
  ### Data Model
   - Describe the data model, including any entities, attributes, and relationships.
   - Provide a diagram of the data model, if appropriate.

  ### Breaking changes
   - If this change is not backwards compatibile, detail a strategy to migrate the previous implementation.

## Phase 2:
  TODO: ....begin talking to other cores to initiate load balancing
  > Repeat design, data model and breaking changes for each phase
 
<hr/>

### User Interface Design
- Provide a detailed description of the user interface, including any wireframes or mockups.
- Describe any user interactions or navigation flows.

### Security
- Detail any security considerations and how they will be addressed in the design.

### Deployment
- Describe how the system will be deployed, including any infrastructure, software, or other dependencies.
- Provide a high-level overview of the deployment process.

### Testing
- Outline the testing strategy, including any unit tests, integration tests, or other types of testing that will be performed.
- Describe how testing will be automated and integrated into the development process.

### Conclusion
- Summarize the main points of the document and provide any additional information that is relevant to the project.
