# ADR: Enhanced Integration of Keycloak with GitLab for Authentication and User Management

**Date:** 2024-03-05

**Status:** Proposed

## Context
The initiative to integrate Keycloak with GitLab is designed to streamline authentication mechanisms, enhance security, and improve user experience. This integration aims to refine login processes, bolster security protocols, and ensure compliance with Information Assurance (IA) standards. Utilizing Keycloak as the primary Identity and Access Management (IAM) solution alongside GitLab and leveraging OAuth2 for authentication consolidates user management and authentication, simplifies credential management, and enhances the security landscape.

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

#### SCIM Integration
**Pros:**
- Easy to Mix Systems: SCIM is a common framework for sharing user info between different programs or online services, making them work smoothly.
- Saves Time: It handles adding, updating, and removing user accounts by itself, which means less work for people managing these tasks.
- Fewer Mistakes: Since it's automated, there's less chance someone will type something wrong or forget to update important info.

**Cons:**
- Can Be Tricky to Set Up: Getting SCIM to work just right with all your systems can be complicated.
- Learning Curve: If you're not familiar with SCIM, it might take a while to get the hang of it.

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
- No exteranl depdency: You can do this with what GitLab already offers, no need for external OSS tools.

**Cons:**
- More Work: You'll have to write and maintain the code yourself, which can be a lot of work.

### Deprovision Users
Keycloak will be used to de-provision users and manage user access rights, ensuring timely revocation of access when users leave the organization or change roles.

#### Automated Deprovisioning via SCIM
**Pros:**
- Ensures Timely Revocation of Access: Automatically removes users' access rights as soon as their status changes, improving security.
- Reduces Manual Workload: Lessens the burden on IT staff by handling the de-provisioning process automatically, minimizing the chance of oversight.
Supports Real-Time Updates: It can integrate with broader system architectures to ensure updates are processed immediately, keeping user access rights current.

**Cons:**
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

#### Automated Group Management via Keycloak
**Pros:**
- Easier Access Management: Linking GitLab with Keycloak groups means when someone's group membership changes in Keycloak, it updates in GitLab too. This keeps access rights tight without extra effort.
- Automatic Updates: Group memberships are updated automatically, saving a lot of manual work and ensuring no one is accidentally left in or out of a group.
- Better Security: With automatic updates, you can be sure that everyone has the access they need and nothing more, keeping your projects and data safer.

**Cons:**
- Requires Careful Configuration: Setting up Keycloak to work correctly with GitLab requires careful planning and configuration.
- Learning Curve: 
#### Manual Group Management
**Pros:**
You're in Charge: Hand-managing groups means you make all the calls about who's in what group, giving you tight control over project access.

**Cons:**
- Takes More Time: If you're doing this for a lot of people or if groups change often, it can consume a lot of time.
- Easy to Make Mistakes: When juggling many group memberships, it's easy to miss something or mess up, which can lead to security slip-ups.

### Instance Admin Group

#### Automated Assignment via Keycloak Roles
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

**_TBD_**
1. **Authentication:** 
2. **Provisioning Users:** 
3. **Deprovisioning Users:**  
4. **GitLab Group Integration:** 
5. **Instance Admin Group:** 
