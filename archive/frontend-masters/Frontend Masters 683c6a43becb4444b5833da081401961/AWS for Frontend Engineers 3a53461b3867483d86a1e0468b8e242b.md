# AWS for Frontend Engineers

# Repository

[https://github.com/stevekinney/aws-v2](https://github.com/stevekinney/aws-v2)

# S3

S3 is effectively a *key/value* store where the value is an object and an object is a file.

At a high-level:

- In our S3 account, you have your buckets.
- You can put objects (read: *files*) in your buckets.
- You can read from your buckets as well.
- You can host web pages out of your buckets.

A slightly lower, but still high level:

- Infinitely scalable.
- Files can be as small as zero bytes or as large as 5 terabytes.
- 99.9% availability (built for 99.99%).
- 99.999999999% durability.

S3 has a few different storage tiers:

- **Standard**—this is what we’ll be using today.
- **Standard-Infrequent Access**.
- **One Zone-Infrequent Access**.
- **Glacier Instant Retrieval**
- **Glacier Flexible Retrieval**
- **Glacier Deep Archive**
- **S3 Intelligent-Tiering**—I don't know, but I'll pay you to optimize this for me, AWS.

### Data consistency on S3

- Putting new objects in S3 is immediate. You’ll get back a 200 response.
- Updating and removing objects is eventually consistent. Users might get an old version. (This has literally never happened to me.)

### Costs

- Uploading to S3 is free.
- You get charged for storage.
- You get charged for requests. (We’ll learn how to mitigate this later.)

## Further reading

- [AWS S3 Redirection Rules](https://docs.aws.amazon.com/AmazonS3/latest/userguide/how-to-page-redirect.html)

# Route 53

[aws-v2/09 - Route 53.md at main · stevekinney/aws-v2](https://github.com/stevekinney/aws-v2/blob/main/content/09%20-%20Route%2053.md)

# Cloudfront

After creating an distribution you will need to update s3 policies as follows

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::shines1729/*"
        },
        {
            "Sid": "AllowCloudFrontServicePrincipal",
            "Effect": "Allow",
            "Principal": {
                "Service": "cloudfront.amazonaws.com"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::shines1729/*",
            "Condition": {
                "StringEquals": {
                    "AWS:SourceArn": "arn:aws:cloudfront::570517961192:distribution/E32470YMCYIPIQ"
                }
            }
        }
    ]
}
```