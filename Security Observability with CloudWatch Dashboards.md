<h1>Security Observability with CloudWatch Dashboards</h1>
This lab teaches you how to create actionable security monitoring dashboards in AWS CloudWatch to detect indicators of compromise, including unauthorized user creation, access key generation, and suspicious policy changes. This approach can also applied to any security events captured in CloudTrail logs. You'll build visual dashboards that automatically surface these IOCs (Indicators of Compromise) from raw CloudTrail data, transforming thousands of log entries into actionable security insights without relying on another third party service.

<h2>Overview & Scenario</h2>
Your organization is experiencing suspicious IAM activity: new users, access keys, and rapid policy changes. How would you know? CloudTrail logs contain the answers, but they’re buried in thousands of entries.

While GuardDuty catches some threats and can help you with investigations, it’s important to understand alternatives available to you in AWS. Maybe you don’t want to enable GuardDuty, or it misses certain events you care about.

In this lab, you’ll build CloudWatch dashboards that take raw CloudTrail data and turn it into visual security insights that your team can use. Instead of scrolling through JSON logs, you’ll create near real-time monitoring that highlights IAM indicators of compromise resources and tracks security trends.

By the end, you’ll have a practical dashboard that automatically detects and visualizes IAM IoCs that could help you stop an ongoing breach.

