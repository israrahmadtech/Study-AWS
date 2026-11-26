# Amazon EC2 (Elastic Compute Cloud)

## 1. Introduction

Amazon EC2 stands for **Amazon Elastic Compute Cloud**.

It is an AWS service that provides virtual servers in the cloud.

These virtual servers are called **EC2 Instances**.

With EC2, you can rent computing resources from AWS and use them to run applications, host websites, deploy backend servers, process data, and perform many other computing tasks.

You do not need to purchase a physical server or maintain its hardware.

AWS provides the underlying infrastructure, while you configure and manage your virtual server.

### In Simple Words

Imagine you have a personal computer.

It has:

- A processor (CPU).
- RAM.
- Storage.
- An operating system.
- An internet connection.

You can install software on it, write code, run applications, and host websites.

Amazon EC2 provides a similar computer, but it runs inside AWS Cloud.

You can access and manage it remotely through the internet or a private network.

### Real-World Example

Suppose you build a MERN Stack application.

Your application has:

- React frontend.
- Node.js backend.
- Express APIs.
- MongoDB database.

You want your backend to run continuously so users can access your APIs even when your personal computer is turned off.

You can:

1. Launch an EC2 instance.
2. Install Node.js on it.
3. Upload your backend code.
4. Install its dependencies.
5. Start your backend server.
6. Configure networking and security.
7. Connect your frontend to the backend.

Now your backend can run on an AWS virtual server instead of depending on your personal computer.

---

## 2. Why Do We Need Amazon EC2?

Before cloud computing became widely available, companies often purchased physical servers to host their applications.

They had to manage:

- Physical server hardware.
- RAM and CPU upgrades.
- Storage devices.
- Electricity.
- Cooling systems.
- Network infrastructure.
- Hardware maintenance.

This required money, time, and technical resources.

Amazon EC2 provides virtual computing capacity without requiring you to purchase the physical server yourself.

### Main Reasons to Use EC2

1. **Application Hosting:** Run backend applications and APIs.
2. **Website Hosting:** Host websites and web applications.
3. **Development:** Create development and testing environments.
4. **Database Hosting:** Run supported database software on a virtual server when self-managing the database is appropriate.
5. **Background Jobs:** Run scripts, workers, and scheduled tasks.
6. **Data Processing:** Perform computing-intensive tasks.
7. **Custom Environments:** Install and configure the software you need.
8. **Scalability:** Change instance sizes or add more instances as application requirements grow.

### Traditional Server vs Amazon EC2

| Feature | Traditional Physical Server | Amazon EC2 |
|---|---|---|
| Hardware | You purchase physical hardware | AWS provides underlying hardware |
| Initial investment | Usually higher | Can start with smaller usage |
| Maintenance | You manage physical hardware | AWS manages underlying infrastructure |
| Configuration | Depends on purchased hardware | Select an available instance type |
| Scaling | May require hardware upgrades | Can change or add instances |
| Availability | Depends on your infrastructure | Can use AWS infrastructure and availability options |
| Payment | Hardware purchase and operating costs | Usage-based pricing options |

**Important:** EC2 does not mean AWS manages your entire application. You are still responsible for configuring the operating system, installing software, managing your application, and securing the server.

---

## 3. What Is an EC2 Instance?

An **EC2 Instance** is a virtual server running inside AWS Cloud.

Whenever you launch an EC2 instance, AWS allocates computing resources to it according to the instance type you select.

An instance can have:

- CPU.
- RAM.
- Storage.
- Network connectivity.
- An operating system.
- Installed applications and software.

### Example

Imagine you launch an instance with:

- Operating System: Ubuntu Linux.
- CPU: 2 vCPUs.
- RAM: 2 GB.
- Storage: 20 GB.
- Application: Node.js backend.

This instance behaves like a remote Linux computer.

You can connect to it and execute commands such as:

```bash
sudo apt update
sudo apt install nodejs npm
node -v
npm -v
```

You can also install Git, Nginx, PM2, Docker, and other required software.

### Important Terminology

| Term | Meaning |
|---|---|
| EC2 | AWS service that provides virtual computing capacity |
| Instance | A virtual server |
| Instance ID | Unique identifier of an instance |
| Instance Type | Defines the computing resources |
| AMI | Template used to launch an instance |
| EBS | Persistent block storage for an instance |
| Key Pair | Credentials used for secure login |
| Security Group | Virtual firewall for an instance |

---

## 4. Understanding the Main Components of EC2

