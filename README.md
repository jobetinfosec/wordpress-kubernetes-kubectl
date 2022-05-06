### I) Install Nginx ingress

a) Run<br />
`helm upgrade --install ingress-nginx ingress-nginx --repo https://kubernetes.github.io/ingress-nginx --namespace ingress-nginx --create-namespace`

b) Display Ingress public IP running<br />
`kubectl --namespace ingress-nginx get services -o wide -w ingress-nginx-controller`

c) Go to your DNS maintainer's site and create an A DNS record for your WordPress site<br />



### II) Install Cert-Manager

`helm repo add jetstack https://charts.jetstack.io`

`helm repo update`

`helm install cert-manager jetstack/cert-manager --namespace cert-manager --create-namespace --version v1.7.1 --set installCRDs=true`



### III) Setup secrets

a) Open kustomization.yaml and change passwords as you wish<br />

b) Run<br />
`kubectl apply -k .`


### IV) Create MySQL persistent volume

Run<br />
`kubectl apply -f mysql-pv-volume.yaml` 

then<br />
`kubectl apply -f mysql-pv-claim.yaml`


### V) Create WordPress persistent volume

Run<br />
`kubectl apply -f wordpress-pv-volume.yaml`

then<br />
`kubectl apply -f wordpress-pv-claim.yaml`


### VI) Install MySQL service

a) Display current secrets

Run<br />
`kubectl describe secret`

Take note of current values of mysql-password-, mysql-user-, mysql-user-password- and mysql-database-<br />

b) Open mysql-service.yaml file and then replace the XXX placeholders with the current values from above<br />

c) Then run<br />
`kubectl apply -f mysql-service.yaml`


### VII) Install WordPress service

a) Open wordpress-service.yaml and replace XXX placeholders with the current values from above<br />

b) Run<br />
`kubectl apply -f wordpress-service.yaml`


### VIII) Create Let's Encrypt SSL certificate

a) Open wp-production-issuer.yaml file and replace EMAIL_ADDRESS with the address you wish

b) Run<br />
`kubectl apply -f wp-production-issuer.yaml`


### IX) Apply SSL certificate to your domain

a) Open wordpress-ingress.yaml file and replace FQDN_NAME with the FQDN name of your WordPress site

b) Run<br />
`kubectl apply -f wordpress-ingress.yaml`


### X) Run WordPress 5-minutes setup

Open your browser and go to https://FQDN_NAME

The WordPress setup page should be displayed in your browser!




