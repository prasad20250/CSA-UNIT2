1. Objective

This unit focuses on implementing Identity and Access Management (IAM) controls in AWS using groups, least-privilege policies, MFA, and IAM roles.

The objective is to ensure that users and cloud resources receive only the permissions required for their intended tasks.

2. IAM Groups

Three logical IAM groups are used:

Group

Purpose

PBL-Admins

Administrative access

PBL-Developers

Development-related permissions

PBL-Auditors

Read-only audit access

Group-based access control makes permission management easier and supports the principle of least privilege.

3. Admin Access

The administrative user is:

pbl-terraform-admin

The user is a member of:

PBL-Admins

The PBL-Admins group has:

AdministratorAccess

attached for administrative operations.

4. Developer Least-Privilege Policy

Custom policy:

PBL-Developer-LeastPrivilege

This policy is designed to provide only the AWS permissions required for the developer role/tasks defined in the project instead of granting unrestricted administrator access.

5. Auditor Read-Only Policy

Custom policy:

PBL-Auditor-ReadOnly

The Auditor policy provides read-oriented access for reviewing AWS resources without granting unnecessary modification permissions.

This follows the principle of least privilege for auditing activities.

6. EC2 → S3 IAM Role

A dedicated IAM role was created for the EC2 instance:

PBL-EC2-S3-Role

The role uses the EC2 service as its trusted principal:

ec2.amazonaws.com

A corresponding instance profile was created:

PBL-EC2-S3-Instance-Profile

The EC2 instance assumes the role without storing AWS access keys directly on the server.

7. EC2 S3 Least-Privilege Policy

Custom policy:

PBL-EC2-S3-LeastPrivilege

The policy is attached to the EC2 IAM role and is intended to provide only the S3 permissions required by the EC2 workload.

During verification:

aws sts get-caller-identity

confirmed that the EC2 instance was operating through:

PBL-EC2-S3-Role

An unrestricted:

aws s3 ls

returned AccessDenied for s3:ListAllMyBuckets, which demonstrates that the role does not have unrestricted S3 listing permissions.

Bucket-specific access was tested against the project S3 bucket.

8. Admin MFA

The administrative IAM user:

pbl-terraform-admin

has a passkey/security-key MFA device assigned.

The AWS IAM Security Credentials evidence shows:

Multi-factor authentication (MFA)

One MFA device

Passkey/security-key device

Confirmation that the passkey MFA device is assigned

This provides an additional authentication factor for the administrative account.

Evidence note: The supplied screenshot proves MFA device assignment. If the evaluator specifically requires a policy-level rule that denies access when MFA is not present, that separate policy evidence should also be included.

9. IAM Security Design

The Unit 2 implementation demonstrates:

Group-based permission management

Administrative access separation

Developer least-privilege access

Auditor read-only access

EC2 IAM role-based authentication

No hardcoded AWS credentials required by EC2

S3 access restrictions

MFA for the administrative user

10. Evidence

The corresponding Unit 2 evidence PDF contains screenshots and explanations for:

IAM groups

IAM users

Admin group and permissions

Developer least-privilege policy

Auditor read-only policy

EC2 S3 least-privilege policy

EC2 IAM role

EC2 instance profile

Admin MFA configuration

EC2 IAM/S3 permission verification

Recommended GitHub structure:

Unit-2/
├── README.md
└── evidence/
    └── Unit_2_Cloud_Security_PBL.pdf

11. Security Notice

Never upload the following to GitHub:

AWS access keys

AWS secret access keys

Passwords

Private keys

Session credentials

API tokens

Other confidential credentials

Review screenshots before making the repository public and redact sensitive information where necessary.

Conclusion

Unit 2 demonstrates AWS IAM implementation using role-based and group-based access control, least-privilege policies, EC2-to-S3 IAM roles, and MFA for the administrative account.

The configuration reduces unnecessary permissions and avoids hardcoded AWS credentials on the EC2 instance.
