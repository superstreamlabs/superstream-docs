# Authentication

We offer multiple authentication methods to ensure secure and seamless access to our platform, catering to a variety of use cases. Whether you're integrating Superstream into an existing enterprise system or setting up individual access, our authentication options provide flexibility without compromising on security. This guide outlines the available authentication methods, including API key authentication, OAuth, and role-based access control (RBAC), to help you choose the best approach for your needs.

### Manage permissions via Active Directory

A custom attribute named **`superstream_role`** must be created to define user permissions within **Superstream.** Currently, the attribute supports the following values:

* **`admin`** – Full access to manage and configure Superstream.
* **`read-only`** – Limited access with view-only permissions.

This attribute should be assigned to all users requiring access to Superstream.&#x20;

This role system will be expanded to support tag-based roles in the future, enabling more granular permission control.

