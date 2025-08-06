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

<img width="956" height="232" alt="image" src="https://github.com/user-attachments/assets/a19d1ab1-94cc-4b29-a5df-1c85b285e7ab" />

Locate and click on the database named “unencrypted-production-db”

On the database details page, scroll down to the “Configuration” tab

Under “Storage”, verify that “Encryption” is set to “Not enabled”

<img width="791" height="365" alt="image" src="https://github.com/user-attachments/assets/9f848030-955c-415a-8b7a-5ef88069d885" />


<h2>Step 2: Create a Snapshot of the Unencrypted Databas</h2>

Click on “Actions”

Select “Take snapshot”

For “Snapshot name”, enter “unencrypted-db-snapshot”

<img width="959" height="328" alt="image" src="https://github.com/user-attachments/assets/b5429e7d-d45e-47bb-8869-63a4fdb0e0c0" />

Click “Take snapshot”

Wait for the snapshot creation process to complete (Status: Available & Progress: Completed) (this can take ~2-5 minutes)


<img width="812" height="278" alt="image" src="https://github.com/user-attachments/assets/fcf6efd5-24cc-46fa-953b-fc8809711fab" />

Step 3: Create an Encrypted Copy of the Snapshot

Select the snapshot you just created (“unencrypted-db-snapshot”)

Click on “Actions”

Select “Copy snapshot” (refresh if it’s greyed out)

For “New DB snapshot identifier”, enter “encrypted-db-snapshot”

For the “Destination Region” you will need to pick US East (N. Virginia) or this won’t work

For “Target DB option group” leave the default of “No preference”


For “Resource tags” check the box

<img width="958" height="335" alt="image" src="https://github.com/user-attachments/assets/e7c1fdd1-6e19-4bfd-80ca-15376abc34f6" />


Under “Encryption”, check the box for “Enable encryption”

For “AWS KMS key”, select the default RDS key (aws/rds) – NOTE: in production you would want to create a customer-managed key and use its ARN here. We have other labs that show how to create KMS keys and use them to encrypt resources at rest
<img width="959" height="225" alt="image" src="https://github.com/user-attachments/assets/43228dd3-4f5c-4f29-ae76-67a0bccf4f42" />


Click “Copy snapshot”

Wait for the encrypted snapshot creation process to complete (Status: Available) (again, this can take ~2-5mins)

<img width="959" height="323" alt="image" src="https://github.com/user-attachments/assets/65f8b5ec-9223-4d92-b4f6-af5ac9e2a46b" />

<h2>Step 4: Restore a New Database Instance from the Encrypted Snapshot</h2>
Note: if a setting is not mentioned in these instructions, leave them as the default.

Once the encrypted snapshot is available, select it

Click on “Actions”

Select “Restore snapshot”

For the “DB Engine” make sure “MySQL Community” is selected

Select the “Single-AZ DB instance deployment (1 instance)” deployment option

<img width="959" height="424" alt="image" src="https://github.com/user-attachments/assets/7f316143-7d43-40ca-9db9-7190d76d7509" />

For “DB instance identifier”, enter “encrypted-production-db”

For the “DB instance class” make sure you select “Burstable classes (includes t classes) and then search for and select db.t4g.micro or this will fail

<img width="959" height="376" alt="image" src="https://github.com/user-attachments/assets/e5f15ed3-09f8-472b-91c7-309d469f4314" />

Verify that “Encryption” shows as “Enabled” (this should be pre-selected since the snapshot is encrypted)

<img width="959" height="174" alt="image" src="https://github.com/user-attachments/assets/c13a0ad5-1bdd-4e52-82f1-8cd67aca8654" />

For the “Existing VPC security groups” select the one created by the lab, not the ‘default’

<img width="829" height="110" alt="image" src="https://github.com/user-attachments/assets/ad72bd7a-b785-41eb-8784-d0365017a799" />

Under “Additional configuration”, you may want to set the same Database name as the original

Click “Restore DB instance”

Wait for the new database instance to be created and become available, which can take a few minutes

<img width="806" height="254" alt="image" src="https://github.com/user-attachments/assets/ed08dde7-680b-4e11-94a9-c489978bb2c6" />


Note: If you get an access denied error, it means you missed one of the settings above.

<h2>Step 5: Verify the New Encrypted Database</h2>

Once the new database is available, select it from the Databases list

On the database details page, scroll down to the “Configuration” tab

<img width="810" height="270" alt="image" src="https://github.com/user-attachments/assets/59409e3f-e2ed-429f-85bf-6c0d3806603b" />

<img width="641" height="313" alt="image" src="https://github.com/user-attachments/assets/13fff576-4447-4ec3-bef3-cc663f3031da" />

Under “Storage”, verify that “Encryption” is set to “Enabled” and shows the KMS key used (aws/rds)

<h2>Step 6: (Optional) Update Application Connection Strings</h2>
In a real-world scenario, you would now:

Update your application connection strings to point to the new encrypted database endpoint

Test your applications to ensure they work with the encrypted database

Once confirmed working, decommission the original unencrypted database

<h2>Conclusion</h2>
I have successfully implemented encryption for an existing RDS database using the snapshot-copy-restore approach, which is the recommended method to add encryption to an unencrypted RDS instance while minimizing service interruption.

This approach follows AWS best practices for addressing encryption requirements in existing database deployments.

<h2>Key Takeaways</h2>
You cannot enable encryption on an existing RDS instance after it’s created

The snapshot-copy-restore method is the recommended approach for adding encryption to existing databases

This method allows for validation of the encrypted database before cutting over from the original database

While this approach requires a brief application outage during cutover, it minimizes risk and provides a rollback option