When creating an EC2 instance, AWS asks you to configure several important components.

Let's understand them one by one.

### 4.1 Amazon Machine Image (AMI)

AMI stands for **Amazon Machine Image**.

An AMI is a template used to launch an EC2 instance.

It provides the information required to create the instance, including its operating system and configured software.

Think of an AMI as a ready-made computer setup.

### Example

When launching an EC2 instance, you can select an AMI such as:

- Amazon Linux 2023.
- Ubuntu Server.
- Microsoft Windows Server.
- Other supported Linux distributions.

If you select Ubuntu Server, your EC2 instance will start with an Ubuntu operating system.

You can then install Node.js, Nginx, Git, and other tools.

### Why Do We Need AMIs?

- Choose an operating system.
- Launch instances with preconfigured software.
- Create reusable server templates.
- Launch multiple instances with similar configurations.
- Create custom images for your applications.

### Important

An AMI is not the running server itself.

It is a template used to create an EC2 instance.

---

### 4.2 Instance Types

An EC2 Instance Type determines the computing resources available to your virtual server.

These resources include:

- CPU.
- Memory (RAM).
- Network performance.
- Storage capabilities.
- Graphics capabilities, where supported.

Different applications require different resources.

For example:

A small Node.js API may work with a smaller instance, while a large application with many users may require more CPU and RAM.

### Common EC2 Instance Families

| Instance Family | Main Purpose | Example Use |
|---|---|---|
| T Family | General-purpose burstable computing | Small websites and development servers |
| M Family | General-purpose computing | Application servers |
| C Family | Compute-intensive workloads | CPU-heavy processing |
| R Family | Memory-intensive workloads | Memory-heavy databases and applications |
| I Family | Storage-intensive workloads | High-performance storage workloads |
| G / P Families | Accelerated computing | Graphics, AI, and machine learning workloads |

### Understanding Instance Type Names

Example:

` t3.micro `

Let's understand its parts:

- `t`: Instance family.
- `3`: Generation.
- `micro`: Instance size.

Other sizes may include:

- Small.
- Medium.
- Large.
- Xlarge.
- 2xlarge.

The available sizes and specifications depend on the instance family and AWS Region.

### Important

A larger instance generally provides more computing resources and may cost more.

Do not select a large instance without understanding your application's requirements.

Start with an appropriate small configuration for learning and testing, then scale when needed.

---

### 4.3 Amazon EBS (Elastic Block Store)

EBS stands for **Amazon Elastic Block Store**.

It provides persistent block storage volumes that can be attached to EC2 instances.

Think of EBS as the hard drive or SSD of your virtual computer.

An EC2 instance needs storage to hold its operating system, applications, files, and other data.

### Example

Suppose you launch an Ubuntu EC2 instance with a 20 GB EBS volume.

That volume can store:

- Ubuntu operating system files.
- Node.js.
- Backend source code.
- Application dependencies.
- Logs.
- Other application files.

### EBS Volume Types

AWS provides different EBS volume types for different workloads.

Some common types include:

| Volume Type | Purpose |
|---|---|
| gp3 | General-purpose SSD storage |
| gp2 | Previous-generation general-purpose SSD |
| io2 | High-performance provisioned IOPS SSD |
| st1 | Throughput-oriented HDD |
| sc1 | Cold HDD storage |

For many general-purpose application servers, gp3 is a common starting point.

### Important: EBS vs Instance Store

These are different types of storage.

**EBS:**

- Persistent block storage.
- Can remain available independently of the running state of an instance.
- Can be detached and attached to supported instances.
- Data is generally retained when an instance is stopped.
- Data may be deleted when the volume is deleted, depending on its configuration.

**Instance Store:**

- Temporary storage physically associated with the host.
- Designed for temporary data.
- Data can be lost when the instance stops, hibernates, or terminates.

### Example

Your backend stores uploaded files on an EBS volume.

If you stop the instance, the data on an EBS volume is generally retained.

However, if you terminate the instance, the root EBS volume may be deleted depending on its Delete on termination setting.

Always understand your storage configuration before terminating an instance.

---

### 4.4 Key Pairs

A Key Pair is used to securely authenticate when connecting to an EC2 instance.

For Linux instances, you commonly use an SSH key pair.

A key pair consists of:

1. Public Key.
2. Private Key.

AWS places the public key on the instance during launch, while you keep the private key.

The private key is used by your SSH client to prove that you are authorized to connect.

### Example

