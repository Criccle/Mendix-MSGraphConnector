# Mendix-MSGraphConnector

<table>
<tr> 
    <td bgcolor="#DDD"> Name</td><td>Mendix-MSGraphConnector</td>
</tr>
<tr> 
    <td bgcolor="#DDD"> Author</td><td>Schalk Kruger</td>
</tr>
<tr> 
    <td bgcolor="#DDD"> Company</td><td>EPI-USE</td>
</tr>
<tr> 
    <td bgcolor="#DDD"> Type</td><td>Module</td>
</tr>
<tr> 
    <td bgcolor="#DDD"> Latest version</td><td>1.0.0-beta</td>
</tr>
<tr> 
    <td bgcolor="#DDD"> Package name</td><td>MSGraphConnector_1.0.0-beta.mpk</td>
</tr>
<tr> 
    <td bgcolor="#DDD"> Released</td><td>7-5-2017</td>
</tr>
<tr> 
    <td bgcolor="#DDD"> Mendix Version</td><td>Mendix 7.3.0</td>
</tr>
</table>

# Description
A Mendix Microsoft Graph connector that allows users to login to a Mendix application using their personal or work Office 365 accounts. Use the MS Graph authorization tokens to access other APIs such as email and calendar items.

For more information visit: https://developer.microsoft.com/en-us/graph/docs/concepts/overview

# Typical usage scenario
If you want to have your users login to their Mendix app with their Office 365 credentials. Also use the module if you want to access other Microsoft Graph APIs such as email and calendar items.

# Features and limitations

* Authenticate users via their personal or work Office 365 accounts.
* Use 2-factor authentication provided by Microsoft Graph
* Style your own fault pages
* Logging for all access attempts
* CSRF protection
* Microsoft Graph Token management.
* Determine how you identify your authorized users based on the data provided by MS Graph, options are mail or principleName.</li>
* Customize your user resolution microflow.
* Still allow users to login with their Mendix accounts or as local users.
* Except Community Commons the module requires no external modules, everything is included.
* Choose to use custom request handlers or optionally use the DeepLink module.
* Limited to MS Graph API only.

# Installation & Configuration

1. Download and install the Community Commons modules from the Mendix App store.
2. Download the MSGraphConnector module package from a desired release folder.
3. Import the MSGraphConnector module package into your project.
4. Add the following page to your project navigation: "_USE ME/Pages/Admin/MSGraph_Administration"

5. Add the following microflow to your application startup microflow: "_USE ME/Startup/StartWithCustomRequestHandlers", with the following default values:
    * ContextPath: 'msgraph/'
    * SigninPath: 'signin'
    * CallbackPath: 'callback'
    * LogoutPath: 'logout'

    The startup microflow will create a msgraph request handler for your application and expose the following urls:
    * Signin: {YourApplicationURI}/msgraph/signin - use to signin to the application with Office 365</li>
    * OAuth Callback: {YourApplicationURI}/msgraph/callback - callback url used by MS Graph to post auth tokens</li>
    * Logout: {YourApplicationURI}/msgraph/logout - logout of the application</li>
    
    Send your users the signin url to directly login to the application. 

    <b>IMPORTANT</b>: When deploying your application in a Mendix Cloud environment remember to register the "msgraph/" request handler     or as configured in ContextPath.

6. Edit your login page and add the following line of code just beneath the "Sign up with your Mendix account" link:

    ```html
    <a id="ssoButton" href="msgraph/signin" class="login-sso-button btn btn-default" style="margin-top: 20px;"><img id="ms-signin-button" alt="sign in" src="./MSSignInButton.svg" class=""></a>
    ```

    An example login page and the MSSignInButton.svg image file can be downloaded from github in the resources folder.
    <b>IMPORTANT</b>: If you change the default values above for the startup microflow ensure you change the href accordingly to your signin       path.

7. Ensure your account administration pages include a Email attribute.
8. Enable your project security and configure the MSGraphConnector user roles.

9. Register your application on  Microsoft App Registration Portal at https://apps.dev.microsoft.com, for more information please refer to this guide: https://developer.microsoft.com/en-us/graph/docs/concepts/auth_register_app_v2
    <b>IMPORTANT</b>: Configure the same RedirectURL as in your application setup in step 11.

10. Build and run your application and login as an Administrator.
11. Navigate to the MSGraph_Administration page and complete the Application Configuration:

    * First select the Default Permissions tab, and click the Create Default Permissions button.

    * Next, on the Application Setup tab:

        * Name: Name of your application.
        * CliendId: From Microsoft App Registration Portal</li>
        * Secret: From Microsoft App Registration Portal</li>
        * RedirectURL: {your application url}/msgraph/callback or as configured in the CallbackPath.</li>
        * MS Graph User Identifier: available options are: mail/principleName. This is how your Mendix application will identify your user.
        * Error Handling: Choose how the module should handle any MSGraph API related errors.</li>
        * Select the following default permissions: offline_access, User.ReadWrite</li>
        * Save Application Configuration.</li>

12. Create a New local user account and capture your email address.

__Congratulations, the next time you login you should be able to login with a Microsoft 365 personal or work account.__

13. Other configuration to consider:
    * Review the constant values in "_USE ME/Config"
    * CookieName: A cookie is temporarily stored on the user's pc while logging into MSGraph. Consider the changing to cookie name to a          more secure name.
    * MicrosoftGraphBaseAPIUrl: The Microsoft Graph base API enpoint url.
    * MicrosoftOnlineBaseAuthUrl: The Microsoft Graph base authorization Url.
    * MicrosoftOnlineBaseAuthUrlAdminConsent: The Microsoft Graph admin authorization Url.
    * MicrosoftOnlineBaseAuthUrlAdminConsent: The Microsoft Graph admin authorization Url.
    * ResolveUserMicroflow: You can configure a custom user resolver microflow. The value must a fully qualified name including the            module name, default: MSGraphConnector.ResolveUserByEmail

    * The MSGraphConnector Module include custom html pages to handle errors and information during logging into MS Graph, these can be         found in your project directory in the resources/MSGraph folder. You can create custom pages where necessary.

### Token Management:
The MSGraphConnector module stores user authorization tokens which could be used to access other MSGraph APIs such as mail and calendar. For more information visit: https://developer.microsoft.com/en-us/graph/docs/concepts/overview. Also see the user samples in the module folder. 
It is important that user tokens are deleted when a user logs out of the application. You can use the custom logout microflow in "_USE ME/Microflow/Login" to handle this.
Microsoft Graph authorization tokens are usually valid for 1 hour, you should implement the refresh token api to keep users logged in and using the API.

# Dependencies:
* Community Commons Function Library (Available in the Mendix app store)
* Deeplink (Optional) (Available in the Mendix app store)
* URLRedirector Widget (Optional) (Included)
* json-simple.jar (Included)

# Known bugs
* The first user login is slow, this is due to the httpClient starting. Will consider to include a StartModule microflow in a future release.

# Frequently Asked Questions
* Ask your question at the Mendix Community <a href="https://mxforum.mendix.com/" target="_blank">Forum</a>