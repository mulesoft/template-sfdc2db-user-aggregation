
# Template: Salesforce and Database User Aggregation

Aggregates users from Salesforce and a database into a CSV file. This basic pattern can be modified to collect from more or different sources and to produce formats other than CSV. You can trigger this manually or programmatically with an HTTP call. 

Users are sorted such that the accounts only in Salesforce appear first, followed by users only in the database, and lastly by users found in both systems. The custom sort or merge logic can be easily modified to present the data as needed. This template also serves as a base for building APIs using the Anypoint Platform. A database table schema is included to make testing this template easier.

Aggregation templates can be easily extended to return a multitude of data in mobile friendly form to power your mobile initiatives by providing easily consumable data structures from otherwise complex backend systems.

![de71f6ed-d0fe-4d8e-9975-90c0801bf153-image.png](https://exchange2-file-upload-service-kprod.s3.us-east-1.amazonaws.com:443/de71f6ed-d0fe-4d8e-9975-90c0801bf153-image.png)

**Note**: Any references in the video to DataMapper have been updated in the template with DataWeave transformations.

[![YouTube Video](http://img.youtube.com/vi/u7kKnHS5qio/0.jpg)](https://www.youtube.com/watch?v=u7kKnHS5qio)

[//]: # (![]\(https://www.youtube.com/embed/u7kKnHS5qio?wmode=transparent\))


# License Agreement

This template is subject to the conditions of the [MuleSoft License Agreement](https://s3.amazonaws.com/templates-examples/AnypointTemplateLicense.pdf "MuleSoft License Agreement").

Review the terms of the license before downloading and using this template. You can use this template for free with the Mule Enterprise Edition, CloudHub, or as a trial in Anypoint Studio.

# Use Case

As a Salesforce administrator I want to aggregate users from a Salesforce instance and a database, and compare them to see which users are in one of the two and which users are in both.

This template has these features: 

- Generates a CSV results report that is sent by email to addresses you configure.
- Extracts data from two systems, aggregates data, compares values of fields for the objects, and generates a report of the differences. 
- Gets users from a Salesforce instance and a database table, compares by the email address of the users, and generates a CSV file that shows users in A, users in B (being "B" the database), and users in A and B. The report is then emailed to a configured group of email addresses.

# Considerations

To make this template run, there are certain preconditions that must be considered. All of them deal with the preparations in both, that must be made for all to run smoothly. Failing to do so could lead to unexpected behavior of the template.

**Note:** This template illustrates the aggregation use case between Salesforce and a database, thus it requires a database instance to work.

The template comes package with a SQL script to create the database table it uses.

It is your responsibility to use that script to create the table in an available schema and change the configuration accordingly.

The SQL script file can be found in src/main/resources/sfdc2jdbc.sql.

## Database Considerations

To get this template to work:

- This template may use date time or timestamp fields from the database to do comparisons and take further actions.
- While the template handles the time zone by sending all such fields in a neutral time zone, it cannot handle time offsets.
- We define time offsets as the time difference that may surface between date time and timestamp fields from different systems due to a differences in the system's internal clock.
- Take this in consideration and take the actions needed to avoid the time offset.

### As a Data Destination

There are no considerations with using a database as a data destination.

## Salesforce Considerations

Here's what you need to know about Salesforce to get this template to work.

### FAQ

- Where can I check that the field configuration for my Salesforce instance is the right one? See: [Salesforce: Checking Field Accessibility for a Particular Field](https://help.salesforce.com/HTViewHelpDoc?id=checking_field_accessibility_for_a_particular_field.htm&language=en_US "Salesforce: Checking Field Accessibility for a Particular Field")
- Can I modify the Field Access Settings? How? See: [Salesforce: Modifying Field Access Settings](https://help.salesforce.com/HTViewHelpDoc?id=modifying_field_access_settings.htm&language=en_US "Salesforce: Modifying Field Access Settings")

### As a Data Source

If the user who configured the template for the source system does not have at least _read only_ permissions for the fields that are fetched, then an _InvalidFieldFault_ API fault displays.

```
java.lang.RuntimeException: [InvalidFieldFault [ApiQueryFault [ApiFault  
exceptionCode='INVALID_FIELD'
exceptionMessage='
Account.Phone, Account.Rating, Account.RecordTypeId, Account.ShippingCity
^
ERROR at Row:1:Column:486
No such column 'RecordTypeId' on entity 'Account'. If you are attempting to use a custom field, be sure to append the '__c' after the custom field name. Reference your WSDL or the describe call for the appropriate names.'
]
row='1'
column='486'
]
]
```

# Run it!

Simple steps to get Salesforce and Database User Aggregation running.

## Running On Premises

In this section we help you run your template on your computer.

### Where to Download Anypoint Studio and the Mule Runtime

If you are a newcomer to Mule, here is where to get the tools.

- [Download Anypoint Studio](https://www.mulesoft.com/platform/studio)
- [Download Mule runtime](https://www.mulesoft.com/lp/dl/mule-esb-enterprise)

### Importing a Template into Studio

In Studio, click the Exchange X icon in the upper left of the taskbar, log in with your

Anypoint Platform credentials, search for the template, and click **Open**.

### Running on Studio

After you import your template into Anypoint Studio, follow these steps to run it:

- Locate the properties file `mule.dev.properties`, in src/main/resources.
- Complete all the properties required as per the examples in the "Properties to Configure" section.
- Right click the template project folder.
- Hover your mouse over `Run as`
- Click `Mule Application (configure)`
- Inside the dialog, select Environment and set the variable `mule.env` to the value `dev`
- Click `Run`

### Running on Mule Standalone

Complete all properties in one of the property files, for example in mule.prod.properties and run your app with the corresponding environment variable. To follow the example, this is `mule.env=prod`.

After this, to trigger the use case, browse to the endpoint using the port you configured in your file. If you set `9090` as the port, browse to `http://localhost:9090/synccontacts`. This URL creates the CSV report and sends it to the emails you set.

## Running on CloudHub

While creating your application on CloudHub (or you can do it later as a next step), go to Runtime Manager > Manage Application > Properties to set the environment variables listed in "Properties to Configure" as well as the **mule.env**.

After your app is running, if you chose `useraggregation` as the domain name to trigger the use case, browse to `http://useraggregation.cloudhub.io/generatereport`, which invokes the app and sends the CSV report to the emails you configure.

### Deploying your Anypoint Template on CloudHub

Studio provides an easy way to deploy your template directly to CloudHub, for the specific steps to do so check this

## Properties to Configure

To use this template, configure properties (credentials, configurations, etc.) in the properties file or in CloudHub from Runtime Manager > Manage Application > Properties. The sections that follow list example values.

### Application Configuration

**HTTP Connector Configuration**

- http.port `9090` 

**SalesForce Connector Configuration**

- sfdc.username `bob.dylan@org`
- sfdc.password `DylanPassword123`
- sfdc.securityToken `avsfwCUl7apQs56Xq2AKi3X`

**database Connector Configuration**

- db.host `localhost`
- db.port `3306`
- db.user `mule_user`
- db.password `mule_pwd`
- db.databasename `template-sfdc2db-user-aggregation`

**SMTP Services Configuration**

- smtp.host `smtp.example.com`
- smtp.port `587`
- smtp.user `exampleuser@example.com`
- smtp.password `ExamplePassword456`

**Email Details**

- mail.from `exampleuser1@example.com`
- mail.to `exampleuser2@example.com`
- mail.subject `Users Report`
- mail.body `Find attached your users report`
- attachment.name `users_report.csv`

# API Calls

Salesforce imposes limits on the number of API calls that can be made. However, this template makes only one API call to Salesforce during aggregation.

# Customize It!

This brief guide intends to give a high level idea of how this template is built and how you can change it according to your needs.

As Mule applications are based on XML files, this page describes the XML files used with this template.

More files are available such as test classes and Mule application files, but to keep it simple, we focus on these XML files:

- config.xml
- businessLogic.xml
- endpoints.xml
- errorHandling.xml

## config.xml

Configuration for connectors and configuration properties are set in this file. Even change the configuration here, all parameters that can be modified are in properties file, which is the recommended place to make your changes. However if you want to do core changes to the logic, you need to modify this file.

In the Studio visual editor, the properties are on the _Global Element_ tab.

## businessLogic.xml

The functional aspect of this template is implemented in its XML directed by a flow responsible for conducting the aggregation of data, comparing records and finally formatting the output as a report.

Using the Scatter-Gather component, the template queries the data in different systems. After that the aggregation is implemented in a DataWeave 2 script using Transform component.

Aggregated results are sorted by:

1. Users only in Salesforce.
2. Users only in the database.
3. Users in both Salesforce and the database.

These are transformed to CSV format. The final report in CSV format is sent to email addresses you configured in the mule.\*.properties file.

## endpoints.xml

The file where you find the endpoint to start the aggregation. This template uses an HTTP Listener as the way to trigger the use case.

### Trigger Flow

**HTTP Inbound Endpoint** - Start Report Generation

- `${http.port}` is set as a property to be defined either on a property file or in CloudHub environment variables.
- The path configured by default is `generatereport` and you are free to change for the one you prefer.
- The host name for all endpoints in your CloudHub configuration should be defined as `localhost`. CloudHub then routes requests from your application domain URL to the endpoint.

## errorHandling.xml

This is the right place to handle how your integration reacts depending on the different exceptions.

This file provides error handling that is referenced by the main flow in the business logic.

