# Defect Report template

### Summary

A summary of the bug should be no more than 60 words. Make sure the summary reflects the problem and where it is.

**What - Where - When (Action)**

### Description:

A detailed description of the bug.

Use the following fields for the description field:

**Reproduction steps:**

Clearly mention the steps to reproduce the bug. For example:

1. Go to '...'
2. Click on '....'
3. Scroll down to '....'
4. Observed result.

**Expected result:** How the application should behave during the above steps.

**Actual result:** What is the actual result when running the above steps (the bug behavior).

**Screenshots**

If applicable, add screenshots or screen recordings to explain your problem.

### Platform/Environment

You may need to write specifications such as the version of the project, the operating system, or the hardware if applicable.

**Desktop (please complete the following information):**

- OS: [e.g. iOS]
- Browser [e.g. chrome, safari]
- Version [e.g. 22]

**Smartphone (please complete the following information):**

- Device: [e.g. iPhone6]
- OS: [e.g. iOS8.1]
- Browser [e.g. stock browser, safari]
- Version [e.g. 22]

### Logs / Stack trace

```
Insert your log/stack trace here
```

Besides that, we also need to add more about Priority and Severity:

**Priority:** When should a bug be fixed? Priority is generally set Low/Medium/High types.
**Priority Types:**

- Low: The Defect is an irritant but repair can be done once the more serious Defect has been fixed
- Medium: During the normal course of the development activities defect should be resolved. It can wait until a new version is created
- High: The defect must be resolved as soon as possible as it affects the system severely and cannot be used until it is fixed

**Severity:** This describes the impact of the bug.
**Types of Severity:**

- Critical: This defect indicates complete shut-down of the process, nothing can proceed further
- Major: It is a highly severe defect and collapses the system. However, certain parts of the system remain functional
- Medium: It causes some undesirable behavior, but the system is still functional
- Low: It won’t cause any major break-down of the system

### Some Bonus Tips To Write A Good Bug Report

Given below are some additional tips on how to write a good Bug report:

#### Report the problem immediately

If you find any bugs while testing, then you do not need to wait to write a detailed bug report later.

#### Reproduce the bug three times before writing a Bug report detail

Your bug should be reproducible. Make sure that your steps are robust enough to reproduce the bug without any ambiguity. If your bug is not reproducible every time, then you can still file a bug mentioning the periodic nature of the bug.

#### Test the same bug occurrence on other similar modules

Sometimes the developer uses the same code for different similar modules. So there is a higher chance for the bug in one module to occur in other similar modules as well. You can even try to find the more severe version of the bug you found.

#### Write a good bug summary

Bug summary will help the developers to quickly analyze the bug’s nature. A poor quality report will unnecessarily increase development and testing time. Communicate well with your bug report summary. Keep in mind that the bug summary can be used as a reference to search for the bug in the bug inventory.

#### Read the Bug report before hitting the Submit button

Read all the sentences, wordings and steps that are used in the bug report. See if any sentence is creating ambiguity that can lead to misinterpretation. Misleading words or sentences should be avoided in order to have a clear bug report.

#### Do not use abusive language.

It’s nice that you did a good work and found a bug but do not use this credit for criticizing the developer or to attack any individual.
