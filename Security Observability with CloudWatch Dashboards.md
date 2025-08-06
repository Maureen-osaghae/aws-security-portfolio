<h1>Security Observability with CloudWatch Dashboards</h1>
This lab teaches you how to create actionable security monitoring dashboards in AWS CloudWatch to detect indicators of compromise, including unauthorized user creation, access key generation, and suspicious policy changes. This approach can also applied to any security events captured in CloudTrail logs. You'll build visual dashboards that automatically surface these IOCs (Indicators of Compromise) from raw CloudTrail data, transforming thousands of log entries into actionable security insights without relying on another third party service.

<h2>Overview & Scenario</h2>
Your organization is experiencing suspicious IAM activity: new users, access keys, and rapid policy changes. How would you know? CloudTrail logs contain the answers, but they’re buried in thousands of entries.

While GuardDuty catches some threats and can help you with investigations, it’s important to understand alternatives available to you in AWS. Maybe you don’t want to enable GuardDuty, or it misses certain events you care about.

In this lab, you’ll build CloudWatch dashboards that take raw CloudTrail data and turn it into visual security insights that your team can use. Instead of scrolling through JSON logs, you’ll create near real-time monitoring that highlights IAM indicators of compromise resources and tracks security trends.

By the end, you’ll have a practical dashboard that automatically detects and visualizes IAM IoCs that could help you stop an ongoing breach. 

<img width="1536" height="1320" alt="image" src="https://github.com/user-attachments/assets/64eb376b-eed3-43c5-a4e5-0a52aa04ec22" />

Let’s get started!

<h2>Learning Objectives</h2>

<h2>What I did:</h2>

• Create CloudWatch dashboards for security monitoring

• Use different widget types for security visualization

• Apply CloudWatch math expressions for advanced metrics

• Display relevant logs in dashboards

<h2>Lab Setup</h2>
This lab already has a few things set up for me:

• CloudTrail sending logs to CloudWatch Logs

• Pre-configured metric filters for CreateUser, CreateAccessKey, and PutUserPolicy events (these could indicate threat actor activity)

• Lambda function to generate test security events so we can make sure this works properly

• IAM user with dashboard creation and Lambda execution permissions

<h2>Step 1: Verify Metrics Are Working</h2>

Navigate to CloudWatch > Metrics > All Metrics.

Navigate to CloudWatch > Metrics > All Metrics. Look for the SecurityMonitoring namespace.

Once you see it, click on the namespace.

You’ll see “Metrics with no dimensions” since no data has been collected yet. Click on that.

<img width="958" height="364" alt="image" src="https://github.com/user-attachments/assets/a8fd6044-8bd3-4080-a8a0-c191b44744c2" />

<img width="957" height="194" alt="image" src="https://github.com/user-attachments/assets/7c00d73a-e5e8-46ac-9965-801a19fc3885" />


You should see three metrics:

CreateUserCount

CreateAccessKeyCount

PutUserPolicyCount

NOTE: These custom metrics can take a few minutes to appear. If you don’t see them right away, refresh every 2-3 minutes until you see them. It should take less than 5 minutes.

We’re ready to move on to the next step.

<h2>Step 2: Create Your First Security Dashboard</h2>

Navigate to CloudWatch > Dashboards (in the left menu – you may have to expand it to see the option) and click Create dashboard.

<img width="959" height="325" alt="image" src="https://github.com/user-attachments/assets/3069d692-7395-4d2b-b998-5db6cf6a3871" />


Name it: Security-Monitoring-Dashboard

<img width="923" height="399" alt="image" src="https://github.com/user-attachments/assets/0e54c354-5805-4570-9bcd-4f9196a9e792" />


<h2>Add a Line Chart Widget</h2>

Click Add widget (if it doesn’t automatically open up already) and select Line.

Add all three security metrics:

SecurityMonitoring > CreateUserCount

SecurityMonitoring > CreateAccessKeyCount

SecurityMonitoring > PutUserPolicyCount

<img width="891" height="385" alt="image" src="https://github.com/user-attachments/assets/d89e6e06-d378-444a-97ff-4297c5d777b3" />

Click “Create widget.” That’s how easy it is!

Of course, we should customize it a bit more.

Widget options:

To customize our widget, we can click on the 3 dots in the corner of the graph and “Edit.”

Change the title to: IAM Security Events Timeline (by clicking on the pencil icon next to the current title)

Set the period to: 1 minute

Statistic: Sum

<img width="932" height="464" alt="image" src="https://github.com/user-attachments/assets/018fe3c2-5b0f-41fa-8e99-8256b9522fdd" />


<img width="393" height="345" alt="image" src="https://github.com/user-attachments/assets/fb222809-4f41-4a68-a9ba-2450607f84b1" />

