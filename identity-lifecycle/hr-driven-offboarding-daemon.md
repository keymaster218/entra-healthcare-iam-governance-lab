# HR-Driven Offboarding Daemon

## Purpose

The HR-driven offboarding daemon automates the removal of workforce access when an employee or contractor is disabled or terminated in the authoritative HR system.

The daemon runs without interactive user sign-in. It checks HR workforce status on a scheduled basis, identifies identities that require offboarding, removes access in Microsoft Entra ID, and creates audit records for each action.

The primary goals are to:

* Reduce the time terminated workforce members retain access.
* Apply offboarding actions consistently.
* Reduce reliance on manual notifications.
* Create evidence that access removal was completed.
* Support healthcare identity governance and least-privilege practices.

---

## Business Problem

Manual offboarding processes can create security and compliance risks when:

* HR status changes are not communicated promptly.
* User accounts remain enabled after employment ends.
* Group memberships are not removed consistently.
* Active sessions remain valid after account disablement.
* Different administrators perform different offboarding steps.
* There is no centralized evidence showing when access was revoked.

In a healthcare environment, delayed access removal may allow former workforce members to retain access to systems containing sensitive operational information or electronic protected health information.

---

## Solution Overview

The daemon performs the following process:

1. Runs every hour.
2. Reads employee records from a mock HR data source.
3. Identifies employees marked `Disabled` or `Terminated`.
4. Matches each HR record to a Microsoft Entra ID user.
5. Determines whether the identity is eligible for automated offboarding.
6. Disables the Entra ID account if it is still active.
7. Revokes active sign-in sessions.
8. Removes the user from applicable security groups.
9. Records each attempted action and result in an audit log.
10. Records exceptions and failures for follow-up.

The initial lab implementation will use fictional identities and mock HR data.

---

## Architecture

The daemon includes the following components:

| Component           | Purpose                                                            |
| ------------------- | ------------------------------------------------------------------ |
| Mock HR data source | Provides authoritative workforce status for the lab                |
| Offboarding daemon  | Processes HR records and performs access-removal actions           |
| Microsoft Entra ID  | Stores workforce identities, account status, and group memberships |
| Microsoft Graph     | Provides the API used to locate users and update access            |
| App registration    | Allows the daemon to authenticate without user interaction         |
| Audit log           | Records processing results, exceptions, and errors                 |
| Scheduler           | Runs the daemon every hour                                         |

The logical flow is:

HR data source
      |
      v
Offboarding daemon
      |
      v
Microsoft Graph
      |
      v
Microsoft Entra ID
      |
      v
Audit log and remediation evidence

---

## Authentication Model

The daemon uses application-only authentication.

No employee or administrator signs in to run the process.

The application authenticates to Microsoft Graph using:

* A Microsoft Entra app registration
* OAuth 2.0 client credentials flow
* Microsoft Graph application permissions
* Administrative consent

For the first lab version, the application may use a client secret.

A production implementation should prefer:

* Managed identity
* Certificate-based authentication
* Workload identity federation

Long-lived secrets should be avoided where possible.

---

## Scheduling

The daemon is designed to run once every hour.

Possible scheduling options include:

* Windows Task Scheduler
* Azure Automation
* Azure Functions timer trigger
* Azure Logic Apps
* A containerized scheduled job
* GitHub Actions for demonstration purposes

The first development version may be run manually while the processing logic is tested.

The final lab should demonstrate or document an hourly schedule.

---

## Authoritative HR Data

The HR system is treated as the authoritative source for employment status.

The mock HR data should include:

| Field               | Purpose                                                |
| ------------------- | ------------------------------------------------------ |
| `employeeId`        | Unique workforce identifier                            |
| `userPrincipalName` | Primary identifier used to locate the Entra ID account |
| `displayName`       | Human-readable identity reference                      |
| `department`        | Current department                                     |
| `employmentType`    | Employee, contractor, or temporary worker              |
| `employmentStatus`  | Active, Leave, Disabled, or Terminated                 |
| `terminationDate`   | Date employment ended, when applicable                 |
| `lastUpdated`       | Timestamp of the most recent HR status change          |

Example statuses include:

* `Active`
* `Leave`
* `Disabled`
* `Terminated`

Only records marked `Disabled` or `Terminated` are eligible for automated access removal.

Approved leave should not trigger offboarding.

---

## Identity Matching

The daemon uses `userPrincipalName` as the primary matching attribute between the HR record and the Entra ID account.

