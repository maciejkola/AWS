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

###### Copying local file to bucket:
> aws s3 cp *local_file* s3://*bucket_name*/*destination_file*

###### URL for accessing file:
> http://*buckt_name*.s3-website.*aws_region*.amazonaws.com

###### User Policy to upload-only file to folder in specific bucket:
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "AllowListingOfFolder",
            "Action": [
                "s3:ListBucket"
            ],
            "Effect": "Allow",
            "Resource": [
                "arn:aws:s3:::BUCKET_NAME"
            ],
            "Condition": {
                "StringLike": {
                    "s3:prefix": [
                        "FOLDER_NAME/*"
                    ]
                }
            }
        },
        {
            "Sid": "AllowCopyToFolder",
            "Effect": "Allow",
            "Action": [
                "s3:PutObject"
            ],
            "Resource": [
                "arn:aws:s3:::BUCKET_NAME/FOLDER_NAME/*"
            ]
        }
    ]
}
```

## CloudFormation
###### create stack:
> ?
