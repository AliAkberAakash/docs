---
title: Cloud Organization Single Sign-On (SSO)
summary: Learn about Cloud Organization Single Sign-On.
toc: true
docs_area: manage
---

{% include feature-phases/preview.md %}

{% include_cached cockroachcloud/sso-intro.md %}

Cloud Organization SSO allows you to centralize authentication and user provisioning for your {{ site.data.products.db }} organization. When Cloud Organization SSO is enabled, you can optionally disable email and password authentication to require your users to authenticate to {{ site.data.products.db }} using SSO.

This page describes Cloud Organization SSO. To enable and configure it, refer to [Configure Cloud Organization SSO](configure-cloud-org-sso.html).

## Flexibile support for multiple SSO providers and protocols

Support for SSO using Github, Google, and Microsoft is built into {{ site.data.products.db }}, along with email and password authentication. Cloud Organization SSO allows you to selectively enable or disable these authentication methods and to connect to your identity provider using [Security Access Markup Language (SAML)](https://en.wikipedia.org/wiki/Security_Assertion_Markup_Language) and [OpenID Connect (OIDC)](https://openid.net/connect/) identity protocols.

With support for SAML and OIDC, {{ site.data.products.db }} allows enterprise organizations to use a wide variety of self-hosted or SaaS enterprise IdP solutions, such as Okta, Active Directory, Onelogin, etc., to authenticate to the {{ site.data.products.db }} Console.

## Custom sign-in page

When Cloud Organization SSO is enabled, members of your organization log in using a custom sign-in page and choose from the authentication methods you have enabled, rather than the [public sign-in URL](https://cockroachlabs.cloud). You configure the custom sign-in page when you enable Cloud Organization SSO.

## Extended configuration options

With Cloud Organization SSO, you can configure a list of enabled and disabled authentication methods. Only enabled authentication methods display on the [custom sign-in page](#custom-sign-in-page).

## Autoprovisioning

Autoprovisioning allows you to centralize management of your users in an IdP and removes the need to [invite users to your organization](https://www.cockroachlabs.com/docs/cockroachcloud/console-access-management.html#invite-team-members-to-cockroachdb-cloud). When autoprovisioning is enabled, the first time a new user successfully signs in using the [custom sign-in page](#custom-sign-in-page), a {{ site.data.products.db }} account is automatically created for them and and are assigned the [Developer role](/docs/cockroachcloud/console-access-management.html#developer) by default.

Autoprovisioning is optional, and can be configured separately for each enabled authentication method.

## Frequently Asked Questions (FAQ)

### Will it work if I try to sign in with SSO without explicitly setting it up for my account, if I already use an email from an SSO Provider such as Gmail?

Yes, as long as the email address associated with your SSO provider is exactly the same as the one associated with your existing {{ site.data.products.db }} account. After successfully signing in, you will be prompted to approve the updated authentication method for your account.

To view your current authentication method, visit **My Account** in the [{{ site.data.products.db }} Console](https://cockroachlabs.cloud).

### Once I change my active login method to a new SSO provider, can I still sign in using my email/password combination or my GitHub identity?

No. Only one authentication method can be active for each {{ site.data.products.db }} Console user. Visit **My Account** in the [{{ site.data.products.db }} Console](https://cockroachlabs.cloud) to view or update your active authentication method.

### Does this change how administrators invite users?

The [workflow for inviting team members](console-access-management.html#invite-team-members-to-cockroachdb-cloud) to {{ site.data.products.db }} remains the same. However, if Cloud Organization SSO is enabled for your {{ site.data.products.db }} organization and autoprovisioning is enabled for the authentication method they use to sign in, then an account is created automatically after they successfully authenticate.

### As an admin, how do I deprovision a user's access to {{ site.data.products.db }} Console if they leave the relevant project?

If Cloud Organization SSO is enabled, then deprovisioning a user at the level of the IdP also removes their access to the {{ site.data.products.db }} organization.

To remove a user's access to {{ site.data.products.db }} without deprovisioning the user from the IdP (such as when a user changes teams but does not leave the organization entirely), you can [remove their {{ site.data.products.db }} user identity from your {{ site.data.products.db}} organization](console-access-management.html#delete-a-team-member).

### Can admins require a particular authentication method for their {{ site.data.products.db }} organization?

Yes, when Cloud Organization SSO is enabled for your {{ site.data.products.db }} organization, only the authentication methods you have enabled are displayed to your users.

### Which authentication flows are supported with Enterprise Authentication? Is it possible to enable the identity provider initiated flow?

The primary flow is to initiate Cloud Organization SSO through the {{ site.data.products.db }} Console.

If you need to initiate SSO integration from the IdP, contact your account team for assistance.

### What default role is assigned to users when auto-provisioning is enabled in a {{ site.data.products.db }} organization?

The `Developer` role is assigned by default to auto-provisioned users.

## What's next?
- [Configure Cloud Organization SSO](configure-cloud-org-sso.html)
- Learn more about [authenticating to {{ site.data.products.db }}](authentication.html).
