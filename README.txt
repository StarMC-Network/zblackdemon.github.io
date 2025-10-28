//
//
// 123gg456.com v1.0
// MADE BY `123gg456_`. IF REUSING TO SELL SOMEWHERE ELSE AFTER EDIT, DONT FORGET TO CREDIT ME.
// IF ANY QUESTIONS OR NEED HELP TO SETUP, CONTACT ME AT https://123gg456.com/contact OR ON DISCORD ITSELF
// 
// YOU HAVE TO ADD A API KEY (random generated/self made password) IN ORDER FOR CONTACT FORM SYSTEM TO WORK
// /api/discord-webhook/contact/route.ts && /components/ui/contact-page.tsx && .env (optional if you intend to use that)
//  
//
// DON'T BE SHY TO HELP ME FIX THE MISTAKES I MADE IN THIS!!
// ALSO DONT FORGET TO DONATE ME :)      (if you can....)
//
//

HERE IS HOW TO SETUP NGINX WEBSERVER FOR THIS TEMPLATE.
(Replace <domain.com with ur domain>)
Steps:
1) run the following commands

apt install certbot nginx -y 
systemctl stop nginx
certbot certonly --standalone -d <domain.com>
systemctl start nginx
rm /etc/nginx/sites-enabled/default

2) now go to ur dns records of domain and point the the domain on ur IP
3) follow these steps
i. run this command

nano /etc/nginx/sites-enabled/<domain.com>

ii. paste this in there
-------------------------------------------------------------------------------


# Redirect www and HTTP to HTTPS
server {
    listen 80;
    server_name <domain.com> <www.domain.com>;
    return 301 https://<domain.com>$request_uri;
}

# Main HTTPS block
server {
    listen 443 ssl;
    server_name <domain.com>;

    ssl_certificate /etc/letsencrypt/live/<domain.com>/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/<domain.com>/privkey.pem;
    include /etc/letsencrypt/options-ssl-nginx.conf;
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;

    location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}


-------------------------------------------------------------------------------

iii. press ctrl+x and y then press enter to continue

4) Now finally run these commands:

cd /path/to/serverfiles
npm i --force
npm run build
npm install pm2 --force
pm2 start "npm run start" --name webserver
pm2 save
pm2 startup
systemctl restart nginx

5) Check if your webserver is online, if not go again thru all the steps!


Thanks for using my product :)