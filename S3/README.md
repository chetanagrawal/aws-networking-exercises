1. Download sample website content zip file. Unzip it locally and then upload all the files and folders directly in the S3 bucket without any top level subfolders.

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
