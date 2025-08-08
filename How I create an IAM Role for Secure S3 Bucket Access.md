<h1>How I create an IAM Role for Secure S3 Bucket Access</h1>
Create and configure an IAM role to provide read-only access to a specific prefix within an S3 bucket, following security best practices and the principle of least privilege. I'll work with IAM roles, customer-managed policies, and permissions boundaries while learning to troubleshoot access issues in a realistic scenario that simulates common workplace tasks. Capture the flag by downloading the flag.txt file within the S3 bucket.

<h2>Scenario</h1>

<h2>Create an IAM Role to provide read access to a specific Amazon S3 bucket prefix.</h2>
Create an IAM role named “S3ReadAccessRole“

Attach the pre-created permissions boundary policy named “S3ReadPermissionsBoundary” (Note: this is required to prevent privilege escalation and you won’t be able to create the role without it)

Use the pre-created “S3ReadOnlyPolicy” customer managed policy. It’s currently misconfigured and will need to be updated after creating the role.

A general-purpose S3 bucket was created and deployed to this account. Identify its name as I will need that information for the next step. (Note: I purposefully don’t have access to objects in that bucket as the user)

Modify the IAM policy to only provide access to this bucket and more specifically, this prefix within that bucket: engineering

Assume the role and capture the flag by accessing the flag.txt file within engineering (the flag is the text within the Cybr{…} brackets)

<img width="767" height="347" alt="image" src="https://github.com/user-attachments/assets/e89c7761-c033-4639-9dc4-8f3b84bb2760" />

If you get an “access denied” error, it is either intended, or you took a wrong step. Triple check that you’re using the correct role name and IAM policy, and that you are including the permissions boundary when creating the role. 

This lab environment was carefully crafted and thoroughly tested to provide least privilege. If you truly believe there’s an issue with permissions in the lab, contact us, but 99% of the time it is user error 🙂
While you can use “s3:*“ to initially troubleshoot, this isn’t your final solution as that’s nowhere near least privilege! It can be helpful to start with broad permissions like this to make sure it all works, and then narrow down to least privilege
You do not need to attach or use the deployed S3LabUser-lab-policy. This policy is only there for the IAM user and can be ignored.
Feel free to Google or use ChatGPT! Most employers allow this! But also try to do most of it on your own, and only use those tools to get through sticking points. GPTs can give bad advice when it comes to IAM policies that will take you in the wrong directions, so you will need to think critically.
Remember that this scenario only requires an IAM role, not a bucket policy. The bucket policy is already correctly configured.