The employee ID is retained as a secondary identifier for reconciliation and audit logging.

The daemon should not rely on display name because:

* Multiple people may have the same name.
* Names may change.
* Formatting may differ between systems.
* Display names are not guaranteed to be unique.

A production system could also store the HR employee ID in a dedicated Entra ID employee attribute.

---

## Eligibility Checks

Before modifying an identity, the daemon verifies that:

* The HR status is `Disabled` or `Terminated`.
* The Entra ID account exists.
* The account is a workforce user.
* The account is not listed as exempt.
* The identity is not a service principal or application identity.
* The identity is not an emergency access account.
* The record has not already been processed without subsequent changes.
* Required identifying fields are present.

Records that fail validation are logged for manual review.

---

## Automated Offboarding Actions

For each eligible identity, the daemon attempts to perform the following actions.

### Disable the User Account

If the Entra ID account is enabled, the daemon changes the account status to disabled.

If the account is already disabled, the daemon records that no additional action was required.

The daemon does not delete the user account.

Account deletion is treated as a separate retention and records-management decision.

### Revoke Active Sessions

The daemon revokes the user's active sign-in sessions to reduce the likelihood that an existing session remains usable after account disablement.

Session revocation may not invalidate every application session immediately. Application-specific session behavior should be considered in a production implementation.

### Remove Group Memberships

The daemon removes the identity from applicable Entra ID security groups.

This may include:

* Departmental access groups
* Application access groups
* Azure access groups
* Clinical support groups
* Administrative support groups
* License-assignment groups where approved

Certain groups may be excluded from automatic removal, such as groups used for:

* Records retention
* Legal hold
* Investigation
* Archived identity tracking
* Approved offboarding workflows

The exclusion list must be documented.

### Record Results

The daemon creates an audit entry for each action attempted.

A single user may generate multiple audit records during one run.

---

## Exempt and Protected Identities

The daemon must not automatically modify:

* Emergency access accounts
* Service accounts
* Shared clinical accounts
* Application identities
* Service principals
* Managed identities
* Test identities approved for exclusion
* Accounts subject to legal hold
* Accounts subject to an active investigation
* Identities explicitly placed on an approved exemption list

An exempt identity should be skipped and logged with the reason for exclusion.

Exemptions should include:

* Identity
* Business owner
* Technical owner
* Reason
* Approval
* Effective date
* Expiration date
* Review date

Exemptions must not remain indefinite without review.

---

## Idempotency

The daemon must be safe to run repeatedly.

Because it runs every hour, it may encounter the same terminated employee more than once.

The process should not fail when:

* The account is already disabled.
* Sessions have already been revoked.
* A group membership has already been removed.
* The employee was processed during a previous run.

Instead, the daemon should record outcomes such as:

* `AlreadyDisabled`
* `NoActiveMembership`
* `PreviouslyProcessed`
* `NoActionRequired`

This prevents repeated runs from creating inconsistent results.

---

## Audit Logging

Each processing run receives a unique run identifier.

Audit records should include:

| Field                  | Description                                     |
| ---------------------- | ----------------------------------------------- |
| `runId`                | Unique identifier for the daemon execution      |
| `timestamp`            | Time the action occurred                        |
| `employeeId`           | HR workforce identifier                         |
| `userPrincipalName`    | Entra ID identity                               |
| `employmentStatus`     | HR status that triggered processing             |
| `action`               | Action attempted                                |
| `result`               | Success, skipped, no action required, or failed |
| `details`              | Additional processing information               |
| `errorCode`            | Error identifier, when applicable               |
| `applicationIdentity`  | Daemon application that performed the action    |
| `durationMilliseconds` | Time required to process the action             |

Example actions include:

* `UserLookup`
* `DisableAccount`
* `RevokeSessions`
* `RemoveGroupMembership`
* `SkipExemptAccount`
* `ValidateHRRecord`

Example results include:

* `Success`
* `Failed`
* `Skipped`
* `AlreadyDisabled`
* `AccountNotFound`
* `NoActionRequired`

Audit logs should not include:

* Client secrets
* Access tokens
* Passwords
* Unnecessary HR information
* Sensitive investigation details

---

## Error Handling

The daemon must continue processing other employees when one record fails.

One failure should not terminate the entire hourly run unless the failure affects all processing, such as an authentication outage.

Possible errors include:

