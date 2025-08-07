# Real-Time Personalised Customer Experience Engine (AWS-Powered)

## 📌 Summary
This project simulates a personalised marketing engine using serverless AWS tools. It dynamically tailors product offers and notifications based on user interests and location, improving customer engagement in retail. Built for the Customer Services & Experience unit, it integrates API Gateway, Lambda, DynamoDB, and CloudWatch, with advanced logging and tracing for monitoring.

---

## 🧠 Architecture Overview

**Figure 1: High-Level Architecture Diagram**  
*Architecture of the real-time personalized recommendation system on AWS. The workflow includes an API Gateway endpoint (/recommend) protected by an API Key (usage plan), invoking a Lambda function that calls Amazon Personalize to get product recommendations. Amazon CloudWatch is used for logging and monitoring (execution logs and metrics), ensuring the system’s operations are observable and reliable.*  
![Figure 1 – Architecture Diagram](Figure%201.png)

---

## 🛠️ AWS Services Used

- **Amazon API Gateway**: REST API endpoint to receive customer preferences.
- **AWS Lambda**: Processes requests and stores data in DynamoDB.
- **Amazon DynamoDB**: Stores customer interest and location data.
- **Amazon CloudWatch**: Provides monitoring, custom metrics, logs, and tracing.
- **AWS X-Ray**: Enables tracing across Lambda/API Gateway for debugging.

---

## 💡 Key Features

- POST endpoint `/recommend` to receive customer interest and location
- Serverless pipeline (API → Lambda → DynamoDB)
- Real-time logging and error monitoring with CloudWatch
- Tracing via AWS X-Ray for full observability
- Custom CloudWatch metrics and alarms for failures/success
- Data-driven personalisation simulation

---

## 🧪 Sample Payload (JSON)

```json
{
  "customer_id": "12345",
  "interest": "organic_food",
  "location": "Cape Town"
}
```

---

## 🧾 Lambda Snippet (Python)

```python
import json
import boto3
import time
import os

def lambda_handler(event, context):
    dynamodb = boto3.resource('dynamodb')
    table = dynamodb.Table(os.environ['CustomerInterestTable'])

    payload = json.loads(event['body'])
    timestamp = int(time.time())

    item = {
        'customer_id': payload['customer_id'],
        'timestamp': timestamp,
        'interest': payload['interest'],
        'location': payload['location']
    }

    try:
        table.put_item(Item=item)
        return {
            'statusCode': 200,
            'body': json.dumps({'message': 'Customer preference recorded.'})
        }
    except Exception as e:
        return {
            'statusCode': 500,
            'body': json.dumps({'error': str(e)})
        }
```

---

## 📊 CloudWatch Monitoring & Metrics

**Figure 2: CloudWatch Alarm for InsertError**  
*AWS API Gateway “Logs and Tracing” settings (initial state) for the dev deployment stage of the recommendation API. At this stage, CloudWatch Logs are disabled and no detailed metrics or tracing are enabled – this is the default configuration before any monitoring features are turned on.*  
![Figure 2 – InsertError Alarm](Figure%202.png)

**Figure .3: Logging level in API Gateway**  
*: Updating the log level in API Gateway to “Errors and info”. In this screenshot, the CloudWatch log level is being changed from Off to Errors and Info, which enables capturing both successful requests and errors. This step is part of configuring the API Gateway to record detailed execution information for the recommendation service.*  
![Figure .3 – API Gateway Logging](Figure%203.png)

**Figure .4: Enabling X-Ray tracing in Lambda**  
*: Confirmation of successful logging update in API Gateway. After saving the settings, AWS provides a notification confirming that logging and tracing have been enabled for the stage. This includes enabling CloudWatch Logs at the specified level, turning on Detailed Metrics, and activating X-Ray tracing for request workflows.*  
![Figure .4 – X-Ray Tracing](Figure%204.png)

**Figure .5: CloudWatch log showing full end-to-end receipt**  
*Logs and Tracing configuration panel after enabling all options. This screenshot shows that CloudWatch Logs are now set to capture Errors and Info, Detailed Metrics collection is active, and Data Tracing (X-Ray) is enabled for the dev stage. The API is now fully instrumented to collect performance and diagnostic data.*  
![Figure .5 – End-to-End Log](Figure%205.png)

**Figure .6: Custom CloudWatch metric for success tracking**  
*CloudWatch verification of enabled logging for the LiveLoungeAPI. This view confirms that the LiveLoungeAPI’s CloudWatch log group and metrics are receiving data. With logging and monitoring active, the team can use CloudWatch dashboards, set up alarms for high error rates or latency, and perform root cause analysis on any invocation issues in the recommendation system.*  
![Figure .6 – CloudWatch Success Metric](Figure%206.png)

**Figure .7: Logs & Tracing Settings Page**
*This screenshot shows the default Logs and Tracing settings for the dev stage of the LiveLoungeAPI in API Gateway. At this point, CloudWatch logs are disabled, and metrics or tracing options have not been activated yet. This serves as the initial baseline prior to enabling observability features.*  
![Figure .7 – Logs & Tracing Settings Page](Figure%207.png)

**Figure .8: Successful Logging Update Notification**  
*This screenshot confirms that logging and tracing settings were successfully updated for the dev stage. This includes enabling:
•	CloudWatch Logs (errors and info)
•	Detailed Metrics for latency and error monitoring
•	Data Tracing for full execution details per request*  
![Figure .8 – Successful Logging Update Notification ](Figure%208.png)

**Figure .9: Logs & Tracing Section of the dev Stage (Post-Configuration)**  
*This screenshot shows the active state of all observability settings after configuration. Under the Logs and Tracing panel:
•	CloudWatch Logs are set to Errors and info logs
•	Detailed Metrics are shown as Active
•	Data Tracing is marked as Active, capturing full lifecycle traces for each API request*  
![Figure .9 – Logs & Tracing Section of the dev Stage (Post-Configuration)](Figure%209.png)

**Figure .10: Enabled CloudWatch Logging Display**  
*This final view confirms that CloudWatch observability features are fully operational for the LiveLoungeAPI. These settings allow real-time API monitoring via AWS CloudWatch, enabling dashboards, custom alarms, and root cause analysis for failed or slow requests.*  
![Figure .10 – Enabled CloudWatch Logging Display](Figure%2010.png)
---

## 🔐 IAM & Security

- Lambda IAM role with least privilege (PutItem only)
- CloudWatch permissions for logs/metrics
- Environment variables for table names
- No PII exposed in logs or metrics

---

## 📁 Suggested Repo Structure

```
personalised-customer-api/
├── lambda/                # Lambda function code
├── images/                # Architecture and log screenshots
├── monitoring/            # CloudWatch metrics setup
├── docs/                  # Word docs, explanations
└── README.md              # This file
```

---

## 🔗 Use Cases

- Real-time targeted promotions
- Location-based marketing messages
- Event-triggered customer engagement

---

## 🧩 Future Extensions

- Add Personalize or Bedrock for actual ML-based recommendations
- Store historical data in S3 for analytics
- Integrate with SES or SNS for outbound campaigns

---

## 🙋 Contact

**Joshua Barradas**  
GitHub: [@Joshuabarradas234](https://github.com/Joshuabarradas234)  
Email: barradasjoshua48@gmail.com  
Location: Leeds, UK
