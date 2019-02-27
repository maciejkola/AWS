# CheatSheet

## S3
###### Make bucket:
> aws s3 mb s3://*bucket_name*

###### Enable bucket for static website hosting:
> aws s3 website s3://*bucket_name* --index-document index.html

###### Changing bucket policy:
> aws s3api put-bucket-policy --bucket *bucket_name* --policy file://*path_to_json_file*

###### Example policy: public access (for web hosting):
```
{
    "Version": "2008-10-17",
    "Id": "PolicyForWebHosting",
    "Statement": [
        {
            "Sid": "1",
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::cloudfront:user/CLOUD FRONT USER"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::BUCKET_NAME/*"
        }
    ]
}
```
