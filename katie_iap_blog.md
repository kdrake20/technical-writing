# Table of contents

- [Table of contents](#table-of-contents)
  - [What is Google Cloud Identity-Aware Proxy](#what-is-google-cloud-identity-aware-proxy)
  - [How IAP Works](#how-iap-works)
    - [Authentication](#authentication)
    - [Authorization](#authorization)
  - [Securing with Signed Headers](#securing-with-signed-headers)
    - [Additional Resources](#additional-resources)

## What is Google Cloud Identity-Aware Proxy

Google Cloud Identity-Aware Proxy (IAP) intercepts web site requests, authenticating using Google identities or an external identity provider to authorize the user making the request. IAP is configured as a service, allowing only authorized users' requests to reach the site. IAP protects web sites running on platforms including App Engine, Compute Engine, and other services behind a Google Cloud Load Balancer.

IAP isn't restricted to Google Cloud; IAP uses IAP Connector to protect your on-premises applications. While IAP uses Google identities by default, external identity providers can be used with Identity Platform, such as:

- OAuth (GitHub, Microsoft, Facebook, etc.)
- OIDC
- SAML

## How IAP Works

Only users with the correct Identity and Access Management (IAM) role can access resources protected by IAP. In the case of on-premises apps, requests are routed through the IAP Connector, which forwards the request through a site-to-site connection established with Cloud Interconnect from Google Cloud to the on-premises network.

Access policies can be defined centrally and applied to all of the applications and resources, scaling across the organization.

### Authentication

Requests to the Google Cloud resources come through App Engine, Cloud Load Balancing (HTTPS), or internal HTTP load balancing. During authentication, the user is directed to an OAuth 2.0 Google Account sign-in flow (or valid external Identity Provider). Subsequent sign-ins will be enabled using a browser cookie to store the authorization token.

If the requested credentials are valid, the authentication server uses those credentials to get the user's identity (email address and user ID). The authentication server then uses the identity to check the user's IAM role.

### Authorization

After authentication, IAP applies the relevant IAM policy to check if the user is authorized to access the requested resource. When IAP is enabled for a resource, it automatically creates an OAuth 2.0 client ID and secret. If the user is authorized, they are granted access to the requested resource. Fine-grain access controls can be applied using the IAM policy supporting the requested resource.

## Securing with Signed Headers

Identity-Aware Proxy (IAP) can be configured to use JSON Web Tokens (JWT) to authorize requests. Signed headers provide secondary security in instances where IAP is bypassed. Signed headers protect against common occurrences of accidental disabling of IAP and misconfigured firewalls.

IAP JWT provides additional security by verifying the header, payload, and signature of the JWT, which is in the HTTP request header `x-goog-iap-jwt-assertion`. By default, only headers `x-goog-authenticated-user-{email,id}` are passed.

### Additional Resources

- [Identity-Aware Proxy overview](https://cloud.google.com/iap/docs/concepts-overview)
- [Overview of IAP for on-premises apps](https://cloud.google.com/iap/docs/cloud-iap-for-on-prem-apps-overview)
- [Enabling IAP for on-premises apps](https://cloud.google.com/iap/docs/enabling-on-prem-howto)
- [Securing your app with signed headers](https://cloud.google.com/iap/docs/signed-headers-howto)
