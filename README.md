# lib_GoogleSheet_ui_ngx
This is the Google Sheet connector for Convertigo platform. Install this library to enable writing and reading from Google Sheets for your Convertigo applications

# Installation

* In your Convertigo Studio use File->Import->Convertigo->Convertigo Project and hit the 'Next' button

* In the Dialog 'Project remote URL' field Paste :

        lib_GoogleSheet_ui_ngx=https://github.com/convertigo/c8oprj-lib-googlesheets-ui-ngx.git:branch=8.0.0

* And click the 'Finish' button
* This will also automatically import the lib_OAuth project

# Usage

## Shared Actions

In order to authenticate with Google and browse the available documents in a Google Drive, the library provides a Shared Action you can use in your client apps.

Shared Action  | Usage
------| ------
DisplayGoogleDrivePicker   | This will display a Google Driver Picker widget in your Mobile or desktop app. If you are not already authenticated to Google, a Google Login will be displayed followed by a consent page. When all pages are filled by the user, the Google Driver picker will appear. When the user selects a document and hits the ok button, the SharedAction will return in the __out__ object the ID of the document picked. You can use this __out__ bound to the __SheetID__ variable when calling the SheetXXXX sequences. 


## Sample Application

You will find in this project a sample application using the Google Sheet Library, Use this as a reference and tutorial about using the library. This demonstrates :
- Use of the DisplayPicker Shared Action
- use of the SheetAddRow Sequence
- use of the SheetGetRange Sequence

## Using in a Backend only environment.

If you want to run the Sequences headless (by APIs or within a scheduled job) then the connector will need to authenticate to Google with no end user Interaction.

This can be done in the following way :

*  The connector will have to store a Refresh Token in a Convertigo user profile. To do this it will use the **lib_UserManager_Ngx** to store the refresh token as an User attribute. This implies that a Convertigo user account must be created. You can do this by executing the **AddUser** Sequence in **lib_UserManager_Ngx**
* When the Shared Action **DisplayGoogleDrivePicker** is executed, it will check if the current Convertigo session is Authenticated (If the Step **SetAuthenticatedSession** has been executed for this session). If it is, the Google Refresh Token will be persisted as an attribute to the current Authenticated User. **Caution!** This will only occur the first time a User asks access to the Google resources causing the Google Consent Screen to be displayed.
* If you want to run a Sequence such as **SheetGetRange** headless (from an API of from a Scheduled job): 

    * Make sure that a Convertigo Account exists and that the Google refresh token for this user is set up for this account. (See previous step). 
    * Authenticate with this user by having the **SetAuthenticatedUser** step called.
    * Have the **SheetXXX** sequences called. The connector will automatically retrieve the refresh token from the authenticated user account, refresh the access token from google and perform the operation.
