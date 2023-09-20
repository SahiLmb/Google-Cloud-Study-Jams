# Dataflow: Qwik Start - Templates # GSP192 

## Run in cloudshell

### Check for region in task 3
```cmd
export REGION=
```
```cmd
gcloud services disable dataflow.googleapis.com
gcloud services enable dataflow.googleapis.com
```
```cmd
bq mk taxirides
bq mk \
--time_partitioning_field timestamp \
--schema ride_id:string,point_idx:integer,latitude:float,longitude:float,\
timestamp:timestamp,meter_reading:float,meter_increment:float,ride_status:string,\
passenger_count:integer -t taxirides.realtime
gsutil mb gs://$DEVSHELL_PROJECT_ID/
sleep 45
```
## Perform task 3 from lab instructions
