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

**Figure A.2: CloudWatch Alarm for InsertError**  
*Shows alarm triggering on Lambda insert failures.*  
`![Figure A.2 Placeholder – Insert image URL here]`

**Figure A.3: Logging level in API Gateway**  
*Shows log level updated to 'Errors and info' with execution logging enabled.*  
`![Figure A.3 Placeholder – Insert image URL here]`

**Figure A.4: Enabling X-Ray tracing in Lambda**  
*Enables request tracing across API Gateway → Lambda → DynamoDB.*  
`![Figure A.4 Placeholder – Insert image URL here]`

**Figure A.5: CloudWatch log showing full end-to-end receipt**  
*Confirms data ingestion, storage, and log stream completion.*  
`![Figure A.5 Placeholder – Insert image URL here]`

**Figure A.6: Custom CloudWatch metric for success tracking**  
*Tracks successful customer interest submissions.*  
`![Figure A.6 Placeholder – Insert image URL here]`

**Figure A.7: Dev stage with logging and tracing enabled**  
*Shows settings on the API Gateway stage level.*  
`![Figure A.7 Placeholder – Insert image URL here]`

**Figure A.8: API test result in Postman**  
*Postman POST response confirms end-to-end delivery and response.*  
`![Figure A.8 Placeholder – Insert image URL here]`

**Figure A.9: DynamoDB table view**  
*Shows structure of CustomerInterest table: customer_id, timestamp, interest, location.*  
`![Figure A.9 Placeholder – Insert image URL here]`

**Figure A.10: CloudWatch Logs Summary View**  
*Aggregated view showing Lambda logs for multiple inserts.*  
`![Figure A.10 Placeholder – Insert image URL here]`

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