<h1>Encrypting an unencrypted RDS database</h1>

You've recently joined a company as their cloud security engineer and are reviewing their established AWS environment. During your assessment, you notice that a business-critical RDS database is running without encryption enabled, which doesn't align with security best practices. In this lab, you'll implement encryption for this database using the best practice approach while minimizing service interruption.

<h2>Scenario</h2>
You’ve recently joined a company as their cloud security engineer and are reviewing their established AWS environment. During your assessment, you notice that a business-critical RDS database is running without encryption enabled, which doesn’t align with security best practices.

In this lab, I’ll implement encryption for this database using the best practice approach while minimizing service interruption.

<h1>Lab Objectives</h1>h1
Verify that the production RDS database is running without encryption

Create a snapshot of the unencrypted database

Create an encrypted copy of the snapshot

Restore a new database instance from the encrypted snapshot

Verify that the new database is encrypted

<h2>Lab Steps</h2>

Step 1: Verify the Existing Unencrypted Database

Log in to the AWS Management Console using the provided credentials

Navigate to the RDS service

Insert image

Locate and click on the database named “unencrypted-production-db”

On the database details page, scroll down to the “Configuration” tab

Under “Storage”, verify that “Encryption” is set to “Not enabled”

<h2>Step 2: Create a Snapshot of the Unencrypted Databas</h2>

Click on “Actions”

Select “Take snapshot”

For “Snapshot name”, enter “unencrypted-db-snapshot”

Insert image

Click “Take snapshot”

Wait for the snapshot creation process to complete (Status: Available & Progress: Completed) (this can take ~2-5 minutes)

Step 3: Create an Encrypted Copy of the Snapshot

Select the snapshot you just created (“unencrypted-db-snapshot”)

Click on “Actions”

Select “Copy snapshot” (refresh if it’s greyed out)

For “New DB snapshot identifier”, enter “encrypted-db-snapshot”

For the “Destination Region” you will need to pick US East (N. Virginia) or this won’t work

For “Target DB option group” leave the default of “No preference”

For “Resource tags” check the box

Under “Encryption”, check the box for “Enable encryption”

For “AWS KMS key”, select the default RDS key (aws/rds) – NOTE: in production you would want to create a customer-managed key and use its ARN here. We have other labs that show how to create KMS keys and use them to encrypt resources at rest

Click “Copy snapshot”

Wait for the encrypted snapshot creation process to complete (Status: Available) (again, this can take ~2-5mins)

insert image

<h2>Step 4: Restore a New Database Instance from the Encrypted Snapshot</h2>
Note: if a setting is not mentioned in these instructions, leave them as the default.

Once the encrypted snapshot is available, select it

Click on “Actions”

Select “Restore snapshot”

For the “DB Engine” make sure “MySQL Community” is selected

Select the “Single-AZ DB instance deployment (1 instance)” deployment option

insert IMAGE