When creating an EC2 instance, AWS may ask you to create or select a key pair.

You can create a key pair named:

`my-ec2-key`

AWS provides a private key file, commonly with a `.pem` extension.

Example:

`my-ec2-key.pem`

Keep this file secure.

### Connecting Through SSH

A typical Linux SSH command looks like this:

```bash
ssh -i my-ec2-key.pem ubuntu@YOUR_PUBLIC_IP
```

Here:

- `ssh`: Starts an SSH connection.
- `-i`: Specifies the private key file.
- `my-ec2-key.pem`: Your private key.
- `ubuntu`: Example default username for Ubuntu AMIs.
- `YOUR_PUBLIC_IP`: Public IP address of your instance.

The username depends on the AMI. For example, Amazon Linux commonly uses `ec2-user`, while Ubuntu commonly uses `ubuntu`.

### Important Security Rules

- Never share your private key.
- Never upload your `.pem` file to GitHub.
- Never put private keys inside frontend code.
- Keep a secure backup of your private key.
- Restrict access to the private key file.

**Remember:** A Key Pair is used for connecting to your server. An IAM Role is used to grant AWS permissions to the server or another trusted identity. They are not the same thing.

---

### 4.5 Security Groups

A Security Group is a virtual firewall that controls network traffic associated with an EC2 instance.

It determines which incoming and outgoing connections are allowed.

Security Groups are essential for protecting your server.

### Two Important Types of Rules

#### Inbound Rules

Inbound rules control traffic coming into your EC2 instance.

For example:

You want users to access your website through HTTP and HTTPS.

You can configure:

- HTTP: Port 80.
- HTTPS: Port 443.

You also need SSH access to manage a Linux server.

SSH commonly uses:

- Port 22.

#### Outbound Rules

Outbound rules control traffic leaving your EC2 instance.

For example:

Your backend may need to connect to:

- MongoDB Atlas.
- External APIs.
- Package repositories.
- Other AWS services.

The relevant outbound traffic must be allowed.

### Common Ports

| Port | Protocol / Service | Common Purpose |
|---|---|---|
| 22 | SSH | Linux server administration |
| 80 | HTTP | Website traffic |
| 443 | HTTPS | Secure website traffic |
| 3000 | Application-specific | Common development server port |
| 3306 | MySQL | MySQL database connections |
| 5432 | PostgreSQL | PostgreSQL database connections |
| 27017 | MongoDB | MongoDB connections |

These are common defaults, not universal requirements. Applications can use different ports.

### Example Security Group for a Web Server

| Type | Port | Source | Purpose |
|---|---:|---|---|
| SSH | 22 | Your public IP | Server administration |
| HTTP | 80 | 0.0.0.0/0 | Public website |
| HTTPS | 443 | 0.0.0.0/0 | Secure public website |

For production, restrict administrative access to trusted IP addresses or use a private access method.

Avoid exposing database ports such as 3306 or 27017 publicly.

### Important Security Group Concepts

- Security Groups are stateful.
- Return traffic for an allowed connection is automatically permitted.
- Inbound rules control incoming traffic.
- Outbound rules control outgoing traffic.
- Security Groups can be associated with multiple instances.
- You can attach multiple Security Groups to an instance.

**Remember:** Security Groups control network traffic. IAM Policies control access to AWS API actions. They solve different security problems.

---

### 4.6 VPC (Virtual Private Cloud)

VPC stands for **Virtual Private Cloud**.

It is a logically isolated virtual network in AWS where you launch and manage resources.

Your EC2 instance runs inside a VPC.

Think of a VPC as your private network environment in AWS.

### Example

Suppose you create a VPC for your MERN application.

Inside this VPC, you can have:

- Public subnet.
- Private subnet.
- EC2 instances.
- Network interfaces.
- Route tables.
- Security Groups.

### What Is a Subnet?

A subnet is a smaller network range inside a VPC.

Subnets help organize resources and control network placement.

### Public Subnet

A public subnet has a route to an Internet Gateway for internet connectivity.

An EC2 instance also needs appropriate IP addressing and security rules to be reachable from the internet.

### Private Subnet

A private subnet does not have a direct route to an Internet Gateway for inbound internet access.

Resources in private subnets can still communicate through appropriately configured private networking and other AWS networking services.

### Example Architecture

```text
                 Internet
                    |
                    v
             Internet Gateway
                    |
                    v
                  VPC
                    |
          +---------+---------+
          |                   |
          v                   v
    Public Subnet        Private Subnet
          |                   |
          v                   v
       EC2 Server        Private Resources
```