<img width="432" height="250" alt="image" src="https://github.com/user-attachments/assets/1cc95b48-ae7f-470f-a075-9ec9a6c4c08c" />

Note: notice how under the “Details” it tells us that this is currently only looking at us-east-1. Make sense, since we’re in N. Virginia and that’s where our CloudTrail/CloudWatch setup is. We would have to repeat this across multiple regions if we wanted complete coverage, but keep in mind that increases costs.

Click Update widget.

Click on Save (or enable Autosave) in the top right corner. If you don’t do this, it won’t save your dashboard.

<h2>Add a Number Widget</h2>
To demonstrate more graph types, let’s add another widget. Click on the + icon in the top right corner and select Number.

<img width="630" height="320" alt="image" src="https://github.com/user-attachments/assets/80d4a35c-1fca-4aca-97e9-318b1c15ac25" />
<img width="959" height="296" alt="image" src="https://github.com/user-attachments/assets/37dbc5b9-2670-40c4-967a-7185390ef6f4" />


Add the same three metrics and “Create widget”.

This time, edit and configure them as:

Title: Total IAM Security Events (Last 5 minutes)
Period: 5 minutes
Statistic: Sum

<img width="872" height="413" alt="image" src="https://github.com/user-attachments/assets/d4cc7530-3fa2-4f6d-b330-318c0a1a7bed" />

This gives you a quick summary view for the past hour.

Click on Save (if you haven’t enabled Autosave).

<h2>Step 3: Add Math Expressions</h2>

Why Use Math Expressions?

Next, let’s add math expressions. Math expressions allow you to combine multiple metrics into a single calculated value. This is powerful for security monitoring because:

Aggregate View: Instead of viewing 3 separate lines (which we already see with our “IAM Security Events Timeline” widget), you get one “total security activity” score that combines all 3 metrics
Trend Analysis: This can make it easier to spot overall increases in suspicious activity versus individual metrics
Alerting: You can set alarms on the combined score rather than individual metrics
Let’s create a combined security score using math expressions.

Let’s create a combined security score using math expressions.

Add a new Line widget.

First, add your three metrics:

Create the widget and then edit it.

<img width="880" height="421" alt="image" src="https://github.com/user-attachments/assets/ea005255-2f9a-48eb-9ddf-17142dc43a40" />

<h2>Add the math expression:</h2>

Click Add math dropdown
Select Start with empty expression
Enter this expression: m1 + m2 + m3

The system will automatically assign m1, m2, m3 to your three metrics in the order you added them.
Widget options:

<img width="865" height="371" alt="image" src="https://github.com/user-attachments/assets/954fc5af-fd08-4fc5-a812-80e8312f5953" />


Title: Combined IAM Security Activity Score
Hide the individual metrics (uncheck ID “m1” and “Label” CreateAccessKeyCount, and do the same for m2 & m3. e1 should be all that’s still checked)
Set the Statistic to Sum and the Period to 1 minute
Rename the “Expression1” label to “Total IAM Security Activity”

<img width="899" height="419" alt="image" src="https://github.com/user-attachments/assets/3127efc9-f17a-4b58-8c7d-7a7a02f18568" />

Update Widget

This will display only the math expression result.

<h2>Step 4: Create a Stacked Area Chart</h2

Why Use a Stacked Area Chart?

A stacked area chart is valuable for security monitoring because:

Proportional Analysis: Shows not just total activity, but the breakdown of which types of events are most common

Trend Identification: Reveals if certain attack patterns are increasing (e.g., more CreateUser events vs. CreateAccessKey events)

Attack Pattern Recognition: Helps identify if an attacker is focusing on specific IAM tactics

Visual Comparison: Easy to see which security events dominate during different time periods

Create a stacked area chart:

Add a Stacked area widget.

Add the same three security metrics.

Widget options:

Title: IAM Security Events Breakdown
Period: 1 minute
Statistic: Sum

<img width="881" height="413" alt="image" src="https://github.com/user-attachments/assets/b5d35cb0-feee-4367-9bea-b15086d34aea" />


This shows the proportion of different security events over time.

<h2>Step 5: Add Log Insights Widget</h2>

Add a Logs table widget.

Start by adding a new widget, and then click on the “Logs” tab. Then, select “Logs table.”

<img width="654" height="287" alt="image" src="https://github.com/user-attachments/assets/140f2eaf-e50b-4c1a-8260-ef9dc6ed95e3" />


On the next page, click on the dropdown under “Selection criteria” to select the correct log group. There shouldn’t be that many to select from, but look for one that looks something like /aws/cloudtrail/security-observability-with-cloudwatch-dashboards

