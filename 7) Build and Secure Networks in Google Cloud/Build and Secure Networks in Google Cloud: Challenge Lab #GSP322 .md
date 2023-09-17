# Build and Secure Networks in Google Cloud: Challenge Lab  
# GSP322 

## Run the following commands in the Cloud Shell Terminal.(gcloud)
### Let's start with defining some variables given by Cloud Skill Boosts

* For exporting the zone, first go to *Compute Engine* > *VM Instances* > Copy `Any Instance Zone`(both instances zone should be in the same.)

```
export ZONE=
```

```
export SSH_IAP_NETWORK_TAG=
```

```
export SSH_INTERNAL_NETWORK_TAG=
```

```
export HTTP_NETWORK_TAG=
```
## Task 1 : Remove the overly permissive rules

```
gcloud compute firewall-rules delete open-access --quiet
```

## Task 2 : Start the bastion host instance
```
gcloud compute instances start bastion --zone=$ZONE
```

## Task 3 : Create a firewall rule that allows SSH (tcp/22) from the IAP service and add network tag on bastion
```cmd
gcloud compute firewall-rules create gcsj \
    --action=ALLOW \
    --network=acme-vpc \
    --rules=tcp:22 \
    --source-ranges=35.235.240.0/20 \
    --target-tags=$SSH_IAP_NETWORK_TAG \
    --description="Allow SSH from IAP service" \
    --gcloud compute instances add-tags bastion --tags=$SSH_IAP_Network_tag --zone=$ZONE
```

## Task 4 : Create a firewall rule that allows traffic on HTTP (tcp/80) to any address and add network tag on juice-shop
```
gcloud compute firewall-rules create gdsc \
    --action=ALLOW \
    --network=acme-vpc \
    --rules=tcp:80 \
    --source-ranges=0.0.0.0/0 \
    --target-tags=$HTTP_NETWORK_TAG \
    --description="Google Cloud Study Jams" \
gcloud compute instances add-tags juice-shop --tags=$HTTP_Network_Tag --zone=$ZONE
```
## Task 5 : Create a firewall rule that allows traffic on SSH (tcp/22) from acme-mgmt-subnet network address and add network tag on juice-shop
```
gcloud compute firewall-rules create mgmcet \
    --action=ALLOW \
    --network=acme-vpc \
    --rules=tcp:22 \
    --source-ranges=192.168.10.0/24 \
    --target-tags=$SSH_INTERNAL_NETWORK_TAG \
    --description="Google Cloud Study Jams" \
gcloud compute instances add-tags juice-shop --tags=$SSH_Internal_Network_tag --zone=$ZONE \
gcloud compute ssh bastion --zone=$ZONE --quiet
```
## Task 6 : SSH to bastion host via IAP and juice-shop via bastion
```
gcloud compute ssh juice-shop --internal-ip
```