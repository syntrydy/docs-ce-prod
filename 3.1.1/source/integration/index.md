# Single Sign-On (SSO) Integration Guide
The SSO integration guide will help you understand how to use SAML or OpenID Conenct to secure and integrate custom developed, open source, off-the-shelf, and SaaS web and mobile applications with your Gluu Server. 

This list of integration methods is not exhaustive. However, the strategies described below have been tested with the Gluu Server and are well understood by our support and development staff.  

## I- Server Side Web Apps
Many applications are "server-side", meaning the web page displays content but most of the dynamic business logic resides on the web server. Two design patterns have emerged for securing server-side web applications: 

1. Use of web server filters and reverse proxies;            
2. Leveraging OAuth2 directly in your application. 

Which approach to use depends on whether you prefer easier devops (option 1), or a tighter integration with centralized security policies into your application (option 2).

#### A- Web Server Filters
Web Server filters are a tried and true approach to achieving SSO with web applications. The web server filter enforces the presence of a token in a HTTP Request. If no token is present, the Web server may re-direct the person, or return a meaningful code or message to the application. Your devops team will love this approach–they can just manage the web server configuration files. It will be crystal clear to them what policies apply to what URLs. 

We recommend the following SAML and OpenID Connect web server filters: 
  
- SAML: [Shibboleth SP](./sswebapps/saml-sp.md)     

- OpenID Connect: [Apache mod_auth_openidc](./sswebapps/openidc-rp.md), [Nginx lua-resty-openidc](https://github.com/zmartzone/lua-resty-openidc)

#### B- Client Software 
Client software performs some of the heavy lifting for developers around leveraging OAuth 2.0 directly in their applications. Calling the API’s directly will enable “smarter” handling of authentication in your applications. For example, transaction level security can be more easily implemented by calling the OAuth 2.0 APIs directly. This can have a positive impact on usability. Also, giving developers more ability to leverage centralized policies, increases re-use of policies, and ultimately results in better security. 

We recommend the following client software to implement OpenID Connect in server-side web applications:

- [oxd](https://gluu.org/docs/oxd)

## II- Single Page Apps
Single Page Applications (SPAs) can be seen as a mix between traditional Web SSO and API access: the application itself is loaded from a (possibly protected) regular website and the Javascript code starts calling APIs in another (or the same) domain(s). For this use case, the OpenID Connect spec points to using the “Implicit grant type” for passing token(s) to the SPA since that grant was specifically designed with the “in-browser client” in mind. 

We recommend the following client software to implement OpenID Connect in SPA’s:

- [AppAuth JS](https://github.com/openid/AppAuth-JS/)
- [Gluu's OIDC JS Client](./spa/oauth-js-implicit.md)


## III- Native Apps
To integrate native apps with your Gluu Server, we recommend the AppAuth libraries for iOS , MacOS, and Android. AppAuth strives to directly map the requests and responses of those specifications, while following the idiomatic style of the implementation language. In addition to mapping the raw protocol flows, convenience methods are available to assist with common tasks like performing an action with fresh tokens.

- [AppAuth iOS and macOS](https://github.com/openid/AppAuth-iOS)
- [AppAuth Android](https://github.com/openid/AppAuth-Android)

## IV- SaaS Applications 
Integrating SaaS applications with your Gluu Server is fairly straightforward. Presumably the app already supports SAML or OpenID Connect and provides documentation for configuring your IDP (Gluu Server) for SSO. We have documentation for configuring the Gluu Server for SSO to a few popular SaaS applications like Google and Salesforce. If we do not have a guide for configuring the app in question, simply perform a Goolge search for `<SaaS Provider> SAML` or `<SaaS Provider> OpenID Connect`. Follow the provider's instructions for configuring your IDP for SSO and test (and re-test!). 

!!!Note
    If the SaaS application in question does not already support SAML or OpenID Connect, our best advice is find a similar product or provider that does integrate with your standards-based security infrastructure. 



