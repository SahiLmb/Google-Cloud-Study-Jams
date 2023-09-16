# Create and Manage Cloud Resources: Challenge Lab 

## Task 1 
### Create Variables and replace the values which are specified in your lab
(Note: <i>everyone will have different values.</i>)

```
export INSTANCE_NAME=[replace with your instance name]
export ZONE=[replace with your zone name]
export REGION=[replace with your region name]
export PORT=[replace with your port]
export FIREWALL_NAME=[replace with your firewall name]

gcloud compute instances create $INSTANCE_NAME \
--network nucleus-vpc \
--zone $ZONE \
--machine-type e2-micro \
--image-family debian-10 \
--image-project debian-cloud
```
## Task 2 
### Creating and authenticating the cluster

```
gcloud container clusters create nucleus-backend \
--num-nodes 1 \
--network nucleus-vpc \
--zone $ZONE

gcloud container clusters get-credentials nucleus-backend \
--zone $ZONE
```

* To create a new Deployment from the hello-app container image, run the following kubectl create command:

```
kubectl create deployment hello-server \--image=gcr.io/google-samples/hello-app:2.0
```


*  To create a Kubernetes Service, which is a Kubernetes resource that lets you expose your application to external traffic, run the following kubectl expose command:

```
kubectl expose deployment hello-server \
--type=LoadBalancer \
--port $PORT
```

## TASK 3 
### Create 2 nginx web server(can copy this template from lab).

```
cat << EOF > startup.sh
#! /bin/bash
apt-get update
apt-get install -y nginx
service nginx start
sed -i -- 's/nginx/Google Cloud Platform - '"\$HOSTNAME"'/' /var/www/html/index.nginx-debian.html
EOF
```

* Create an instance template, target pool and managed instance group(MIG).
```
gcloud compute instance-templates create web-server-template \
--metadata-from-file startup-script=startup.sh \
--network nucleus-vpc \
--machine-type g1-small \
--region $ZONE

gcloud compute target-pools create nginx-pool --region=$REGION

gcloud compute instance-groups managed create web-server-group \
--base-instance-name web-server \
--size 2 \
--template web-server-template \
--region $REGION
```

*  Create a firewall rule named as Firewall rule to allow traffic (80/tcp).
```
gcloud compute firewall-rules create $FIREWALL_NAME \
--allow tcp:80 \
--network nucleus-vpc
```

* Create a health check.
```
gcloud compute http-health-checks create http-basic-check
gcloud compute instance-groups managed \
set-named-ports web-server-group \
--named-ports http:80 \
--region $REGION
```

*  Create a backend service, and attach the managed instance group with the named port (http:80).

```
gcloud compute backend-services create web-server-backend \
--protocol HTTP \
--http-health-checks http-basic-check \
--global
```
```
gcloud compute backend-services add-backend web-server-backend \
--instance-group web-server-group \
--instance-group-region $REGION \
--global
```

* Create a URL map, and target the HTTP proxy to route requests to your URL map.

```
gcloud compute url-maps create web-server-map \
--default-service web-server-backend
```
```
gcloud compute target-http-proxies create http-lb-proxy \
--url-map web-server-map
```

* Create a forwarding rule.

```
gcloud compute forwarding-rules create http-content-rule \
--global \
--target-http-proxy http-lb-proxy \
--ports 80
```
```
gcloud compute forwarding-rules create $FIREWALL_NAME \
--global \
--target-http-proxy http-lb-proxy \
--ports 80
gcloud compute forwarding-rules list
```
## <i> Wait for two mins before clicking the last progress check button and then keep on clicking the button until the green tick is displayed.</i>
## Done with  Quest 5!!


