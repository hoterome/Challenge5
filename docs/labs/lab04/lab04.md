# Lab 4

## API Security

### Securing APIs with OpenID Connect and Red Hat Single Sign On

* Duration: 20 mins
* Audience: API Owners, Product Managers, Developers, Architects

## Overview

Once you have APIs in your organization and have applications being written, you also want to be sure in many cases that thee various types of users of the APIs are correctly authenticated. In this lab you will discover how to set up the widely used OpenID connect pattern for Authentication. 

### Why Red Hat?

The Red Hat SSO product provides important functionality for managing identities at scale. In this lab you can see how it fits together with 3scale and OpenShift.

### Skipping The Lab

We know sometime we don't have enough time to go over step by step on the labs. So here is a [short video](https://vimeo.com/middlewarepro/3scale-security-oidc-demo) where you can see how to configure OpenID Connect for your service using Red Hat Single Sign On.

If you are planning to follow to the next lab, there is an already running OpenID Connect secured API proxy for the Location API Service in this endpoint:

```bash
https://location-service-sso.amp.apps.GUID.openshiftworkshop.com
```

### Environment

**URLs:**

Check with your instruction the *GUID* number of your current workshop environment. Replace the actual number on all the URLs where you find **GUID**. 

Example in case of *GUID* = **1234**: 

```bash
https://master.GUID.openshiftworkshop.com
```

becomes =>

```bash
https://master.1234.openshiftworkshop.com
```

**Credentials:**

Your username is your asigned user number. For example, if you are assigned user number **1**, your username is: 

```bash
user1
```

The password to login is always the same:

```bash
r3dh4t1!
```

## Lab Instructions

### Step 1: Get Red Hat Single Sign On Service Account Credentials

