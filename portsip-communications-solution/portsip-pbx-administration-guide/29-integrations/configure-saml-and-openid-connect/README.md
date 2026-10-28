# Configure SAML and OpenID Connect

PortSIP PBX integrates with external identity providers (IdPs) through OpenID Connect (OIDC) and SAML 2.0, allowing users to sign in with their existing organizational credentials. This simplifies access to the PBX and enables organizations to manage authentication through their established identity platform.

This flexibility is particularly valuable for enterprises undergoing mergers and acquisitions, universities and government agencies with separate IT departments, and multinational organizations with independently managed subsidiaries. With identity provider integration configured at the tenant level, each PortSIP PBX tenant can use the identity provider appropriate to its organization.

PortSIP PBX provides the following capabilities:

1. **Choose your federation protocol**\
   Use OpenID Connect (OIDC) or SAML 2.0 to integrate with your organization’s identity provider.
2. **Enable single sign-on**\
   Allow users to access PortSIP PBX using their existing organizational credentials, reducing the need to manage separate PBX passwords.
3. **Configure identity integration per tenant**\
   Configure the identity provider connection and SSO access settings for each tenant to accommodate different organizational requirements.
4. **Synchronize and link user accounts**\
   Connect to a compatible SCIM user directory to synchronize users automatically or manually link external identities to existing PBX users and extensions.

Administrators should also consider access requirements for users outside their organization. SMBs and enterprises may need to authenticate a small number of external users, while larger organizations may already use identity providers that support federation across organizational boundaries. PortSIP PBX administrators should work closely with their identity and security teams to determine how these users will authenticate, which PBX accounts and permissions they require, and whether the proposed configuration meets organizational security and compliance requirements.

This guide uses **Keycloak** and **Shibboleth** as examples, providing detailed, step-by-step instructions for configuring their integration with PortSIP PBX. You can also use this guide as a reference when integrating other identity providers that support **OpenID Connect (OIDC)** or **SAML 2.0**, adapting the configuration to the provider’s specific settings and requirements.
