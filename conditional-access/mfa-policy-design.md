# Multi-Factor Authentication Policy Design

## Purpose

This policy defines how Northstar Health Services uses Microsoft Entra Conditional Access to require multi-factor authentication for workforce identities.

The policy is designed to reduce the risk of unauthorized access caused by stolen passwords, phishing, password reuse, and credential compromise.

MFA is one component of the organization’s broader identity security program and works alongside:

* Role-based access control
* Privileged access governance
* Identity lifecycle management
* Access reviews
* Sign-in monitoring
* Emergency access procedures

---

## Policy Objectives

The MFA policy is designed to:

* Require MFA for workforce access to organizational resources.
* Apply stronger authentication requirements to privileged users.
* Reduce reliance on passwords as the only authentication factor.
* Block access from authentication methods that cannot support MFA.
* Protect administrative portals and sensitive systems.
* Maintain controlled emergency access.
* Provide evidence that authentication controls are enforced.
* Minimize unnecessary exclusions and policy gaps.

---

## Scope

This policy applies to:

* Employees
* Contractors
* Temporary workforce members
* Standard user accounts
* Administrative accounts
* Users accessing Microsoft 365
* Users accessing Azure resources
* Users accessing applications integrated with Microsoft Entra ID
* Privileged users and administrators

The following identity types require separate evaluation:

* Emergency access accounts
* Service accounts
* Application identities
* Managed identities
* Workload identities
* Shared clinical or operational accounts

Non-human identities must not be excluded from review simply because interactive MFA does not apply to them. They require separate authentication and credential controls.

---

## Policy Summary

| Policy Element     | Design                                                                 |
| ------------------ | ---------------------------------------------------------------------- |
| Policy name        | CA-Workforce-Require-MFA                                               |
| Identity scope     | All workforce users                                                    |
| Resource scope     | All cloud resources                                                    |
| Access requirement | Require multi-factor authentication                                    |
| Session control    | Reauthentication requirements based on risk and business need          |
| Initial state      | Report-only                                                            |
| Production state   | Enabled after testing                                                  |
| Exclusions         | Approved emergency access accounts and documented technical exceptions |
| Review frequency   | Quarterly and after significant environment changes                    |

---

## Target Identities

The policy targets all workforce users unless a documented exclusion applies.

Included identities may include:

* Health Information Management users
* Revenue Cycle users
* Clinical Application Support users
* IT Service Desk users
* Security Operations users
* Cloud Operations users
* Compliance users
* Department managers
* Contractors
* Separate administrative accounts

Privileged identities may also be targeted by a separate, stricter Conditional Access policy.

---

## Target Resources

The baseline MFA policy applies to all cloud resources protected by Microsoft Entra ID.

This approach reduces the likelihood that an application will be unintentionally left outside the MFA requirement.

Applications may be handled differently only when:

* The application cannot support the required authentication flow.
* A documented operational dependency exists.
* The risk has been reviewed.
* Compensating controls are implemented.
* The exception has an owner and expiration date.

---

## Access Controls

To receive access, the user must successfully complete multi-factor authentication.

The policy should not rely on password authentication alone.

Where supported, the organization should encourage or require stronger authentication methods for privileged and high-risk access.

Examples include:

* Microsoft Authenticator
* FIDO2 security keys
* Passkeys
* Certificate-based authentication
* Windows Hello for Business

SMS and voice authentication may be retained only where required by business constraints and accepted through risk review.

---

## Privileged User Requirements

Privileged users require stronger protections because their accounts can modify identity, security, and resource configurations.

A separate privileged access policy should require:

* MFA for every privileged access session
* A separate administrative account
* Strong authentication methods
* Access from an approved or compliant device
* Restricted access to administrative portals
* Limited session duration
* Additional sign-in monitoring
* Just-in-time role activation where available

Privileged identities include users assigned roles such as:

* Global Administrator
* Privileged Role Administrator
* Security Administrator
* Conditional Access Administrator
* User Administrator
* Authentication Administrator
* Azure Owner
* User Access Administrator

Permanent privileged access should be minimized.

---

## Emergency Access Accounts

Emergency access accounts provide a controlled method for restoring access when normal authentication systems or Conditional Access policies fail.

