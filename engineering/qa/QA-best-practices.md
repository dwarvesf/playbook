---
title: null
description: null
date: null
redirect:
  - /s/-Lyd4A
---

# QA best practises

This is a summary of best practices our QA engineers at Dwarves use and recommends to be used. It is not supposed to be a detailed description and sometimes cannot fully be used for all tasks but as an overview of the most important QA processes and a list of good practices that should be used.

## QA practices

Everyone in Dwarves has QA responsibilities, even if there is a named QA manager or QA specialists. To us, QA means three things.

1. The service runs as expected and is considered to be created with good practices.
2. The service can be easily and cost efficiently maintained and operated.
3. The service can be continuously improved and modified cost efficiently.

On high level QA work and practices can be divided into two processes.

1. QA during agile sprints
2. QA during deployment process

Furthermore there are other QA actions.

#### Test-driven development

We use TDD and ATDD (Acceptance Test Driven Development) methods when applicable.

Method forces the implementation to follow architecture and consider modules that are used. Implementation starts by writing the automated test first and then implementing the functionality to pass the test.

#### Pair programming

We use Pair programming method when applicable. This is a very convenient way to share knowledge and experience about the project and software under development.

#### Code review

Reviewing the code helps other team members to get information on certain functionality and gives a possibility to give feedback to the responsible person and also ensures knowledge sharing between team members.

#### Manual functional testing

Manual testing is mostly done using Exploratory testing methodology and found errors are either fixed right away or prioritised and recorded to task/story/error management tool. Exploring the app or service can be started right after something functional is “ready”.

Exploratory testing is a very powerful tool in e2e testing where the whole system is covered by testing. In the method tester goes beyond what can be defined in a test case, applies user-like thinking as well as tries to break the system by various error scenarios and is never “done” with testing.

#### Error management

Issues found are recorded to a specific tool or board with a information like priority, environment (software and device information), steps to reproduce, expected result, time and date and a screenshot.
Tool like BaseCamp actively used also for error tracking.

## QA in Sprints (Definition of Done)

One of the principles of agile is that the master code branch should always be potentially shippable. That means it should be always production quality. This is achieved by the following means:

1. Each user story (or feature) is developed individually in their own feature branch. The purpose of this is to ensure that each update to master branch is at the same time 1) as small as possible and 2) potentially shippable, complete story.

2. Before the feature branch can be merged to the master branch, it must pass a list of actions, requirements and practices called Definition of Done (DoD). This is defined together with the customer PO and the development team and can be modified during project should project needs change.

### DoD example for a project

- Manual regression test case (usually UI test cases) have been updated for the smoke / sanity checks (Test manager)
- Automated unit test cases have been written (Developer)
- Feature development is completed. Acceptance criteria of the story have been fulfilled (Developer)
- Code review (both feature and test cases) is done by another developer (developer)
- Functional test cases have been passed in Demo environment (Automated & manual)
- The user story (feature) has been approved (Product Owner) [1]
- Integration tests done against stub/mockups, e2e when possible [2]
- Concept documentation updated (Product Owner) [3]
- All other documentation has been updated (Led by project manager)
- All documentation changes have been reviewed and approved (customer / PO) [4]
- Feature branch merged successfully to Master branch (Developer)
- Automated tests passed in Test environment (Updated and run automatically by CI)
- Functional tests passed in test environment / Exploratory tests have been done (Led by test manager)

Following items in proposed DoD have been bolded due

1. Acceptance of the user story here means that the PO is happy with the design, UX and UI of a user story. In practice this means that once the user story is approved it can still have QA issues (performance, bugs etc). It should be noted that new POs, especially in an organization where agile methods have not been used before, have often found this responsibility quite heavy and challenging. Traditionally similar approvals are made during project steering groups.
2. Usually it’s very difficult to run these in Demo environment due to security policies / high overhead of maintaining this. TEST environment should be integrated with appropriate test environment
3. Similar to the first item. In traditional project methods this is usually done by Vendor’s PM after approvals have been made during project steering group. In agile concept documentation is normally not maintained at all. However as the concept documentation is most likely an acceptance criteria of this project it needs to be maintained to showcase the changes that we have made during the project.
4. Approval of the documentation should be part of DoD especially initially. Each organization has their own documentation practices and the purpose of this is to ensure that we start doing it “right” from the very beginning.
