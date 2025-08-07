
markdown
Copy
Edit
# Real-Time Personalised Customer Experience Engine (AWS-Powered)

## 📌 Summary
This project simulates a personalised marketing engine using serverless AWS tools. It dynamically tailors product offers and notifications based on user interests and location, improving customer engagement in retail. Built for the Customer Services & Experience unit, it integrates API Gateway, Lambda, DynamoDB, and CloudWatch, with advanced logging and tracing for monitoring.

---

## 🧠 Architecture Overview

**Figure A.1: High-Level Architecture Diagram**  
*This figure shows how user interest data flows from the frontend through API Gateway to Lambda, DynamoDB, and back. It also shows CloudWatch logging for tracing requests and a custom metrics alarm.*  
`![Figure A.1 Placeholder – Insert image URL here]`

---

## 🛠️ AWS Services Used

- **Amazon API Gateway**: REST API endpoint to receive customer preferences.
- **AWS Lambda**: Processes requests and stores data in DynamoDB.
- **Amazon DynamoDB**: Stores user interest and location data.
- **Amazon CloudWatch**: Tracks metrics, logs, alarms, and traces.

---

## 🔄 System Flow Summary

1. User submits interest via frontend.
2. API Gateway forwards to Lambda.
3. Lambda writes data to DynamoDB.
4. CloudWatch captures logs, metrics, and trace for observability.

---

## 🧪 Sample JSON Payload

```json
{
  "customerId": "12345",
  "interest": "electronics",
  "location": "Cape Town"
}
📂 Lambda Function Snippet (Python)
python
Copy
Edit
import boto3
import json
import os

dynamodb = boto3.resource('dynamodb')
table = dynamodb.Table(os.environ['CustomerTable'])

def lambda_handler(event, context):
    data = json.loads(event['body'])
    table.put_item(Item={
        'customerId': data['customerId'],
        'interest': data['interest'],
        'location': data['location']
    })
    return {
        'statusCode': 200,
        'body': json.dumps({'message': 'Success'})
    }
📊 CloudWatch Monitoring & Logs
Figure A.2 – InsertError Alarm Configuration
Shows CloudWatch alarm setup for failed insertions in Lambda logs. This ensures real-time notification of function errors affecting customer data handling.
![Figure A.2 Placeholder – Insert image URL here]

Figure A.3 – API Gateway Logging Enabled
CloudWatch log level is changed from Off to “Errors and Info”, enabling both success and error logs for all incoming API requests.
![Figure A.3 Placeholder – Insert image URL here]

Figure A.4 – API Gateway Tracing Activated
X-Ray tracing enabled through API Gateway for the recommendation API. This helps track end-to-end request journeys for performance insights.
![Figure A.4 Placeholder – Insert image URL here]

Figure A.5 – Full Observability Enabled (API Gateway)
This screenshot shows the active state of all observability settings after configuration.
Under the Logs and Tracing panel:
• CloudWatch Logs set to Errors and info
• Detailed Metrics shown as Active
• Data Tracing marked as Active
![Figure A.5 Placeholder – Insert image URL here]

Figure A.6 – CloudWatch Metrics for Success Tracking
Visualises a custom CloudWatch metric tracking successful Lambda executions, confirming API health over time.
![Figure A.6 Placeholder – Insert image URL here]

Figure A.7 – Default Logs and Tracing Configuration
Shows the default API Gateway stage settings before observability configuration. Logging, tracing, and metrics are disabled at this stage.
![Figure A.7 Placeholder – Insert image URL here]

Figure A.8 – Successful Logging Update Notification
Confirms CloudWatch Logs, Metrics, and Tracing were successfully enabled for the dev stage of the API Gateway.
![Figure A.8 Placeholder – Insert image URL here]

Figure A.9 – Final Observability State (Post-Configuration)
Logs and Tracing configuration panel after enabling all options.
• CloudWatch Logs: Errors and Info
• Metrics: Enabled
• Tracing: Active (X-Ray)
![Figure A.9 Placeholder – Insert image URL here]

Figure A.10 – CloudWatch Dashboard (LiveLoungeAPI)
Confirms real-time observability with API latency, error rate, and invocation count tracking.
![Figure A.10 Placeholder – Insert image URL here]

✅ Monitoring Summary
Metric	Description	Threshold
Lambda Error Rate	Tracks failed inserts to DynamoDB	< 1%
API Gateway Latency	Monitors API response time	< 300ms
X-Ray Tracing Coverage	% of requests traced end-to-end	> 95%
CloudWatch Alarm Trigger	Detects anomalies in request handling	Immediate

🔐 Compliance Considerations
POPIA: Data captured is anonymised, stored securely with IAM-based access.

Audit Logging: All API calls and Lambda executions are logged.

Least Privilege: Lambda roles are scoped to only DynamoDB put access.

💡 Future Enhancements
Amazon Personalize for advanced recommendations

Integrate SageMaker for user segmentation

Add mobile push via Amazon Pinpoint

📁 Suggested Repo Structure
python
Copy
Edit
customer-personalisation-api/
├── lambda/                    # Lambda function code
├── docs/                     # Diagrams and screenshots
├── cloudwatch/               # Monitoring config and metrics
├── frontend/                 # (Optional) Web form for input
└── README.md                 # This file
🧑‍💻 Author
Joshua Barradas
AWS Projects Portfolio
Cloud & AI Solutions Architect
LinkedIn: linkedin.com/in/joshua-barradas-433292212
