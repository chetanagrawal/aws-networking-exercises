1. Download sample website content zip file. Unzip it locally and then upload all the files and folders directly into the S3 bucket without any further top level subfolders. See below:

![image](https://github.com/chetanagrawal/aws-networking-exercises/assets/5749307/d7c8aa3b-f17e-4950-86ae-c5ffe7351ea5)

2. Bucket policy
```
{
  "Version":"2012-10-17",
  "Statement":[{
        "Sid":"PublicReadGetObject",
        "Effect":"Allow",
        "Principal": "*",
      "Action":["s3:GetObject"],
      "Resource": ["arn:aws:s3:::your-bucket-name/*" ]
   }
  ]
}
```
>[!IMPORTANT]
> Replace your-bucket-name with your actual bucket name