These accounts may be excluded from the baseline MFA policy when required by the emergency access design.

Emergency access controls include:

* A limited number of accounts
* Cloud-only identities
* Long, securely stored credentials
* No routine daily use
* No dependency on the same MFA method being recovered
* Continuous monitoring
* Alerts for every sign-in attempt
* Periodic access testing
* Documented ownership
* Quarterly review
* Immediate investigation of unexpected use

Emergency access exclusions must be explicit and must not use broad groups that could unintentionally include standard users.

---

## Service and Application Accounts

Interactive service accounts should be eliminated where possible.

Preferred alternatives include:

* Managed identities
* Workload identities
* Service principals
* Certificate-based authentication
* Application credentials stored in an approved secret-management system

Service accounts must not be broadly excluded from MFA without review.

Each exception must document:

* Account owner
* Business purpose
* Applications or services used
* Authentication method
* Permissions
* Credential storage
* Credential rotation requirements
* Monitoring controls
* Review date
* Expiration date

---

## Legacy Authentication

Legacy authentication methods that cannot support modern authentication and MFA should be blocked.

Legacy authentication creates risk because it may allow an attacker to bypass MFA requirements using only a username and password.

The organization should:

* Identify legacy authentication usage.
* Determine the associated application or protocol.
* Migrate affected systems to modern authentication.
* Create only temporary, documented exceptions.
* Monitor exception usage.
* Remove exceptions after migration.

Blocking legacy authentication should be implemented through a dedicated Conditional Access policy so that the control can be tested and monitored separately.

---

## Policy Exclusions

Exclusions are limited to identities or applications with a validated technical or emergency requirement.

Every exclusion must include:

* Identity or application excluded
* Business justification
* Technical reason
* Risk description
* Compensating controls
* Business owner
* Technical owner
* Approval
* Effective date
* Expiration date
* Review date

Exclusions must not remain open indefinitely.

Broad exclusions such as entire departments should be avoided.

---

## Deployment Approach

The MFA policy is deployed in phases.

### Phase 1: Design

The IAM and Security teams define:

* Included identities
* Target resources
* Authentication requirements
* Emergency access exclusions
* Service account handling
* Monitoring requirements
* Success criteria

### Phase 2: Report-Only Validation

The policy is placed in report-only mode.

The IAM team reviews sign-in results to identify:

* Users who would be blocked
* Applications that do not support the control
* Service accounts using interactive authentication
* Legacy authentication usage
* Unexpected exclusions
* Users without registered MFA methods

### Phase 3: Pilot

The policy is enabled for a pilot population.

The pilot should include users from different departments and access patterns, such as:

* Health Information Management
* Revenue Cycle
* Clinical Application Support
* IT Service Desk
* Security Operations
* Cloud Operations

Pilot findings are documented and remediated before broader deployment.

### Phase 4: Production Rollout

The policy is enabled in stages for the remaining workforce population.

The IAM team confirms that:

* Emergency access accounts are functional.
* User communications are complete.
* Support teams are prepared.
* Authentication methods are registered.
* Exceptions are documented.
* Monitoring is active.

### Phase 5: Ongoing Monitoring

After deployment, the IAM team monitors:

* MFA failures
* Sign-in interruptions
* Excluded identities
* Legacy authentication attempts
* Emergency account use
* Authentication method changes
* Policy changes
* Help desk volume
* High-risk sign-ins

---

## User Registration

Users must register approved authentication methods before enforcement.

The registration process should include:

* Identity verification
* Registration instructions
* Approved authentication methods
* Recovery procedures
* Help desk support
* Lost-device procedures
* Replacement-device procedures
* Restrictions on weak authentication methods

Authentication method registration should be monitored to identify users who have not completed enrollment.

---

## Authentication Method Governance

Authentication methods should be evaluated according to security strength and business need.

The organization should maintain an approved authentication method standard that defines:

* Methods allowed for standard users
* Methods required for privileged users
* Methods permitted for account recovery
* Methods prohibited or being phased out
* Registration requirements
* Device replacement procedures
* Lost-device response
* Help desk verification requirements

Changes to authentication methods should generate audit records and may require additional verification.

