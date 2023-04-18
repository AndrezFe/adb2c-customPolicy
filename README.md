# Custom Policy to AD B2C

---

## Overview

This is a sample policy to demonstrate how to use a custom policy to integrate with an external identity provider. In this case, we are using Azure Active Directory B2C.

## Prerequisites

- [Azure Active Directory B2C](https://docs.microsoft.com/en-us/azure/active-directory-b2c/active-directory-b2c-overview) tenant

- [Azure Active Directory B2C custom policy](https://docs.microsoft.com/en-us/azure/active-directory-b2c/active-directory-b2c-reference-policies) knowledge

- [Azure Active Directory B2C custom policy](https://docs.microsoft.com/en-us/azure/active-directory-b2c/active-directory-b2c-reference-policies) sample

- [Azure Active Directory B2C custom policy](https://docs.microsoft.com/en-us/azure/active-directory-b2c/active-directory-b2c-reference-policies) documentation

## Setup

### Step 1: Clone or download this repository

From your shell or command line:

`git clone https://github.com/AndrezFe/adb2c-customPolicy`

### Step 2: Register the sample with your Azure Active Directory B2C tenant

1. Sign in to the [Azure portal](https://portal.azure.com).

2. On the top bar, click on your account and under the **Directory** list, choose the Active Directory tenant where you wish to register your application.

3. Click on **All services** in the left-hand nav, and choose **Azure Active Directory**.

4. Click on **App registrations** and choose **New registration**.

5. When the **Register an application page** appears, enter your application's registration information:

   - In the **Name** section, enter a meaningful application name that will be displayed to users of the app, for example `adb2c-customPolicy`.

   - In the **Supported account types** section, select **Accounts in this organizational directory only**.

   - In the **Redirect URI (optional)** section, select **Web** in the combo-box and enter the following redirect URI: `https://jwt.ms`.

6. Click on **Register** to create the application.

7. In the app's registration screen, find and note the **Application (client) ID**. You use this value in your app's configuration file(s) later in your code.

8. In the app's registration screen, click on the **Authentication** menu.

   - In the **Redirect URIs** section, ensure that the Web redirect URI is `https://jwt.ms`.

   - In the **Advanced settings** section, set **Logout URL** to `https://jwt.ms`.

   - In the **Advanced settings** | **Implicit grant** section, check **ID tokens** as this sample requires the [Implicit grant flow](https://docs.microsoft.com/en-us/azure/active-directory/develop/v2-oauth2-implicit-grant-flow) to be enabled to sign-in the user, and call an API.

9. Click on **Save**.

### Step 3: Configure the sample to use your Azure Active Directory B2C tenant

In the `TrustFrameworkExtensions.xml` file, replace the following with your tenant information:

```xml
<TenantName>your-tenant-name</TenantName>
<TenantId>your-tenant-id</TenantId>
```

### Step 4: Upload the policy to your Azure Active Directory B2C tenant

1. Sign in to the [Azure portal](https://portal.azure.com).

2. On the top bar, click on your account and under the **Directory** list, choose the Active Directory tenant where you wish to register your application.

3. Click on **All services** in the left-hand nav, and choose **Azure Active Directory**.

4. Click on **App registrations** and choose **New registration**.

5. When the **Register an application page** appears, enter your application's registration information:

   - In the **Name** section, enter a meaningful application name that will be displayed to users of the app, for example `adb2c-customPolicy`.

   - In the **Supported account types** section, select **Accounts in this organizational directory only**.

   - In the **Redirect URI (optional)** section, select **Web** in the combo-box and enter the following redirect URI: `https://jwt.ms`.

6. Click on **Register** to create the application.

7. In the app's registration screen, find and note the **Application (client) ID**. You use this value in your app's configuration file(s) later in your code.

8. In the app's registration screen, click on the **Authentication** menu.

   - In the **Redirect URIs** section, ensure that the Web redirect URI is `https://jwt.ms`.

   - In the **Advanced settings** section, set **Logout URL** to `https://jwt.ms`.

   - In the **Advanced settings** | **Implicit grant** section, check **ID tokens** as this sample requires the [Implicit grant flow](https://docs.microsoft.com/en-us/azure/active-directory/develop/v2-oauth2-implicit-grant-flow) to be enabled to sign-in the user, and call an API.

9. Click on **Save**.

## Contribute

If you would like to contribute to this sample, see [CONTRIBUTING.MD](CONTRIBUTING.md).

## Questions and comments

We'd love to get your feedback about this sample. You can send your questions and suggestions to us in the Issues section of this repository.

Questions about Azure Active Directory B2C should be posted to [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-ad-b2c). Make sure that your questions or comments are tagged with [azure-ad-b2c].

## Additional resources

- [Azure Active Directory B2C](https://docs.microsoft.com/en-us/azure/active-directory-b2c/active-directory-b2c-overview)
