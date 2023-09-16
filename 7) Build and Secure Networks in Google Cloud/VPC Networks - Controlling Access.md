# VPC Networks - Controlling Access #GSP213 

## Run in cloudshell
### Assigned Zone is in task 1, step 3.
```cmd
export ZONE=[input your assigned zone from the lab here]
```
### Make a PROJECT_ID variable.
```cmd
export PROJECT_ID=$(gcloud config get-value project)
```
### Creating blue server
```cmd
gcloud compute instances create blue \
  --zone=$ZONE \
  --machine-type=e2-medium \
  --tags=web-server
```
### Creating green server without network tag
```cmd
gcloud compute instances create green \
  --zone=$ZONE \
  --machine-type=e2-medium
```
### Do `Install nginx and customize the welcome page` as per instructions in lab.

## Task 2. Create the firewall rule
```cmd
gcloud compute firewall-rules create allow-http-web-server \
  --network=default \
  --action=ALLOW \
  --direction=INGRESS \
  --source-ranges=0.0.0.0/0 \
  --target-tags=web-server \
  --rules=tcp:80,icmp
```
## Create a test-vm
```cmd
gcloud compute instances create test-vm --machine-type=e2-micro --subnet=default --zone=$ZONE
```
## Task 3. Explore the Network and Security Admin roles
### IAM & admin > Service Accounts > Create service account > Name= `Network-admin` > Role > Compute Engine > Compute Network Admin > Save
<br/>

### follow all the instructions in the lab and close the lab once the score gets 100/100 or you can continuw with the extra activities.
