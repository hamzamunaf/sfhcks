# Salesforce H@cks
*stuff I'd like to keep …* 

## ~/PermissionSetAssignment.cls
Assigne Permission Sets from one User to an other

Place source and target user IDs and execute anonymous.  
![PermissionSetAssignment](https://github.com/HeikoKramer/sfhcks/blob/master/img/psa.png)

## ~/ObjectNameAndLabel.cls
This sends you an email with API Names & Labels of those org's objects. 

Replace that variable with your mail address:  
![mail](https://github.com/HeikoKramer/sfhcks/blob/master/img/mail.png)

### HTML Version
Quick way to get table format  >> it will send you those values tagged as html table.  
Copy mail body into texteditor >> save as .html file >> open in browser  
Then copy it into Excel or wherever you need it.    
![html](https://github.com/HeikoKramer/sfhcks/blob/master/img/html.png)

### Plain Text Version
If you just need a quick overview on those org's object names and labels.
This will send you a text-list of those Names & Labels readable from your mail client.
![plain](https://github.com/HeikoKramer/sfhcks/blob/master/img/plain.png)

## ~/EmailAlertRecipients.cls
Had to find all Email Alerts which where sending mails to a certain public group.  
Wrote this Anonymous Apex to find a recipent in alert metadata using Tooling API.

### How to execute 
Just specify your recipient in line 23 and fire >> results in the debug log
![recipient](https://github.com/HeikoKramer/sfhcks/blob/master/img/recipient.png)

## ~/ValidationRuleOnOff.cls
Trouble with some Validation Rules while loading data into a Sandbox …  
So why not switch them all off and back on again when we're done?

All we need is this 3 Steps **anonymous Apex ON/OFF switch for Validation Rules**  
![comments](https://github.com/HeikoKramer/sfhcks/blob/master/img/comments.png)


### Step 1 
some VRs in the org … some active, some inactive  
![vrBefore](https://github.com/HeikoKramer/sfhcks/blob/master/img/vrBefore.png)

execute STEP 1 will use the Tooling API to find active VRs and store their data in Cases

see … three cases created in demo org (testes with 39 Validation Rules in Sandbox)  
![cases](https://github.com/HeikoKramer/sfhcks/blob/master/img/cases.png)

The VR's enpoint get stored in the Subject, the Metadata in the Description of the Case  
![caseDetail](https://github.com/HeikoKramer/sfhcks/blob/master/img/caseDetail.png)


### Step 2
execute STEP 2 change Metadata active:false and perform a PATCH method call on all VRs  

All Validation Rules get switched OFF  
![vrWhile](https://github.com/HeikoKramer/sfhcks/blob/master/img/vrWhile.png)


### Step 3
when all loads are done, STEP 3 can be executed to restore the prior VC state  

prior active Validation Rules get switched ON  
![vrAfter](https://github.com/HeikoKramer/sfhcks/blob/master/img/vrAfter.png)

those data storage cases get deleted at the end of STEP 3 
