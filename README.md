# Hosting-a-Static-Website-on-Amazon-S3-Using-AWS-CLI
This project demonstrates the use of AWS Command Line Interface (AWS CLI) to automate the deployment and management of a static website hosted on Amazon S3. Executed from an Amazon EC2 instance, the workflow includes:
- Creating an Amazon S3 bucket for website hosting.

- Creating an IAM user with full access to Amazon S3.

- Uploading website files to the S3 bucket to serve as a public-facing site for a fictional Café & Bakery.

- Developing a batch script to streamline and automate the update process whenever website files are modified locally.

This hands-on project highlights practical skills in AWS infrastructure setup, access management, and CLI-based automation for cloud-native website hosting.

<img width="1654" height="787" alt="image" src="https://github.com/user-attachments/assets/64c79fa7-7e23-4bf6-a1e6-27dc23b18704" />

Clients can access the deployed static website via a public Amazon S3 URL (e.g., http://<bucket-name>.s3-website-<region>.amazonaws.com). The S3 bucket used for hosting can be created and managed through either the AWS Management Console or the AWS CLI.


## Connect to an Amazon Linux EC2 instance using SSM
Run the following commands to change the user and home directory:
```
sudo su -l ec2-user
pwd
```

## Configure the AWS CLI
Open the SSH terminal and run
``aws configure``

When prompted, input the following:

AWS Access Key ID: Paste the provided AccessKey.

AWS Secret Access Key: Paste the provided SecretKey.

Default region name: Enter region

Default output format: Enter format

## Create an S3 bucket using the AWS CLI
To create an Amazon S3 bucket using the AWS CLI, use the ``aws s3api create-bucket`` command with the following options:

- Specify the region using --region name-of-region.

- Include --create-bucket-configuration LocationConstraint=region to set the bucket's location.
  
```
aws s3api create-bucket --bucket iloribucket --region us-west-2 --create-bucket-configuration LocationConstraint=us-west-2
```

<img width="669" height="127" alt="Screenshot 2025-08-01 225057" src="https://github.com/user-attachments/assets/545ec87a-8885-490f-9b86-62193753b61a" />
<img width="647" height="554" alt="Screenshot 2025-08-01 231620" src="https://github.com/user-attachments/assets/b9213a0f-fc34-4efb-80ee-aa416df3699d" />
bucket created !!!!


## Create a new IAM user that has full access to Amazon S3

###### To create a new IAM user and assign a login password using AWS CLI:
```
aws iam create-user --user-name awsS3user
```
<img width="510" height="158" alt="Screenshot 2025-08-01 230607" src="https://github.com/user-attachments/assets/e7d48bbe-b0f5-4b6b-8139-b9cdde191564" />

###### Create a login profile (console password):

```aws iam create-login-profile --user-name awsS3user --password your password```

<img width="668" height="129" alt="Screenshot 2025-08-01 230851" src="https://github.com/user-attachments/assets/29cefeeb-f8ef-47a5-98ca-b58960c9c62f" />

This gives the user awsS3user access to the AWS Management Console with the specified password.

## Extract the files that you need for this project

by running the following commands:
```
cd ~/sysops-activity-files
tar xvzf static-website-v2.tar.gz
cd static-website
````
cd ~/sysops-activity-files
cd = change directory

~ = your home directory (e.g., /home/ec2-user)

This command moves you into the sysops-activity-files folder located in your home directory.

tar xvzf static-website-v2.tar.gz
tar = tool for extracting and compressing archives

Options:

x = extract files

v = verbose (shows file names being extracted)

z = uncompress .gz (gzip) files

f = filename to work with

static-website-v2.tar.gz = the compressed file being extracted

Result: This extracts the contents of the archive static-website-v2.tar.gz (likely a website's files).
 cd static-website
Changes into the newly extracted folder called static-website, which was unpacked from the .tar.gz file.
<img width="594" height="292" alt="Screenshot 2025-08-01 232439" src="https://github.com/user-attachments/assets/bb5cfc8c-e9a7-4291-97da-ee5f3dfb2d52" />


## Upload files to Amazon S3 by using the AWS CLI

- To host a static website on S3:
Enable website hosting (set index.html as the homepage):
```
aws s3 website s3://<your-bucket>/ --index-document index.html
```
Upload website files to the bucket (with public-read ACL):
```
aws s3 cp /home/ec2-user/sysops-activity-files/static-website/ s3://<your-bucket>/
```
<img width="1167" height="181" alt="Screenshot 2025-08-01 235926" src="https://github.com/user-attachments/assets/fc967e20-c2fe-40a6-82c9-0383598e9575" />

In the Amazon S3 console, select your bucket.

Go to the Properties tab.

Scroll down to confirm Static website hosting is enabled (enabled by the aws s3 website CLI command).

To view the site, click the Bucket website endpoint URL—it opens your static site in a new browser tab.

<img width="1356" height="720" alt="Screenshot 2025-08-02 001841" src="https://github.com/user-attachments/assets/f7050621-9b7f-44f1-8c3d-6522bec3606a" />


## Real-World Use Cases
1. Portfolio or Small Business Website
Freelancers, small businesses (e.g., cafés, shops, bloggers) can host informational or marketing websites at minimal cost.

2. Internal Company Dashboards
Static dashboards or documentation pages can be hosted for internal use.

3. Training & Onboarding
Teaching AWS CLI, IAM, and S3 concepts to students or new cloud engineers.

4. Prototype Deployment
Quickly deploy MVPs (minimum viable products) without managing backend servers.

5. Content Delivery
Host publicly downloadable content (e.g., PDFs, product brochures, images) with global access.



## Future Improvements
- Add CloudFront CDN for improved performance and HTTPS

- Implement CI/CD pipeline with GitHub Actions

- Add bucket policy JSON for secure public access

- Enable S3 versioning and lifecycle rules


