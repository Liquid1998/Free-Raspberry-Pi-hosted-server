# Free-Raspberry-Pi-hosted-server
Deploy internet wide apache2 server or website on Raspberry pi

# buy your domain for free
we will buy a domain for free from digitalplat website.

go to https://dash.domain.digitalplat.org and search for required domain with the TLD to be us.kg at the end.

for example <your_domain_name>.us.kg
If it is available, sign up and grab that domain.

It will require KYC key that you can grab from one of the notification in dashboard of digitalplat.

after filling all the necessary details for whois save it. leave ns field empty. if it requires put ns1 and ns2. we will change it later after we get NS info from cloudflare.

# Cloudflare NS server

sign up to cloudflare and put the domain as your domain that you grabbed from digitalplat website.

cloudflare will provide you with 2 NS servers like xxx.ns1.cloudflare.com and yyy.ns2.cloudflare.com

put those two NS servers in your digitalplat websites domain.

# Host apache2 server on raspberry pi

login to your raspberry pi os and install apache2 package using sudo apt-get install apache2.

turn on the apache2 server using. sudo service apache2 start and you will see default apache2 page on http://localhost:80 .

Now we have to put that server or website internet wide that means you can access your website from anywhere using your domain and for that we will use cloudflare tunnel.

# Cloudflare tunnel

install cloudflared in your raspberry pi using sudo apt-get install cloudflared.

if you can not find that package in raspberry pi, you can follow below method by putting keyring in your raspberry pi.

```bash
sudo mkdir -p --mode=0755 /usr/share/keyrings

curl -fsSL https://pkg.cloudflare.com/cloudflare-main.gpg | sudo tee /usr/share/keyrings/cloudflare-main.gpg >/dev/null

echo "deb [signed-by=/usr/share/keyrings/cloudflare-main.gpg] https://pkg.cloudflare.com/cloudflared any main" | sudo tee /etc/apt/sources.list.d/cloudflared.list

sudo apt-get update && sudo apt-get install cloudflared
```

## Now we will create tunnel that will connect our localhost apache webserver to our domain that we bought.

```bash

cloudflared tunnel login
```
Running this command will:
Open a browser window and prompt you to log in to your Cloudflare account. After logging in to your account, select your hostname.
Generate an account certificate, the cert.pem file, in the default cloudflared directory.

```bash

cloudflared tunnel create <Tunnel_Name>
```
Running this command will:
Create a tunnel by establishing a persistent relationship between the name you provide and a UUID for your tunnel. At this point, no connection is active within the tunnel yet.
Generate a tunnel credentials file in the default cloudflared directory.
Create a subdomain of .cfargotunnel.com.

Afetr that, In your .cloudflared directory, create a config.yml file using any text editor. This file will configure the tunnel to route traffic from a given origin to the hostname of your choice.

config.yml:

```bash
url: http://localhost:8000
tunnel: <Tunnel-UUID>
credentials-file: /root/.cloudflared/<Tunnel-UUID>.json
```

Now assign a CNAME record that points traffic to your tunnel subdomain:

```bash
cloudflared tunnel route dns <tunnel_UUID or tunnel_NAME> <hostname or domain name that you bought from digitalplat>
```

## Run the tunnel

```bash
cloudflared tunnel run <UUID or NAME>
```

Now, open private window and try to access your domain.
