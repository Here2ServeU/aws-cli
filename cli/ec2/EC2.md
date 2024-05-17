# The following steps will help you launch an EC2 instance using the AWS CLI. You will also install NGINX on the instance.

# To create a key pair.
aws ec2 create-key-pair --key-name nginx --query 'KeyMaterial' --output text > nginx.pem

# To change permissions of the key pair.
chmod 400 nginx.pem

# To create a security group. 
aws ec2 create-security-group --group-name t2s-nginx-sg --description "Security group for SSH and HTTP" --vpc-id vpc-02110db7947ceb3f5


# To allow SSH access on port 22. 
aws ec2 authorize-security-group-ingress --group-id sg-071a908845cdaa34a --protocol tcp --port 22 --cidr 172.31.0.0/16

aws ec2 authorize-security-group-ingress --group-id sg-071a908845cdaa34a --protocol tcp --port 22 --cidr 216.249.239.209/32
   
# To find your system IP: 
curl ifconfig.me
curl icanhazip.com

# To allow HTTP access (port 80).
aws ec2 authorize-security-group-ingress --group-id sg-071a908845cdaa34a --protocol tcp --port 80 --cidr 0.0.0.0/0

# To create an ec2 instance and install NGINX on it. 
aws ec2 run-instances \
  --image-id ami-04b70fa74e45c3917 \
  --count 1 \
  --instance-type t2.micro \
  --key-name nginx \
  --security-group-ids sg-05eb76ee2b0381efc \
  --subnet-id subnet-05cb29a9a04e93491 \
  --tag-specifications
'ResourceType=instance,Tags=[{Key=Name,Value=webserver-nginx}]' \

# Install and Start NGINX
sudo apt update
sudo apt install -y nginx
sudo systemctl start nginx
sudo systemctl enable nginx

# To verify the instance and NGINX installation. 
aws ec2 describe-instances --filters "Name=tag:Name,Values=webserver-nginx"

# To check NGINX status.
ssh -i /path/to/your-key.pem ubuntu@<Public-IP>
systemctl status nginx
