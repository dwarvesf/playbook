---
title: Collaboration guidelines
description: Guidelines for project collaboration between Dwarves and our Clients.
date: 2020-01-01
authors:
  - huynguyenh
  - tieubao
tags:
  - consulting
  - operation
  - project
redirect:
  - /s/559tdA
---

Guidelines for project collaboration between Dwarves and our Clients.

## Tools

- Project management & communication: Basecamp (invited as Client)
- Sprint planning & tracking: Jira/Basecamp
- Meeting: Google Meet
- Source control platform: Github/Gitlab (self-hosted)
- Design: Figma/Sketch

## Schedule

- Project kick-off meeting: **1 session**
- Sprint length: **1 week**/**2 week**
- Sprint planning meeting: **1 per sprint**
- Sprint review & retrospective meeting: **1 per sprint**
- Sprint **daily standup** meetings (can as well be done via a communication channel or Basecamp's check-ins)
- Project/team feedback meeting (with Account Manager): **1 per week** or **1 every 2 weeks**

Meeting notes for Sprint planning and Sprint retrospective will be sent within 30 minutes after the meeting.

## Workflow

### Project management

- All team members must be involved in Sprint planning
- Milestones and features (epics) should be put on Jira/Basecamp during the Project kick-off phase
- New features/change requests will be put into Icebox/Backlog, estimated, and planned for future Sprints
- Bugs can be added to the current Sprint depends on priority, complexity, and available points of the Sprint
- The intention of every Sprint is “Potentially Shippable” Software, things can go wrong and features might get pushed to the next Sprint
- Every 2 weeks, the Team Lead, Product Manager/Project Manager, and Account Manager will have a quick 15 minutes meeting to review the work progress and resolve any conflicts (if any)

### Design <> development

- The design team should at least provide:
  - Color palette (all that are used throughout the UI)
  - Heading / Font size should be defined by scale (similar to [Tailwind's](https://tailwindcss.com/docs/font-size/#usage))
  - Base components (.eg headings, button variants, and states,...)
- A new design version is expected to be available and reviewed **by the development team** before the Sprint started

### Backend <> frontend

- Pre-requisite:
  - API versioning & documentation (.eg Swagger)
  - Dedicated environments for Development / Staging / Production
- Backend should enable CORS on either API gateway or at the application level
- Both sides should agree on the same glossary/naming conventions / data schema, preferably within the first Sprint

### Quality assurance <> development

- Bugs/issues raised should include:
  - Steps to reproduce, behaviorally described from application's entry to bug encounter
  - Affected platform, version, feature, language (if applicable)
  - Severity level
- For Frontend (web), each feature's Pull Request will have a dedicated environment for testing. Use it to run feature tests before it got merged

## Release

- We release the end of each Sprint (on Sprint wrap-up day - the day before the Retrospective meeting)
- Hotfix releases are ad-hoc
- What's included in a release:
  - Semantic version tagging
  - release notes, changelogs, known issues
  - release's artifacts
- Backend and Frontend (or each micro-service in the system) will be released individually based on the Semantic version

## Customer feedback

We received and response to feedback via direct message, email, or video calls. Specific feedback should be directed to specific PIC in a project team:

- Implementation/Development: Team Lead, Project Lead
- Sprint result: Project Lead, Account Manager
- Project progress: Project Lead, Account Manager
- Pricing/Billing: Account Manager
- Communication issues: Project Lead

## Issues management

We promise to respond and resolve in a timely fashion when problems arise, depends on priority & severity.

After issues were resolved, we will conduct an issue investigation and provide preventive measures (if applicable).

There are **4 levels** of severity:

### Critical: 4

The production system is down or a major function is unusable and there is no acceptable alternative method to achieve the required results. Support in such emergency issues is available via our hotline.

- Contact channel: hotline
- Response time: within 10 minutes
- Promised resolve time: less than 30 minutes

### Major: 3

Major features of the product are failed and/or performance issues impacting the normal functioning of multiple users.

- Contact channel: email, primarily communication channel
- Response time: within 30 minutes
- Promised resolve time: from 2 to 5 hours

### Moderate: 2

Moderate loss of application functionality or performance degradation. The system is still operating but doesn't meet promised standard operation.

- Contact channel: primary communication channel
- Response time: within 45 minutes
- Promised resolve time: within 7 hours

### Minor: 1

Minor issues such as visual incorrectness .eg color or font size, product feature requests, and how-to questions.

- Contact channel: primary communication channel
- Response time: within 1 business day
- Promised resolve time: within 1 business day

Subject to the above limitations, we promise to respond to support requests within twenty-four (24) hours.

## Billing

Through every month, our Accountant and Account Manager will compose and send invoices to our Clients during the **23rd-25th**. The total invoice will base on the deployed resources, whether the project type is Fixed-price or Time & Materials (T&M). This method will enable a constant monthly cost, allows the Accountant from both parties to control the project costs & budgets more efficiently, as well as helping the Project Manager to foresee and eliminate any arising miscellaneous fees.

Our rates are always recorded in **NET** amount, excluded from all types of taxes, bank fees, and account-related fees.

Our invoice will contain the necessary information for the Client to proceed with the internal approval and wiring requirements:

- Bank information
- Invoice description
- Invoice details

The Client will have **10** **(Ten) business days** to complete these invoices. If the Client needs to extend the payment deadline, the Client is required to submit a written request to Dwarves Foundation within *72 hours* after the invoice date. A new deadline then will be decided and agreed by both parties, before it can be implemented officially.

Our Accountant and Account Manager will send out reminders to the Clients who may have forgotten about the outstanding invoices. If any Client failed to complete the payment on time and failed to submit a request for an extended payment deadline to Dwarves Foundation, we will add a *10% interest rate/month* to the outstanding invoice(s).

## Project responsibility scope

To avoid us as a project team from stepping on each other's foot and to ensure a healthy relationship for the team and Client.

**Team lead**

- Main PIC of the offshore development team
- Define technical stacks (together with the Client's team, if applicable)
- Main PIC of project's infrastructure (if applicable, there are cases where infrastructure is managed/provided by Client's team)
- Daily/bi-weekly/weekly progress sync-up and report to Client
- Receive, discuss and make plans for project milestones
- Run the development Sprint with offshore team and Client

**Account Manager**

- Receive Client's direct feedback
- Make improvement plans for the offshore team from provided feedback
- Maintaining the relationship between both sides
- Ensuring development time, workforce and workload maps accurately to the Client's budget

**Project Manager?**

- Designing and applying appropriate project management standards
- Managing the production of the required deliverables, and project administration
- Adopting any delegation and use of project assurance roles within agreed reporting structures
- Managing project risks, including the development of contingency plans.
  liaison with program management (if the project is part of a program) and related projects to ensure that work is neither overlooked nor duplicated.
- Monitoring overall progress and use of resources, initiating corrective action where necessary
  applying change control and configuration management processes.
- Reporting through agreed lines on project progress through highlight reports and end-stage assessments.
- Liaison with appointed project assurance representatives to assure the overall direction and integrity of the project.
- Maintaining an awareness of potential interdependencies with other projects and their impact
  adopting and applying appropriate technical and quality strategies and standards
- Identifying and obtaining support and advice required for the management, planning and control of the project.
- Conducting a project evaluation review to assess how well the project was managed
  preparing any follow-on action recommendations
