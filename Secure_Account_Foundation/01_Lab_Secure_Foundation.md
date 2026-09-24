# Secure Account Foundation
## Scenario:
A new AWS account needs to be secured before any workloads are deployed. I implemented identity management, audit logging, and cost guardrails following AWS best practices.

 ![Governance Diagram](./Diagrams/Goverance-Flow-Chart.png)




## [Verification](Verification-commands.md)


## Lessons Learned
- When mapping permissions in IAM Identity Center sets you have to go to AWS Accounts > Select Account > Assign user & Group > Select Group > Select Permission. This will have the permission set be attached to the correct group which is different then IAM.
