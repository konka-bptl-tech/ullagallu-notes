# Manual setup
---
1. Create RDS instance

   * Create Route53 Record

2. Launch Elastic Cache valkey

   * Create Route53 Record

3. Just Launch EC2 instance and pass userdata script

```bash
#!/bin/bash
dnf update -y
dnf install git telnet redis6 mariadb105 -y
```

```bash
# connect rds
mysql -h <dns> -u <username> -p # It will prompts for password enter
```

* Load the Schema

```sql
CREATE DATABASE IF NOT EXISTS crud_app;
USE crud_app;

CREATE TABLE entries (
  id INT AUTO_INCREMENT PRIMARY KEY,
  amount INT NOT NULL,
  description VARCHAR(255) NOT NULL
);
CREATE USER IF NOT EXISTS 'crud'@'%' IDENTIFIED BY 'CrudApp@1';
GRANT ALL ON crud_app.* TO 'crud'@'%';
FLUSH PRIVILEGES;
```

* Checking the connection:

```bash
redis6-cli -h dns-name -p port --tls --insecure
```

* Checking connection with telnet:

```bash
telnet dns-name port 
```

> To come out from telnet: Press `Ctrl + ]` then type `quit` and press Enter.

### 4. Prepare AMI without hardcoding environment variables
### ✅ `backend` Folder Structure:

```
carvo
|
packer.sh
backend/
├── backend.sh
├── service.sh
└── backend.pkr.hcl
```

* Go to AWS shell
* `mkdir carvo` folder
* Create `packer.sh` and place below commands into that:

```bash
#!/bin/bash
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo
sudo yum -y install packer
```

---

### 5. Create folder `backend` and create `backend.sh`

> This is

```bash
# NodeExporter verion of backend.sh
#!/bin/bash

# Variables
USERID=$(id -u)
TIMESTAMP=$(date +%F-%H-%M-%S)
SCRIPT_NAME=$(basename "$0" | cut -d "." -f1)
LOG_FILE="/tmp/${TIMESTAMP}-${SCRIPT_NAME}.log"
REPO_URL="https://github.com/sivaramakrishna-konka/3-tier-vm-backend.git"
APP_DIR="/app"
NODE_EXPORTER_VERSION="1.9.1"
NODE_EXPORTER_USER="ec2-user"
NODE_EXPORTER_DIR="/home/${NODE_EXPORTER_USER}/node_exporter"

# Colors
R="\e[31m"
G="\e[32m"
Y="\e[33m"
N="\e[0m"

echo "Script started at: $TIMESTAMP"
echo "Log file: $LOG_FILE"

# Logging function
LOG() {
    local MESSAGE="$1"
    local STATUS="$2"
    if [ "$STATUS" -eq 0 ]; then
        echo -e "$MESSAGE ..... ${G}Success${N}" | tee -a "$LOG_FILE"
    else
        echo -e "$MESSAGE ..... ${R}Failed${N}" | tee -a "$LOG_FILE"
        exit 1
    fi
}

# Check if script is run as root
if [ $USERID -ne 0 ]; then
    echo -e "${R}Please run this script with sudo privileges${N}" | tee -a "$LOG_FILE"
    exit 1
else
    echo -e "${G}Starting installation process...${N}" | tee -a "$LOG_FILE"
fi

# Update system and install packages
dnf update -y &>>"$LOG_FILE"
LOG "System update" $?

curl -fsSL https://rpm.nodesource.com/setup_20.x | bash - &>>"$LOG_FILE"
LOG "Adding Node.js 20 repo" $?

dnf install git telnet nodejs amazon-cloudwatch-agent -y &>>"$LOG_FILE"
LOG "Installing git, telnet, Node.js, CloudWatch Agent" $?

# Add 'expense' user if not present
if ! id -u expense &>/dev/null; then
    useradd expense &>>"$LOG_FILE"
    LOG "Adding user 'expense'" $?
fi

# Backend app setup
if [ ! -d "$APP_DIR" ]; then
    mkdir "$APP_DIR" &>>"$LOG_FILE"
    LOG "Creating app directory" $?

    git clone "$REPO_URL" "$APP_DIR" &>>"$LOG_FILE"
    LOG "Cloning backend repo" $?
else
    echo "Directory $APP_DIR already exists. Skipping cloning." | tee -a "$LOG_FILE"
fi

cd "$APP_DIR" && npm install &>>"$LOG_FILE"
LOG "Installing backend npm dependencies" $?

# Node Exporter Setup
cd /home/$NODE_EXPORTER_USER || exit 1
wget https://github.com/prometheus/node_exporter/releases/download/v${NODE_EXPORTER_VERSION}/node_exporter-${NODE_EXPORTER_VERSION}.linux-amd64.tar.gz &>>"$LOG_FILE"
LOG "Downloading Node Exporter v${NODE_EXPORTER_VERSION}" $?

tar -xvzf node_exporter-${NODE_EXPORTER_VERSION}.linux-amd64.tar.gz &>>"$LOG_FILE"
mv node_exporter-${NODE_EXPORTER_VERSION}.linux-amd64 node_exporter &>>"$LOG_FILE"
rm -f node_exporter-${NODE_EXPORTER_VERSION}.linux-amd64.tar.gz
LOG "Extracted Node Exporter" $?

# Create systemd service for Node Exporter
cat <<EOF > /etc/systemd/system/node_exporter.service
[Unit]
Description=Prometheus Node Exporter
After=network.target

[Service]
User=${NODE_EXPORTER_USER}
ExecStart=${NODE_EXPORTER_DIR}/node_exporter
Restart=on-failure

[Install]
WantedBy=default.target
EOF

LOG "Node Exporter systemd service file created" $?

# Reload and start Node Exporter service
systemctl daemon-reexec &>>"$LOG_FILE"
systemctl daemon-reload &>>"$LOG_FILE"
systemctl enable node_exporter &>>"$LOG_FILE"
systemctl start node_exporter &>>"$LOG_FILE"
LOG "Node Exporter service started" $?

echo -e "${G}Script execution completed successfully.${N}" | tee -a "$LOG_FILE"
```

