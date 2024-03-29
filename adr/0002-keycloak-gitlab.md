# ADR: Enhanced Integration of Keycloak with GitLab for Authentication and User Management

**Date:** 2024-03-05

**Status:** Proposed

## Context
The initiative to integrate Keycloak with GitLab aims to streamline authentication mechanisms, bolster security measures, and enhance the user experience. This integration is focused on refining the login process, enhancing security protocols, and ensuring compliance with Information Assurance (IA) standards. By leveraging Keycloak as the primary Identity and Access Management (IAM) solution in conjunction with GitLab and adopting SAML for authentication, this approach centralizes user management and authentication, simplifies the management of credentials, and strengthens the overall security framework.

### Authentication
Keycloak will serve as GitLab's primary authentication provider, enabling users to log in using their Keycloak credentials. This will streamline the login process and provide a seamless user experience.

#### OAuth2
**Pros:**
- Standardized and widely supported, facilitating easier integration and adoption.
- Simplifies the authentication flow, improving user experience.
- Provides a secure method for token-based authentication and authorization.

**Cons:**
- May not support all user management and provisioning features natively.

#### SAML Integration
**Pros:**
- Lets you automatically sync user groups between Keycloak and GitLab, making access control more accessible.

**Cons:**
- Setting it up can be complicated, so careful alignment between Keycloak and GitLab is needed.
- To use all features, you might need a higher tier of GitLab, which could mean higher costs.
- If you have a lot of users, you might need to pay for more expensive versions of Keycloak, which could be a bit of a hassle.

### Provision Users
Keycloak will provision users and manage user roles, groups, and permissions. This ensures user access is managed centrally and consistently across all applications and services.

This may be done automatically on the first SSO login by the user or automated (Keycloak data periodically/realtime pushed to GitLab).

Note: Supporting pushed users is helpful to GitLab users, because this allows other users within the GitLab environment to begin interacting with the new user before their first login.

#### Automatic provisioning on first SSO login
**Pros:**
- Common standard SSO integration method
- The new GitLab user account is created on demand, no provisioning delay for the new user to login.

**Cons:**
- New user accounts don't exist in GitLab before the new user's first login. This prevents other users in GitLab from interacting with the new user (assigning tasks, @ mentions, etc) before their first login.

#### SCIM Integration