**Important:** Public and private subnet configurations depend on route tables, gateways, IP addressing, and security rules. Simply naming a subnet "public" does not make it public.

---

## 5. How Do These EC2 Components Work Together?

Now let's connect everything.

Imagine you want to launch a Linux server for your Node.js backend.

You configure:

1. **AMI:** Select Ubuntu Server.
2. **Instance Type:** Select a suitable CPU and RAM configuration.
3. **EBS:** Select storage for the operating system and application files.
4. **Key Pair:** Configure secure login.
5. **Security Group:** Allow only required network traffic.
6. **VPC and Subnet:** Select the network where the instance will run.

AWS uses these configurations to launch your EC2 instance.

### Complete Overview

```text
                 AWS CLOUD
                     |
                     v
                    VPC
                     |
                     v
                  Subnet
                     |
                     v
               EC2 Instance
                     |
       +-------------+-------------+
       |             |             |
       v             v             v
      AMI       Instance Type     EBS
   (Ubuntu)    (CPU and RAM)   (Storage)
                     |
               Security Group
                     |
                 Key Pair
                     |
                     v
               Running Server
```

The AMI, instance type, EBS, key pair, security group, and networking are the main components you need to understand when launching an instance.

---

## 6. What Happens After Launching an EC2 Instance?

After launching an instance, AWS starts your virtual server.

For a Linux instance, the general process looks like this:

1. AWS provisions the instance.
2. The selected operating system boots.
3. The instance initializes its configured storage and networking.
4. The instance becomes available for connections.
5. You connect using SSH or EC2 Instance Connect.
6. You install and configure your required software.
7. You deploy and run your application.

### Example

You launch an Ubuntu EC2 instance.

After connecting, you can run:

```bash
sudo apt update
```

Install Git:

```bash
sudo apt install git -y
```

Install Node.js using an appropriate installation method.

Check versions:

```bash
node -v
npm -v
```

Clone your backend repository:

```bash
git clone YOUR_REPOSITORY_URL
```

Move into the project:

```bash
cd YOUR_PROJECT_FOLDER
```

Install dependencies:

```bash
npm install
```

Start your application:

```bash
npm start
```

For a production server, you would normally use a process manager such as PM2 or a properly configured system service so that your application can recover after a process or server restart.

---

## 7. What Is SSH?

SSH stands for **Secure Shell**.

It is a network protocol that allows you to securely access and manage a remote computer.

When you launch a Linux EC2 instance, you can use SSH to open its terminal from your personal computer.

### Example

Your local computer is running Windows.

Your EC2 instance is running Ubuntu Linux.

You can connect to Ubuntu from your Windows terminal using SSH.

```bash
ssh -i my-ec2-key.pem ubuntu@YOUR_PUBLIC_IP
```

After a successful connection, you can execute Linux commands on the remote server.

For example:

```bash
pwd
ls
mkdir my-app
cd my-app
```

These commands run on the EC2 server, not on your personal computer.

### SSH vs RDP

| Feature | SSH | RDP |
|---|---|---|
| Main use | Remote terminal access | Remote desktop access |
| Common operating systems | Linux | Windows |
| Default port | 22 | 3389 |
| Interface | Command line | Graphical desktop |

**Remember:** SSH is commonly used to manage Linux EC2 instances, while RDP is commonly used for Windows instances.

---

## 8. What Can We Use Amazon EC2 For?

EC2 is a general-purpose virtual computing service.

You can use it for many different workloads.

### 8.1 Hosting Node.js Applications

You can deploy:

- Node.js backend.
- Express APIs.
- REST APIs.
- WebSocket servers.

Example:

A backend API for an e-commerce application.

### 8.2 Hosting React Applications

You can build your React application and serve its static files through Nginx or another web server running on EC2.

### 8.3 Hosting Next.js Applications

You can run a Next.js application on EC2 using Node.js.

For example:

- Build your Next.js application.
- Configure the server.
- Start the production application.
- Configure Nginx as a reverse proxy.
- Connect your domain.

EC2 provides flexibility because you manage the server environment yourself.

### 8.4 Running Background Workers

You can run background processes that handle tasks such as:

- Sending emails.
- Processing uploaded files.
- Generating reports.
- Processing queued jobs.
- Performing scheduled operations.

### 8.5 Running Docker Containers

You can install Docker on EC2 and run containerized applications.