---

### In the same folder create `service.sh`

```bash
#!/bin/bash
set -e

APP_DIR="/app"
SERVICE_FILE="/etc/systemd/system/backend.service"
SECRET_NAME="test/curd/db_crentials"

# Install necessary tools
echo "Installing awscli and jq"
dnf install -y awscli jq

# Fetch Secrets from Secrets Manager
echo "Fectching Secrets from SecretsManager"
SECRETS=$(aws secretsmanager get-secret-value --secret-id $SECRET_NAME --query SecretString --output text)
DB_USER=$(echo "$SECRETS" | jq -r .DB_USER)
DB_PASSWORD=$(echo "$SECRETS" | jq -r .DB_PASSWORD)

# Fetch Configuration from SSM Parameter Store
echo "Fetching non-sensitive data from ParameterStore"
DB_HOST=$(aws ssm get-parameter --name "/test/crud/DB_HOST" --with-decryption --query "Parameter.Value" --output text)
DB_NAME=$(aws ssm get-parameter --name "/test/crud/DB_NAME" --with-decryption --query "Parameter.Value" --output text)
REDIS_HOST=$(aws ssm get-parameter --name "/test/crud/REDIS_HOST" --with-decryption --query "Parameter.Value" --output text)

# Generate systemd service file
echo "Generate service file for backend"
cat <<EOF > $SERVICE_FILE
[Unit]
Description=Backend Service

[Service]
User=expense
Environment="DB_HOST=${DB_HOST}"
Environment="DB_USER=${DB_USER}"
Environment="DB_PASSWORD=${DB_PASSWORD}"
Environment="DB_NAME=${DB_NAME}"
Environment="REDIS_HOST=${REDIS_HOST}"
ExecStart=/usr/bin/node ${APP_DIR}/server.js
SyslogIdentifier=backend
Restart=always

[Install]
WantedBy=multi-user.target
EOF

# Start service
echo "Restart enable and start backend"
systemctl daemon-reload
systemctl enable backend
systemctl start backend
```

