This is a personal project focused on deploying Nexloud via docker or kubernetes based on the user's preference.

What is used:

nextcloud

mariadb - database

swag - certificate

duckdns - domain

redis - caching frequently accessed data (optional)

Pre-requisites

First - create an acoount in duckdns from here: https://www.duckdns.org/ Register your domain name and IP and open ports 443 and 80 to the IP address. Once all this is done copy the token for the DuckDNS web and paste it in the yaml file as the value for DUCKDNSTOKEN. Information  with " " is custom that you have to replace. Also /path/to/folder is information that is adjustedable according to the enviroment you use.

Installation via Docker:

After everything is corrected in the yaml file use: -docker compose pull / -docker compose up -d These two commands will pull the images and deploy the containers. To verify if instances is up use: -docker ps You should see four containers: nextcloud , swag,  mariadb and redis. Now we find the file named nextcloud.subdomain.conf.sample under SWAG's /config/nginx/proxy-confs folder and rename it to nextcloud.subdomain.conf, then restart the SWAG container . The restart can be done with: -docker stop swag -docker compose up -d

If this is the first time accessing Nextcloud, simply navigate to https://nextcloud.example.duckdns.org and the Nextcloud set up page should be up. Fill out the info, use the mariadb user and the password that were selected in the environment variable and  use mariadb as the Database Host address (container name as dns hostname).

For more information reffer to the website I used as pointer: https://docs.linuxserver.io/general/swag/#nextcloud-subdomain-reverse-proxy-example


Installation via Kubernetes:

Make sure NGINX Ingress Controller is installed via helm and setup:  helm repo add ingress-nginx https://charts.ingress-nginx.io

Make sure cert-manager is installed via helm and setup: helm repo add jetstack https://charts.jetstack.io

Deploy each of the kubernetes yaml config files to your cluster via: kubectl apply -f <yaml-file-name>

You can check the status of your application on the cluster via: kubectl get pods / kubectl get svc / kubectl describe pod <pod-name>

For more information reffer to:

https://kubernetes.io/docs/home/
https://helm.sh/docs/