Note: Using SCIM integration in GitLab requires using SAML for authentication. See [Configure SCIM for self-managed GitLab instances](https://docs.gitlab.com/ee/administration/settings/scim_setup.html) documentation.

**Pros:**
- Easy to Mix Systems: SCIM is a common framework for sharing user info between different programs or online services, making them work smoothly.
- Saves Time: It handles adding, updating, and removing user accounts by itself, which means less work for people managing these tasks.
- Fewer Mistakes: Since it's automated, there's less chance someone will type something wrong or forget to update important info.

**Cons:**
- Keycloak does not natively support SCIM. There are some open source plugins to add support, but they would need review.

#### Manual Provisioning
**Pros:**
- Full control over the user provisioning process.
- Can be implemented without additional development or integration efforts.

**Cons:**
- Time-consuming and not scalable.
- Prone to human error, potentially leading to security and compliance issues.

#### Custom Code
**Pros:**
- Flexibility: You can tailor the provisioning process to your exact needs, ensuring it fits your organization perfectly.
- No exteranl dependency: You can do this with what GitLab already offers, no need for external OSS tools.

**Cons:**
- More Work: You'll have to write and maintain the code yourself, which can be a lot of work.

### Deprovision Users
Users need to be deprovisioned (GitLab user account disabled or deleted) promptly from GitLab when they lose access via Keycloak (Keycloak user account disabled/deleted or removed from a group that gives GitLab access).

Note: it is possible to authenticate to GitLab with Personal Access Tokens (PATs) and SSH keys, these bypass SSO because the user does not login with a browser. It is critical to promptly deactivate or delete users from GitLab to terminate access.

Note: deleted and deactivated users do not consume a license. GitLab bills per user active in the past 30 days(?). To minimize cost, users should be deactivated promptly.

#### Automated Deprovisioning via SCIM

**Pros:**
- Ensures Timely Revocation of Access: Automatically removes users' access rights as soon as their status changes, improving security.
- Reduces Manual Workload: Lessens the burden on IT staff by handling the de-provisioning process automatically, minimizing the chance of oversight.
- Supports Real-Time Updates: It can integrate with broader system architectures to ensure updates are processed immediately, keeping user access rights current.

**Cons:**
- Keycloak does not natively support SCIM. There are some open source plugins to add support, but they would need review.
- Requires Careful Configuration: Setting up SCIM to work correctly with GitLab and Keycloak requires careful planning and configuration.
- Potential for Misconfiguration: Misconfigurations can lead to unauthorized access or the accidental removal of access rights, impacting operations and security.
- Complexity: Integrating SCIM with existing systems can be complex.

#### Manual Deprovisioning
**Pros:**
Direct Control Over the Process: This gives administrators complete oversight of when and how users are deprovisioned, ensuring no automated system can mistakenly alter access rights.
- Simple to Implement: It doesn't require introducing new tools or technologies beyond what's already in use, making it straightforward to execute.

**Cons:**
Labor-Intensive: Each de-provisioning event requires manual action, which can be time-consuming, especially for organizations with a high volume of user changes.
- Prone to Delays: The manual process is susceptible to delays, which can lead to unauthorized access if users without access retain it due to oversight or backlog.
- Not Scalable:

#### Custom Code
**Pros:**
- Flexibility: You can tailor the de-provisioning process to your exact needs, ensuring it fits your organization perfectly.    

**Cons:**
- More Work: You'll have to write and maintain the code yourself, which can be a lot of work.

#### GitLab Group Integration

Within GitLab, groups are often used to control users' access to groups of repositories. Groups can be manually configured by a GitLab group owner, manually by a GitLab instance admin or automatically from SAML groups. GitLab does not currently support SCIM groups.

#### Automated Group Management via Keycloak
**Pros:**
- Easier Access Management: Linking GitLab with Keycloak groups means when someone's group membership changes in Keycloak, it updates in GitLab too. This keeps access rights tight without extra effort.
- Automatic Updates: Group memberships are updated automatically, saving a lot of manual work and ensuring no one is accidentally left in or out of a group.
- Better Security: With automatic updates, you can be sure that everyone has the access they need and nothing more, keeping your projects and data safer.

**Cons:**
- Requires administering groups from Keycloak instead of GitLab. This may be more complex and not all users may have Keycloak group admin access. (However, this may scale better to large numbers of groups.)
- Requires Careful Configuration: Setting up Keycloak to work correctly with GitLab requires careful planning and configuration.
- Learning Curve: more complex to administer groups from Keycloak instead of GitLab. Another admin tool to learn and some complexity from not using GitLab directly.

#### Manual Group Management
**Pros:**
- Simple to set up, likely works well for small GitLab installations
- We are in Charge: Hand-managing groups means you make all the calls about who's in what group, giving you tight control over project access.
**Cons:**
- No integration with Keycloak groups required, less complexity

### Instance Admin Group

#### Automated Assignment via Keycloak Roles
Note: requires using SAML authentication. See [SAML - Administrator groups]
(https://docs.gitlab.com/ee/integration/saml.html#administrator-groups) GitLab documentation.

**Pros:**
- Streamlines the process of granting administrative privileges to designated users.
- Reduces the risk of unauthorized access by tightly controlling the assignment of admin roles.

**Cons:**
- It requires careful configuration to map Keycloak roles accurately to GitLab admin privileges.
- Potential for misconfiguration, leading to unintended access rights.

#### Manual Assignment of Admin Privileges
**Pros:**
- Direct control over who is granted administrative access to the GitLab instance.
- Simple to manage on a small scale without the need for additional integration.

**Cons:**
- Labor-intensive, especially in larger organizations or those with frequent changes in administrative responsibilities.
- Risk of oversight or delay in updating access rights, potentially impacting operations and security.

## Decision
Keycloak will be integrated with GitLab to enhance authentication and user management capabilities. The following decisions have been made:

1. **Authentication:** Use SAML. Select SAML instead of OIDC to support SAML group integration and SCIM in the future.
2. **Provisioning Users:** Automatic provisioning on first SSO login, users will only be created after their first login.  Long term, explore a user provisioning solution for all of UDS such as adding SCIM support to Keycloak.
3. **Deprovisioning Users:**  Short term, use manual deprovisioning and document a procedure for GitLab instance administrators. Long term, explore a user deprovisioning solution for all of UDS such as adding SCIM support to Keycloak.
4. **GitLab Group Integration:** Manual group creation should be supported. Additionally, test SAML Group Sync which should work with Keycloak.
5. **Instance Admin Group:** Manual admin creation should be supported. Additionally, test GitLab SAML Administrator groups which should work with Keycloak.
