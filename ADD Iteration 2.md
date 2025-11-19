# Iteration 1
## Step 1: Review Inputs
(Not needed for further Iterations).

## Step 2: Establish Iteration Goal by Selecting Drivers
The goal of this iteration is to identify structures to support primary functionality by decomposing the high-level layers from Iteration 1 into more concrete modules and components. This will help in:
- Understanding how core functionalities are implemented across layers
- Addressing QA-5 (Modifiability) by defining clear module boundaries and interfaces
- Supporting team allocation and task distribution

The selected drivers for this iteration are:
- UC-3 (Check Next Exam Date) – Represents the core student query functionality
- UC-4 (View Personalized Dashboard) – Requires integration of multiple data sources and personalization logic
- UC-5 (Proactive Assignment Notification) – Involves proactive data retrieval and notification
- QA-5 (Modifiability) – To ensure the system can evolve with new data sources and AI models

## Step 3: Choose One or More Elements of the System to Refine
The elements selected for refinement in this iteration are the Presentation Layer, the Application/Business Layer, and the Integration Layer. These layers are central to supporting the selected primary use cases because they directly encapsulate the components responsible for user interaction, conversational logic, and data retrieval from external university systems.

## Step 4: Choose One or More Design Concepts That Satisfy the Selected Drivers
| Design Decisions and Location | Rationale |
| ----------- | ----------- |
| Create a Domain Model for the AIDAP system | Before decomposing functionality, it is necessary to define key entities (e.g., User, Course, Exam, Assignment) and their relationships. This ensures a shared understanding of the problem space and supports consistent module design |
| Identify Domain Objects that map to functional requirements | Each major function should be encapsulated within a domain object. This supports modifiability (QA-5) and clear responsibility assignment |
| Decompose Domain Objects into layered components | This follows the Layered pattern and supports separation of concerns. It allows teams to work on different parts of the system independently |
| Use Spring Boot and Hibernate for business and data layers | Spring Boot supports rapid development and dependency injection, which aids testing and modifiability. Hibernate provides ORM capabilities and integrates well with Spring. The team is familiar with these technologies |


## Step 5: Instantatiate Architectural Elements, Allocate Repsonsibilites, & Define Interfaces
| Design Decisions and Location | Rationale |
| ----------- | ----------- |


## Step 6: Sketch Views and Record Design Decisions


## Step 7: Perform Analysis of Current Design and Review Iteration Goal and Achievement of Design Purpose

| Not Addressed | Partially Addressed | Comepletely Addressed | Explanation |
|:---------------:|:---------------------:|:-----------------------:|-------------|

