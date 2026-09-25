# Ticket
## Symptoms: 
Support-Tester User attempted to create a S3 Bucket in US-EAST-2. 

## Investigations: 

Reviewed the cloud trail log September 23, 2026, 14:43:34 (UTC-07:00)and found error 
```bash
"User: arn:aws:sts:::assumed-role/AWSReservedSSO_ReadOnlyAccess_2c0f05e2ef253c2d/Support-tester is not authorized to perform: s3:CreateBucket on resource: \"arn:aws:s3:::patsdemoerro\" because no identity-based policy allows the s3:CreateBucket action".
```
Led me to investigate Users group and permission policy set. 

## Root Cause: 

The Support-tester user is not able to create S3 Buckets in the AWS infrastructure due to security policy. 

## Resolution: 
Informed user that due to company security policy that testers are currently restricted to setting up buckets for budgetary and security concerns. As well as they believe the testing group needs this ability to speak with the management to so we can change the access.

## Logs

CreateBucket Info
Details Info
Event time
September 23, 2026, 14:43:34 (UTC-07:00)
User name
Support-tester
Event name
CreateBucket
Event source
s3.amazonaws.com
AWS access key
ASIASHQTJ74OLNV475R5
Source IP address
70.170.150.0
Event ID
b67be2b5-6a80-4902-8964-77782af0ff86
Request ID
N5T0STDQ3ZV9J6F5
AWS region
us-east-2
Error code
AccessDenied
Read-only
false
Resources referenced (1) Info
Resources referenced describes the name or ID of resources that were read or changed by an event

Resource type
	
Resource name
	
AWS Config resource timeline

AWS::S3::Bucket
patsdemoerro 
Enable AWS Config resource recording 
Event record Info
Copy
JSON view
```JSON
{
    "eventVersion": "1.11",
    "userIdentity": {
        "type": "AssumedRole",
        "principalId": "AROASHQTJ74OJOHCZ4CXO:Support-tester",
        "arn": "arn:aws:sts:::assumed-role/AWSReservedSSO_ReadOnlyAccess_2c0f05e2ef253c2d/Support-tester",
        "accountId": "",
        "accessKeyId": "ASIASHQTJ74OLNV475R5",
        "sessionContext": {
            "sessionIssuer": {
                "type": "Role",
                "principalId": "AROASHQTJ74OJOHCZ4CXO",
                "arn": "arn:aws:iam:::role/aws-reserved/sso.amazonaws.com/AWSReservedSSO_ReadOnlyAccess_2c0f05e2ef253c2d",
                "accountId": "",
                "userName": "AWSReservedSSO_ReadOnlyAccess_2c0f05e2ef253c2d"
            },
            "attributes": {
                "creationDate": "2026-09-23T21:42:28Z",
                "mfaAuthenticated": "false"
            }
        },
        "onBehalfOf": {
            "userId": "c46854f8-4001-7080-57c9-f74de5be582b",
            "identityStoreArn": "arn:aws:identitystore:::identitystore/d-90667e1d76"
        }
    },
    "eventTime": "2026-09-23T21:43:34Z",
    "eventSource": "s3.amazonaws.com",
    "eventName": "CreateBucket",
    "awsRegion": "us-east-2",
    "sourceIPAddress": "70.170.150.0",
    "userAgent": "[Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/152.0.0.0 Safari/537.36]",
    "errorCode": "AccessDenied",
    "errorMessage": "User: arn:aws:sts:::assumed-role/AWSReservedSSO_ReadOnlyAccess_2c0f05e2ef253c2d/Support-tester is not authorized to perform: s3:CreateBucket on resource: \"arn:aws:s3:::patsdemoerro\" because no identity-based policy allows the s3:CreateBucket action",
    "requestParameters": {
        "CreateBucketConfiguration": {
            "LocationConstraint": "us-east-2",
            "xmlns": "http://s3.amazonaws.com/doc/2006-03-01/"
        },
        "bucketName": "patsdemoerro",
        "Host": "patsdemoerro.s3.us-east-2.amazonaws.com"
    },
    "responseElements": null,
    "additionalEventData": {
        "SignatureVersion": "SigV4",
        "referrer": "https://us-east-2.console.aws.amazon.com/",
        "CipherSuite": "TLS_AES_128_GCM_SHA256",
        "bytesTransferredIn": 191,
        "startTime": 1790199814363,
        "AuthenticationMethod": "AuthHeader",
        "endTime": 1790199814385,
        "x-amz-id-2": "YAfy96SMmvx/LYj8Y1bcgoAe4YP6NBBDd2qvE87D/JTJn1v30zdpO2v+w8fYI8JgIOZQFpWzikA=",
        "httpStatusCode": 403,
        "bytesTransferredOut": 490
    },
    "requestID": "N5T0STDQ3ZV9J6F5",
    "eventID": "b67be2b5-6a80-4902-8964-77782af0ff86",
    "readOnly": false,
    "eventType": "AwsApiCall",
    "managementEvent": true,
    "recipientAccountId": "",
    "eventCategory": "Management",
    "tlsDetails": {
        "tlsVersion": "TLSv1.3",
        "cipherSuite": "TLS_AES_128_GCM_SHA256",
        "clientProvidedHostHeader": "patsdemoerro.s3.us-east-2.amazonaws.com"
    }
}
```


