# User Authentication: Identity-Aware Proxy #GSP499 

## Run in Cloudshell
```cmd
gcloud services enable appengineflex.googleapis.com
gsutil cp gs://spls/gsp499/user-authentication-with-iap.zip .
unzip user-authentication-with-iap.zip
cd user-authentication-with-iap
cd 1-HelloWorld
gcloud app deploy
cd ~/user-authentication-with-iap/2-HelloUser
gcloud app deploy
cd ~/user-authentication-with-iap/3-HelloVerifiedUser
gcloud app deploy
LINK=$(gcloud app browse)
LINKU=${LINK#https://}
cat > details.json << EOF
{
  App name: IAP Example
  Application home page: $LINK
  Application privacy Policy link: $LINK/privacy
  Authorized domains: $LINKU
  Developer Contact Information: xyz@gmail.com
}
EOF
cat details.json
```
### Press 10 and y when asked (3 times it will ask for y)
### Identity-Aware Proxy > Enable Api > GO TO IDENTITY-AWARE PROXY > CONFIGURE CONSENT SCREEN > Select Internal > Create
### Fill the blanks from the Terminal > SAVE AND CONTINUE >SAVE AND CONTINUE > BACK TO DASHBOARD
### Reload the previous page > Turn on App Engine app > Check the box > ADD PRINCIPAL 
### "USERNAME" > Role= `IAP-Secured Web App User` > SAVE
