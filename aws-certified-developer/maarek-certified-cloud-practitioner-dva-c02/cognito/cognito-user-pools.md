# Cognito User Pools

### Cognito User Pools (CUP) - User Features

* Create a serverless database of user for your web & mobile apps.
  * What does "serverless database" means in this context?
  * That means that users can use simple login (username and password) to connect into the application.
  * Fetures:
    * Simple login
    * Password rest
    * email & phone verification
    * mfa
    * Federated identities: users from facebook, google, SAML...

<figure><img src="../../../.gitbook/assets/image (281).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (280).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### Cognito User Pools (CUP) - Integrations

#### **API Gateway - Authentication**

<figure><img src="../../../.gitbook/assets/image (282).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### **Aplication Load Balancer - Authentication**

<figure><img src="../../../.gitbook/assets/image (283).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

* Users can be securely authenticated by your Application Load Balancer.
* The work of authenticating users can be offloaded to your load balancer.
* Your applications can be allowed to focus on their business logic.
* Users can be authenticated through:
  * Identity Provider (IdP) that is OpenID Connect (OIDC) compliant.
  * Cognito User Pools:
    * Social IdPs, such as Amazon, Facebook, or Google.
    * Corporate identities using SAML, LDAP, or Microsoft AD.
* An HTTPS listener must be used to set authenticate-oidc & authenticate-cognito rules.
* `OnUnauthenticatedRequest` – authenticate (default), deny, allow options can be set.

#### Cognito Authentication

<figure><img src="../../../.gitbook/assets/image (275).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

* `GET /api/data` will be performed by users.
* The ALB will be set up with HTTPS with the action authenticate-cognito.
* Cognito will authenticate the user.
* The payload and the request will be passed on to Amazon ECS with the added information of the user that was doing the request from Cognito.
* This is very helpful because more information is now available to deal with your application, for example, with Amazon ECS.
  * A specific response can be returned that is based on what the user information is.

#### OIDC User Authentication

<figure><img src="../../../.gitbook/assets/image (274).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

* (1) The client will make a HTTP request.
* (2) The user will be redirected by the ALB for authentication to the **authentication endpoint** of the **identity provider**.
* (3) An **authorization code** will be granted by the identity provider after successful authentication.
  * The authorization code will be passed to the ALB.
* (4) The ALB will send the authorization code to the token endpoint.
* (5) The grant code will be switched and exchanged for an ID token and an access token.
* (6) A request will be made by the ALB to the user info endpoint of the identity provider to exchange the access token for user claims.
* (7) The user claims, which include the user id and user attributes, will be obtained.
* (8) Once the user claims are obtained, the request will be sent to Amazon ECS along with the original request and the user claims.
* (9) The response will be received.
* (10) The response will be passed to the user.

### Cognito User Pools - Hosted Authentitation UI

Amazon Cognito provides a hosted UI that allows for a configurable web interface for user sign in. The hosted UI can be accessed from a domain name that is added to the user pool.

There are two options for adding a domain name to a user pool:

* (1) using an Amazon Cognito domain.
* (2) domain name that you own.

The Cognito Hosted UI is a full **OAuth** server backed by the Cognito API. **It is possible to host your own login page and redirect to your site on successful authentication**.

The hosted UI supports several Amazon Cognito authentication operations:

* sign up as a new user in your app.
* verify an email address or phone number.
* set up multi-factor authentication (MFA).
* sign in with a local username and password.
* sign in with a third-party identity provider (IdP).
* reset a password.

**AWS Console Config**

<figure><img src="../../../.gitbook/assets/image (323).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (324).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (325).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (328).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

#### Hosted UI Custom Domain

* For custom domains, you must create an ACM certificate in us-east-1
* the custom domain must be defined in the "app integration" section

<figure><img src="../../../.gitbook/assets/image (329).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### Adaptative Authentication

* If the login appears suspicious, sign-ins are blocked or MFA is required.
* Each sign-in attempt is examined by Cognito and a risk score (low, medium, high) is generated to determine how likely the sign-in request is from a malicious attacker.
* Only when risk is detected, users are prompted for a second MFA.
* The risk score is based on different factors such as whether the user has used the same **device, location, or IP address**.
* Checks for compromised credentials, account takeover protection, and phone and email verification are performed.
* Integration with CloudWatch Logs is done for sign-in attempts, risk score, failed challenges, etc.

### Cognito User Pools - Lambda Triggers

<figure><img src="../../../.gitbook/assets/image (327).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

<figure><img src="../../../.gitbook/assets/image (326).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

### What is Returned After Log in with a Cognito User Pool?

* :warning::warning: :warning: When a login is made with a Cognito User Pool, a JWT Token or a JSON Web Token is received from the pool.
* These tokens are Base64 encoded.
* What is called the header, the payload, and the signature are included in these tokens.
* Base64 allows these tokens to be transmitted over the network easily.

<figure><img src="../../../.gitbook/assets/image (330).png" alt=""><figcaption><p>Font: MAAREK, 2023</p></figcaption></figure>

* To ensure the JWT can be trusted, the signature must be verified.
* The validity of JWT tokens issued by Cognito User Pools can be verified with the help of libraries.
* The user information (sub UUID, given\_name, email, phone\_number, attributes…) is contained in the Payload.
* All user details from Cognito / OIDC can be retrieved from the sub UUID.
