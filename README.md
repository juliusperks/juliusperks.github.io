# juliusperks.github.io

**DNS Basics: Custom Domain Setup Tutorial**

**Prerequisites:**
- A registered Domain (Namecheap has been used for this tutorial)
- A GitHub Account
- A DigitalOcean droplet (or other cloud VM) running Ubuntu with NGINX

**Task 1: Custom Domain for GitHub Pages**
*Step 1: Create the GitHub Pages Repository*
1) Go to GitHub -> New Repository
2) Name it <username>.github.io
3) Set to Public
4) Check "Add a README file"
5) Click Create Repository

*Step 2: Add an index.html*
1) In the repo, click Add file -> Create new file
2) Name it index.html
3) Add some basic content:
       <h1>Hello</h1>
4) Click Commit changes

*Step 3: Configure DNS Records in Namecheap*
1) Log into Namecheap -> Domain List -> Click Manage -> Advanced DNS tab
2) Add 5x A records and 1x CNAME record

Type    Host    Value                        TTL
A       @       185.199.108.153              Automatic
A       @       185.199.109.153              Automatic
A       @       185.199.110.153              Automatic
A       @       185.199.111.153              Automatic
A       lab     <DigitalOcean Droplet IP>    Automatic
CNAME   www     <username>.github.io         Automatic

3) Save changes

*Step 4: Configure Custom Domain in GitHub*
1) Go to your repo -> Settings -> Pages
2) Under "Custom domain" enter your domain from Namecheap
3) Click save
4) Wait for the DNS pass for pass (may take up a few minutes)
***note:*** If you have recently bought the domain, then the A record changes may not have propogated through yet. Wait an hour or so.
5) Check "Enforce HTTPS"

*Step 5: Setup website on Droplet VM*
1) SSH onto your droplet
2) Create the site config:
sudo nano /etc/nginx/sites-available/lab.<Namecheap Domain>
3) Add this config:
server {
    listen 80;
    server_name lab.juliusperks.online;
    root /var/www/html;
    index index.html;
}
4) Enable the site:
sudo ln -s /etc/nginx/sites-available/lab.<Namecheap domain> /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
5) Install SSL Certificate
sudo apt update && sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d lab.juliusperks.online
6) Verify
curl -I https://lab.juliusperks.online
