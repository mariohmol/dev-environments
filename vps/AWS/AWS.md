

## CloudFront

Create a new CloudFront distribution like the example:

**General**
Origin Domain: test.mydomain.com
Name: test.mydomain.com
Viewer protocol policy: Redirect HTTP to HTTPS
Alternate domain names: Associate an domain
Default root object: index.html

**Behaviors**

**Errors Page**
HTTP  error code: 403/404
Customize error response: yes
Response page path:  /index.html
HTTP Response code: 200


Allowed HTTP methods: All

```json
{
    "Version": "2022-05-09",
    "Statement": [
        {
            "Sid": "PublicReadGetObject",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::mydomain.com/*"
        }
    ]
}
```

After you create the CF, copy the files to S3 and invalidate the current files:
```sh
# Copy all files for the first time from a local folder to an S3 bucket
aws s3 cp <local_files> s3://<bucket_name>/<folder - if exists>

# Sync the local folder to an existing S3 bucker
aws s3 sync <local_folder> s3://<bucket_name>/<folder - if exists>


# Invalidate the current files on CloudFront
# How to use it
aws cloudfront create-invalidation --distribution-id <CloudFront distribution id> --paths <list invalidates paths>
# example with specific files
aws cloudfront create-invalidation --distribution-id DIST_ID --paths "/example-path/example-file.jpg" "/example-path/example-file2.png"
# example of all files
aws cloudfront create-invalidation --distribution-id DIST_ID --paths "/*"
```




