# Lab M7.10 - FinOps Automation and Reporting

## What I Did
In this lab, I built a fully automated FinOps workflow to replace manual cost tracking and reporting.

- Created three Lambda functions:
  - **daily-cost-report** → generates daily cost insights using Cost Explorer
  - **idle-resource-checker** → identifies unused resources (EBS, EC2, Elastic IPs)
  - **finops-weekly-digest** → combines cost data and waste detection into one report
- Configured EventBridge rules to run everything automatically (daily and weekly)
- Set up SNS to send all reports via email
- Tested each component manually to ensure the full flow works correctly

## Architecture
- EventBridge → Lambda (daily-cost-report) → SNS → Email (daily)
- EventBridge → Lambda (idle-resource-checker) → SNS → Email (weekly)
- EventBridge → Lambda (finops-weekly-digest) → SNS → Email (weekly)

## Key Findings
- The Cost Explorer API makes it easy to automate cost visibility without relying on the console
- Idle resources can silently generate unnecessary costs if not monitored
- Combining cost data and cleanup insights into a single report makes it easier to take action
- EventBridge provides a simple way to automate recurring workflows without managing infrastructure

## Results
- Daily cost reporting is fully automated
- Idle resources are detected on a weekly basis
- A combined weekly digest provides both financial and operational insights
- The entire setup runs serverless with no manual intervention required

## Screenshots
- screenshots/daily-cost-email.png
- screenshots/idle-resource-email.png
- screenshots/weekly-digest-email.png

## How to Test

aws lambda invoke --function-name daily-cost-report --payload '{}' /tmp/daily.json  
aws lambda invoke --function-name idle-resource-checker --payload '{}' /tmp/idle.json  
aws lambda invoke --function-name finops-weekly-digest --payload '{}' /tmp/digest.json  

## Conclusion
This lab demonstrates how FinOps processes can be automated end-to-end. By combining cost tracking, waste detection, and reporting into a single workflow, it provides a practical setup that improves cost visibility and helps reduce unnecessary cloud spending.