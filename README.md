# Salesforce H@cks
This repo is a lose collection of Salesforce stuff I find useful enough to keep. <br>
Likely it will mostly contain anonymous Apex scripts, but let's see where this is going. <br> 
I have an other Salesforce related repo for the [command line stuff](https://github.com/HeikoKramer/sfdx) <br>

## ON / OFF Switches
The purpose for my [on-/off switch scripts](https://github.com/HeikoKramer/sfhcks/tree/master/OnOffSwitches) was a data load intense full sandbox replacement project I have participated in. <br> 
The last things you need when loading a production backup into a partial sandbox via multiple etl jobs are validations and triggers. <br>
My scripts saved me from deactivating all that stuff manually, keep note of what was active, and manual activation after the load. <br>
The below shown method works with **validation rules**, **process builder** and **workflow rules**. <br>
It could probably quickly be adopted for **flows**, but wont unfortunately work with *Apex triggers*. <br> 
<br>
## Method:
* STEP1: Query vr, pb or wfr via **tooling api** -> store their metadedata in cases. <br>
* STEP2: Loop through those cases, switch active elements off via **tooling api**. <br>
* STEP3: Loop again through those cases, switch prior activated elements back on via **tooling api**. <br>

Each STEP is marked in the script. Execute them one-by-one. <br>
**NOTE:** You could run into troubles if you've active trigger, routing or validation logic set on the case object. <br>
If you have a lot of rules in your org you might hit api call limits. <br> 
<br>
## Direct links to ON/OFF switch for element:
[Process Builder](https://github.com/HeikoKramer/sfhcks/blob/master/OnOffSwitches/ProcessBuilderOnOff) <br>
[Validation Rules](https://github.com/HeikoKramer/sfhcks/blob/master/OnOffSwitches/ValidationRuleOnOff) <br>
[Workflow Rules](https://github.com/HeikoKramer/sfhcks/blob/master/OnOffSwitches/WorkflowRuleOnOff) <br>

<br>
<br>

old readme will fade out, I'm refactoring … <br>
----


## ~/PermissionSetAssignment.cls (work in progress)
Place source and target user IDs and execute anonymous to assigne Permission Sets from one User to an other.
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

## ~/ProcessBuilderOnOff.cls
Trouble with some Processes while loading data into a Sandbox …  
So why not switch them all off and back on again when we're done?

### Step 1 
some Processes in the org … some active, some inactive  
![Process1](https://github.com/HeikoKramer/sfhcks/blob/master/img/Process1.png)

execute STEP 1 will use the Tooling API to find and store active Processe data in Cases

see … two cases were created which hold the active process data
![Process2](https://github.com/HeikoKramer/sfhcks/blob/master/img/Process2.png)

The Process enpoint is stored in the Subject, the Metadata in the Description of the Case  
![Process3](https://github.com/HeikoKramer/sfhcks/blob/master/img/Process3.png)


### Step 2
execute STEP 2 change Metadata activeVersionNumber:null and perform a PATCH method call  

All Processes get switched OFF  
![Process4](https://github.com/HeikoKramer/sfhcks/blob/master/img/Process4.png)


### Step 3
when you are done, execute STEP 3 to restore the prior Procces Builder state  

only prior active Processes get switched back ON again
![Process5](https://github.com/HeikoKramer/sfhcks/blob/master/img/Process5.png)

those data storage cases get deleted at the end of STEP 3 
