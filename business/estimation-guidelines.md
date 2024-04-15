---
tags:
  - consulting
  - internal
  - estimation
  - guideline
title: Estimation Guidelines
date: 2023-12-08
description: When we conduct an estimation, it is recommended to abandon the transitional “exact hours” assessment method, instead, use the story point based on the Fibonacci number (1, 2, 3, 5, 8, 13, 21…). The number expresses an estimation of the overall effort required to fully implement a backlog item or any piece of work.
authors:
  - huytq
  - monotykamary
menu: consulting
type: consulting
hide_frontmatter: false
---

When we conduct an estimation, it is recommended to abandon the transitional “exact hours” assessment method, instead, use the story point based on the Fibonacci number (1, 2, 3, 5, 8, 13, 21…). The number expresses an estimation of the overall effort required to fully implement a backlog item or any piece of work.

Below’s an example of an estimation table based on the matrix of complexity, uncertainty, and effort:

| Story Points | Reference | Uncertainty | Risk | Efforts | FE Example | BE Example |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | 1. Stories that take < 0.25 day to complete<br>2. Stories that only involve FE OR BE<br>3. Stories that is clear, no need to investigate/find root cause | Low<br>- 100% Clear | Low | Less than half a day: 1 hour or less | Small UI update that doesn’t require BE work: <br>- Color, Font, Positioning that doesn’t require relayout<br>- Sorting (no BE work)<br>- Only impact 1-2 screens/controls | - Configurations only<br> |
| 2 | 1. Stories that take < 0.5 day to complete<br>2. Stories that is more of FE OR BE work, the other is minimal<br>3. Behavioural work, involve calculation/ logics  | Low<br>- Involves some changes to current calculation/logic | Low | Around half a day to 1 day | - Calculate/Sum/Count numbers<br>- Small UI change but on multiple screens (3 or more) | - Minor changes to existing API (Add/edit/remove fields...)<br>- Minor change on calculations to current API |
| 3 | 1. Stories that take around 1 day to complete<br>2. Change behavior/calculation/logic of current function that we need to rework the function<br>3. New calculation/logic that is different than existing default from sources | Low<br>- Need to spend time to check the logic/calculation/ reproduce | Low | Around 1 working Day |  |  |
| 5 | 1. Stories that take 1-2 days to complete<br>2. Only need single/simple new endpoint to complete<br>3. Little or no migration data needed<br>4. Little or no DevOps involvement | Low - Medium<br>Some clarification needed but the main flow is clear | Medium | Around 3 working Days |  |  |
| 8 | 1. Stories that take half a Sprint to complete<br>2. New feature that requires multiple endpoints/screen<br>3. Need some research to figure out solutions<br>4. Migration data needed<br>5. Medium Server/DevOps involvement | Medium - High<br>- Completely new feature <br>- Need some research to figure out solution | Medium → High<br>- Follows current architecture design<br>- Might impact other feature(s)<br>- Might need migration of data | Around 5 working Days |  |  |
| 13 | 1. We are not sure if it works<br>2. Need leaders to take a look into new approach/solution and test to see if it works<br>3. POC work<br>4. Architecture change and/or new coding approach  | High<br>- Research/POC/ Architecture Stories | High<br>- We are not sure if it work or not | If cannot deliver in a working week, please break it down |  |  |
