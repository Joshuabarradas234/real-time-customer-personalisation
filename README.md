# Real-Time Personalised Customer Experience Engine (AWS-Powered)

## 📌 Summary
This project simulates a personalised marketing engine using serverless AWS tools. It dynamically tailors product offers and notifications based on user interests and location, improving customer engagement in retail. Built for the Customer Services & Experience unit, it integrates API Gateway, Lambda, DynamoDB, and CloudWatch, with advanced logging and tracing for monitoring.

---

## 🧠 Architecture Overview

**Figure A.1: Architecture of the real-time personalized recommendation system on AWS**  
*This figure shows how user interest data flows from the frontend through API Gateway to Lambda, DynamoDB, and back. It also shows CloudWatch logging for tracing requests and a custom metrics alarm.*  
![Figure A.1 – Architecture Diagram](Figure%201.png)

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

**Figure A.2: : AWS API Gateway “Logs and Tracing” settings (initial state) for the dev deployment stage of the recommendation API**
*Shows alarm triggering on Lambda insert failures.*  
![Figure A.2 – InsertError Alarm](Figure%202.png)

**Figure A.3: : Updating the log level in API Gateway to “Errors and info”**  
*Shows log level updated to 'Errors and info' with execution logging enabled.*  
![Figure A.3 – API Gateway Logging](Figure%203.png)

**Figure A.4: Confirmation of successful logging update in API Gateway**  
*Enables request tracing across API Gateway → Lambda → DynamoDB.*  
![Figure A.4 – X-Ray Tracing](Figure%204.png)

**Figure A.5: : Logs and Tracing configuration panel after enabling all options**  
*Confirms data ingestion, storage, and log stream completion.*  
![Figure A.5 – End-to-End Log](Figure%205.png)

**Figure A.6: : CloudWatch verification of enabled logging for the LiveLoungeAPI**  
*Tracks successful customer interest submissions.*  
![Figure A.6 – CloudWatch Success Metric](Figure%206.png)

**Figure A.7: Logs & Tracing Settings Page**  
*Shows settings on the API Gateway stage level.*  
![Figure A.7 – Dev Stage Logging](Figure%207.png)

**Figure A.8:Successful Logging Update Notification**  
*Postman POST response confirms end-to-end delivery and response.*  
![Figure A.8 – Postman Test](Figure%208.png)

**Figure A.9: Logs & Tracing Section of the dev Stage (Post-Configuration)**  
*Shows structure of CustomerInterest table: customer_id, timestamp, interest, location.*  
![Figure A.9 – DynamoDB Table](Figure%209.png)

**Figure A.10: CloudWatch Logs Summary View**  
*Aggregated view showing Lambda logs for multiple inserts.*  
![Figure A.10 – CloudWatch Summary](Figure%2010.png)

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
