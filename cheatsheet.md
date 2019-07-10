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
  "Version":"2012-10-17",
  "Statement":[{
	"Sid":"PublicReadGetObject",
        "Effect":"Allow",
	  "Principal": "*",
      "Action":["s3:GetObject"],
      "Resource":["arn:aws:s3:::BUCKET_NAME/*"]
    }]
}
```

###### Copying local file to bucket:
> aws s3 cp *local_file* s3://*bucket_name*/*destination_file*

###### Copying bucket content to local folder:
> aws s3 sync s3://*bucket_name* .

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

## CloudFront
###### CloudFront S3 Origin can be set in two ways:
as website endpoint: and then it cannot be used with origin access identity
> example-bucket.s3-website-us-east-1.amazonaws.com

as REST API endpoint: and this way you can use origin access identity to restrict access to S3 bucket
> example-bucket.s3.amazonaws.com
CloudFront will create proper policy for S3 bucket

###### Remember to:
- on CloudFront: set alias (cname) on distribution (to example.com)
- on CloudFront: set Default Root Object to index.html
- on Route 53: set Route 53 hosted zone with example.com as alias (IPv4 & IPv6)

## Cognito
###### sign up user:
> aws cognito-idp sign-up --client-id *Application-Client-ID* --region *region* --username *email* --password *password-for-the-user* --user-attributes Name="email",Value="*email*"

response should look like this:
```
{
    "UserConfirmed": false,
    "CodeDeliveryDetails": {
        "Destination": "x***@x***.xxx",
        "DeliveryMedium": "EMAIL",
        "AttributeName": "email"
    },
    "UserSub": "xxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
}
```

###### authenticate user with the pool (make user active):
> aws cognito-idp admin-confirm-sign-up --region *region* --user-pool-id *user-pool-id* --username *mail*


###### get user token (valid for 30 days by default)
> aws cognito-idp admin-initiate-auth --region *region* --cli-input-json file://auth.json
where auth.json:
```
{
    "UserPoolId": "*user-pool-id*",
    "ClientId": "*client-id*",
    "AuthFlow": "ADMIN_NO_SRP_AUTH",
    "AuthParameters": {
        "USERNAME": "blabla",
        "PASSWORD": "bleble"
    }
}
```
