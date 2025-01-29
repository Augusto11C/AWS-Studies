# Cognito

* If you are asking yourself, "Are users already present in IAM?", the answer is yes, they are.
  * Your web and mobile application users will be handled by Cognito, which is located outside of AWS.
  * Keywords to look for in the exam include "hundreds of users," "mobile users," and "authentication."
  * Mechanisms like SAML will be used for authentication.

### Cognito User Pools vs. Cognito Identity Pools

**Cognito User Pools (Authentication = Identity Verification)**

* Acts as a user directory, creating a database of users for your web and mobile application.
* Facilitates federated logins through Public Social providers, OpenID Connect (OIDC), and Security Assertion Markup Language (SAML).
* Provides the ability to customize the hosted UI for authentication, including the logo, for a personalized user experience.
* Incorporates triggers with AWS Lambda during the authentication flow, allowing for custom workflows.
* Enables the adaptation of the sign-in experience to different risk levels through features like Multi-Factor Authentication (MFA) and adaptive authentication.

**Cognito Identity Pools (Authorization = Access Control)**

* Enables your application to obtain AWS credentials for your users, granting them access to AWS services.
* Supports user login through Public Social providers, OIDC, SAML, and Cognito User Pools.
* Accommodates unauthenticated users or “guests”, providing limited access to resources.
* Maps users to IAM roles and policies, with the ability to leverage policy variables for granular access control.

When combined (CUP + CIP), they provide a comprehensive solution for both authentication (verifying identity) and authorization (granting access).
