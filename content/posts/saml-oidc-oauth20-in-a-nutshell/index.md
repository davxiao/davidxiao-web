---
title: "SAML, OIDC and OAuth 2.0 in a Nutshell"
date: 2022-08-31
draft: false
---

# SAML and OIDC

[What is the Real Difference Between SAML and OIDC](https://www.onelogin.com/blog/real-difference-saml-oidc)

> Both SAML and OIDC are authentication protocols and some times were referred to as identity protocols.
> 
> The basic login flow for both is the same.
> 
> - A user logs in to the Identity Provider.
> - The user selects which app to access.
> - The user’s information is passed from the IdP to the user’s browser or other endpoint.
> - The endpoint passes the information on to the application.
> - The application confirms they are authorized to access the resources.
> - The user is allowed into the application.
> 
> OIDC is built off of the OAuth 2.0 protocol. Whereas OAuth 2.0 is used to set up so that two applications such as two websites can trust each other and send data back and forth, OIDC works at the individual or user level.
> 
> In comparison to SAML, OIDC login flows work in the same way. But, there are three main differences:
> 
> - SAML transmits user data in XML format. OIDC transmits user data in JSON format.
> - SAML calls the user data it sends a SAML Assertion. OIDC calls the data Claims.
> - SAML calls the application or system the user is trying to get into the Service Provider. OIDC calls it the Relying Party.
> 
> OIDC is gaining in popularity. It is considered simpler to implement than SAML and easily accessible through APIs because it works with RESTful API endpoints. This also means it works better with mobile applications.

# OAuth 2.0
[Introduction to Identity - What is OAuth 2.0](https://auth0.com/intro-to-iam/what-is-oauth-2/)
> OAuth 2.0 is an authorization protocol and not an authentication protocol. As such, it is designed primarily as a means of granting access to a set of resources, for example, remote APIs or user’s data.
> 
> OAuth 2.0 uses Access Tokens. An Access Token is a piece of data that represents the authorization to access resources on behalf of the end-user. OAuth 2.0 doesn’t define a specific format for Access Tokens. However, in some contexts, the JSON Web Token (JWT) format is often used. This enables token issuers to include data in the token itself. Also, for security reasons, Access Tokens may have an expiration date.