---

## Help Desk Procedures

The help desk must follow identity verification procedures before assisting with MFA resets or authentication method changes.

The help desk must not rely solely on information that an attacker could easily obtain.

Reset procedures should include:

* Verification of the user’s identity
* Documentation of the request
* Recording the technician who performed the action
* Notification to the user
* Review of unusual or high-risk requests
* Escalation for privileged accounts
* Audit logging

Privileged user resets should require enhanced verification or approval.

---

## Session Controls

Session requirements should balance security with clinical and operational needs.

Potential controls include:

* Sign-in frequency
* Persistent browser session restrictions
* Reauthentication for sensitive applications
* Device compliance requirements
* Application-enforced restrictions
* Limited web access from unmanaged devices

More restrictive session controls may be applied to:

* Privileged users
* High-risk sign-ins
* Unmanaged devices
* Sensitive applications
* External locations

Clinical workflow impacts should be evaluated before enforcing frequent reauthentication.

---

## Risk-Based Authentication

Where available, sign-in and user risk signals may be used to apply additional controls.

Examples include:

* Requiring MFA for risky sign-ins
* Requiring secure password reset for compromised users
* Blocking access when risk is unacceptable
* Escalating high-risk activity for investigation

Risk-based policies should complement the baseline MFA requirement rather than replace it.

---

## Monitoring and Audit Evidence

The IAM and Security teams retain evidence that the MFA control is operating as intended.

Evidence may include:

* Conditional Access policy configuration
* Policy inclusion and exclusion lists
* Report-only results
* Pilot test results
* Sign-in logs
* MFA success and failure records
* Authentication method registration reports
* Legacy authentication attempts
* Emergency account alerts
* Exception approvals
* Policy change records
* Quarterly review records

Audit evidence should be retained according to organizational policy.

---

## Testing Requirements

Testing must occur before major policy changes are enabled.

Test cases include:

* Standard user successfully completes MFA.
* User without MFA registration is prompted appropriately.
* Incorrect authentication is denied.
* Privileged user receives stronger authentication requirements.
* Emergency access account behaves according to design.
* Legacy authentication is blocked.
* Excluded service account functions only as documented.
* Unmanaged device receives the intended restriction.
* Policy does not unintentionally block required applications.
* Policy changes appear in audit logs.

Testing should use fictional lab identities and non-production resources.

---

## Failure and Recovery Considerations

The MFA design must account for authentication outages and user recovery needs.

Recovery planning includes:

* Emergency access accounts
* Multiple approved authentication methods
* Lost-device procedures
* Replacement-device procedures
* Help desk escalation
* Temporary Access Pass where approved
* Monitoring of recovery actions
* Documented outage procedures

Recovery mechanisms must not create a permanent MFA bypass.

---

## Policy Review

This policy is reviewed:

* Quarterly
* After significant Microsoft Entra changes
* After authentication incidents
* After audit findings
* After major application deployments
* After changes to approved authentication methods
* After emergency access account use
* When new threats or business requirements emerge

The review confirms that:

* Scope remains appropriate.
* Exclusions remain justified.
* Authentication methods remain secure.
* Privileged access controls remain effective.
* Emergency access accounts remain functional.
* Legacy authentication remains blocked.
* Audit evidence is complete.
* User experience issues have been addressed.

---

## HIPAA Security Rule Alignment

This MFA design supports the organization’s implementation of identity verification and access control safeguards.

Relevant IAM outcomes include:

* Verifying workforce identities before access is granted
* Reducing unauthorized access from compromised passwords
* Protecting privileged and sensitive access
* Recording authentication activity
* Supporting individual accountability
* Enforcing consistent access requirements

This policy contributes to HIPAA-aligned security practices but does not independently establish compliance.

---

## Success Criteria

The MFA implementation is considered effective when:

* All in-scope workforce users are covered.
* Privileged users receive stronger protections.
* Emergency access exclusions are limited and monitored.
* Legacy authentication is blocked.
* Exceptions are documented and time-bound.
* Authentication failures are monitored.
* Users can complete approved recovery processes.
* Policy changes are logged.
* Quarterly reviews are completed.
* No undocumented MFA bypass exists.
