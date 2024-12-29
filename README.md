# Flask App Deployment on AWS EC2

## Overview
This guide walks you through the steps to set up and deploy a Flask application on an AWS EC2 instance. The app will run on a production-ready server using Gunicorn.

---

## Prerequisites

1. **AWS Account**
2. **An existing EC2 instance** running Ubuntu.
3. **Basic knowledge** of terminal commands.
4. **A configured security group** for your EC2 instance.

---

## Step 1: Launch the Ubuntu EC2 Instance

1. Log in to your AWS Management Console
2. Navigate to the **EC2 Dashboard**
3. Click **Launch Instance**
4. Select an **Ubuntu Server AMI** (e.g., Ubuntu 20.04 LTS)
5. Choose an **Instance Type** (e.g., t2.micro for free tier)
6. Create a key pair (.pem)
7. Configure security groups:
   - Allow SSH (port 22)
   - Add a rule for HTTP (port 80)
   - Add a rule for custom TCP (port 5000)
8. Launch the instance and connect via SSH

---

## Step 2: Update the Instance

Run the following commands to update your instance:

```bash
sudo apt update
sudo apt upgrade -y
```

---

## Step 3: Install Python and Pip

Install Python and Pip:

```bash
sudo apt install python3-pip -y
```

Verify the installation:

```bash
python3 --version
pip --version
```

---

## Step 4: Set Up the Flask App

1. Clone your Flask app repository:

```bash
git clone <repository-url>
```

2. Navigate to the project folder (in my case *spamemail*):

```bash
ls
cd <folder-name>
ls
```

3. Check the `requirements.txt` file:

```bash
nano requirements.txt
```

4. If you can't access the requirements.txtEnsure the file uses Unix-style line endings. Convert if needed:

```bash
file requirements.txt
sudo apt install dos2unix -y
dos2unix requirements.txt
```

5. Install the dependencies:

```bash
pip install -r requirements.txt
```

---

## Step 5: Run the Flask App (Development Mode)

Start the app:

```bash
python3 app.py
```

Verify the app is running by navigating to `http://<your-ec2-public-ip>:5000` in your browser.

---

## Step 6: Open Port 5000 in the Security Group

1. Go to the **EC2 Dashboard** in AWS.
2. Select your instance and go to the **Security** tab.
3. Edit the **Inbound Rules** of your security group to allow TCP traffic on port 5000.

---

## Step 7: Set Up Gunicorn for Production

1. Install Gunicorn:

```bash
pip install gunicorn
```

2. Run the app with Gunicorn:

```bash
gunicorn --workers 3 --bind 0.0.0.0:5000 app:app
```

Replace `app:app` with your actual module and app name if different.


This concludes the setup for deploying your Flask application on AWS EC2!
