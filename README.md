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

```bash

```bash
echo "deb [signed-by=/usr/share/keyrings/cloudflare-main.gpg] https://pkg.cloudflare.com/cloudflared any main" | sudo tee /etc/apt/sources.list.d/cloudflared.list
```bash

```bash
sudo apt-get update && sudo apt-get install cloudflared
```bash
