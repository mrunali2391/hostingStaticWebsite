Steps to follow:

creating S3 bucket in AWS account

Uploading static website file in S3

Configuring hosted zone in route 53

Configuring CloudFront and accessing websites using it

Secure access with HTTPS

Implementing a CI/CD pipeline

Creating S3 bucket:

In AWS management console dashboard search for the S3 service and select it. In the Amazon S3 dashboard, click the "Create bucket" button.

You'll be prompted to configure various settings for your new S3 bucket. Here's how:

Bucket name: Keep your bucket name same as your domain name.

Region: Select the AWS region where you want to create the bucket.

Object ownership: Set the ACL disabled

Block Public Access settings for this bucket: Make it enabled so that it should not get accessed publicly. We will be using cloudfront and SSL/TLS certificate to access it securely.

Make the other configurations default as of now. And click on "create bucket" button.

Uploading static website file in S3

Upload your index.html code file to the S3 bucket and click on "Upload" button.

Now, Go to the Bucket Properties -> Static website hosting section. Enable "static website hosting" option. Hosting type should be set as "static website hosting". Mention your html file's name in Index document field.

Configuring hosted zone in route 53

Now move to the route53 service of AWS to configure a hosted zone. In DNS management section, click on "create a hosted zone". Mention your domain name and leave all other fields as set by default. And click on the "create hosted zone" button.

Inside the newly created hosted zone, you will see 2 records got created automatically after hosted zone creation named as NS and SOA records.

Step 1:

NS records will include 4 nameservers which need to get added to your Domain provider. I've used Godaddy as my domain provider, so submitted all the 4 nameservers created in AWS hosted zone to the Domain management section -> nameserver in the domain provider.

Step 2:

Inside hosted zone click on the new record. If you want to create a record with the same name as the domain name leave the subdomain field empty. Select A type record. Enable Alias. And route traffic to the S3 website endpoint. Select the region in which you have created your S3 bucket. Once you select the region, your S3 endpoint will be auto-populated in the below field. Leave all the other options as default. And click on the "create record" button.

4. Configuring CloudFront

CloudFront supports SSL/TLS encryption between the viewer (user's browser) and CloudFront, as well as between CloudFront and the origin server (e.g., your S3 bucket). This ensures that data transmitted to and from your website is encrypted and secure, preventing eavesdropping and tampering.

You can use your own SSL/TLS certificates with CloudFront to secure communication between viewers and your website. This is particularly important for websites that require a specific level of security or need to use Extended Validation (EV) certificates.

Steps to follow:

Go to the CloudFront service of AWS and click on the "Create Distribution" button. To configure it we will have to fill up a long form which I'll guide using the fields mentioned below:

# Origin domain: Here you will have to mention your S3 endpoint which will be auto-populated the field.

# origin path and name: this should be set as given by default.

# Origin access: The "Origin Access Identity" (OAI) is a feature that allows you to restrict access to your Amazon S3 bucket origin, providing an additional layer of security for your content. Here are steps to configure it :

a. choose "Legacy access identities"

b. create a new OAI.

c. choose an "update the bucket policy" option.

d. In "Viewer protocol policy" field, select "Redirect HTTP to HTTPS"

e. In "Allowed HTTP methods" field, select "GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE"

5.Secure access with HTTPS

In the "Custom SSL certificate" section, we can create a certificate by clicking on the "Request Certificate" link which will redirect you to AWS's certificate manager service. Mention your fully qualified domain name and leave all other options as set by default. Click on the "Request" button.

g. Validation method: You have to validate that the domain is yours to claim the certificate. In this case, as recommended by AWS, domain validation is better. After that leave other default and click on request. You will see your certificate pending. Click on view certificate or certificate name, you will see CNAME records to insert into the Route 53 to verify it. You will see the option of exporting to Route 53. Click on that and it will automatically insert the record in your required hosted zone.

h. After that, return to the Cloudfront and refresh that small refresh icon beside the request custom certificate option. Drop down and you will see your verified certificate.

i. In "Default root object", mention your HTML file's name.

k. In the "Alternate domain" field, mention your subdomain. And click on "Create distribution".

k. Now go to the Route53 service's hosted zone again and create another A-type record, where you should select Cloudfront distribution and www as a subdomain.

Now you will be able to access your website in a secure fast and reliable way with your custom domain
