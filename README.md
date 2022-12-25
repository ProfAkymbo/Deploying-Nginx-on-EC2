# Deploying-Nginx-on-EC2
A step by step guide to deploy Nginx web server displaying IP address of the EC2 instance using bash script


Here we write a bash script that we can use to deploy an Nginx web server on an Amazon Elastic Compute Cloud (EC2) instance, and configure it to display the IP address of the instance:

'''
#!/bin/bash

# Update package manager
sudo apt-get update

# Install Nginx web server
sudo apt-get install -y nginx

# Get the IP address of the EC2 instance
INSTANCE_IP=$(curl http://169.254.169.254/latest/meta-data/public-ipv4)

# Create a new configuration file for the default Nginx server block
sudo bash -c "cat > /etc/nginx/sites-available/default <<EOF
server {
    listen 80 default_server;
    listen [::]:80 default_server;

    root /var/www/html;
    index index.html index.htm index.nginx-debian.html;

    server_name _;

    location / {
        try_files \$uri \$uri/ =404;
    }
}
EOF"

# Create a new index.html file in the root directory of the Nginx server
sudo bash -c "cat > /var/www/html/index.html <<EOF
<html>
<head>
  <title>EC2 instance IP address</title>
</head>
<body>
  <h1>EC2 instance IP address: $INSTANCE_IP</h1>
</body>
</html>
EOF"

# Restart Nginx to apply the changes
sudo service nginx restart
'''


To use this script, copy and paste it into a file, then make the file executable by running the following command:

'''
chmod +x deploy_nginx.sh
'''

Then, you can run the script by typing:

'''
./deploy_nginx.sh
'''

This script will update the package manager on the EC2 instance, install Nginx, retrieve the public IP address of the instance using the EC2 metadata service, create a new configuration file for the default Nginx server block, create a new index.html file in the root directory of the Nginx server, and restart Nginx to apply the changes.

Once the script has completed, you should be able to access the Nginx web server by visiting the public IP address of the EC2 instance in a web browser. The page should display the IP address of the EC2 instance.