---

### Create `backend.pkr.hcl` in `backend` folder

```hcl
packer {
  required_plugins {
    amazon = {
      source  = "github.com/hashicorp/amazon"
      version = "~> 1"
    }
  }
}

source "amazon-ebs" "amz3_gp3" {
  ami_name      = "sivab-{{timestamp}}"
  instance_type = "t3.micro"
  region        = "us-east-1"
  
  source_ami_filter {
    filters = {
      name                = "al2023-ami-2023*"
      architecture        = "x86_64"
      root-device-type    = "ebs"
    }
    most_recent = true
    owners      = ["amazon"]
  }

  ssh_username  = "ec2-user"

  # Adding tags to the AMI
  tags = {
    Name        = "sivab-packer-image"
    Environment = "Development"
    Owner       = "Konka"
    CreatedBy   = "Packer"
    Monitor     = "true"
  }
}

build {
  name    = "sivab"
  sources = ["source.amazon-ebs.amz3_gp3"]

  provisioner "file" {
    source      = "backend.sh"
    destination = "/tmp/backend.sh"
  }

  provisioner "file" {
    source      = "service.sh"
    destination = "/tmp/init-backend-service.sh"  # ✅ Use /tmp instead of /usr/local/bin
  }

  provisioner "shell" {
    inline = [
      "chmod +x /tmp/backend.sh",
      "sudo /tmp/backend.sh",
      "chmod +x /tmp/init-backend-service.sh",
      "sudo mv /tmp/init-backend-service.sh /usr/local/bin/init-backend-service.sh",
      "sudo chown root:root /usr/local/bin/init-backend-service.sh"
    ]
  }
}

```
---
### Create AMI
```bash
cd backend
packer init .
packer fmt .
packer validate .
packer build .
```
---
### **5. Create Secrets in Secrets Manager and Non-Sensitive Data in Parameter Store**

#### 📌 Secrets Manager

* Go to **AWS Secrets Manager** → **Store a new secret**
* Choose **Other type of secret** → Enter key-value pairs:

```json
{
  "DB_USER": "crud",
  "DB_PASSWORD": "CrudApp@1"
}
```

* Secret name:

```
test/curd/db_crentials
```

> 🔒 Store credentials in secret name path format like:

```
<environment>/<project_name>/<credentials_name>
```

---

#### 📌 Parameter Store (secure string type)

Go to **AWS Systems Manager > Parameter Store** and create below parameters as **SecureString**:

```bash
/test/crud/DB_HOST        = test-db.konkas.tech
/test/crud/DB_NAME        = crud_app
/test/crud/REDIS_HOST     = test-redis.konkas.tech
```

---

### **6. Create IAM Policy to Allow Access to Secrets & Parameters**

Create the following IAM policy (update `REGION` and `ACCOUNT_ID` accordingly):

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ssm:GetParameter",
        "ssm:GetParameters"
      ],
      "Resource": [
        "arn:aws:ssm:REGION:ACCOUNT_ID:parameter/test/crud/*"
      ]
    },
    {
      "Effect": "Allow",
      "Action": [
        "secretsmanager:GetSecretValue"
      ],
      "Resource": [
        "arn:aws:secretsmanager:REGION:ACCOUNT_ID:secret:test/curd/db_crentials-??????"
      ]
    }
  ]
}
```

---

* Go to **IAM** → Create Role
* Trusted entity: **EC2**
* Attach above **policy**
* Name the role:

```
backend-credentials-role
```

---

### **7. Create Launch Template (for Backend)**

* Go to **EC2 > Launch Templates**
* Create new Launch Template:

  * Use **backend AMI**
  * Select **existing key pair**
  * Under **Advanced Details** → Add User Data (optional if required)
  * Under **IAM Instance Profile** → Select the above created IAM Role:
    `backend-credentials-role`

---

### **8. Launch Auto Scaling Group (ASG)**

* Go to **EC2 > Auto Scaling Groups**
* Create ASG using:

  * The above Launch Template
  * Attach to **VPC + Subnet**
  * Set desired/min/max capacity
  * Attach to existing or new Target Group

---

### **9. Create Target Group and Launch ALB**

* Go to **EC2 > Load Balancers** → Create ALB:

  * Type: **Application Load Balancer**
  * Listener: HTTP (port 80)
  * Select VPC and Subnets
  * Target Group: Use **backend ASG** instances

---

### **10. Create Route 53 Record for ALB DNS**

* Go to **Route 53 > Hosted Zones**
* Create new **A record** or **CNAME**

  * Name: `backend.konkas.tech` (example)
  * Value: ALB DNS name

---

## **11. Create `frontend` folder and files inside**

### 📁 Folder structure

```
frontend/
├── frontend.sh
├── boostrap-script.sh
├── frontend.pkr.hcl
```

---

### `frontend.sh`

```bash
#!/bin/bash

