---
title: Single sign-on (SSO)
product_label:
  - advanced
  - enterprise
description: Learn how to set up and use SSO in injixo.
---

In this article, you will learn:

- what SSO is.
- about the requirements for using SSO.
- how to activate SSO for your injixo account.
- how to test SSO with your current user.
- how to log in with SSO.
- how to enforce SSO for all users.
- how to revoke a user's access.
- how to deactivate SSO and switch back to email and password based login.

## What is SSO?

SSO is an authentication mechanism that allows users to log in to multiple applications and websites with just one set of credentials. A trust relationship is set up between a central identity management service (identity provider/IdP) and an application (service provider/SP), in this case injixo.

The identity provider is a third-party product like Microsoft Azure AD or Okta. The user authenticates against the identity provider which redirects them to the application of choice. The main benefits of SSO are aligned password services and security policies such as two-factor authentication and password rotation across all connected applications.

Note: The current implementation has been tested with Microsoft Azure AD, Okta, and Google. You can integrate other IdPs on your own but there is no guarantee that they will work. Please contact your Customer Success team if you have trouble using any other IdP.

## Requirements

The following requirements must be met in order to integrate injixo into the identity provider of choice:

- SAML 2.0 protocol support
- Federation metadata URL access via web
- The email address registered in injixo and your IdP must be connected to a mailbox

> To improve security, activate encrypted assertions/token encryption (strongly recommended).

## Activate SSO for your account

1. Register injixo as a new SAML or SSO application in your IdP portal. Next, continue in injixo.

2. As a user with _Admin access_, go to _Account > Security_{:.breadcrumbs}. Configure SSO under _Single Sign-On_ in the section _1. Configuration_.

3. Download the injixo SAML Metadata file to complete the SAML configuration on IdP side.
   {{ 1 | image: 'Metadata file download option', '60%' }}
   If your IdP does not support a configuration file upload, you will also see the service provider details in this section.

4. Optional: Download [this injixo logo](/assets/img/common/injixo-logo.png) and insert it into the configuration of your new web application on your IdP.

5. Once the IdP configuration is set up, copy your Federation Metadata URL from the details section of your IdP application configuration. This metadata URL is uniquely created for your registered application.

   Your IdP may not be able to provide a federation metadata URL. In this case, you have to download the Federation Metadata XML file and host it yourself. As an example, learn how to set up your own custom [SAML application with Google](https://support.google.com/a/answer/6087519?hl=en).

6. In the section _Identity Provider_, enter the **URL** into the field **Federation Metadata URL**
   {{ 2 | image: 'Input field for yourIdP Federation Metadata URL', '60%' }}

7. Optional: In the section _Token Encryption_, click _Download_{:.doc-button} to download the **injixo certificate**, which you can upload to your IdP to activate encrypted assertions (or token encryption) in the encryption section of your IdP application.

SSO is now active but all users can still log in with their username and password.

{{ 3 | image: 'Option to download the injixo certificate for encrypted assertions', '60%' }}

> Deactivate the login via username and password after testing.
>
> To ensure a higher level of security, [enforce SSO](#enforce-sso-for-all-users).

## Test the SSO configuration

Click _Test configuration_{:.doc-button} to test the login via the IdP. You are redirected to your IdP, where you have to login. If the configuration is correct, you will be redirected back to injixo. The successful test is required to enforce SSO in the next step.

{{ 4 | image: 'Successfully testing the SSO configuration for the current user', '80%' }}

## Enforce SSO for all users

Deactivate all logins with username and password by enforcing SSO for all users. However, you need to make sure that:

- all injixo users exist in the IdP and in injixo.
- the injixo email matches the appropriate IdP identifier.
- all injixo users are assigned to the IdP injixo SSO application.

Once SSO is enforced, it is no longer possible to:

- login using username and password (all existing passwords will be invalidated).
- reset passwords inside injixo (neither by user nor by admin).
- manage access to injixo outside the IdP.

Enforce SSO in the section _3. Enforcement_. The button _Enforce_{:.doc-button} is selectable after you have tested your SSO configuration successfully.

{{ 5 | image: 'Button to enforce SSO for all users', '80%' }}

## Change email addresses when SSO is enforced

When you enforce SSO, users cannot change their email addresses themselves because the injixo email address needs to match the appropriate IdP identifier.

Only admin users can change email addresses in injixo.

## Log in using SSO

When SSO is set up, your users can log in via [https://www.injixo.com/login/sso](https://www.injixo.com/login/sso). They need to enter their email address and are redirected to the identity provider login screen. If users are already logged in, they will be automatically redirected back to injixo. Otherwise, they need to enter the IdP password.

## Revoke access to injixo

To revoke a user's access to injixo via SSO, you must delete the injixo user assignment in the IdP. Note: If the user still exists in injixo, they will still be billed. To avoid costs, you must also {% link_new deactivate or remove the user | features/administration/deactivate-employees.md %}.

## Deactivate SSO

To deactivate SSO and allow logins with username and password again, users with the _Admin_ role can deactivate SSO. This will delete the IdP connection and all entered configuration details. After SSO has been deactivated, all active users receive an email to set a new injixo password. Afterwards, logging in with user name and password via [https://www.injixo.com/login](https://www.injixo.com/login) is possible again.