* Microsoft Graph authentication failure
* Insufficient application permissions
* Missing HR fields
* Duplicate HR records
* Entra ID account not found
* Group removal failure
* Session revocation failure
* Network or API timeout
* Microsoft Graph throttling
* Invalid exclusion configuration

Errors should be:

* Logged
* Associated with the affected identity
* Assigned a clear error code
* Retried where appropriate
* Escalated when manual action is required

---

## Retry Behavior

Transient failures may be retried.

Examples include:

* Network timeouts
* Microsoft Graph throttling
* Temporary service unavailability

The daemon should use limited retries with increasing wait periods.

It should not retry indefinitely.

Permanent failures, such as an invalid user principal name or insufficient permission, should be logged for manual review.

---

## Least-Privilege Permissions

The daemon application should receive only the Microsoft Graph application permissions required to perform its documented functions.

The lab should evaluate permissions needed to:

* Read user accounts
* Update user account status
* Revoke sign-in sessions
* Read group memberships
* Remove group memberships

Each permission should be documented with:

* Permission name
* Business purpose
* Actions enabled
* Associated risk
* Approval requirement
* Whether a narrower alternative exists

Because application permissions operate without a signed-in user, they require administrative consent and careful monitoring.

---

## Security Requirements

The daemon design includes the following controls:

* Application-only authentication
* Least-privilege Graph permissions
* Secret or certificate protection
* No credentials stored in source control
* Explicit account exclusions
* Audit logging
* Error logging
* Repeatable processing
* Separation of HR data and authentication credentials
* No automatic user deletion
* Manual review for failures
* Monitoring of application activity
* Periodic review of application permissions

Configuration files containing secrets must be excluded through `.gitignore`.

---

## Testing Scenarios

The lab should test the following cases:

| Test Case                                  | Expected Result                |
| ------------------------------------------ | ------------------------------ |
| Active employee                            | No offboarding action          |
| Employee on approved leave                 | No offboarding action          |
| Terminated employee with enabled account   | Account disabled               |
| Disabled employee with active sessions     | Sessions revoked               |
| Terminated employee with group memberships | Applicable memberships removed |
| Account already disabled                   | Logged as already disabled     |
| Entra ID account not found                 | Failure logged for review      |
| Emergency access account                   | Skipped and logged             |
| Service account                            | Skipped and logged             |
| Missing user principal name                | Validation failure logged      |
| Duplicate HR record                        | Duplicate detected and logged  |
| One user fails processing                  | Remaining users continue       |
| Graph API returns transient failure        | Limited retry attempted        |
| Group already removed                      | Logged as no action required   |

All testing must use fictional identities and non-production resources.

---

## Success Criteria

The component is considered successful when:

* It runs without an interactive user login.
* It processes mock HR data.
* It identifies disabled and terminated employees.
* It safely ignores active and leave-status employees.
* It disables eligible Entra ID accounts.
* It revokes active sessions.
* It removes applicable group memberships.
* It skips exempt identities.
* It generates clear audit records.
* It continues processing after individual failures.
* It can be run repeatedly without harmful duplicate actions.
* It can be scheduled to run every hour.
* No credentials or sensitive values are committed to GitHub.

---

## Relationship to Quarterly Access Reviews

The offboarding daemon is an operational control.

It removes access shortly after an authoritative HR status change.

The quarterly access review is a governance control.

It validates that:

* Terminated employees no longer retain access.
* Disabled accounts have no inappropriate memberships.
* Automated processing completed successfully.
* Exemptions remain valid.
* Errors were remediated.
* Role changes were reflected in access assignments.

Together, these controls provide both timely access removal and periodic validation.

---

## HIPAA Security Rule Alignment

This component supports HIPAA-aligned workforce access practices by helping the organization:

* Terminate access when workforce authorization ends.
* Enforce consistent identity lifecycle procedures.
* Maintain records of access-removal actions.
* Reduce inappropriate access to systems containing ePHI.
* Support accountability through audit evidence.

This component supports access governance but does not independently establish HIPAA compliance.

---

## Future Enhancements

Future versions may include:

* Direct integration with an HRIS API
* Event-driven processing instead of hourly polling
* Managed identity authentication
* Certificate-based authentication
* Azure Key Vault integration
* Azure Functions deployment
* Centralized Log Analytics ingestion
* Alerting for failed offboarding actions
* Automatic ticket creation
* Manager and application-owner notifications
* License removal
* Privileged Identity Management assignment removal
* Administrative unit support
* Reconciliation reports
* Dashboard metrics