Query:
<img width="958" height="402" alt="image" src="https://github.com/user-attachments/assets/38680a0c-f6fa-4b4c-8fde-012bec3080ac" />


This will gather the timestamp, eventName, userIdentity type, and sourceIPaddress from CloudTrail logs for our IAM events we’ve been filtering for. It will then sort the results by timestamp in a descending order and limit results to 20.

We can’t run this query yet sinc we haven’t generated security events, so instead just click on “Save.”

Name it “Recent IAM Security Events” and click on Save.

Name it “Recent IAM Security Events” and click on Save.

Next, “Add to dashboard.”

Everything should be prepopulated for you:

Confirm and “Add to dashboard”

Click on “Save” in the top right to save your dashboard.

<img width="959" height="446" alt="image" src="https://github.com/user-attachments/assets/a560709d-afff-4241-bd55-1b71f84c465a" />

Side bar: Do you need all these widgets?
Probably not. We’re creating them so you can see what’s possible with CloudWatch dashboards, but you probably wouldn’t need everything we just created in a single dashboard. Try to focus on what would make this actionable, where less is often better in this situation.

If you create new widgets, they should give you unique insights and a different perspective. If they repeat similar information, they’re probably redundant. Just something to keep in mind!

<img width="554" height="314" alt="image" src="https://github.com/user-attachments/assets/d62d130f-57f9-4dc9-a47c-ea7f593de19f" />

<img width="677" height="401" alt="image" src="https://github.com/user-attachments/assets/aba980da-6df0-4ad5-a297-a513ad9fbbb5" />

Step 6: Generate Security Events
Now let’s populate your dashboards with test data using AWS CloudShell.

In a separate tab, open CloudShell by clicking the CloudShell icon in the top toolbar of the AWS Console (or searching for it).

Execute the Lambda function Run this command 3-4 times with a few seconds between each execution:

    aws lambda invoke --function-name $(aws sts get-caller-identity --query Account --output text)-generate-security-events response.json && cat response.json && echo

<img width="959" height="431" alt="image" src="https://github.com/user-attachments/assets/b244bc5c-7330-46de-b7c7-c23f20722bd7" />

Note: it will take a few seconds for the function to finish executing each time.

This command:

Gets your account ID automatically (the function name includes the account ID which is why this is needed)

Invokes the security event generation Lambda function

Shows the response with details of what events were created

Adds a line break for readability

Wait a few seconds between each execution to allow CloudTrail to process the events.

<img width="959" height="404" alt="image" src="https://github.com/user-attachments/assets/8fa17058-0ecb-40b5-8fab-6f1dd4af6457" />

<h2>Step 7.1: Test Your Dashboard</h2>

Let’s go back to the dashboard tab now.

Set your dashboard time range to Last 30 minutes by clicking on “Custom” to see all your test data.

Wait about 2-5 minutes for CloudTrail to catch up as it’s not a real-time service. Also, some graphs may populate before others.

<img width="668" height="427" alt="image" src="https://github.com/user-attachments/assets/10b55e03-5391-4a3c-9336-1d1289ce4654" />


<img width="959" height="415" alt="image" src="https://github.com/user-attachments/assets/12028a9c-212d-492a-90b6-da35843cedab" />

<img width="952" height="356" alt="image" src="https://github.com/user-attachments/assets/9e4ca9b1-25fa-4db8-8c66-c5ce6225a681" />

<h2>Step 7.2: Bonus – how would you use this dashboard to investigate a threat?</h2>

What can you tell from the logs shown in the dashboard? If you saw this in your organization’s production environment, what would you do with that information?

This is a good exercise for you to think about. As a starting point example, if you look up the IP address, you’ll see that it’s an Amazon IP address coming from an EC2.compute.amazonaws.com hostname. This would indicate the threat actor is launching their attack from an AWS environment, which we know is true since we used CloudShell to call Lambda, and Lambda executed the actions.

<h2>Conclusion</h2>
That’s it! 
I’ve built a working security dashboard in CloudWatch.

What started as buried CloudTrail logs is now a visual monitoring system that actually tells you what’s happening in your environment. When someone creates users, generates access keys, or modifies policies, you’ll know about it.

Same approach can be apply it to other security events in your own AWS environment. Maybe you want to track failed login attempts, or monitor when someone accesses sensitive S3 buckets. The pattern is the same: CloudTrail logs, metric filters, and dashboards.

A few things worth considering as you move forward:

Set up some alarms so you get notified when things spike
Add other IAM events that could indicate a breach (refer to our privilege escalation course for more)
Share this with your team and see what they think is missing
Re-create the dashboards and widgets you want to keep with IaC
The reality is that most security incidents could have been caught earlier if someone was actually looking at the right data. Now you know how to make that data visible!












