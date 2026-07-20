# HIPAA Security Rule Mapping

## Purpose

This document maps the identity and access management controls implemented within the Healthcare IAM Governance Lab to relevant provisions of the HIPAA Security Rule.

The mapping demonstrates how IAM governance supports the confidentiality, integrity, and availability of electronic protected health information (ePHI). It is intended as an educational example rather than a comprehensive HIPAA compliance assessment.

---

# Control Mapping

| HIPAA Citation        | Security Rule Requirement             | IAM Control(s) Implemented                                                                                                                | Evidence                                   |
| --------------------- | ------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------ |
| §164.308(a)(1)(ii)(A) | Risk Analysis                         | Quarterly access reviews identify inappropriate or unnecessary access.                                                                    | Access review reports, remediation records |
| §164.308(a)(3)(ii)(B) | Workforce Clearance Procedure         | Role-based access is assigned according to workforce responsibilities.                                                                    | RBAC model, security group assignments     |
| §164.308(a)(3)(ii)(C) | Termination Procedures                | Automated HR-driven offboarding daemon removes workforce access following termination or disability.                                      | Offboarding audit logs                     |
| §164.308(a)(4)(i)     | Information Access Management         | Access is granted through security groups following least-privilege principles.                                                           | Group design documentation                 |
| §164.308(a)(4)(ii)(B) | Access Authorization                  | Department managers and application owners review and approve access.                                                                     | Quarterly access review evidence           |
| §164.308(a)(4)(ii)(C) | Access Establishment and Modification | Identity lifecycle procedures govern onboarding, role changes, and offboarding.                                                           | Identity lifecycle documentation           |
| §164.312(a)(1)        | Access Control                        | Microsoft Entra ID provides centralized identity and access management using RBAC.                                                        | Entra configuration documentation          |
| §164.312(a)(2)(i)     | Unique User Identification            | Every workforce member receives an individual identity. Shared administrative accounts are prohibited except where specifically approved. | Workforce identity documentation           |
| §164.312(a)(2)(iii)   | Automatic Logoff                      | Session controls may be implemented by healthcare applications and workstation policies outside the scope of this lab.                    | N/A                                        |
| §164.312(a)(2)(iv)    | Encryption and Decryption             | Authentication and access controls complement encryption controls implemented separately.                                                 | Conditional Access documentation           |
| §164.312(b)           | Audit Controls                        | Microsoft Entra audit logs and daemon-generated audit records provide evidence of identity lifecycle events.                              | Audit log samples                          |
| §164.312(d)           | Person or Entity Authentication       | Multi-factor authentication is required through Conditional Access policies.                                                              | Conditional Access policy documentation    |

---

# IAM Controls Included in this Lab

The Healthcare IAM Governance Lab demonstrates the following identity governance controls:

* Role-Based Access Control (RBAC)
* Group-based access assignment
* Least-privilege administration
* Multi-factor authentication
* Conditional Access
* Privileged administrative account separation
* Quarterly access reviews
* Identity lifecycle management
* Automated workforce offboarding
* Audit logging
* Security group governance

---

# Administrative Safeguards

This lab primarily supports the HIPAA Administrative Safeguards through documented governance processes.

Examples include:

* Workforce access reviews
* Role-based authorization
* Identity lifecycle management
* Privileged access governance
* Access approval workflows
* Documentation of exceptions
* Audit evidence retention

---

# Technical Safeguards

The technical implementation demonstrates several safeguards through Microsoft Entra ID and Azure identity services.

Examples include:

* Multi-factor authentication
* Conditional Access
* Individual user identities
* Group-based authorization
* Administrative role separation
* Audit logging
* Automated access removal
* Microsoft Graph automation using application authentication

---

# Physical Safeguards

Physical safeguards are largely outside the scope of this project.

Examples not implemented include:

* Facility access controls
* Workstation security
* Device disposal
* Physical media handling

These controls remain important components of a complete HIPAA compliance program.

---

# Shared Responsibility

This project demonstrates IAM controls that support HIPAA compliance but does not, by itself, make an organization HIPAA compliant.

Compliance depends on the combined effectiveness of:

* Administrative safeguards
* Physical safeguards
* Technical safeguards
* Organizational policies
* Workforce training
* Risk management
* Ongoing monitoring and improvement

---

# Assumptions

This lab assumes:

* Human Resources is the authoritative source for workforce status.
* Microsoft Entra ID is the authoritative identity provider.
* Security groups are used instead of direct permission assignments whenever possible.
* Privileged accounts are separate from standard workforce identities.
* Identity lifecycle processes are documented and consistently followed.
* Quarterly access reviews are completed and documented.
* Audit evidence is retained according to organizational policy.

---

# Limitations

The Healthcare IAM Governance Lab is designed to demonstrate IAM engineering concepts.

The following are intentionally outside the scope of this project:

* Full HIPAA risk analysis
* Electronic health record configuration
* Clinical workflow authorization
* Network security architecture
* Encryption key management
* Business associate agreements
* Incident response procedures
* Disaster recovery planning

---

# Conclusion

Identity and access management is a foundational component of protecting electronic protected health information. The controls implemented throughout this lab demonstrate how governance, automation, and least-privilege access can work together to reduce identity-related risk while supporting the access management objectives of the HIPAA Security Rule.

Although IAM represents only one part of a comprehensive HIPAA compliance program, strong identity governance significantly improves an organization's ability to control, monitor, and audit access to sensitive healthcare systems and data.
