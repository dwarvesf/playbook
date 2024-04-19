---
tags:
  - business
  - guidline
  - project
title: Collaboration Guideline
date: 2023-10-16
description: guidelines for project collaboraton DF and Client
authors:
  - Han 🐸
menu: memo
type: playbook
---


Guidelines for project collaboration between DF and Clients.

## Tools
* Project management & communication: Basecamp (invited as Client)
* Sprint planning & tracking: Jira/Basecamp
* Meeting: Google Meet
* Source control platform: Github/Gitlab (self-hosted)
* Design: Figma/Sketch

## Schedule
* Project kick-off meeting: **1 session**
* Sprint length: **1 week/2 weeks**
* Sprint planning meeting: **1 per sprint**
* Sprint review & retrospective meeting: **1 per sprint**
* Sprint **daily standup** meetings (can as well be done via communication channel or Basecamp’s check-ins)
* Project/team feedback meeting (with Account Manager): **1 per week** or **1 every 2 weeks**

Meeting notes for Sprint planning and Sprint retrospective will be sent within 30 minutes after meeting.

## Workflow
### Project Management
* All team members must be involved in Sprint planning
* Milestones and features (epics) should be put on Jira/Basecamp during Project kick-off phase
* New features/change requests will be put into Icebox/Backlog, estimated and planned for future Sprints
* Bugs can be added to the current Sprint depends on priority, complexity and available points of the Sprint
* The intention of every Sprint is “Potentially Shippable” Software, things can go wrong and features might get pushed to the next Sprint
* Every 2 weeks, the Team Lead, Product Manager/Project Manager and Account Manager will have a quick 15 minutes meeting to review the work progress and resolve any conflicts (if any)

### Design <> Development
* Design team should at least provide:
- Color palette (all that are used throughout the UI)
- Heading / Font size should be defined by scale
- Base components (.eg headings, button variants and states,…)
* New design version is expected to be available and reviewed **by development team** before the Sprint started

### Backend <> Frontend
* Prerequisite:
- API versioning & documentation (.eg Swagger)
- Dedicated environments for Development / Staging / Production
* Backend should enable CORS on either API gateway or at application level
* Both sides should agree on the same glossary / naming conventions / data schema, preferably within the first Sprint

### Quality Assurance <> Development
* Bugs/issues raised should include:
- Steps to reproduce, behaviorally described from - application’s entry to bug encounter
- Affected platform, version, feature, language (if applicable)
- Severity level
* For Frontend (web), each feature’s Pull Request will have a dedicated environment for testing. Use it to run feature tests before it got merged

## Release
* We release the end of each Sprint (on Sprint wrap-up day - the day before the Retrospective meeting)
* Hotfix releases are ad-hoc
* What’s included in a release:
- Semantic version tagging
- Release notes, changelogs, known issues
- Release’s artifacts
* Backend and Frontend (or each microservice in the system) will be released individually based on Semantic version

## Customer feedback
We received and responded to feedback via direct message, email or video calls. Specific feedback should be directed to specific PIC in a project team:

* Implementation/Development: Team Lead, Project Lead
* Sprint result: Project Lead, Account Manager
* Project progress: Project Lead, Account Manager
* Pricing/Billing: Account Manager
* Communication issues: Project Lead

## Issues management
We promise to respond and resolve in a timely fashion when problems arise, depending on priority & severity.
After issues are resolved, we will conduct an investigation and provide preventive measures (if applicable).

There are **4 levels** of severity:

### Critical: 4
Production system is down or a major function is unusable and there is no acceptable alternative method to achieve the required results. Support in such emergency issues is available via our hotline.

* Contact channel: hotline
* Response time: within 10 minutes
* Promised resolve time: less than 30 minutes

### Major: 3
Major features of the product are failed and/or performance issues impacting the normal functioning of multiple users.

* Contact channel: email, primary communicate channel
* Response time: within 30 minutes
* Promised resolve time: from 2 to 5 hours

### Moderate: 2
Moderate loss of application functionality or performance degradation. The system is still operating but doesn’t meet promised standard operation.

* Contact channel: primary communicate channel
* Response time: within 45 minutes
* Promised resolve time: within 7 hours

### Minor: 1
Minor issues such as visual incorrectness .eg color or font size, product feature requests and how-to questions.

* Contact channel: primary communicate channel
* Response time: within 1 business day
* Promised resolve time: within 1 business day

Subject to the above limitations, we promise to respond to support requests within twenty-four (24) hours.