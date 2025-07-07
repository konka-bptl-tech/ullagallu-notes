# Phase-1
---
1. Create RDS instance
   - Create Route53 Record
   - Connect and Load Schema
```bash
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

2. Luanch Elastic Cache valkey
   - Create Route53 Record
   
3. Just Launch Ec2 instance install sudo dnf install redis6
- Checking the connection: 
```bash 
redis6-cli -h dns-name -p port --tls --insecure
```
- Checking connection with telnet: 
```bash 
telnet dns-name port 
``` 
- To come out from telnet: Press `Ctrl + ]` then type `quit` and press Enter. </pre>

4. Prepare AMI wihtout service file
- Go to aws shell install packer using below commands
```bash
#!/bin/bash
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://rpm.releases.hashicorp.com/AmazonLinux/hashicorp.repo
sudo yum -y install packer
```
5. create folder backend and create backend.sh

```bash
#!/bin/bash
# Variables
USERID=$(id -u) # User id
TIMESTAMP=$(date +%F-%H-%M-%S)
SCRIPT_NAME=$(basename "$0" | cut -d "." -f1)
LOG_FILE="/tmp/${TIMESTAMP}-${SCRIPT_NAME}.log"
REPO_URL="https://github.com/sivaramakrishna-konka/3-tier-vm-backend.git"
APP_DIR="/app"
# SERVICE_FILE="/etc/systemd/system/backend.service"
# CW_CONFIG_FILE="/opt/aws/amazon-cloudwatch-agent/etc/amazon-cloudwatch-agent.json"

# Colors for terminal output
R="\e[31m"   # Red
G="\e[32m"   # Green
Y="\e[33m"   # Yellow
N="\e[0m"    # Reset

echo "Script started executing at: $TIMESTAMP"
echo "Log file: $LOG_FILE"

# Function to log command success or failure
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

# Check if the script is run as root
if [ $USERID -ne 0 ]; then
    echo -e "${R}Please run this script with sudo privileges${N}" | tee -a "$LOG_FILE"
    exit 1
else
    echo -e "${G}Here we go to installation${N}" | tee -a "$LOG_FILE"
fi

dnf update -y &>>"$LOG_FILE"
LOG "Updating the packages" $?

curl -fsSL https://rpm.nodesource.com/setup_20.x | bash - &>>"$LOG_FILE"
LOG "Downloading Node.js setup script" $?

# Install required packages
dnf install git telnet nodejs amazon-cloudwatch-agent -y &>>"$LOG_FILE"
LOG "Installing git, telnet, Node.js 20, and CloudWatch Agent" $?

# Add 'expense' user if not exists
if ! id -u expense &>/dev/null; then
    useradd expense &>>"$LOG_FILE"
    LOG "Adding 'expense' user" $?
fi

# Clone backend repository if directory doesn't exist
if [ ! -d "$APP_DIR" ]; then
    mkdir "$APP_DIR" &>>"$LOG_FILE"
    LOG "Creating directory $APP_DIR" $?

    git clone "$REPO_URL" "$APP_DIR" &>>"$LOG_FILE"
    LOG "Cloning expense-backend repository to $APP_DIR" $?
else
    echo "Directory $APP_DIR already exists. Skipping cloning." | tee -a "$LOG_FILE"
fi

# Install dependencies for the backend
cd "$APP_DIR" && npm install &>>"$LOG_FILE"
LOG "Installing npm dependencies for the backend" $?
echo "Script execution completed successfully." | tee -a "$LOG_FILE"
```

```bash
# Prometheus
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

- create backend.pkr.hcl in backend folder
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

  provisioner "shell" {
    inline = [
      "chmod +x /tmp/backend.sh",
      "sudo /tmp/backend.sh"
    ]
  }
}
```

- Create AMI

```bash
cd backend
packer init .
packer fmt .
packer validate .
packer build .
```

5. Create secrets in secrets manager and non sensitive data in parameter store
- aws secrets manager --> store a new secret --> other type of secret[enter key value pairs] --> secretname --> review and enter
```json
{
  "DB_USER": "crud",
  "DB_PASSWORD": "CrudApp@1"
}
```
- enter the path like <environment>/<project_name>/<credentials_name> test/curd/db_crentials

- go to parameter store enter all non-sensitive data in secure string type
```bash
/test/crud/DB_HOST[name]=test-db.konkas.tech[value]
/test/crud/DB_NAME[name]=crud_app[value]
/test/crud/REDIS_HOST[name]=test-redis.konkas.tech[value]
```
6. Create IAM policy to get secrets and paramter strore

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
          "arn:aws:ssm:REGION:ACCOUNT_ID:parameter/test/crud/*" # Replace with your actual arn 
        ]
      },
      {
        "Effect": "Allow",
        "Action": [
          "secretsmanager:GetSecretValue"
        ],
        "Resource": [
          "arn:aws:secretsmanager:REGION:ACCOUNT_ID:secret:crud/db/credentials-??????" # Replace with actual arn
        ]
      }
    ]
  }
```

- create iam role backend crednetials and ssm parameter and attach above policy

7. Create Luanch template with backend-ami choose key and in assign instance profile above create role and enter user data

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
8. Luanch ASG with above template
9. create target group and launch alb
10. create route 53 record for ALB DNS

11. Create folder frontend and inside create
- frontend.sh
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
- create frontend.pkr.hcl
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
  # Adding tags to the AMI
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

  provisioner "shell" {
    inline = [
      "chmod +x /tmp/frontend.sh",
      "sudo /tmp/frontend.sh"
    ]
  }
}
```
- create AMI
```bash
cd frontend
packer init .
packer fmt .
packer validate .
packer build .
```
11. Create S3 bucket and place nginx.conf in it
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

        # Serve frontend
        location / {
            try_files $uri $uri/ /index.html;
        }

        # Proxy API requests to backend
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
12. Create IAM role with below policy
```json
{
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowReadAccessToNginxConf",
      "Effect": "Allow",
      "Action": [
        "s3:GetObject"
      ],
      "Resource": "arn:aws:s3:::your-bucket-name/path/to/nginx.conf"
    }
  ]
}
```
- create iam role
13. Create Luanch template with frontend-ami choose key and in assign instance profile above create role and enter user data
```bash
#!/bin/bash
aws s3 cp s3://test-siva-nginx-conf/nginx.conf /etc/nginx/nginx.conf
systemctl restart nginx
```
14. create ASG TG and ALB record for ALB DNS

15. Check the entries in redis

```markdown    
redis6-cli -h test-redis.konkas.tech -p 6379 --tls --insecure GET "all_entries" | jq .
```
16. create certificate frontend-vm.konkas.tech same as create records to Route53

17. create cloud front 
```markdown
create distribution --> choose origin frontend-vm.konkas.tech --> Alternative Name frontend-vm.konkas.tech --> certificate choose frontend-vm.konkas.tech

Behaviours --> edit --> Allowed HTTP methods[ALLHTTPMethodsChoose][RedirectHTTPTOHTTPS]
```


```markdown
Our developers make sure api response should be 15 ms latency application architectural response we enable elastic cache it ensure less latency some times it returns cache connnect we enable cloud front for edge locations
- most of the errors are coming by misconfiguration
```
---

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