This is useful when you want more control over the application environment.

### 8.6 Hosting Custom Software

You can install and run many types of software as long as they are supported by the selected operating system and instance configuration.

---

## 9. When Should We Use EC2?

EC2 is useful when you need control over your server and its environment.

Consider EC2 when:

- You need to configure the operating system.
- You want to install custom software.
- You need control over server processes.
- You want to deploy Node.js applications manually.
- You need to configure Nginx or Apache.
- You want to run Docker containers on a virtual machine.
- You need a custom background worker.
- You want to manage your own server infrastructure.

### When Might EC2 Not Be Necessary?

EC2 is not the only way to deploy applications on AWS.

Depending on your requirements, other services may be more suitable.

| Requirement | AWS Service to Consider |
|---|---|
| Virtual server with operating system control | EC2 |
| Managed relational database | RDS |
| Object/file storage | S3 |
| Serverless functions | Lambda |
| Managed container deployment | ECS / Fargate |
| Next.js hosting with supported workflows | Amplify |
| Content delivery network | CloudFront |
| DNS management | Route 53 |

For example, if you only need object storage for user images, you generally do not need an EC2 instance just for that. Amazon S3 is designed for object storage.

---

## 10. EC2 Instance Lifecycle

An EC2 instance can be in different states.

Understanding these states is important because they affect availability and billing.

### 10.1 Pending

The instance is being launched.

AWS is preparing the required resources.

### 10.2 Running

The instance is active and can run applications.

Compute charges generally apply while the instance is running.

### 10.3 Stopping

The instance is in the process of shutting down.

### 10.4 Stopped

The instance is shut down.

For instances backed by EBS root volumes, you can generally start the instance again.

The EBS volumes remain available, and storage charges continue.

### 10.5 Shutting Down

The instance is preparing to terminate.

### 10.6 Terminated

The instance has been permanently terminated.

You cannot restart a terminated instance.

### Lifecycle Overview

```text
             Launch
                |
                v
             Pending
                |
                v
             Running
                |
         +------+------+
         |             |
         v             v
       Stop          Reboot
         |
         v
      Stopping
         |
         v
       Stopped
         |
         v
       Starting
         |
         v
       Running

       Running
          |
          v
       Terminate
          |
          v
      Terminated
```

### Stop vs Reboot vs Terminate

| Action | What Happens |
|---|---|
| Stop | Shuts down the instance |
| Start | Starts a stopped instance |
| Reboot | Restarts the operating system |
| Terminate | Permanently deletes the instance |

### Important Billing Notes

- Running instances generally incur compute charges.
- Stopped EBS-backed instances generally do not incur instance compute charges.
- EBS storage can continue to incur charges while the instance is stopped.
- Other associated resources may also incur charges.
- Terminating an instance does not necessarily delete every resource associated with it.

Always review your EC2 and AWS billing information.

**Important:** Stop is not the same as Terminate. Do not terminate an instance if you intend to use that same instance again.

---

## 11. EC2 Pricing Models

Amazon EC2 offers different purchasing options.

The main options include:

### 11.1 On-Demand Instances

You pay for computing capacity according to the applicable usage rates without a long-term commitment.

Useful for:

- Learning.
- Testing.
- Development.
- Unpredictable workloads.
- Short-term server requirements.

### 11.2 Reserved Instances

Reserved Instances provide pricing discounts in exchange for a commitment to a particular usage configuration over a specified term.

They are useful for workloads with predictable long-term requirements.

### 11.3 Savings Plans

Savings Plans provide discounted pricing when you commit to a consistent amount of eligible compute usage over a specified term.

### 11.4 Spot Instances

Spot Instances use spare AWS compute capacity and can provide significant discounts.

However, AWS can interrupt Spot Instances when it needs the capacity back.

Useful for workloads that can tolerate interruptions, such as:

- Batch processing.
- Fault-tolerant jobs.
- Certain testing workloads.
- Distributed data processing.

### Important

Pricing depends on factors such as:

- AWS Region.
- Instance type.
- Operating system.
- Purchasing option.
- Storage.
- Data transfer.
- Other attached resources.

Always check the current AWS pricing page before launching resources for a real project.

---

## 12. How to Launch an EC2 Instance in AWS Console

Now let's understand the practical process.

We will use a Linux instance as an example.

### Step 1: Open EC2

1. Sign in to the AWS Management Console.
2. Search for `EC2`.
3. Open the Amazon EC2 service.
4. Select the AWS Region where you want to create the instance.