1. Open a browser window and navigate to:

    ```bash
    http://sso-rh-sso.apps.GUID.openshiftworkshop.com/auth/admin/userX/console/
    ```

    *Remember to replace the GUID with your [environment](#environment) value and your user number.*

1. Log into Red Hat Single Sign On using your designated [user and password](#environment). Click on **Sign In**.

    ![00-login-sso.png](images/00-login-sso.png "RH SSO Login")

1. Select **Clients** from the left menu.

    ![00-clients.png](images/00-clients.png "Clients")

    *A 3scale-admin client and service account was already created for you*.

1. Click on the **3scale-admin** link to view the details.

    ![00-3scale-admin.png](images/00-3scale-admin.png "3scale admin account")

1. Click the **Credentials** tab.

    ![00-sa-credentials.png](images/00-sa-credentials.png "Service Account Credentials")

1. Take notice of the service account **Secret**. Copy and save it or write it down as you will use it to configure 3scale.

    ![00-sa-secret.png](images/00-sa-secret.png "Service Account Secret")

### Step 2: Add User to Realm

1. Click on the Users menu on the left side of the screen.

    ![00-users.png](images/00-users.png "Users Menu")

1. Click the Add user button.

    ![00-add-user.png](images/00-add-user.png "Add User")

1. Type **apiuser** as the Username.

    ![00-username.png](images/00-username.png "User Details")

1. Click on the **Save** button.

1. Click on the **Credentials** tab to reset the password. Type **apipassword** as the *New Password* and *Password Confirmation*. Turn OFF the *Temporary* to avoid the password reset at the next login.

    ![00-user-credentials.png](images/00-user-credentials.png "User Credentials")

1. Click on **Reset Password**.

1. Click on the **Change password** button in the pop-up dialog.

    ![00-change-password.png](images/00-change-password.png "Change Password Dialog")

    *Now you have a user to test your integration.*

### Step 3: Configure 3scale Integration

1. Open a browser window and navigate to:

    ```bash
    https://userX-admin.apps.GUID.openshiftworkshop.com/
    ```

    *Remember to replace the GUID with your [environment](#environment) value and your user number.*

1. Accept the self-signed certificate if you haven't.

    ![selfsigned-cert](images/00-selfsigned-cert.png "Self-Signed Cert")

1. Log into 3scale using your designated [user and password](#environment). Click on **Sign In**.

    ![01-login.png](images/01-login.png)

1. The first page you will land is the *API Management Dashboard*. Click on the **API** menu link.

    ![01a-dashboard.png](images/01a-dashboard.png)

1. This is the *API Overview* page. Here you can take an overview of all your services. Click on the **Integration** link.

    ![02-api-integration.png](images/02-api-integration.png)

1. Click on the **edit integration settings** to edit the API settings for the gateway.

    ![03-edit-settings.png](images/03-edit-settings.png)

1. Scrolll down the page, under the *Authentication* deployment options, select **OpenID Connect**. 

    ![04-authentication.png](images/04-authentication.png)

1. Click on the **Update Service** button.

1. Dismiss the warning about changing the Authentication mode by clicking **OK**.

    ![04b-authentication-warning.png](images/04b-authentication-warning.png)
    
1. Back in the service integration page, click on the **edit APIcast configuration**.

    ![05-edit-apicast.png](images/05-edit-apicast.png)

1. Scroll down the page and expand the authentication options by clicking the **Authentication Settings** link.

    ![05-authentication-settings.png](images/05-authentication-settings.png)

1. In the **OpenID Connect Issuer** field, type in your previously noted client credentials with the URL of your Red Hat Single Sing On instance:

    ```bash
    http://3scale-admin:CLIENT_SECRET@sso-rh-sso.apps.GUID.openshiftworkshop.com/auth/realms/userX
    ```

    *Remember to replace the GUID with your [environment](#environment) value, your user number and the CLIENT_SECRET you get in the [Step 1](#step-1-get-red-hat-single-sign-on-service-account-credentials)*.

    ![06-openid-issuer.png](images/06-openid-issuer.png "OpenID Connect Issuer")

1. Scroll down the page and click on the **Update Staging Environment** button.

    ![08-back-integration.png](images/08-back-integration.png "Back Integration")

1. After the reload, scroll down again and click the **Back to Integration &amp; Configuration** link.

    ![07-update-environment.png](images/07-update-environment.png "Update Environment")

1. Promote to Production by clicking the **Promote to Production** button.

    ![08a-promote-production.png](images/08a-promote-production.png "Update Environment")

### Step 4: Create a Test App

1. Go to the *Developers* tab and click on **Developer**.

    ![09-developers.png](images/09-developers.png "Developers")

1. Click on the **Applications** link.

    ![10-applications.png](images/10-applications.png "Applications")

1. Click on **Create Application** link.

    ![11-create-application.png](images/11-create-application.png "Create Application")

1. Select **Basic** plan from the combo box. Type the following information:

    * Name: **Secure App**
    * Description: **OpenID Connect Secured Application**

    ![12-application-details.png](images/12-application-details.png "Application Details")

1. Finally, scroll down the page and click on the **Create Application** button.

    ![13-create-app.png](images/13-create-app.png "Create App")

1. Note the *API Credentials*. Write them down as you will need the **Client ID** and the **Client Secret** to test your integration.

    ![14-app-credentials.png](images/14-app-credentials.png "App Credentials")

*Congratulations!* You have now an application to test your OpenId Connect integration.

## Steps Beyond

So, you want more? Login to the Red Hat Single Sign On admin console for your realm if you are not there already. Click on the Clients menu. Now you can check that 3scale zync component creates a new Client in SSO. This new Client has the same ID as the Client ID and Secret from the 3scale admin portal.

### Test the integration

You can try to use Postman or OpenID Connet playground to test your integration. Remember to update the *Redirect URL*.

## Summary

Now that you can secure your API using three-leg authentication with Red Hat Single Sign-On, you can leverage the current assets of your organization like current LDAP identities or even federate the authentication using other IdP services.

For more information about Single Sign-On, you can check its [page](https://access.redhat.com/products/red-hat-single-sign-on).

You can now proceed to [Lab 5](../lab05/lab05.md)

## Notes and Further Reading

* [Red Hat 3scale API Management](http://microcks.github.io/)
* [Red Hat Single Sign On](https://access.redhat.com/products/red-hat-single-sign-on)
* [Setup OIDC with 3scale](https://developers.redhat.com/blog/2017/11/21/setup-3scale-openid-connect-oidc-integration-rh-sso/)
