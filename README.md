# Cloud Computing Project Deployment Guide

**Project Overview**
This project involves deploying a lightweight web application on AWS using a Flask backend and a static HTML/CSS/JavaScript frontend. The application is containerized, with the backend running in Docker on an Amazon EC2 instance, while the frontend is served using AWS Elastic Beanstalk. Amazon RDS (PostgreSQL) is used for database operations, and Amazon S3 stores uploaded images.


Project Details
Repository Link: https://github.com/kissasium/CloupComputingProject

Frontend URL: http://kissaproject0572.eba-hqs3ana7.ap-southeast-2.elasticbeanstalk.com/

Backend URL: http://3.105.45.13:5000


**Architecture Diagram**

This architecture diagram represents the deployment setup of the web application on AWS.

Frontend: Static HTML/CSS/JS served via Elastic Beanstalk.

Backend: Flask application running inside a Docker container on an EC2 instance.

Database: Amazon RDS (PostgreSQL) for storing application data.

File Storage: Amazon S3 for uploading and storing files.

Step-by-Step Deployment Guide

Setting Up AWS Services
**Elastic Beanstalk (Frontend)**
Elastic Beanstalk is used to deploy and manage the frontend (static HTML/CSS/JS) with auto-scaling and load balancing. Follow these steps to deploy the frontend:

Install the EB CLI:
```
pip install awsebcli


Initialize the Elastic Beanstalk Application:
```
eb init -p python-3.7 flask-app

Create the Elastic Beanstalk Environment:
```
eb create flask-env

Deploy the Frontend:

After the environment is created, deploy the frontend:

```
eb deploy

The application will now be accessible via the provided URL (e.g., http://kissaproject0572.eba-hqs3ana7.ap-southeast-2.elasticbeanstalk.com/).

**EC2 (Backend)**

The Flask backend is deployed on an EC2 instance inside a Docker container.

Create EC2 Instance:

1. Log in to the AWS Management Console.

2. Navigate to EC2 > Launch Instance > Select Amazon Linux 2 AMI.

3. Create a new Key Pair (you will use this to SSH into your EC2 instance).

4. Attach a Security Group to allow inbound traffic on ports 80 (HTTP), 443 (HTTPS), and 5000 (Flask).

SSH into EC2 Instance:

```
ssh -i "your-key.pem" ec2-user@<your-ec2-ip>


Install Docker:
Inside the EC2 instance:
```
sudo yum update -y
sudo yum install -y docker
sudo service docker start
sudo usermod -a -G docker ec2-user


Deploy Flask Backend Inside Docker:

Clone the GitHub Repository:

```
git clone https://github.com/kissasium/CloupComputingProject.git
cd CloupComputingProject

Build Docker Image:
```
docker build -t flask-backend .
Run Flask App in Docker:

```
docker run -p 5000:5000 flask-backend

The Flask application will now be accessible via the EC2 public IP on port 5000 (e.g., http://3.105.45.13:5000).



**Amazon RDS (Database)**

Set up PostgreSQL on Amazon RDS for database management.

**Create RDS Instance:**

1. Navigate to the RDS service on AWS.

2. Select PostgreSQL as the database engine.

3. Configure the database instance (DB instance identifier, username, password, etc.).

4 Ensure the RDS instance is created inside the same VPC as the EC2 instance for private connectivity.

**Access Database:**

1. After the RDS instance is created, use the connection details (endpoint, username, password) to connect to the database.

2. Update your Flask application to connect to RDS using these credentials.

**Amazon S3 (File Storage)**

Use Amazon S3 to store and manage file uploads from the frontend.

**Create S3 Bucket:**

1. Go to the S3 service on AWS and create a new bucket (e.g., your-s3-bucket-name).

2. Set proper bucket policies to allow your backend to access and upload files.

**Integrate S3 in Flask:**

1. Configure the Flask app to upload images to S3 when received from the frontend.

2.Use the AWS SDK (Boto3) to interact with S3 from the backend.