USERID=$(id -u)
TIMESTAMP=$(date +%F-%H-%M-%S)
SCRIPT_NAME=$(basename "$0" | cut -d "." -f1)
NODE_EXPORTER_VERSION="1.9.1"
NODE_EXPORTER_USER="ec2-user"
NODE_EXPORTER_DIR="/home/${NODE_EXPORTER_USER}/node_exporter"
LOG_FILE="/tmp/${TIMESTAMP}-${SCRIPT_NAME}.log"

# Colors
R="\e[31m"
G="\e[32m"
Y="\e[33m"
N="\e[0m"

echo "Started at: $TIMESTAMP"
echo "Log: $LOG_FILE"

LOG() {
    local MESSAGE="$1"
    local STATUS="$2"
    if [ "$STATUS" -eq 0 ]; then
        echo -e "$MESSAGE ..... ${G}Success${N}" | tee -a "$LOG_FILE"
    else
        echo -e "$MESSAGE ..... ${R}Failed${N}" | tee -a "$LOG_FILE"
        exit 1
    fi
}

# Check root
if [ "$USERID" -ne 0 ]; then
    echo -e "${R}Run with sudo/root${N}" | tee -a "$LOG_FILE"
    exit 1
fi

echo -e "${G}Starting frontend setup...${N}" | tee -a "$LOG_FILE"

# Install Node.js and Nginx
dnf install -y nginx git &>>"$LOG_FILE"
LOG "Installing Nginx and Git" $?

curl -fsSL https://rpm.nodesource.com/setup_20.x | bash - &>>"$LOG_FILE"
dnf install -y nodejs &>>"$LOG_FILE"
LOG "Installing Node.js" $?

# Enable and start Nginx
systemctl enable nginx &>>"$LOG_FILE"
LOG "Enabling Nginx" $?
systemctl start nginx &>>"$LOG_FILE"
LOG "Starting Nginx" $?

