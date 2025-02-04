---
title: GoogleApps authentication
description: Octopus Deploy can use GoogleApps authentication to identify users.
position: 15
---

To use GoogleApps authentication with Octopus Server, GoogleApps must be configured to trust Octopus - by setting it up as an app. This section covers the details on how to configure the app.

## Configure GoogleApps {#GoogleAppsauthentication-ConfiguringGoogleApps}

### Configure an app {#GoogleAppsauthentication-SetupanApp}

To configure an app within GoogleApps, you must have a Developer account at [https://developers.google.com](https://developers.google.com).  This account will own the app configuration, so we recommend you create an account for company use, rather than using an individual account.

Once you have an account, log in to [https://console.developers.google.com](https://console.developers.google.com) and the following actions:

1. Create a project for Octopus (this might take a minute or so) and then within that project
2. Select **{{API Manager,Credentials menu}}**.
3. Set the **OAuth consent screen** information. In the **Application Logo** box, upload a logo to make it easy to identify the application. You can download the Octopus logo [here](https://octopus.com/images/company/Logo-Blue_140px_rgb.png "width=500").
4. Add the Top Level Domain of your Octopus instance to the **Authorized domains** list. If you are setting Google Apps up for **Hosted Octopus** this will be `octopus.app` or `octopus.com`.
5. Select the Credentials tab and Create a new **OAuth client ID** for a **Web app**.
6. Enter a **Name** for identification, e.g. Octopus.  This is the name that will appear when the user is asked to allow access to their details.
7. Add `https://octopus.example.com/api/users/authenticatedToken/GoogleApps` (replacing `https://octopus.example.com` with the url of your Octopus Server) to the **Authorized Redirect URIs**.

:::hint
**Tips:**
- **Reply URLs are case-sensitive** - Be aware that the path in this URL after the domain name was **case sensitive** during our testing.
- **Not using SSL?** - We highly recommend using SSL, but we know its not always possible. You can use `http` if you do not have SSL enabled on your Octopus Server. Please beware of the security implications in accepting a security token over an insecure channel.
Octopus integrates with [Let's Encrypt](/docs/security/exposing-octopus/lets-encrypt-integration.md) making it easier to setup SSL on your Octopus Server.
:::

## Configure Octopus Server {#GoogleAppsauthentication-ConfiguringOctopusDeployServer}

You can configure the GoogleApps settings from the command line. You will need the **Client ID** from the Credentials tab and your **hosted domain name**.

Once you have those values, run the following from a command prompt in the folder where you installed Octopus Server:

```powershell
Octopus.Server.exe configure --googleAppsIsEnabled=true --googleAppsClientId=ClientID --googleAppsHostedDomain=yourdomain.com
```

Alternatively these settings can be defined through the user interface by selecting **{{Configuration,Settings,GoogleApps}}** and populating the fields `Hosted Domain`, `Client Id` and `Is Enabled`.

![Settings](images/google.png "width=500")

### Octopus user accounts are still required {#GoogleAppsauthentication-Octopususeraccountsarestillrequired}

Even if you are using an external identity provider, Octopus still requires a [user account](/docs/security/users-and-teams/index.md) so you can assign those people to Octopus teams and subsequently grant permissions to Octopus resources. Octopus will automatically create a [user account](/docs/security/users-and-teams/index.md) based on the profile information returned in the security token, which includes an **Identifier**, **Name**, and **Email Address**.

**How Octopus matches external identities to user accounts**
When the security token is returned from the external identity provider, Octopus looks for a user account with a **matching Identifier**. If there is no match, Octopus looks for a user account with a **matching Email Address**. If a user account is found, the External Identifier will be added to the user account for next time. If a user account is not found, Octopus will create one using the profile information in the security token.

:::success
**Already have Octopus user accounts?**
If you already have Octopus user accounts and you want to enable external authentication, simply make sure the Email Address matches in both Octopus and the external identity provider. This means your existing users will be able to sign in using an external identity provider and still belong to the same teams in Octopus.
:::

### Getting permissions

!include <admin-user>

## Troubleshooting {#GoogleAppsauthentication-Troubleshooting}

We do our best to log warnings to your Octopus Server log whenever possible. If you are having difficulty configuring Octopus to authenticate with GoogleApps, be sure to check your [server logs](/docs/support/log-files.md) for warnings.

### Double and triple check your configuration {#GoogleAppsauthentication-DoubleandTriplecheckyourconfiguration}

Unfortunately security-related configuration is sensitive to everything. Make sure:

- You don't have any typos or copy-paste errors.
- Remember things are case-sensitive.
- Remember to remove or add slash characters as we've instructed - they matter too!

### Check OpenID Connect metadata is working {#GoogleAppsauthentication-CheckOpenIDConnectmetadataisworking}

You can see the OpenID Connect metadata by going to [https://accounts.google.com/.well-known/openid-configuration](https://accounts.google.com/.well-known/openid-configuration).

### Inspect the contents of the security token {#GoogleAppsauthentication-Inspectthecontentsofthesecuritytoken}

Perhaps the contents of the security token sent back by GoogleApps aren't exactly the way Octopus expected, especially certain claims which may be missing or named differently. This will usually result in the GoogleApps user incorrectly mapping to a different Octopus User than expected. The best way to diagnose this is to inspect the JSON Web Token (JWT) which is sent from GoogleApps to Octopus via your browser. To inspect the contents of your security token:

1. Open the Developer Tools of your browser and enable Network logging making sure the network logging is preserved across requests.
2. In Chrome Dev Tools this is called "Preserve Log":

![Preserve Log Checkbox](/docs/security/authentication/images/5866122.png "width=500")

3. Attempt to sign into Octopus using GoogleApps and find the HTTP POST coming back to your Octopus instance from GoogleApps on a route like `/api/users/authenticatedToken/GoogleApps`. You should see an `id_token` field in the HTTP POST body. 

![ID Token](images/5866125.png "width=500")

4. Grab the contents of the `id_token` field and paste that into [https://jwt.io/](https://jwt.io/) which will decode the token for you.  

![jwt.io](/docs/security/authentication/images/5866123.png "width=500")

5. Don't worry if jwt.io complains about the token signature, it doesn't support RS256 which is used by GoogleApps.
6. Octopus uses most of the data to validate the token, but primarily uses the `sub`, `email` and `name` claims. If these claims are not present you will likely see unexpected behavior.
7. If you are not able to figure out what is going wrong, please send a copy of the decoded payload to our [support team](https://octopus.com/support) and let them know what behavior you are experiencing.
