---
title: null
description: null
date: null
---

# Project Setup

Following setup helps us maintain stability and increase the transparency among team members.

### Agreement

We use GDrive for file storage as specified in our Handbook. As the initiator, you will need to create a subfolder with the project name in the `Sales > Agreement` folder.

NDA, agreements and all kinds of paperwork between parties will be put there. You can ask the Ops or Business team for help if you don't have the access.

### Development & Design artifacts

- For diagrams and design assets, you can find or put them in the `Works` folder.
- Remember to create a group email (e.g. <vault@dwarves.ltd>) in GSuite and add all related members to the project.

### Repository

The codebase is usually in Github or our Gitlab. We have a specific guide for repo setup at [repository-setup.md](setup-repository.md).

A few clients who have an in-house tech team might prefer using their git system. In those cases, we need to set up a symlink to pull out and daily back up the source code to our Gitlab.

### Communication Channel

The communication channel is transparent for clients, the development team and our team.

- If clients have their own communication tool, we ask them to invite <team@d.foundation> and project members (including Account Manager) to join the channel.
- If clients have their own Slack, we would create a shared channel between the 2 workplaces for business communication purpose.
- None of above, we mainly use Basecamp as our main communication channel.

### Workflow

The project workflow includes

- How we **communicate** internal and with the client
- How we **schedule** the meetings and milestone delivery

We use Basecamp for daily activities and milestones management. After having the milestone breakdown, we create a project in Basecamp and put them into it.

Then we fire an email to the stakeholders, including the development team with the summary, workflow explained and linked to all the tools.

1. Invitation to setup [Communication Channel](#communication-channel)
2. Setup project in Basecamp (e.g Todos, Schedule, Message Board)
3. Schedule meetings (e.g planning, retrospective, ...) with Google Meet link included in Basecamp
4. Schedule client meetings using tools to which clients and the team can access (e.g Google Calendar)
5. Setup Repository
6. Link to GDrive shared folder
7. Make sure the project is added to Fortress

### Integration

The integrations are not required to setup. Nonetheless, in some cases, we also want to add integrations from the services we use to our monitoring platform or [communication channel](#communication-channel)

For example, we are working with #dental-marketplace team, there should be integration from Gitlab, and Fabric, and maybe Trello.

### Release

There's a few checklist item to follow at [release.md](release.md). In short

- Release should contain a changelog.
- Things are well-tested and known issues are specified.
- Having people in charge of product quality sign-off the release.
- Be careful with the deployment step, migration, and back-up.

