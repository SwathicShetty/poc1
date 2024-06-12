Setting Up an AWS S3 Bucket and Accessing it Through CloudFront
Introduction
This documentation provides step-by-step instructions for setting up an Amazon S3 bucket and configuring it to be accessed through Amazon CloudFront, a content delivery network (CDN) service. By following these steps, you can efficiently deliver content stored in an S3 bucket to users worldwide with low latency.

Prerequisites
An AWS account with appropriate permissions to create S3 buckets and CloudFront distributions.
AWS CLI installed and configured on your local machine (optional but recommended).
Step 1: Create an S3 Bucket
Sign in to the AWS Management Console.
Open the S3 Console.
Click on "Create bucket".
Enter a unique bucket name.
Choose the region where you want to create the bucket (e.g., eu-north-1 for Stockholm).
Click "Create bucket".
Step 2: Upload Content to the S3 Bucket
Navigate to the newly created bucket in the S3 Console.
Click "Upload".
Select the files or folders you want to upload.
Click "Upload" to start the upload process.
Step 3: Create an Origin Access Identity (OAI) in CloudFront
Go to the CloudFront Console.
Navigate to "Origin Access Identity".
Click "Create Origin Access Identity".
Provide a name for the OAI (optional).
Click "Create".
Step 4: Configure Bucket Policy for CloudFront OAI
Navigate to the Permissions tab of your S3 bucket.

Click "Bucket Policy".

Add a bucket policy that allows access from CloudFront using the OAI:

json
Copy code
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Principal": {
                "AWS": "arn:aws:iam::cloudfront:user/CloudFront Origin Access Identity YOUR_OAI_ID"
            },
            "Action": "s3:GetObject",
            "Resource": "arn:aws:s3:::your-bucket-name/*"
        }
    ]
}
Replace YOUR_OAI_ID with the OAI ID and your-bucket-name with your S3 bucket name.

Step 5: Create a CloudFront Distribution
Go to the CloudFront Console.
Click "Create Distribution".
Select "Web" as the delivery method.
Configure the origin settings:
Origin Domain Name: Select your S3 bucket from the dropdown.
Origin Path: Leave blank.
Origin ID: Auto-populated.
Configure default cache behavior:
Viewer Protocol Policy: Choose "Redirect HTTP to HTTPS" or "HTTPS Only".
Allowed HTTP Methods: Choose the methods you need (e.g., GET, HEAD).
Configure distribution settings:
Price Class: Choose the appropriate price class.
Alternate Domain Names (CNAMEs): Optional.
SSL Certificate: Choose the appropriate option.
Create the distribution.
Wait for the distribution to deploy.
Step 6: Access Content Through CloudFront
Once the distribution is deployed, you can access your content through the CloudFront domain name provided in the CloudFront Console.
Test access by navigating to the CloudFront URL in a web browser.
Conclusion
By following these steps, you have successfully set up an S3 bucket and configured it to be accessed through CloudFront. Your content is now efficiently delivered to users worldwide with low latency, enhancing the overall user experience.

