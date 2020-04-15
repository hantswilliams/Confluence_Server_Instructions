# Confluence_Server_Instructions

## EC2 Instance Setup 
- Note - sure transfer this below into a ansible file and deploy with Terraform in the future 
- NGINX Installation 
  - `sudo apt-get update` 
  - `sudo apt-get install -y nginx`
  - `sudo service nginx start`
  - `sudo rm /etc/nginx/sites-enabled/default`
  - `sudo nano /etc/nginx/sites-available/confluence.com`
  - Populate that new file confluence.com with the following: 
  ```
  server {
    listen 80;
    server_name confluence.hantsawilliams.com;
    location / {
        client_max_body_size 100m;
        proxy_set_header X-Forwarded-Host $host;
        proxy_set_header X-Forwarded-Server $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 
        proxy_pass http://localhost:8090/;
    }
    location /synchrony {
        proxy_set_header X-Forwarded-Host $host;
        proxy_set_header X-Forwarded-Server $host;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_pass http://localhost:8091/synchrony;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "Upgrade";
    }
}
  ```
    - Then enable dynamic linking to sites-enabled: `sudo ln -s /etc/nginx/sites-available/confluence.com /etc/nginx/sites-enabled/confluence.com`
