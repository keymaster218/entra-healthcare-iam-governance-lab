# Quarterly Access Review Process

## Purpose

The quarterly access review process ensures that workforce members retain only the access required for their current job responsibilities.

The review is designed to identify and correct:

* Unnecessary group memberships
* Excessive privileges
* Inactive or orphaned accounts
* Access retained after role changes
* Privileged assignments that are no longer justified
* Access that does not align with least-privilege principles

Quarterly reviews supplement automated onboarding, role-change, and offboarding controls by validating that access remains appropriate over time.

---

## Scope

This process applies to workforce identities managed in Microsoft Entra ID, including:

* Employees
* Contractors
* Temporary workforce members
* Standard user accounts
* Separate administrative accounts
* Security group memberships
* Microsoft Entra role assignments
* Azure role-based access control assignments included in the lab

The following identity types are reviewed through separate control processes:

* Emergency access accounts
* Service accounts
* Application identities
* Managed identities
* Shared accounts approved for specific operational purposes

These accounts may still appear in review evidence when their access affects the reviewed environment.

---

## Review Frequency

Access reviews are performed once every calendar quarter.

The Identity and Access Management team initiates the review and establishes a completion deadline.

Additional reviews may be initiated following:

* Department reorganizations
* Workforce reductions
* Security incidents
* Audit findings
* Major system implementations
* Significant changes to sensitive applications
* Changes to privileged access requirements

---

## Review Objectives

The quarterly review confirms that:

* Workforce members remain active.
* Department and job information are accurate.
* Assigned access supports current responsibilities.
* Group memberships remain appropriate.
* Privileged access remains necessary.
* Administrative access follows least privilege.
* Access removed during prior review cycles remains revoked.
* Exceptions are documented and approved.
* Review evidence is retained for audit purposes.

---

## Roles and Responsibilities

| Role                           | Responsibility                                                                                                |
| ------------------------------ | ------------------------------------------------------------------------------------------------------------- |
| Identity and Access Management | Initiates the review, prepares access data, tracks completion, coordinates remediation, and retains evidence. |
| Department Manager             | Confirms workforce status, role alignment, and continued business need for access.                            |
| Application Owner              | Reviews access to applications and sensitive application roles within their area of responsibility.           |
| Information Security           | Reviews privileged access, sensitive groups, unresolved exceptions, and high-risk findings.                   |
| Human Resources                | Provides authoritative workforce status and department information when clarification is required.            |
| Compliance                     | Confirms that evidence is complete and retained according to organizational requirements.                     |

---

## Review Population

The IAM team prepares a review population that includes:

* User principal name
* Display name
* Employee or contractor identifier
* Department
* Manager
* Employment status
* Account status
* Group memberships
* Administrative account indicator
* Entra role assignments
* Azure RBAC assignments, when included
* Last sign-in date, when available
* Existing exception status

The review population should use current data from authoritative systems whenever possible.

---

## Review Procedure

### 1. Prepare Review Data

The IAM team exports the identities and access assignments included in scope.

The data is organized by:

* Department
* Manager
* Application owner
* Security group
* Privileged role
* Resource assignment

The IAM team performs an initial validation to identify obvious data issues before distributing the review.

Examples include:

* Missing manager assignments
* Duplicate identities
* Disabled users with active access
* Users assigned to departments that no longer exist
* Privileged roles assigned without a documented owner

### 2. Distribute Review Assignments

Each reviewer receives only the records within their responsibility.

Department managers review workforce access for their direct or delegated staff.

Application owners review access to systems and application-specific roles.

Information Security reviews:

* Privileged roles
* Sensitive security groups
* Administrative accounts
* High-impact Azure RBAC assignments
* Exceptions involving elevated access

### 3. Evaluate Access

For each identity and access assignment, the reviewer selects one of the following decisions:

| Decision    | Meaning                                                                                  |
| ----------- | ---------------------------------------------------------------------------------------- |
| Approve     | Access remains appropriate for the current role.                                         |
| Remove      | Access is no longer required.                                                            |
| Modify      | Access is still required, but the current assignment should be changed.                  |
| Investigate | Additional information is required before a final decision.                              |
| Exception   | Access does not meet the standard model but has a documented and approved business need. |

Reviewers evaluate whether:

* The individual is still employed or under contract.
* The department and job role are correct.
* The user still requires the assigned group membership.
* The level of access is appropriate.
* Privileged access remains necessary.
* A less privileged role could meet the business need.
* Administrative access is assigned to a separate account.
* The access conflicts with separation-of-duties expectations.
* Inactive or disabled accounts retain access.

### 4. Submit Review Decisions

Reviewers submit their decisions by the review deadline.

Each removal, modification, investigation, or exception decision must include enough information to support follow-up.

Required information may include:

* Reason for the decision
* Requested access change
* Business owner
* Remediation owner
* Target completion date
* Exception expiration date, when applicable

### 5. Remediate Findings

The IAM team or responsible system owner completes approved access changes.

Remediation may include:

* Removing a user from a security group
* Disabling an account
* Removing an Entra role assignment
* Removing an Azure RBAC assignment
* Replacing standing privileged access with eligible access
* Correcting department or manager information
* Separating standard and administrative accounts
* Escalating unresolved workforce status questions to HR

High-risk findings should be prioritized.

Examples include:

* Active access for a terminated employee
* Unapproved privileged access
* Membership in a sensitive group without a business owner
* An active shared administrative account
* A disabled account with access to sensitive systems

