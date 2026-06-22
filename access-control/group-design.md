# Security Group Design

## Design Objectives

Northstar Health Services uses security groups to simplify access management and support least-privilege access.

Access is assigned to groups rather than directly to users.

Benefits include:

- Simplified onboarding
- Consistent access assignment
- Easier auditing
- Reduced privilege creep
- Improved access reviews

---

## Workforce Security Groups

| Group Name | Purpose |
|------------|----------|
| HIM-Records-Readers | Read-only access to medical records systems |
| RevenueCycle-Billing | Billing and revenue applications |
| ClinicalApp-Support | Clinical application support |
| ITServiceDesk-PasswordReset | Password reset permissions |
| SecurityOps-Readers | Security monitoring and log review |
| CloudOps-Contributors | Cloud resource administration |
| Compliance-Auditors | Audit and reporting access |

---

## Administrative Security Groups

| Group Name | Purpose |
|------------|----------|
| IAM-Eligible-Admins | Privileged administrative roles |
| IAM-IdentityAdmins | Identity administration |
| IAM-SecurityAdmins | Security administration |
| IAM-BreakGlass | Emergency access accounts |

---

## Group Assignment Principles

- Access is granted through group membership.
- Administrative privileges require separate accounts.
- Privileged roles are periodically reviewed.
- Access reviews occur quarterly.
- Emergency access accounts are monitored and excluded from Conditional Access policies.

---

## Naming Standard

Group names follow the format:

Department-Function-AccessLevel

Examples:

- HIM-Records-Readers
- SecurityOps-Readers
- CloudOps-Contributors