<h1 align="center">AWS Lab</h1>

## Task 1 
```attatch IAM role to an ec2 instance which enables it to interact with an s3 bucket, create a new bucket and list buckets from cli```

https://github.com/user-attachments/assets/ecad4b55-970f-4bb8-b364-8432601de8cc

----

## Task 2

```Create a Lambda Function to Send an Email on S3 Object Upload
Create an S3 bucket that will trigger the Lambda function.
Create a Lambda function that sends an email notification when an object is uploaded to the S3 bucket.
Set up an S3 event notification to trigger the Lambda function when a new object is uploaded.
Test the setup by uploading an object to the S3 bucket and verifying that an email notification is received.

```


https://github.com/user-attachments/assets/16909a91-14bd-44f1-ba32-175a1f41c16f

## Excludes 
#### policies added to IAM Role attached to lambda not included in video 
- cloud watch log
- EC2
- Pinpoint Email
- Route 53
- S3
- SES
- SES Mail Manager
- SES v2
----
## lambda function deployment 
```bash
import boto3
import json

def lambda_handler(event, context):
    
    for e in event["Records"]:
        bucketName = e["s3"]["bucket"]["name"]
        objectName = e["s3"]["object"]["key"]
        eventName = e["eventName"]
    
    bClient = boto3.client("ses")
    
    eSubject = 'AWS' + str(eventName) + 'Event'
    
    eBody = """
        <br>
        Hey,<br>
        
        Welcome to AWS S3 notification lambda trigger<br>
        
        We are here to notify you that {} an event was triggered.<br>
        Bucket name : {} <br>
        Object name : {}
        <br>
    """.format(eventName, bucketName, objectName)
    
    send = {"Subject": {"Data": eSubject}, "Body": {"Html": {"Data": eBody}}}
    result = bClient.send_email(Source= "aligalayand@gmail.com", Destination= {"ToAddresses": ["aligalayand@gmail.com"]}, Message= send)
    
    return {
        'statusCode': 200,
        'body': json.dumps(result)
    }

```
