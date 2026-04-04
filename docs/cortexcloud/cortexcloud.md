# Cortex Cloud

Official documentation: [Get started with Cortex Cloud](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Get-started-with-Cortex-Cloud)

## Authentication

### SSO

Documentation: [Authenticate users using SSO](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Posture-Management-Documentation/Authenticate-users-using-SSO)

**SAML Group Mapping:** Maps SAML groups (e.g. Okta, Azure AD) to Cortex Cloud user groups. For Azure AD, use the group GUID, not the display name.

**SSO Only:** If you want to restrict the user login through SSO only, remove any direct role and user group mapping for the user on Cortex Gateway or the Cortex Cloud tenant. This removes Customer Support Portal access for the user. You then need to ensure that you add the SAML group mapping. The user can access and acquire the user group and roles based on SAML group mapping. Once completed, the user is able to access Cortex Cloud using SSO only and will not be able to use Customer Support Portal login method.

#### Azure Entra ID

Documentation: [Set up Azure AD as the Identity Provider Using SAML 2.0](https://docs-cortex.paloaltonetworks.com/r/Cortex-CLOUD/Cortex-Cloud-Runtime-Security-Documentation/Set-up-Azure-AD-as-the-Identity-Provider-Using-SAML-2.0)

Step-by-step integration (video): [https://www.youtube.com/watch?v=nwF3hY3wgc0](https://www.youtube.com/watch?v=nwF3hY3wgc0)

Within Azure AD, assign users to security groups that match the user groups they will belong to in Cortex Cloud. Users can be assigned to multiple Azure AD groups and receive permissions associated with multiple user groups in Cortex Cloud.

**Group naming convention:** Use an identifying word or phrase, such as `Cortex Cloud`, within the group names (e.g. `Cortex Cloud Analysts`). This allows you to filter and send only the relevant group information to Cortex Cloud via the group attribute statement, avoiding sending unrelated groups that could cause noise or permission issues.

!!! note
    When copying the Single Sign-On URL value, remove `idp/saml` and leave the trailing `/`.

!!! note
    If users get error `AADSTS50105` when signing in, the Enterprise Application in Azure AD has **"Assignment required"** enabled. Disable it under Enterprise Applications → Cortex Cloud SSO → Properties → **Assignment required? → No**. This setting is not covered in the Cortex Cloud documentation.