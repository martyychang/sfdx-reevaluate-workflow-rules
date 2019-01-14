# SFDX  App

## Dev, Build and Test

### Steps to reproduce the problem

First, create a scratch org and push source to the org.

```bash
sfdx force:org:create -f config/project-scratch-def.json -s
sfdx force:source:push
sfdx force:org:open
```

Next, create an account in the org and a new opportunity in the "Qualification"
stage under the account.

Then, run the query below.

```sql
SELECT Id, Name, RecordType.Name, StageName, ProspectingStageDate__c, QualificationStageDate__c
FROM Opportunity
ORDER BY LastModifiedDate DESC
```

The following table lists expected outcomes vs. actual outcomes.

Expected | Actual | Pass/Fail
----- | ----- | -----
Record Type is "Accelerated Opportunity" | _Same_ | :white_check_mark:
Stage is "Qualification" | _Same_ | :white_check_mark:
Prospecting Date is stamped | Prospecting Date is empty | :x:
Qualification Date is stamped | Qualification Date is empty | :x:

## Resources


## Description of Files and Directories


## Issues


