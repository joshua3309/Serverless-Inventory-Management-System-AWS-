📦 Serverless Inventory Management System on AWS
A fully serverless architecture for managing store inventory across multiple locations using AWS. This project demonstrates real-time ingestion, processing, alerting, and secure display of inventory data — all without managing servers.

🧠 Overview
This system leverages key AWS services including Lambda, S3, DynamoDB, SNS, and Cognito to provide:

📁 Automatic ingestion of inventory files from S3

🗃️ Storage and querying of inventory data in DynamoDB

📢 Out-of-stock notifications via Amazon SNS

🛡️ Secure access to a live dashboard via Amazon Cognito

🎯 Objectives
Design a scalable and event-driven serverless solution

Automate file processing and database updates

Notify stakeholders about out-of-stock items

Display real-time inventory through an authenticated dashboard

🔧 Architecture Workflow
text
Copy code
+-------------------+
|  Inventory File   |
|  (CSV in S3)      |
+--------+----------+
         |
         v
+-------------------+      DynamoDB Streams      +-------------------+
|   Lambda:         |--------------------------->|   Lambda:         |
|   Load-Inventory  |                            |   Check-Stock     |
+--------+----------+                            +--------+----------+
         |                                                |
         v                                                v
+-------------------+                             +-------------------+
|   DynamoDB Table  |                             |       SNS         |
|   (Inventory)     |                             | (Out-of-Stock Msg)|
+--------+----------+                             +--------+----------+
         |                                                |
         v                                                v
+-------------------+                             +-------------------+
| Cognito + Web UI  | <---------------------------|  Email/SMS Alerts |
+-------------------+     Authenticated Access    +-------------------+
⚙️ AWS Services Used
Service	Role
Amazon S3	Stores uploaded CSV inventory files
AWS Lambda	Processes file uploads and checks inventory status
DynamoDB	NoSQL database to store and stream inventory data
Amazon SNS	Sends alerts for out-of-stock items via email or SMS
Amazon Cognito	Manages user authentication for accessing the inventory dashboard

📂 Project Structure
graphql
Copy code
.
├── load_inventory_lambda.py       # Lambda: Load inventory from S3 to DynamoDB
├── check_stock_lambda.py         # Lambda: Check stock and publish SNS alerts
├── example-inventory.csv         # Sample inventory CSV file
              
🧪 Sample Inventory File (example-inventory.csv)
inventory-berlin.csv
inventory-calcutta.csv
inventory-karachi.csv
inventory-pusan.csv
inventory-shanghai.csv
inventory-springfield.csv
This function is invoked when a CSV file is uploaded to the designated S3 bucket. It reads and inserts each record into the Inventory DynamoDB table.

✅ Python 3.x | boto3, csv, json

🚨 Lambda: Check-Stock (Triggered by DynamoDB Stream)
Triggered by inserts into DynamoDB. If an item's count is 0, it sends an alert message via Amazon SNS.

🔐 Authenticated Dashboard (Optional)
Use Amazon Cognito to provide secure access to a real-time inventory dashboard (can be built using React + Amplify + DynamoDB SDK).

🚀 Deployment Tips
Configure your S3 bucket and enable trigger to the Load-Inventory Lambda.

Create the DynamoDB table (Inventory) with:

Partition key: Store

Sort key: Item

Enable DynamoDB Streams to trigger the Check-Stock Lambda.

Set up an SNS topic named NoStock and subscribe your email/SMS.

Use Cognito to authenticate users for the dashboard.

✅ Future Enhancements
🔍 Add query filters to the dashboard (e.g., by store or item)

📈 Include graphs to visualize inventory trends

💬 Add Slack integration for alerts

🧪 Include automated tests for Lambda functions

📎 Resources
AWS Lambda Docs

Amazon S3 Docs

Amazon DynamoDB Docs

Amazon SNS Docs

Amazon Cognito Docs
