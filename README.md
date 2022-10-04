# Setup DynDNS #

* Create new domain with Strato: cootooloo.com
* Activate DynDNS in the domain settings
* In Fritzbox go to DynDNS setting and configure DynDNS
    ** DynDNS Provider: Strato
    ** Domainname: cootooloo.com
    ** Username: cootooloo.com
    ** Password: <*DynDNS pasword from Strato*>
* Add port forwarding for ports 80 and 443 to the ip if your local machine

# Setup Subdomains #
* Create subdomain: portainer.cootooloo.com
* Set CNAME to 'cootooloo.com.' (do not forget the **dot**)
* Setup Nginx to listen to ports 443 and 80
* Setup proxy to redirect whenever a request originated from that subdomain

```conf
http {
    ...
    server {
        server_name  portainer.cootooloo.com;
        listen 80;

        location / {
            proxy_pass http://portainer:9000/;
        }
    }
}

```

# Create SSL Certificates #
* Install certbot or include certbot installation in Dockerfile
```
apt install certbot python3-certbot-nginx -y
```
* Make sure the corresponding domain is reachable via port 80
```sh
certbot --nginx -d portainer.cootooloo.com
```

Certbot will automatically rewrite nginx.conf so it will use port 443 and certificates instead.