# SFDX  App

## Dev, Build and Test

Clone this repository to get started. The goal of this app is to explore
whether workflow rules can be used together to stamp opportunity stage
entry dates, based on record type. This simplistic example is expected to
work as follows.

* By default opportunities are created without a record type for all profiles
* A workflow rule sets the record type to "Accelerated Opportunity" for
  any opportunities created without a record type
* For opportunities having the "Accelerated Opportunity" record type,
  when an opportunity enters the Prospecting or Qualification stage,
  the corresponding Prospecting Date or Qualification Date field should be
  stamped with the current time
* If an opportunity is created in the Qualification stage, Prospecting Date
  should be populated to match the Qualification Date
* If an opportunity moves backward in stage from Qualification to Prospecting,
  Qualification Date should be cleared

The **Handle Stage Prospecting** and **Handle Stage Qualification** workflow
rules depend on the record type being set correctly, and the
**Handle Record Type Blank** workflow rule is responsible for setting the
record type to "Accelerated Opportunity".

However, even with the **Re-evaluate Workflow Rules after Field Change**
checkbox marked on the field update on Opportunity Record Type, the
stage date fields are never stamped.

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