**Important:** EC2 resources are Region-specific. Make sure you are working in the intended Region.

### Step 2: Launch Instance

1. Open the Instances section.
2. Click **Launch instances**.
3. Enter a name for your instance.

Example:

`my-first-ec2-server`

### Step 3: Select an AMI

Under Application and OS Images, choose an operating system.

For learning, you can select:

`Ubuntu Server`

Choose an appropriate supported AMI available in your Region.

### Step 4: Select an Instance Type

Choose an instance type that fits your learning requirements.

For a basic learning environment, consider a small eligible instance type.

Check its specifications and pricing before launching.

### Step 5: Create a Key Pair

1. Open the Key Pair section.
2. Click Create new key pair.
3. Enter a name.

Example:

`my-first-ec2-key`

4. Select a supported key type.
5. Choose the private key format.
6. Create and securely save the private key file.

Do not lose or expose this private key.

### Step 6: Configure Network Settings

Select the VPC and subnet.

For a basic internet-accessible learning server:

- Enable public IP assignment if needed.
- Configure a Security Group.
- Allow SSH access only from your own public IP address where possible.

Do not expose unnecessary ports.

### Step 7: Configure Storage

Choose an EBS volume size suitable for the operating system and your learning requirements.

For example:

`20 GiB`

The actual storage requirement depends on your software and application.

Review the volume type and Delete on termination setting.

### Step 8: Review and Launch

Review:

- AMI.
- Instance type.
- Key pair.
- Network settings.
- Security Group.
- Storage.

Click **Launch instance**.

### Step 9: Check Instance Status

Open the Instances page.

You should see your newly created instance.

Wait until:

- Instance state becomes `Running`.
- Status checks pass.

### Step 10: Connect to the Instance

Select your instance and click **Connect**.

Depending on your configuration, you can use EC2 Instance Connect or SSH.

For EC2 Instance Connect, the instance must meet the networking and software prerequisites.

For SSH, use your private key:

```bash
ssh -i my-first-ec2-key.pem ubuntu@YOUR_PUBLIC_IP
```

Replace `YOUR_PUBLIC_IP` with the instance's actual public IP address.

You are now connected to your Linux server.

---

## 13. What Is a Public IP and Private IP?

An IP address is used to identify a device or network interface for communication.

EC2 instances can have public and private IP addresses.

### Public IP

A Public IP address can be used to communicate with an instance over the internet when the networking configuration allows it.

Example:

Your laptop connects to an EC2 instance through its public IP address.

### Private IP

A Private IP address is used for communication within private network environments, such as a VPC.

Example:

Your backend EC2 instance communicates with another resource inside the same VPC using private networking.

### Comparison

| Feature | Public IP | Private IP |
|---|---|---|
| Main purpose | Internet-facing communication | Private network communication |
| Internet reachability | Possible with proper configuration | Not directly reachable over the public internet |
| Common use | Public web server | Internal application resources |
| Example | Public-facing web server | Private database server |

### Important

A public IP alone does not make your instance accessible.

You also need appropriate:

- VPC configuration.
- Subnet routing.
- Internet Gateway where required.
- Security Group rules.
- Operating system firewall configuration.

A public IPv4 address can change when an EBS-backed instance is stopped and started.

For a stable public IPv4 address, AWS provides Elastic IP addresses, subject to their applicable pricing and usage rules.

---

## 14. What Is an Elastic IP?

An Elastic IP is a static public IPv4 address designed for use with AWS resources such as EC2 instances.

It can be associated with an instance or network interface.

### Why Do We Need It?

Suppose your application uses this public IP:

`203.0.113.10`

You configure your domain or another service to communicate with that address.

If the instance is stopped and started, its automatically assigned public IPv4 address may change.

An Elastic IP can provide a stable public IPv4 address instead.

### Important

- Elastic IP addresses are public IPv4 addresses.
- They can be associated with supported AWS resources.
- Their use is subject to AWS pricing rules.
- Do not allocate addresses that you do not need.

---

## 15. Deploying a MERN Backend on EC2

Now let's connect EC2 with your actual development work.

Imagine you have built an e-commerce application.

Your architecture is:

```text
              Users
                |
                v
         React Frontend
                |
                v
       Node.js / Express API
                |
                v
            Amazon EC2
                |
        +-------+-------+
        |               |
        v               v
    MongoDB Atlas    Amazon S3
    (Database)       (Images)
```

