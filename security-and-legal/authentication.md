# Authentication

Superstream provides flexible and secure authentication methods to suit teams of all sizes and access models.

#### 🔐 Native Authentication with RBAC and Tag-Based Permissions

All users can authenticate using Superstream's native login system. Access is controlled using:

* **RBAC Roles**:
  * `admin`: Full access to manage, configure, and automate.
  * `read-only`: View-only access without permission to modify settings.
* **Tag-Based Permissions**:
  * Assign granular permissions by associating users with resource tags (e.g. team, environment, service).
  * Enables scoped visibility and control across large organizations.

This system ensures users only see and interact with resources relevant to their role or team.

#### 🔐 Single Sign-On (SSO) via Active Directory

Superstream supports SSO integration for enterprise customers using Active Directory.

* SSO is available upon request—please contact our team to get started.
* A custom user attribute named `superstream_role` must be defined to assign user permissions (`admin` or `read-only`).
* Future support will include tag-based roles via directory attributes.

For detailed guidance on setting up SSO with Active Directory, please reach out to support.

