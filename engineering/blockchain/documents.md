When launching a contract that will have substantial funds or is required to be mission critical, it is important to include proper documentation.

## contact

- Who to contact with issues
- Names of programmers and/or other important parties
- Chat room where questions can be asked

## history

- Testing (including usage stats, discovered bugs, length of testing)
- People who have reviewed code (and their key feedback)

## Known-issues

- Key risks with contract
  - e.g., You can lose all your money, hacker can vote for certain outcomes
- All known bugs/limitations
- Potential attacks and mitigants
- Potential conflicts of interest (e.g., will be using yourself, like Slock.it did with the DAO)

## Procedures

- Action plan in case a bug is discovered (e.g., emergency options, public notification process,
  etc.)
- Wind down process if something goes wrong (e.g., funders will get percentage of your balance
  before attack, from remaining funds)
- Responsible disclosure policy (e.g., where to report bugs found, the rules of any bug bounty
  program)
- Recourse in case of failure (e.g., insurance, penalty fund, no recourse)

## Specification

- Specs, diagrams, state machines, models, and other documentation that helps auditors, reviewers,
  and the community understand what the system is intended to do.
- Many bugs can be found just from the specifications, and they are the least costly to fix.
- Rollout plans that include details listed [here](../precautions.md), and
  target dates.

## Status

- Where current code is deployed
- Compiler version, flags used, and steps for verifying the deployed bytecode matches the source
  code
- Compiler versions and flags that will be used for the different phases of rollout.
- Current status of deployed code (including outstanding issues, performance stats, etc.)
