**Incident management** is the practice of responding to an unplanned event, user disruption, or service interruption and involves restoring the service to an operational or acceptable state. There are 2 general labels for incidents:

- **Incident:** An unplanned interruption to a service or reduction in the service quality.
- **Major incident:** An incident with significant business impact, requiring an immediate coordinated resolution.

A **problem** or **issue** is a not-yet-known root cause behind one or more incidents.

## Purpose

In order to establish trust in our software or the delivery of our software, we need to be timely and transparent with how we manage incidents. Good incident management involves not only high reactivity to difficulties, but proper follow-ups to notify relevant parties of the severity of the issue, how we manage the problem, and ETA and steps to resolution of the issue.

## Incident management

The overall procedure for monitoring alerts and taking action includes determining an incident's possible impact and severity and identifying it as an issue and giving it a priority. If a problem condition cannot be found, we will reject it as a non-impacting event.

### Reporting the problem

In crisis communication, the best immediate action to take for critical problems is to do frequent reporting to the team and related stakeholders. This is to make sure everyone that needs to know, especially the business and customer, understand what is happening and to avoid issues in transparency when exploring the problem.

Once we have an idea of what the problem is, we need to report this incident in a way that the information is self-contained and sufficient. The following data must be collected before an incident is fully classified and prioritized:

- Submitter Source (monitoring alert or alternate source)
- Customer(s) (if applicable)
- System or application (and hostname, if applicable)
- Time of alert
- Scope of impact: estimated number of systems, users, or regions impacted
- Type of impact: general scope of service impairment (e.g., loss of all access, degraded performance, dependent applications impacted, observed customer impact)

### Classifying the problem

After acknowledging the alert, we should triage the problem by assigning it a category and priority level. Jira tickets and templates have common classifiers for high priority problems. How we assign the priority labels are up to use, but in general, the following levels are categorized as such:

#### Priority Level

- **P0**: *This priority level is critical and should have the most immediate response action possible, with ideally a target resolution time of within 1 hour.*
	- complete loss of access to application or API
	- degraded access to or performance
	- loss of access to a data center
- **P1**: *A high priority level that should have minimal response time with ideally a target resolution time of 4 hours.*
	- outage to important outbound third-party interface
	- corruption or loss of data
	- loss of an important function of an application
- **P2**: *There should be some effort in resolving these issues, but response and action can be more relaxed, ideally with a target resolution time of within 24 hours.*
	- irregular or localized performance issue
	- system issues with no noticeable client impact yet
	- single client outage/degradation
- **P3**: *This priority not need any immediate action and can be resolved in batch with other issues, ideally with a target resolution time of within 1 week.*
	- operational issues, procedural problems or service requests that have little or no effect on end-users
	- the default priority level for issues with undetermined severity level

## Postmortems

Documentation on the resolution and aftermath of an incident is key to distilling issues and establish practices to avoid further incidents through reflection:

- After an incident is resolved, the team should gather to identify the root cause of the incident.
- These postmortems are an opportunity for learning and growth, to help avoid it from happening again in the future, designed in a way to be blameless.

Since we often use Jira and Confluence for managing projects, we often use templates available on their platform. The following [template](https://www.atlassian.com/incident-management/postmortem/templates) format is taken from Atlassian (make sure to check their examples as well):

### Incident Summary
A general summary of the incident in a few sentences or paragraph; includes what happened, incident severity, and how long the impact lasted.

#### TEMPLATE
- time of incident and date
- number of users encountered the problem

### Leadup
A description of the sequence of events that led to the incident

#### TEMPLATE
- time before the incident
- related product or service
- introduced change that led to the incident
- description of the impact of the change

### Fault
Describe how a change was implemented that didn't work as expected; if possible, add screenshots or visuals to help illustrate.

### Impact
Describe who was impacted and how severe it was.

#### TEMPLATE
- time of incident
- who was impacted
- severity of impact

### Detection
Describe when the incident was detected. The purpose of this is to find how to reduce our time-to-detection.

#### TEMPLATE
- how was the incident detected
- who was asked to follow-up
- describe the improvement to be used

### Response
Describe who responded to the incident and what actions they took. Make sure to note any delays or obstacles to responding.

#### TEMPLATE
- detail the first response after alert
- detail any follow-ups and delays

### Recovery
Describe how the service was restored and detail the steps to recovery.

#### TEMPLATE 
- the action that mitigated the issue
- why the action was taken
- the outcome of the action

### Timeline
Detail the incident timeline with as much information as possible. Use UTC to standardize for timezones:

#### **TEMPLATE:**
- XX:XX UTC - INCIDENT ACTIVITY; ACTION TAKEN
- XX:XX UTC - INCIDENT ACTIVITY; ACTION TAKEN
- XX:XX UTC - INCIDENT ACTIVITY; ACTION TAKEN

### Root cause identification
The simplest way to interrogate a problem is using the 5 Whys technique. [5 Whys](https://en.wikipedia.org/wiki/5_Whys) is an iterative interrogative technique developed by Sakichi Toyoda and is used to explore the cause-and-effect relationships underlying a problem. For example:

- **Problem**: Our users have an issue viewing updates in tracking orders for their delivery.
- **1st Why**: There is an issue with our view model.
- **2nd Why**: Our view model has issues getting data from our closest database.
- **3rd Why**: Our nearest database has connection issues egressing data.
- **4th Why**: Our nearest database cannot resolve DNS properly to point and egress data.
- **5th Why**: Our database has had a config change that affected the resolution of DNS queries.


### Root cause
Note and detail the final root cause of the incident.

### Backlog check
Review your backlog to find out if there were any unplanned tasks that could have prevented this incident.

### Recurrence
Look back at any old incidents to see if they have if they have the same root cause. If it does, detail what mitigation was attempted and why the incident occured again.

### Lessons learned
Discuss the incident response's positive aspects, its shortcomings, and its potential for improvement.

### Corrective Actions
Descrive actions to stop this kind of situation from happening again. Make sure to note who is responsible and what task will need to be completed in what time span.
