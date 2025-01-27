# Configure Server Side Script

In NetSuite, create a new script for SharpSync to use (example at the bottom)

* Click Setup > Customization > Scripts
* Select the `Script File` box > click the + sign
* Upload the script file from your computer
  * Attach from: `Computer`
  * Filename: Specify with the `.js` extension at the back
  * Folder: This must be `SuiteScripts`. DO NOT use another folder
  * Choose file: choose the file you saved (you can use the example script [here](example-server-side-script.md))
  * Click Save
* Click > Create Script Record
  * Specify a name: `sharpsync-item-create.js`
  * Click the `Deployments` tab > Enter any name
    * Deployed: `Yes`
    * Status: `Released`
  * Click `Save`
* After clicking Save and the page has loaded:
  * Under the `Scripts` tab > click `Edit`. You can make changes here
  *   Under the `Deployments` tab > Click the script name

      * A new page is shown `Script Deployment`
      * In the `URL` field you'll see a deployment url, something like

      > /app/site/hosting/restlet.nl?script=3187\&deploy=1

      * Take note of the id of the script (in the example above it's `3187`)
      * Click to edit the deployment
      * Make sure you have checked the following audiences:
        * `Audience` --> `Roles` --> `All Roles`
        * `Audience` --> `Roles` --> `All Employees`
      * Click `Save`

You are now done configuring the script in NetSuite. The next step is to use this ID in the setup of the script in SharpSync.
