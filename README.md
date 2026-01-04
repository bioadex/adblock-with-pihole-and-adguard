# adblock-with-pihole-and-adguard
using-docker-compose-install-pihole-and-adguard-to-block-ads-and-other-unwanted-sites 
I will be installing pihole on rapsberry pi server while i install adguard on my ubuntu homesever using docker compose file 
You can copy the docker compose file and edit some line in side. 

After the Pihole is running you can decide to hide your server_ip from the URL to change it to customized URL name for easy remember 

firstly confirm the version of the pihole 
pihole version 
Output Result: 
v6.*
My Pihole is version 6 

After you have confirm the pihole version if its version 6 follow this step to hide your ubuntu_server_ip 
make sure that port 80 is free because nginx run on port 80 you can change pihole port to another port something like 8080 
Used this command 

sudo pihole-FTL --config webserver.port "8080,443os,[::]:8080,[::]:443os"      # move the pihole port to port 8080 

sudo systemctl restart pihole-FTL    # to restart pihole-FTL 

sudo ss -tulpn | grep LISTEN     # confirm is pihole is listening to Port 8080 

Step 1: 
Installing Nginx 
sudo apt update
sudo apt install nginx

Step 2:
Confirm Nginx is running and is on port 80 
systemctl status nginx.service 
Output:
Status ---> Running 

Step 3:
sudo nano /etc/nginx/sites-available/pihole   
Paste this inside
server {
    listen 80;
    server_name pihole.yourdomain.com;

    location / {
        proxy_pass http://localhost:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}

Ctrl O Enter Ctrl X       # to save and exit 

sudo ln -s /etc/nginx/sites-available/pihole /etc/nginx/sites-enabled/          # enable it 

Step 4:
Check Configuration and Restart Nginx
sudo nginx -t
sudo systemctl reload nginx

Step 5:
Access pihole with Customized URL 
pihole.yourdomain.com 

Final Note:  If you encounter any problem you can consult google or chatgpt

below are the visualization adguard dashboard

<img width="1467" height="968" alt="image" src="https://github.com/user-attachments/assets/314369c4-813b-403e-9e94-4980ad57c251" />

<img width="1234" height="944" alt="image" src="https://github.com/user-attachments/assets/d107acdf-3d66-40d0-accf-cbc52a18937c" />

Pi-hole dashboard on Raspberrypi 
<img width="1432" height="916" alt="image" src="https://github.com/user-attachments/assets/02b645ee-d73e-4142-97b3-ab0c6302fbd7" />

<img width="1254" height="945" alt="image" src="https://github.com/user-attachments/assets/eea500f5-3a65-462c-909c-38435a0e4304" />




