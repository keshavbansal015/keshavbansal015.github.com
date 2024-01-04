---
layout: post
title: "Host a Static website on AWS"
categories: Cloud_Computing AWS
permalink: :title
---

## Introduction
Last semester(Fall 2023), I took cloud computing where I got a lot of exposure to Cloud technologies 
and some exposure to DevOps technologies.

I am still not remotely confident in going around all these websites and all these tools, so I 
want to continue practicing working on them. 

In this blog post, I want to solve the "Hello World" problem of the cloud. That is Host a static website on AWS.

### What is a Static Website?
- In Simple terms, its just a bunch of HTML pages (hopefully with some CSS, maybe some Javascript.)
- A static website does not have a backend system
- It also does not have any databases.
- It is simply a bunch of HTML pages hosted on the internet, like a simple blog.
- As opposed to a static website, a dynamic website would have a backend and atleast 1 database system.

### Why Choose AWS for Hosting?
- No particular reason, you could very well choose any other cloud provider for this.

### Prerequisites
- AWS account.
- Basic knowledge of HTML/CSS.

### Step 1: Creating a Bucket in AWS S3
- I could use other AWS services also to host the website, but using an S3 bucket is the most common, cost effective, simple way.
- Search for S3 service, and click on Create Bucket.
- Give a name to your bucket, like __"static-website"__ (it has to be globally unique)
- Enable ACLs: helps with writing access control policies for each object in our bucket.
- Uncheck "Block all public access", this would let us apply those ACLs.
- Leave everything as it is and click "Create Bucket".
- __At this point, we have a place to upload our static website files in AWS and permissions to write ACLs for our objects in the S3 bucket.__

### Step 2: Uploading Files to S3
- Now, select the bucket just created.
- And upload the files with all the default settings.
- __At this point, we have our files in the bucket, now we host it and make it public__
- Check if all the files are indeed uploaded, then select them all, clock on "Actions", scroll down and click on "Make public with ACLs".
- And that's it.

### Checking Public Access
- Go to the view where all the files you uploaded are listed.
- click on any one of the files.
- Click on object URL: We should be able to view the file.

### Setup Route 53 for custom URL (Optional)
- Go to Route 53 service, select "Get Started".
- __ To complete this section, our bucket name must match with our Custom URL name.__
- Now, assuming we don't have a custom URL, click on "Register a domain" (Selected by default).
- Search for your custom URL, buy it, and wait for it to get registered.

### Step 4: Connect custom URL to S3 bucket resourse
- Now we need to go back to the S3 bucket, click on properties, scroll down to Static Web Hosting and click on Edit.
- Select "Enable", and use the following configurations:
    - Static website hosting: Enable
    - Hosting type: Host a static website
    - Index document: index.html (or whatever you want)
    - Error document: error.html (or leave it if you want)
- Click on "Save Changes".
- Now go back to the properties view, and copy the URL from the end of the page. It should look like this: http://<bucketname>.<bucketzone>.amazonaws.com
- Now back to the Route 53. Click on Hosted Zones from the side bar.
- Select the Custom URL, and click on "Create Record".
- Flip the Alias switch, enter "Alias to S3 website endpoint" and then choose the zone where your bucket is hosted, eg: US East (N. Virginia) us-east-1.
- Save the changes.

### Enable HTTPS with AWS Certificate Manager (Optional)
- Lookup Cloud Front from AWS services.

- Now, click on your custom URL, 
- Using AWS Route 53 for domain registration and DNS management.
- Linking your custom domain to the S3 bucket.

### Step 5: Enabling HTTPS with AWS Certificate Manager (Optional)
- Steps to secure your website with SSL/TLS using AWS Certificate Manager and CloudFront.

### Step 6: Monitoring and Managing Your Website
- Introduction to tools like AWS CloudWatch for monitoring website traffic and performance.

### Best Practices for Hosting Static Websites on AWS
- Tips for optimizing performance and security.

### Conclusion
Summarize the key points and encourage readers to experiment with AWS for their static websites.

### Further Resources
- Links to AWS documentation and other helpful resources.