### Deployment Process

#### Step 1: Prepare Your Backend

Make sure your backend application works locally.

For example:

```bash
npm install
npm start
```

#### Step 2: Launch EC2

Create a suitable Linux EC2 instance.

Configure:

- AMI.
- Instance type.
- EBS storage.
- Key pair.
- Security Group.
- VPC and subnet.

#### Step 3: Connect to EC2

Connect to the instance using SSH or EC2 Instance Connect.

#### Step 4: Install Required Software

Install:

- Git.
- Node.js.
- npm.
- Nginx, if required.
- PM2 or another suitable process manager.

#### Step 5: Upload Backend Code

Clone your GitHub repository:

```bash
git clone YOUR_REPOSITORY_URL
```

#### Step 6: Configure Environment Variables

Create a production environment configuration.

Example:

```env
PORT=5000
MONGODB_URI=your_mongodb_connection_string
```

Do not commit production secrets to GitHub.

Use an appropriate secret-management approach for production.

#### Step 7: Install Dependencies

```bash
npm install
```

#### Step 8: Start the Application

For a simple test:

```bash
npm start
```

For production, use an appropriate process manager and production configuration.

#### Step 9: Configure Nginx

Nginx can act as a reverse proxy.

For example:

```text
User Request
     |
     v
Nginx (Port 80 / 443)
     |
     v
Node.js Backend (Port 5000)
```

Nginx receives HTTP or HTTPS requests and forwards them to the Node.js application.

#### Step 10: Connect Your Frontend

Configure your frontend to use the backend API's domain or public endpoint.

For production, configure HTTPS and a proper domain.

**Note:** This section explains the overall deployment flow. We will study and implement the deployment process separately.

---

## 16. EC2 vs Other AWS Services

EC2 is not designed to replace every AWS service.

Different AWS services solve different problems.

| Service | Main Purpose |
|---|---|
| EC2 | Virtual servers |
| S3 | Object and file storage |
| RDS | Managed relational databases |
| Lambda | Serverless functions |
| ECS | Container orchestration |
| Fargate | Managed compute for containers |
| Amplify | Application hosting and deployment workflows |
| CloudFront | Content delivery network |
| Route 53 | DNS and domain routing |

### Example

Suppose your application needs:

- Backend hosting.
- Image storage.
- Relational database.
- Content delivery.

You might use:

- EC2 for backend hosting.
- S3 for images.
- RDS for relational database storage.
- CloudFront for content delivery.

Each service performs a different responsibility.

---

## 17. EC2 Security Best Practices

When using EC2, security should be considered from the beginning.

Follow these practices:

1. Use Security Groups with only required inbound ports.
2. Restrict SSH access to trusted IP addresses where possible.
3. Never expose database ports publicly unless there is a specific, properly secured requirement.
4. Keep your operating system and installed software updated.
5. Protect your SSH private keys.
6. Never commit `.pem` files or production credentials to GitHub.
7. Use IAM Roles for EC2 applications that need access to AWS services.
8. Follow the Principle of Least Privilege.
9. Use HTTPS for production websites.
10. Monitor server activity and application logs.
11. Back up important data.
12. Review storage and network configurations before terminating instances.

---

## 18. Important EC2 Concepts to Remember

| Concept | Meaning |
|---|---|
| EC2 | AWS virtual computing service |
| Instance | Virtual server |
| AMI | Template used to launch an instance |
| Instance Type | Defines CPU, RAM, and other computing resources |
| EBS | Persistent block storage |
| Instance Store | Temporary instance storage |
| Key Pair | Used for secure server authentication |
| Security Group | Virtual firewall |
| VPC | Isolated virtual network |
| Subnet | Smaller network inside a VPC |
| Public IP | Publicly routable IP address |
| Private IP | Private network IP address |
| Elastic IP | Static public IPv4 address |
| SSH | Secure remote terminal access |
| RDP | Remote desktop access |
| Nginx | Web server and reverse proxy |
| PM2 | Node.js process manager |

---

## 19. Practical Learning Exercises

After understanding the theory, complete these exercises.

### Exercise 1: Explore EC2

- [ ] Open the EC2 Console.
- [ ] Explore the Instances section.
- [ ] Explore AMIs.
- [ ] Explore Instance Types.
- [ ] Explore Key Pairs.
- [ ] Explore Security Groups.
- [ ] Explore EBS Volumes.
- [ ] Understand the VPC and subnet selection.