### 6. Verify Remediation

The IAM team verifies that approved access changes were completed.

Verification may include:

* Re-exporting group membership
* Confirming account status
* Confirming role assignment removal
* Reviewing Microsoft Entra audit logs
* Reviewing Azure activity logs
* Comparing the post-remediation state to the review decision

A remediation item is not considered complete until the resulting access state has been verified.

### 7. Close the Review

The review is closed when:

* All assigned reviews are complete.
* Required access changes have been completed or formally tracked.
* High-risk findings have been resolved or escalated.
* Exceptions have documented approval and expiration dates.
* Evidence has been retained.
* Final review metrics have been recorded.

---

## Privileged Access Review

Privileged access receives additional scrutiny because it can affect identity configuration, security controls, and sensitive resources.

The review includes:

* Microsoft Entra administrative roles
* Privileged security groups
* Separate administrative accounts
* Azure Owner and User Access Administrator assignments
* Other elevated Azure RBAC roles included in the lab
* Permanent privileged assignments
* Eligible privileged assignments
* Privileged assignments without recent use

Reviewers confirm that:

* The assignment has a current business need.
* The access level is no broader than necessary.
* Standing access is minimized.
* Eligible access is used when appropriate.
* Administrative accounts are separate from standard accounts.
* The assignment has an accountable owner.
* The assignment is included in monitoring and logging.

---

## Inactive and Disabled Accounts

The review identifies accounts that may require additional action, including:

* Disabled accounts that retain group memberships
* Accounts with no recent sign-in activity
* Accounts without an identified manager
* Accounts not matched to an HR record
* Duplicate accounts
* Contractor accounts beyond their expected end date
* Administrative accounts without an active standard user account

Inactive status alone does not automatically prove that access is inappropriate.

The reviewer must consider legitimate reasons for inactivity, such as:

* Approved leave
* Seasonal work
* Limited-use administrative accounts
* Disaster recovery responsibilities
* Approved emergency access

---

## Exceptions

An exception may be requested when access is required but does not align with the standard access model.

Each exception must include:

* Identity or group affected
* Access being retained
* Business justification
* Risk description
* Compensating controls
* Business owner
* Security approval, when required
* Approval date
* Expiration or next review date

Exceptions must not remain open indefinitely.

Expired exceptions are treated as findings unless renewed through the approved process.

---

## Escalation

Items may be escalated when:

* A reviewer does not complete the review by the deadline.
* Workforce status cannot be confirmed.
* Privileged access lacks a documented business need.
* A requested removal is not completed.
* A high-risk finding remains unresolved.
* An exception is requested without sufficient justification.
* The reviewer disputes the ownership of an access assignment.

Escalation may involve:

* The reviewer's manager
* The application owner
* Information Security leadership
* Human Resources
* Compliance
* The designated risk owner

---

## Audit Evidence

The IAM team retains evidence demonstrating that the review was completed.

Evidence includes:

* Review period
* Review start and completion dates
* Identities and assignments reviewed
* Reviewer names or identifiers
* Review decisions
* Access removal requests
* Completed remediation records
* Verification evidence
* Approved exceptions
* Escalation records
* Final metrics
* Review approval or closure record

Evidence should avoid including sensitive HR or healthcare information that is not required to support the control.

---

## Review Metrics

The following metrics may be used to evaluate the process:

| Metric                          | Description                                                      |
| ------------------------------- | ---------------------------------------------------------------- |
| Review completion rate          | Percentage of assigned review items completed by the deadline    |
| Reviewer participation rate     | Percentage of required reviewers who completed their assignments |
| Access removal count            | Number of access assignments removed                             |
| Access modification count       | Number of access assignments changed                             |
| Privileged access removal count | Number of privileged assignments removed                         |
| Exception count                 | Number of approved exceptions                                    |
| Overdue remediation count       | Number of findings not remediated by the target date             |
| Average remediation time        | Average time between review decision and verified completion     |
| Disabled accounts with access   | Number of disabled accounts found with remaining access          |
| Orphaned account count          | Number of identities without a confirmed owner or HR record      |

Metrics are used to identify trends and improve the process rather than to discourage reviewers from identifying access issues.

---

## Relationship to Automated Offboarding

The HR-driven offboarding daemon provides an operational control that detects disabled or terminated employees and removes access between quarterly review cycles.

The quarterly access review provides a governance control that validates:

* Access remains aligned with current roles.
* Automated offboarding completed successfully.
* Role changes were reflected in access assignments.
* Privileged access remains justified.
* Exceptions remain valid.
* Orphaned or inactive identities are investigated.

Together, automated offboarding and quarterly reviews reduce the risk of delayed or inappropriate access removal.

---

## Sample Review Outcomes

Examples of review findings include:

* A billing analyst transferred to another department but retained revenue-cycle access.
* A disabled contractor account remained in an application support group.
* A cloud engineer retained a privileged role after changing positions.
* A manager approved access but requested a narrower role.
* An administrative account existed without a corresponding active workforce identity.
* A sensitive group had no documented application owner.
* A previously approved exception had expired.

---

## Process Completion Criteria

The quarterly review is complete when:

* All in-scope identities and access assignments have a recorded decision.
* All critical findings have been remediated or escalated.
* Standard findings have owners and target dates.
* Required access changes have been verified.
* Exceptions are approved and time-bound.
* Review evidence has been retained.
* Final metrics have been documented.