# Clear existing html
rm -rf /usr/share/nginx/html/* &>>"$LOG_FILE"
LOG "Clearing Nginx HTML folder" $?

# Clone and build frontend
cd /tmp
git clone https://github.com/sivaramakrishna-konka/3-tier-vm-frontend.git frontend-app &>>"$LOG_FILE"
LOG "Cloning frontend repo" $?

cd frontend-app
npm install &>>"$LOG_FILE"
LOG "Running npm install" $?

npm run build &>>"$LOG_FILE"
LOG "Running npm build" $?

# Copy built files to nginx
cp -r dist/* /usr/share/nginx/html/ &>>"$LOG_FILE"
LOG "Copying build to /usr/share/nginx/html" $?

# Node Exporter Setup
cd /home/$NODE_EXPORTER_USER || exit 1
wget https://github.com/prometheus/node_exporter/releases/download/v${NODE_EXPORTER_VERSION}/node_exporter-${NODE_EXPORTER_VERSION}.linux-amd64.tar.gz &>>"$LOG_FILE"
LOG "Downloading Node Exporter v${NODE_EXPORTER_VERSION}" $?

tar -xvzf node_exporter-${NODE_EXPORTER_VERSION}.linux-amd64.tar.gz &>>"$LOG_FILE"
mv node_exporter-${NODE_EXPORTER_VERSION}.linux-amd64 node_exporter &>>"$LOG_FILE"
rm -f node_exporter-${NODE_EXPORTER_VERSION}.linux-amd64.tar.gz
LOG "Extracted Node Exporter" $?

# Create systemd service for Node Exporter
cat <<EOF > /etc/systemd/system/node_exporter.service
[Unit]
Description=Prometheus Node Exporter
After=network.target

[Service]
User=${NODE_EXPORTER_USER}
ExecStart=${NODE_EXPORTER_DIR}/node_exporter
Restart=on-failure

[Install]
WantedBy=default.target
EOF

LOG "Node Exporter systemd service file created" $?

# Reload and start Node Exporter service
systemctl daemon-reexec &>>"$LOG_FILE"
systemctl daemon-reload &>>"$LOG_FILE"
systemctl enable node_exporter &>>"$LOG_FILE"
systemctl start node_exporter &>>"$LOG_FILE"
LOG "Node Exporter service started" $?

echo -e "${G}Frontend deployment complete.${N}" | tee -a "$LOG_FILE"
```

---

### `boostrap-script.sh`

```bash
#!/bin/bash
aws s3 cp s3://test-siva-nginx-conf/nginx.conf /etc/nginx/nginx.conf
systemctl restart nginx
```

---

### `frontend.pkr.hcl`

```hcl
packer {
  required_plugins {
    amazon = {
      source  = "github.com/hashicorp/amazon"
      version = "~> 1"
    }
  }
}

source "amazon-ebs" "amz3_gp3" {
  ami_name      = "sivaf-{{timestamp}}"
  instance_type = "t3.micro"
  region        = "us-east-1"

  source_ami_filter {
    filters = {
      name                = "al2023-ami-2023*"
      architecture        = "x86_64"
      root-device-type    = "ebs"
    }
    most_recent = true
    owners      = ["amazon"]
  }

  ssh_username  = "ec2-user"
  tags = {
    Name        = "sivaf-packer-image"
    Environment = "Development"
    Owner       = "Konka"
    CreatedBy   = "Packer"
    Monitor     = "true"
  }
}

build {
  name    = "sivaf"
  sources = ["source.amazon-ebs.amz3_gp3"]

  provisioner "file" {
    source      = "frontend.sh"
    destination = "/tmp/frontend.sh"
  }

  provisioner "file" {
    source      = "boostrap-script.sh"
    destination = "/usr/local/bin/boostrap-script.sh"
  }

  provisioner "shell" {
    inline = [
      "chmod +x /tmp/frontend.sh",
      "sudo /tmp/frontend.sh",
      "chmod +x /usr/local/bin/boostrap-script.sh",
      "chown root:root /usr/local/bin/boostrap-script.sh"
    ]
  }
}
```

---

## **12. Upload `nginx.conf` to S3**

### Bucket: `test-siva-nginx-conf`

```nginx
events {}

http {
    include /etc/nginx/mime.types;
    default_type application/octet-stream;

    log_format main '$remote_addr - $remote_user [$time_local] "$request" '
                    '$status $body_bytes_sent "$http_referer" '
                    '"$http_user_agent" "$http_x_forwarded_for"';

    access_log /var/log/nginx/access.log main;
    error_log /var/log/nginx/error.log warn;

    server {
        listen 80 default_server;
        server_name _;

        root /usr/share/nginx/html;
        index index.html;

        gzip on;
        gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;

        location / {
            try_files $uri $uri/ /index.html;
        }

        location /api/ {
            proxy_pass http://backend.konkas.tech;
            proxy_http_version 1.1;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header X-Forwarded-Proto $scheme;
        }
    }
}
```

---

## **13. Create IAM Role for Frontend EC2**

### Policy to read from S3:

```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowReadAccessToNginxConf",
      "Effect": "Allow",
      "Action": ["s3:GetObject"],
      "Resource": "arn:aws:s3:::test-siva-nginx-conf/nginx.conf"
    }
  ]
}
```

* Create IAM Role: `frontend-read-nginx-role`
* Attach policy above
* Trust entity: EC2

---

## **14. Create Launch Template for Frontend**

* Use **Frontend AMI**
* Attach **frontend IAM role**
* Optional: Add `bootstrap-script.sh` in User Data if needed

---

## **15. Create ASG, Target Group, ALB for Frontend**

* Create ASG using frontend launch template
* Register with Target Group
* Create Application Load Balancer (ALB)
* Route53 record → point to ALB DNS

---

## **16. Check Redis Entries**

```bash
redis6-cli -h test-redis.konkas.tech -p 6379 --tls --insecure GET "all_entries" | jq .
```

---


Perfect, Konka 🙌 — here is your **final Phase: CloudFront + SSL Setup** — clear and to-the-point.

---

## **16. Create SSL Certificate in ACM (Region: us-east-1)**

### 🛡️ ACM Certificate: `frontend-vm.konkas.tech`

1. Go to **AWS ACM** → **Request a public certificate**
2. Add domain name:

   ```
   frontend-vm.konkas.tech
   ```
3. Choose validation method: **DNS Validation**
4. ACM gives a CNAME record → **Copy it**
5. Go to **Route 53 > Hosted Zone > konkas.tech**
6. **Create Record** (CNAME) → Paste ACM validation record
7. Wait for status: ✅ **"Issued"**

---

## **17. Create CloudFront Distribution**

### 🌀 Steps:

1. Go to **CloudFront** → Click **"Create Distribution"**

---

### 🔧 **Origin Settings**

| Field               | Value                                    |
| ------------------- | ---------------------------------------- |
| **Origin Domain**   | `frontend-vm.konkas.tech`                |
| **Origin Protocol** | **HTTP Only** (default, backend is HTTP) |
| **Name**            | `frontend-origin`                        |

---

### 🌍 **General Settings**

| Field                        | Value                                      |
| ---------------------------- | ------------------------------------------ |
| **Alternate Domain (CNAME)** | `frontend-vm.konkas.tech`                  |
| **Custom SSL Certificate**   | Select: `frontend-vm.konkas.tech` from ACM |
| **Default Root Object**      | `index.html` *(optional)*                  |

---

### ⚙️ **Behavior Settings**

* Click **Edit Behavior**
* Change settings:

  | Field                      | Value                                                               |
  | -------------------------- | ------------------------------------------------------------------- |
  | **Allowed HTTP Methods**   | `GET, HEAD, OPTIONS, PUT, POST, PATCH, DELETE` *(All HTTP methods)* |
  | **Redirect HTTP to HTTPS** | ✅ Enabled                                                           |
  | **Caching**                | Use default (can adjust later)                                      |

---

### ☁️ Final Step:

1. Click **Create Distribution**
2. Wait for status: ✅ **Deployed**
3. Go to Route53 → Create new **A record**

   * Name: `frontend-vm.konkas.tech`
   * Alias to: **CloudFront distribution domain name**
---

```markdown
Our developers ensure that the API response latency remains within 15 ms. To support low-latency performance, we have integrated **Elastic Cache**, which helps in reducing load on the database and speeds up response times. Occasionally, errors related to cache connectivity may occur. Additionally, we have enabled **CloudFront** to serve content from edge locations, further improving performance and reducing latency for global users.

> Note: Most of the issues we encountered were due to **misconfigurations**, which were resolved through proper infrastructure validation and configuration management.
```





























# Phase-2[Monitoring Integration]
---
1. Launch EC2 instance 15GB amazon linux and t3a.small
2. Download the extract and mv package as prometheus
- https://prometheus.io/download/
```bash
wget  wget https://github.com/prometheus/prometheus/releases/download/v2.53.4/prometheus-2.53.4.linux-amd64.tar.gz
tar -xvzf prometheus-2.53.4.linux-amd64.tar.gz
mv prometheus-2.53.4.linux-amd64 prometheus
rm -rf prometheus-2.53.4.linux-amd64.tar.gz
```
2. Create Service file

```bash   
mkdir -p /home/ec2-user/prometheus/data
```
```bash
sudo nano /etc/systemd/system/prometheus.service
```

```bash
[Unit]
Description=Prometheus
Wants=network-online.target
After=network-online.target

[Service]
User=ec2-user
ExecStart=/home/ec2-user/prometheus/prometheus \
--config.file=/home/ec2-user/prometheus/prometheus.yml \
--storage.tsdb.path=/home/ec2-user/prometheus/data \
--web.console.templates=/home/ec2-user/prometheus/consoles \
--web.console.libraries=/home/ec2-user/prometheus/console_libraries

Restart=on-failure

[Install]
WantedBy=multi-user.target
```
```bash
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl enable prometheus
sudo systemctl start prometheus
sudo systemctl status prometheus
journalctl -u prometheus -xe
```

- 9090  = prometheus
- 9100  = Node exporter
- promtool check config prometheus.yml

# Node Exporter Downloads
```bash
wget https://github.com/prometheus/node_exporter/releases/download/v1.9.1/node_exporter-1.9.1.linux-amd64.tar.gz
tar -xvzf node_exporter-1.9.1.linux-amd64.tar.gz
mv node_exporter-1.9.1.linux-amd64 node_exporter
rm -rf node_exporter-1.9.1.linux-amd64.tar.gz
```
```bash
sudo nano /etc/systemd/system/node_exporter.service
```

```bash
[Unit]
Description=Prometheus Node Exporter
After=network.target

[Service]
User=ec2-user
ExecStart=/home/ec2-user/node_exporter/node_exporter
Restart=on-failure

[Install]
WantedBy=default.target
```
```bash
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl enable node_exporter
sudo systemctl start node_exporter
sudo systemctl status node_exporter
```

- curl http://localhost:9100/metrics

# Graafana
- https://grafana.com/docs/grafana/latest/setup-grafana/installation/redhat-rhel-fedora/
# Alert Manager
```bash
wget https://github.com/prometheus/alertmanager/releases/download/v0.28.1/alertmanager-0.28.1.linux-amd64.tar.gz
tar -xvzf alertmanager-0.28.1.linux-amd64.tar.gz
sudo mv alertmanager-0.28.1.linux-amd64 alertmanager
rm -rf alertmanager-0.28.1.linux-amd64.tar.gz
```
```bash
sudo nano /etc/systemd/system/alertmanager.service
```
```bash
[Unit]
Description=Prometheus Alertmanager
After=network.target

[Service]
User=ec2-user
ExecStart=/home/ec2-user/alertmanager/alertmanager \
  --config.file=/home/ec2-user/alertmanager/alertmanager.yml \
  --storage.path=/home/ec2-user/alertmanager/data
Restart=on-failure

[Install]
WantedBy=multi-user.target
```
```bash
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl enable alertmanager
sudo systemctl start alertmanager
sudo systemctl status alertmanager
```
- curl http://localhost:9093


# reasons for backend not running
- I face server errors[5xx] because valkey key is not running if you curl http://test-backend.konkas.tech
  you will get above errors
- For that check backend logs by using 
  - journalctl -u backend
  - telnet -h -p
  - ss -tunlp
- If you face client errors[4xx] because nginx proxy pass unable to reach backend because backend url is wrong or trailing slashes mis configured nginx proxy pass
- tail -f /var/log/nginx/access.log
- tail -f /var/log/nginx/error.log
- journalctl -u nginx

# prometheus-grafana setup
1. Install prometheus
    - prometheus dashboard
2. Launch another instance install NodeExporter test it
3. Install Grafana in Prometheus instance
4. Add the tags to get dynamic instances
