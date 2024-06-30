# Azure Identity

## Basic

**OAuth 2.0** is an authorization framework that allows applications to access resources on behalf of a user.
**Microsoft Identity platform** is built on OAuth 2.0 and OpenID Connect standards.
**MSAL** is a library that helps you implement authentication and authorization in your app using Microsoft Identity.

##  Azure Identity authentication flow

**1. User Authentication**

- **User Initiates Login**: The user attempts to log in to an application (e.g., a web or mobile app).

- **Redirect to Authorization Endpoint**: The application redirects the user to the Azure Active Directory (Azure AD) authorization endpoint, typically with an OpenID Connect (OIDC) authentication request.

**2. Authorization and Token Issuance**

-**User Authentication**: Azure AD prompts the user to authenticate (e.g., by entering credentials, using multi-factor authentication, etc.).

- **Authorization Code**: After successful authentication, Azure AD redirects the user back to the application with an authorization code.

- **Token Request**: The application sends a token request to the Azure AD token endpoint, including the authorization code, client ID, client secret, and redirect URI.

**3. Receiving Tokens**

- **Token Response**: Azure AD responds with an ID token, an access token, and optionally a refresh token.

**4. Accessing Resources**

- **Accessing APIs**: The application uses the access token to call protected APIs. The access token is included in the Authorization header of API requests as a Bearer token.

```http
GET /resource
Authorization: Bearer <access_token>
````

## Differences between access tokens, ID tokens, and refresh tokens

**1- Access Token**:

- **Purpose**: Used for authorization to access protected resources such as APIs.
- **Content** Contains information about the client and granted permissions (scopes) like user's roles, permission that resource server can use to verify the token
- **Format** Usually a JSON Web Token (JWT)
- **Lifespan** Short-lived (typically 1 hour or less)
- **Usage** Sent with requests to APIs or resources as a Bearer token whe making API requests (Sent in the HTTP Authorization header as a Bearer token)


**2- ID Token**

- **Purpose** Used for authentication to verify the user's identity
- **Content** Contains claims about the user (e.g., name, email, etc.)
- **Format** Always a JSON Web Token (JWT)
- **Lifespan** Short-lived (similar to access tokens)
- **Usage** Used by the client application to know who the user is (Primarily used in OpenID Connect (OIDC) scenarios to authenticate users in web and mobile applications. They are returned to the client application after a successful authentication.)


**3- Refresh Token**

- **Purpose** Used to obtain new access tokens without re-authenticating the user
- **Content** Opaque string, not intended to be parsed by the client
- **Format** Usually not a JWT, often a random string
- **Lifespan** Long-lived (can be days, weeks, or months)
- **Usage** Sent to the authorization server to get new access tokens

## Difference between OAuth 2.0 and OpenID Connect

**OAuth 2.0:**

Purpose: Authorization
OAuth 2.0 is primarily an authorization protocol.
It allows an application to obtain limited access to a user's resources on another system without sharing the user's credentials.
It provides access tokens to third-party applications with the user's approval.
OAuth 2.0 doesn't provide any information about the user (authentication).
Example use case: Allowing a third-party app to post on your behalf on a social media platform.


**OpenID Connect (OIDC):**

Purpose: Authentication
OpenID Connect is an identity layer built on top of OAuth 2.0.
It extends OAuth 2.0 to add authentication.
OIDC allows clients to verify the identity of the end-user and to obtain basic profile information.
It provides an ID token in addition to the access token.
The ID token contains claims about the authentication of an end-user.
Example use case: Allowing users to log into a website using their Google or Facebook account.

Key Differences:
Purpose:
OAuth 2.0 is for authorization (granting access to resources).
OpenID Connect is for authentication (verifying user identity).

Tokens:
OAuth 2.0 provides an access token.
OpenID Connect provides both an access token and an ID token.

Scope:
OAuth 2.0 defines scopes for accessing resources.
OpenID Connect introduces standard scopes like 'profile', 'email', etc., for user information.

User Information:
OAuth 2.0 doesn't standardize how to get user information.
OpenID Connect standardizes claims about the user and how to get them.


Complexity:
OAuth 2.0 is simpler but less standardized for authentication scenarios.
OpenID Connect adds more structure but also more complexity.


In practice, many modern implementations use both protocols together. OpenID Connect is used for authenticating the user, while OAuth 2.0 is used for authorizing access to specific resources. This combination provides a comprehensive solution for both authentication and authorization in web and mobile applications.