### Exercise 2: Launch Your First EC2 Instance

- [ ] Select a Linux AMI.
- [ ] Select an appropriate instance type.
- [ ] Create a key pair.
- [ ] Configure a Security Group.
- [ ] Select storage.
- [ ] Launch the instance.
- [ ] Wait for the instance to become Running.

### Exercise 3: Connect to Your Server

- [ ] Connect using EC2 Instance Connect or SSH.
- [ ] Run `pwd`.
- [ ] Run `ls`.
- [ ] Run `whoami`.
- [ ] Check the operating system.
- [ ] Update the package list.
- [ ] Install Git.

### Exercise 4: Understand Instance Management

- [ ] Stop the instance.
- [ ] Understand its state.
- [ ] Start the instance again.
- [ ] Check its public IP.
- [ ] Understand reboot vs stop vs terminate.

### Exercise 5: Prepare for Backend Deployment

- [ ] Install Node.js.
- [ ] Install npm.
- [ ] Clone a test backend repository.
- [ ] Install project dependencies.
- [ ] Run a simple Node.js server.
- [ ] Understand how network ports affect access.

**Important:** Before creating resources, check the current AWS pricing and Free Tier or credit eligibility for your account. Stop or terminate resources when they are no longer required, and review associated storage and networking charges.

---

## 20. Learning Progress

### AWS Learning Roadmap

- [x] IAM Users
- [x] IAM Groups
- [x] IAM Roles
- [x] IAM Policies
- [x] Principle of Least Privilege
- [ ] EC2
- [ ] S3
- [ ] Route 53
- [ ] CloudFront
- [ ] VPC
- [ ] RDS
- [ ] ElastiCache
- [ ] AWS Amplify
- [ ] ECS / Fargate
- [ ] ECR
- [ ] Lambda
- [ ] API Gateway
- [ ] CloudWatch
- [ ] Secrets Manager
- [ ] CI/CD

### EC2 Learning Checklist

- [x] Understand what EC2 is.
- [x] Understand why EC2 is used.
- [x] Understand EC2 Instances.
- [x] Understand AMIs.
- [x] Understand Instance Types.
- [x] Understand EBS.
- [x] Understand Key Pairs.
- [x] Understand Security Groups.
- [x] Understand VPC and Subnets.
- [x] Understand SSH.
- [x] Understand Public IP and Private IP.
- [x] Understand Elastic IP.
- [x] Understand EC2 Lifecycle.
- [x] Understand EC2 Pricing Models.
- [x] Understand EC2 use cases.
- [ ] Launch and connect to an EC2 instance.
- [ ] Install Node.js on EC2.
- [ ] Deploy a Node.js backend.
- [ ] Connect an IAM Role to EC2.
- [ ] Give EC2 controlled access to S3.

**Note:** The theory has been documented. Practical exercises will be marked complete only after they have been performed and tested.

---

## 21. Official AWS Documentation

Use these official AWS resources for deeper learning:

1. [What Is Amazon EC2?](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/concepts.html)
2. [Getting Started With EC2](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EC2_GetStarted.html)
3. [EC2 Instance Types](https://docs.aws.amazon.com/ec2/latest/instancetypes/instance-types.html)
4. [Amazon Machine Images (AMIs)](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AMIs.html)
5. [Amazon EBS](https://docs.aws.amazon.com/ebs/latest/userguide/what-is-ebs.html)
6. [EC2 Key Pairs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html)
7. [Security Groups](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-security-groups.html)
8. [Connecting to Linux Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/connect-linux-inst-eic.html)
9. [Stop and Start EC2 Instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Stop_Start.html)
10. [EC2 Pricing](https://aws.amazon.com/ec2/pricing/)

---

## Conclusion

Amazon EC2 is one of the fundamental AWS services for cloud computing.

It provides virtual servers that developers and organizations can use to host applications, run backend services, process data, and manage custom computing environments.

For MERN and Next.js developers, EC2 is especially useful when they want direct control over their server, operating system, application processes, and deployment environment.

Understanding EC2 is an important step toward learning AWS deployment and cloud infrastructure.

**Next Step:** Launch your first EC2 instance, connect to it, explore its Linux environment, and then learn how to attach an IAM Role to EC2.

---

**Learning Path:** AWS Cloud & DevOps for MERN + Next.js Developers

**Topic:** Amazon Elastic Compute Cloud (EC2)

**Document:** EC2 Fundamentals

**Status:** Theory Completed | Practical Implementation Pending