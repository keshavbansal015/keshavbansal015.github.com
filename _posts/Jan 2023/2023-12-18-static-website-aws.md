---
layout: post
title: "Host a Static Website on AWS"
categories: Cloud_Computing AWS
permalink: :title
---

<img src="/assets/Jan 2023/static-website/Happy programmer.gif" alt="Happy Programmer" class="blog-gif-top">

## Introduction

Last semester (Fall 2023), I took a course on cloud computing where I got a lot of exposure to Cloud technologies and some exposure to DevOps.

Of course, I am still nowhere near confident with working with AWS, GCP, Azure, Terraform, etc., so I want to continue practicing working on them.

In this blog post, I want to solve the "Hello World" problem of the cloud. That is to Host a static website on AWS.


### What is a Static Website?

- In Simple terms, it's just a bunch of HTML pages (hopefully with some CSS, maybe some Javascript.)

- A static website does not have a backend.

- It also does not have any databases.

- It is simply a bunch of HTML pages hosted on the internet.

- As opposed to a static website, a dynamic website would have a backend and >=0 databases.  


### Why Choose AWS for Hosting?

- No particular reason, we could very well choose any other cloud provider for this.


### Prerequisites

- AWS account.

- Basic knowledge of HTML/CSS.


### Step 1: Creating an S3 Bucket in AWS

- We could use other AWS services also to host the website (eg EC2 instance), but using an S3 bucket is the most common, cost-effective, simple way.

- Search for S3 service, and click on Create Bucket.

- Give a name to your bucket, like __"static-website"__ (it has to be globally unique)

- Enable ACLs: helps with writing Access Control Policies for each object in our bucket.

- Uncheck "Block all public access", this would let us apply those ACLs and get our website public.

- Leave everything else as it is and click "Create Bucket".

**At this point, we have a place to upload our static website files in AWS and permissions to write ACLs for our objects (files) in the S3 bucket.**


### Step 2: Uploading Files to S3

- Now, select the bucket just created.

- And upload the files with all the default settings.

- Check if all the files are indeed uploaded, then select them all, clock on "Actions", scroll down, and click on "Make public with ACLs".

- This makes our files publicly accessible.


### Checking Public Access

- Go to the view where all the files you uploaded are listed.

- Click on any one of the files.

- Click on object URL: We should be able to view the file.

  **You now have your Static website hosted on the internet.** *But wait!!!*

### Setup Route 53 for custom URL (Optional)

**To complete this section, our bucket name must match our Custom URL name.**

Now, we are gonna get a custom URL to host our website.

- Go to Route 53 service, and select "Get Started".

- Now, assuming we don't have a custom URL, click on "Register a domain" (Selected by default).

- Search for the custom URL of your choice, buy it, and wait for it to get registered.


### Step 4: Connect custom URL to S3 bucket resource

- Now we need to go back to the S3 bucket, click on properties, scroll down to Static Web Hosting, and click on Edit.

- Select "Enable", and use the following configurations:

	- Static website hosting: Enable
	
	- Hosting type: Host a static website
	
	- Index document: index.html (or whatever you want)
	
	- Error document: error.html (or leave it if you want)

- Click on "Save Changes".

- Now go back to the properties view, and copy the URL from the end of the page. It should look like this: http://<\bucketname>.<\bucketzone>.amazonaws.com

- Now back to Route 53. Click on Hosted Zones from the sidebar.

- Select the Custom URL, and click on "Create Record".

- Flip the Alias switch, enter "Alias to S3 website endpoint" and then choose the zone where your bucket is hosted, eg: US East (N. Virginia) us-east-1, and select your S3 bucket.

- Save the changes. Wait for the changes to propagate.

**Our website is now hosted with a custom URL :)**


### Step 5: Enable HTTPS with AWS Certificate Manager (Optional)

- Lookup CloudFront from AWS services.

- Create a new distribution.

- Choose your S3 bucket and click on "use website endpoint" hosting.

- Under "Default Cache Behavior"

	- Change "Viewer protocol policy": Redirect HTTP to HTTPS
	
	- Add CNAME, in this case, the necessary one would be <\Custom Domain Name>
	
	- Optional: We can add more CNAMEs.

	- Under settings: Click on "Request certificate"
	
- Now, in the AWS Certificate Manager, click on "Next".

- Enter your custom domain name, and click Next.

- Now in the ACM dashboard, we should see a new certificate being created, click on it, and open its detailed view.

- In the Domains section, click "Create records in Route 53".

- Click "Create Records".

- Now back in the CloudFront section, refresh the custom certificate list and select the newly created certificate.

- Enable security, and click on "Create Distribution".


### Step 6: Add SSL certification

- From the CloudFront dashboard, copy the "domain name" of the distribution.

- We go back to Route 53 and edit the "Value/Route traffic to" of the "A" record to the copied value.

- That's it folks, we are done. :)

- Let the changes propagate, and now you have a personal secure static website hosted on AWS.



### References
1. [Configuring a static website on Amazon S3](https://docs.aws.amazon.com/AmazonS3/latest/userguide/HostingWebsiteOnS3Setup.html)  
2. [Configuring a static website using a custom domain registered with Route 53](https://docs.aws.amazon.com/AmazonS3/latest/userguide/website-hosting-custom-domain-walkthrough.html)
