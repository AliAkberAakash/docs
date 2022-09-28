---
title: Configure Cloud Organization Single Sign-On (SSO) (Preview)
summary: Learn how to configure single sign-on (SSO) (Preview) for your CockroachDB Cloud organization.
toc: true
docs_area: manage
---

{% include_cached feature-phases/preview-opt-in.md %}

{% include_cached cockroachcloud/sso-intro.md %}

This page shows how to:
- Configure {{ site.data.products.db }} Organization SSO
- Enable or disable a built-in SSO authentication method
- Configure a custom SSO authentication method using SAML or OIDC
- Update a user's default SSO authentication method


## Configure {{ site.data.products.db }} Organization SSO

This section describes the options for configuring {{ site.data.products.db }} Organization SSO.

1. Log in to [{{ site.data.products.db }} Console](https://cockroachlabs.cloud) as an admin user.
1. Go to **Organization** > **Authentication**.
1. Next to **Enable Authentication**, click **Setup**.
1. Configure the custom page your users will use to sign in, then click **Next**. Refer to [Update the custom login page](#update-the-custom-login-page).
1. The list of default authentication methods displays. By default, **Email** is enabled and **Github**, **Google**, and **Microsoft** are disabled.

   - To enable and configure an authentication method, click its name and fill in the details. Refer to [Configure authentication methods](#configure-authentication-methods).
   - To disable an authentication method, click **Disable**.

     {{site.data.alerts.callout_danger}}
     If you disable Email authentication, users can no longer authenticate to [{{ site.data.products.db }} Console](https://cockroachlabs.cloud) using the passwords that were previously stored for them, and those passwords are no longer retained. Cockroach Labs recommends that you thoroughly test each SSO authentication method that you enable before disabling Email authentication.
     {{site.data.alerts.end}}

     Click **Next**.

1. Select **Enable Cloud Organization SSO**, then click **Enable**.

{{ site.data.products.db }} Organization SSO is enabled, and users can log in using any enabled authentication method.

## Update the custom login page

After you enable Cloud Organization SSO, your users sign in use a custom login page with the authentication methods that you enable, rather than the  [public sign-in URL](https://cockroachlabs.cloud).

To update the custom login URL:

1. Log in to [{{ site.data.products.db }} Console](https://cockroachlabs.cloud) as an admin user.
1. Go to **Organization** > **Authentication**.
1. Set **Vanity Name** to the custom sign-in page. Modify only the portion after the final `/` character.
1. Click **Save**.

## Configure authentication methods

When you enable an authentication method, users can begin authenticating to {{ site.data.products.db }} using that authentication method. A user can log in using any enabled authentication method. When they successfully sign in with a new authentication method for the first time, this becomes their new default method.

When you disable an authentication method, users who are not associated with any other authentication method can no longer sign in and must be provisioned in an identity provider that is associated with an enabled method.

When you enable or disable an authentication method, a notification is displayed with a list of users who must log in with a different authentication method to regain access. Each affected user also receives an email notification prompting them to sign in again.

To configure an authentication method:

1. Log in to [{{ site.data.products.db }} Console](https://cockroachlabs.cloud) as an admin user.
1. Go to **Organization** > **Authentication**.
1. To configure an authentication method, click its name.
1. To enable or disable the authentication method, toggle **Enable**.
1. Optionally, update the authentication method's configuration details, such as its display name, sign-in URL, and signing certificate.
1. Optionally, configure [advanced settings](#configure-advanced-settings) for the authentication method.
1. Click **Save**.

### Configure advanced settings

You can configure the following options separately for each enabled SSO authentication method.

#### Domain Allowlist

By default, users can access your {{ site.data.products.db }} organization from any domain. To limit users to connecting only from specific domains or IP addresses:

1. Log in to [{{ site.data.products.db }} Console](https://cockroachlabs.cloud) as an admin user.
1. Go to **Organization** > **Authentication**.
1. To configure an authentication method, click its name.
1. Click **Advanced Settings**.
1. Next to **Domain Allowlist**, add a comma-separated list of domains in CIDR format. Each domain should begin with a `@`. To ensure that you are not locked out of {{ site.data.products.db }} while you are configuring Cloud SSO, be sure to allow the domain from which you are currently connected to {{ site.data.products.db }}.
1. Click **Save**.

#### Autoprovisioning

By default, autoprovisioning is disabled. In order to log in to {{ site.data.products.db }} using SSO:
- A user's email address must exist in the SSO IdP, **and**
- n admin user must have already added the user to the {{ site.data.products.db }} organization.

When autoprovisioning is enabled for an SSO authentication method, a user who successfully authenticates using that authentication method is automatically added to your {{ site.data.products.db }} organization. Users are assigned the [Developer role](/docs/cockroachcloud/console-access-management.html#developer) by default.

If you disable auto-provisioning, new users will no longer be created automatically, but there is no impact on existing users who have already been auto-provisioned.

To enable auto-provisioning:

1. Log in to [{{ site.data.products.db }} Console](https://cockroachlabs.cloud) as an admin user.
1. Go to **Organization** > **Authentication**.
1. Click the name of an authentication method.
1. Click **Advanced Settings**.
1. Next to **Autoprovisioning**, click **Enable**.

### Add a custom authentication method

By default, SSO using Github, Google, and Microsoft as the authentication method is supported. You can add a custom authentication method to connect to any IdP that supports OIDC or SAML, such as Okta or OneLogin.

1. Log in to your IdP and gather the following information, which you will use to configure {{ site.data.products.db }} SSO:

   - Sign-in URL
   - Signing certificate

1. In a separate browser, log in to [{{ site.data.products.db }} Console](https://cockroachlabs.cloud) as an admin user.
1. Go to **Organization** > **Authentication**.
1. Next to **Authentication Methods**, click **Add**.
1. Set **Configuration** to either **OIDC** or **SAML**.
1. Set **Provider Name** to a display name for the connection.
1. Click **Next**.
1. The **Provider Details** page displays.
1. To edit the connection, click **Edit**.
1. Set **Sign-in URL** to the sign-in URL from your IdP.
1. Next to **Signing Certificate**, paste the entire certificate from your IdP, including the lines `-begin certificate-` and `-end certificate`.
1. Click **Save**.
1. Click **Test**. If errors are shown, edit the configuration to fix the problems and try again.
1. Optionally, [configure advanced settings](#configure-advanced-settings) for the new authentication method.

#### OIDC

To configure a custom OIDC authentication method:

1. Log in to your IdP and gather the following information, which you will use to configure {{ site.data.products.db }} SSO:

   - Issuer URL
   - Client ID
   - Client secret
   - Callback URL

1. In a separate browser, log in to [{{ site.data.products.db }} Console](https://cockroachlabs.cloud) as an admin user.
1. Go to **Organization** > **Authentication**.
1. Next to **Authentication Methods**, click **Add**.
1. Set **Configuration** to **OIDC (OpenID Connect)**.
1. Set **Provider Name** to a display name for the connection.
1. Click **Next**.
1. The **Provider Details** page displays.
1. To edit the connection, click **Edit**.
1. Paste the values from your IdP for the **Issuer URL**, **Client ID**, **Client Secret**, and **Callback URL**.
1. Click **Save**.
1. The authentication method has been added but is disabled. To enable it, toggle **Enable**.
1. Click **Test**. If errors are shown, edit the configuration to fix the problems and try again.
1. Optionally, [configure advanced settings](#configure-advanced-settings) for the new authentication method.

#### SAML

To configure a custom SAML authentication method:

1. Log in to your IdP and gather the following information, which you will use to configure {{ site.data.products.db }} SSO:

   - Sign-in URL
   - Signing certificate

1. In a separate browser, log in to [{{ site.data.products.db }} Console](https://cockroachlabs.cloud) as an admin user.
1. Go to **Organization** > **Authentication**.
1. Next to **Authentication Methods**, click **Add**.
1. Set **Configuration** to **SAML**.
1. Set **Provider Name** to a display name for the connection.
1. Click **Next**.
1. The **Provider Details** page displays.
1. To edit the connection, click **Edit**.
1. Set **Sign-in URL** to the sign-in URL from your IdP.
1. Next to **Signing Certificate**, paste the entire certificate from your IdP, including the lines `-begin certificate-` and `-end certificate`.
1. Click **Save**.
1. Click **Test**. If errors are shown, edit the configuration to fix the problems and try again.
1. Optionally, [configure advanced settings](#configure-advanced-settings) for the new authentication method.
1. Download metadata required by your IdP. Click **Download**. Open the file and make a note of the **Entity ID** and **Login URL**.
1. In the browser where you are logged in to your IdP, update the authentication configuration to use the Entity ID and Login URL from the metadata file.

## Test an authentication method

To test an authentication method:

1. Log in to [{{ site.data.products.db }} Console](https://cockroachlabs.cloud) as an admin user.
1. Go to **Organization** > **Authentication**.
1. Click the name of an authentication method.
1. Click **Test**.

## What's next?

- Read the [Cloud Org SSO Frequently Asked Questions](cloud-org-sso.html#frequently-asked-questions-faq).
- Learn more about [authenticating to {{ site.data.products.db }}](authentication.html).
