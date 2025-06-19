# 🔒 Secure Flask Deployment using AWS Elastic Beanstalk, IAM Roles, RDS, and EC2

This project demonstrates a secure and scalable deployment of a Flask web application on **AWS Elastic Beanstalk**, integrated with **Amazon RDS (MySQL)**, and securely accessed from an **Amazon EC2 instance** using **IAM roles** and **AWS Systems Manager Parameter Store**.

---

## 🧠 Objective

- Deploy a Flask application using AWS Elastic Beanstalk.
- Create an RDS MySQL database integrated within the Beanstalk environment.
- Securely access the RDS database from a separate EC2 instance within the same VPC.
- Store database credentials in **SSM Parameter Store** securely.
- Perform read/write operations from the EC2 instance.
- Monitor resources using Amazon CloudWatch.

---

## 🛠️ Tech Stack

- 🐍 Python (Flask)
- ☁️ AWS Elastic Beanstalk
- 🐬 Amazon RDS (MySQL)
- 🖥️ Amazon EC2 (Ubuntu)
- 🔐 IAM Roles & Policies
- 🔐 AWS Systems Manager Parameter Store
- 📊 Amazon CloudWatch

---

## 🗂️ Project Structure

Secure-Flask-Deployment/
│
├── application.zip # Flask app (application.py + requirements.txt)
├── test_db.py # EC2 script to connect to RDS
├── README.md # Project documentation
└── Screenshots/ # All project-related screenshots

yaml
Copy
Edit

--

## 🔁 Step-by-Step Implementation

### 1️⃣ Flask Application

- Created `application.py` and `requirements.txt`
- Zipped both into `application.zip`

### 2️⃣ Elastic Beanstalk Environment

- Platform: **Python**
- Environment type: **Single instance (Free Tier eligible)**
- RDS Integration:
  - **MySQL**
  - **Not public**
  - Security Group allows EC2 access on port `3306`
- IAM Role created for Beanstalk instance profile

### 3️⃣ EC2 Instance

- Ubuntu instance launched in the **same VPC**
- Installed:
  ```bash
  sudo apt update
  sudo apt install python3-pip -y
  pip3 install pymysql boto3
IAM role attached with AmazonSSMReadOnlyAccess

4️⃣ Secure Parameter Storage
Stored in AWS Systems Manager Parameter Store:

/rds/username → type: SecureString

/rds/password → type: SecureString

5️⃣ Python Script (test_db.py)
python
Copy
Edit
import boto3
import pymysql

ssm = boto3.client('ssm', region_name='ap-south-1')

db_user = ssm.get_parameter(Name='/rds/username', WithDecryption=True)['Parameter']['Value']
db_pass = ssm.get_parameter(Name='/rds/password', WithDecryption=True)['Parameter']['Value']
db_host = 'awseb-xyz.c7kwulie2o4x.ap-south-1.rds.amazonaws.com'  # Replace with your actual endpoint
db_name = 'ebdb'

conn = pymysql.connect(host=db_host, user=db_user, password=db_pass, database=db_name)
cursor = conn.cursor()

cursor.execute("INSERT INTO student (id, name, course) VALUES (5, 'Yuvrani', 'SecureDB');")
conn.commit()

cursor.execute("SELECT * FROM student;")
rows = cursor.fetchall()

print("📋 Student Table Records:")
for row in rows:
    print(row)

cursor.close()
conn.close()
✅ Final Output
bash
Copy
Edit
📋 Student Table Records:
(1, 'Yuvrani', 'Cloud Architecture')
(5, 'Yuvrani', 'SecureDB')
🔒 Security Measures
✅ IAM roles used (no hardcoded credentials)

✅ RDS not publicly accessible

✅ EC2 and RDS share secure VPC

✅ Secrets stored in AWS SSM Parameter Store

📸 Screenshots
Step	Image
Elastic Beanstalk App	Project Complete.png
RDS Instance	My RDS.png
EC2 Ubuntu Server	My Running EC2 Instances.png
SSM Parameters	Parameters.png
Python Code on EC2	Python code.png
Security Group Configurations	Security Groups.png
Parameter Creation	Creation of user parameter.png

📌 How to Run This Project
Clone this repo

Deploy application.zip on Elastic Beanstalk

Store DB credentials in AWS Parameter Store

Launch an EC2 instance and attach IAM role

SSH into EC2 and run test_db.py

View the DB records output in the terminal

👩‍💻 Author
Yuvrani Randive
GitHub: @randiveyuvrani
DevOps | Cloud Enthusiast | Python Developer

