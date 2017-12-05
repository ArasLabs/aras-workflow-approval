# Aras Workflow Approval

This project contains an import package and steps to assist with the installation/configuration of the Aras Workflow Approval add-in for Outlook 365.

You can read [more about the Aras Workflow Approval add-in here](https://appsource.microsoft.com/en-us/product/office/wa104380308?src=office&corrid=6ea8abad-c702-4eba-8036-af2560453303&omexanonuid=aed187de-c29b-4fd7-8823-faa2357c8781).

## Installation

#### Important!
**Always back up your code tree and database before applying an import package or code tree patch!**

### Pre-requisites

1. Aras Innovator installed (version 11.0 SPx preferred)
    * _Note: The Aras instance must be configured to use the HTTPS protocol._
2. Aras Package Import tool
3. Microsoft Outlook 365
4. [Aras Workflow Approval add-in](https://appsource.microsoft.com/en-us/product/office/wa104380308?src=office&corrid=6ea8abad-c702-4eba-8036-af2560453303&omexanonuid=aed187de-c29b-4fd7-8823-faa2357c8781) installed

### Install Steps

#### Import the Package

1. Backup your database and store the BAK file in a safe place.
2. Open up the Aras Package Import tool.
3. Enter your login credentials and click **Login**
    * _Note: You must login as root for the package import to succeed!_
4. Enter the package name in the TargetRelease field.
    * Optional: Enter a description in the Description field.
5. Enter the path to your local `..\ArasWorkflowApproval\Import\imports.mf` file in the Manifest File field.
6. Select **ArasWorkflowApproval** in the Available for Import field.
7. Select Type = **Merge** and Mode = **Thorough Mode**.
8. Click **Import** in the top left corner.
9. Close the Aras Package Import tool.

#### Option 1: Use the Aras Workflow Approval email message template

1. Log in to Aras as an administrator.
2. Navigate to **Administration > Workflow Maps**. 
3. Open and lock your Workflow Map item.
4. Select an Activity.
5. Select the Notifications tab.
6. Add a new relationship.
    1. Select the *New Assignment Template* email template
    2. Event = On Activate
    3. Target = Alternate
    4. Alternate = User who will receive messages
7. Repeat Steps 4-6 for all Activities that should send emails.
8. Save, unlock, and close the Workflow Map.

#### Option 2: Update an existing email message template

1. Log in to Aras as an administrator.
2. Navigate to **Administration > Notification > E-mail Messages**. 
3. Open and lock your E-mail Message item.
4. Add the following to the Body HTML field on your E-mail Message item:

```(html)
    <BODY>
        <style>
            span {
                display: none
            }
        </style>
        <p>You have been assigned a new workflow activity in Aras Innovator. Please open the Aras app above to complete the activity.</p>
        <span id="activity">${Item[1]/name}</span><br/>
        <span id="itemNumber">${Item[2]/item_number}</span><br/>
        <span id="itemType">${Item[@type="ItemType"]/name}</span><br/>
        <span id="database">${Item[3]/database}</span><br/>
        <span id="host">${Item[3]/host}</span><br/>
        <span id="trigger">InBasket</span>
    </BODY>
```

5. Add the following to the Query String field:

```(xml)
    <Item type="Activity" id="${Item/ActivityId}" action="get" select="name,message"/>
    <Item type="${Item/@type}" id="${Item/@id}" action="get" select="item_number"/>
    <Item type="ItemType" action="GetUrl" select="database,url"/>
    <Item type="ItemType" id="${Item/@typeId}" action="get" select="name"/>

```

6. Save, unlock, and close the E-mail Message item.

You are now ready to use the Aras Workflow Approval add-in for Outlook!

## Contributing

1. Fork it!
2. Create your feature branch: `git checkout -b my-new-feature`
3. Commit your changes: `git commit -am 'Add some feature'`
4. Push to the branch: `git push origin my-new-feature`
5. Submit a pull request

For more information on contributing to this project, another Aras Labs project, or any Aras Community project, shoot us an email at araslabs@aras.com.

## Credits

Documented and published by Eli Donahue for Aras Labs. @EliJDonahue

## License

Aras Labs projects are published to Github under the MIT license. See the [LICENSE file](./LICENSE.md) for license rights and limitations.