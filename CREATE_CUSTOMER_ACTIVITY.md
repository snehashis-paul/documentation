# AMOS - CREATE CUSTOMER Activity
## AMOS Portal
* Login to AMOS Portal
* Navigate to - CUSTOMER > CREATE CUSTOMER
* Fill the activity form
* Click on SUBMIT
![](D:\data\AppFlowyDataDoNotRename\images\6a4de19e-7276-40b8-8d55-b38b81b60bb7.png)
### On Submit
* AMOS portal will create a JSON 
```json
{
  "object_name": "WF.CREATE.CUST",
  "execution_option": "execute",
  "inputs": {
    "&OP#": "CREATE CUSTOMER",
    "&REQ#": "requester name",
    "&REQMAIL#": "requester email",
    "&CUST#": "customer name",
    "&ACCESS#": "Give Customer Access to Tools YES/NO",
    "&SECSTR#": "customer security string",
    "&EMAIL#": "customer email id",
    "&BRN#": "curtomer brn"
  }
}
```
 AMOS Portal will make HTTP REST API call to Automic to execute the workflow using above created JSON as request body.

> 📌
> URL: http://192.168.126.86:8088/ae/api/v1/100/executions

|**Credentials**|User - UC/AUTOMIC , Pwd - 1234|
|-|-|
|**Method**|REST-POST|
|**Request Body**|```json
{
  "object_name": "WF.CREATE.CUST",
  "execution_option": "execute",
  "inputs": {
    "&OP#": "CREATE CUSTOMER",
    "&REQ#": "requester name",
    "&REQMAIL#": "requester email",
    "&CUST#": "customer name",
    "&ACCESS#": "Give Customer Access to Tools YES/NO",
    "&SECSTR#": "customer security string",
    "&EMAIL#": "customer email id",
    "&BRN#": "curtomer brn"
  }
}
```|
|**Response**|```json
{
  "run_id": 1016045
}
```|
* The run_id is the automic workflow execution ID. AMOS portal to extract this value and use it as a Request ID to display to requester.
* After the API call, if it is successful and no exceptions then,
* AMOS Portal will connect to PGSQL DB and make an entry to table - amos_user_activity
```sql
INSERT INTO amos_user_activity (user_name, activity_date, activity, in_form_data, ae_wf_id, status) VALUES ('<Requester Name>', current_timestamp, 'CREATE CUSTOMER', '<request JSON>', '<Run ID>', 'OPEN')
```
* If no exception then AMOS portal will display to requester that their request has been submitted.
![](D:\data\AppFlowyDataDoNotRename\images\7e8779b8-cac4-4572-8be1-131cafb972f2.png)
* If there are exceptions then the failure will be displayed
![](D:\data\AppFlowyDataDoNotRename\images\490ae765-04de-4306-95c8-9acf0cfafd0c.png)

## Automic
* When the workflow is executed - WF.CREATE.CUST
* Store the execution ID in variable - &RUNID#
* Store the execution path in variable &PATH#
* put a wait of 60 seconds
* Start the job on Agent 01 - RH-CA-ST-VM-10 192.168.126.86
* Create a variable &JSON#
```
 "{" +
            "\"OP\":\"" + &OP# + "\"," +
            "\"REQ\":\"" + &REQ# + "\"," +
            "\"REQMAIL\":\"" + &REQMAIL# + "\"," +
            "\"CUST\":\"" + &CUST# + "\"," +
            "\"ACCESS\":\"" + &ACCESS# + "\"," +
            "\"SECSTR\":\"" + &SECSTR# + "\"," +
            "\"EMAIL\":\"" + &EMAIL# + "\"," +
            "\"BRN\":\"" + &BRN# + "\"" +
            "}"
```
* In the job execute the JAVA jar in path - skyvera\jobs\wf.create.cust\deploy
> 📌
> java21path\java.exe -jar run.jar &PATH# &JSON# &RUNID#

## JAVA
Folder Structure
```
wf.create.cust/
├─ deploy/
│  ├─ run.jar
├─ config/
│  ├─ cfg_support.properties
├─ log/
│  ├─ dd_MM_yyyy_HH_mm_ss_&RUNID#.log
├─ resource/
│  ├─ xyz.jar
├─ source/
│  ├─ xyz.java
```
Export the eclipse project as runnable jar file called - run.jar
* Place the run.jar file within the deploy folder
* Within the config folder create the properties file - cfg_support.properties
```
email.signature=MT Automation
email.sender=automic@telecom.mu
email.smtp=smtp.telecom.mu
email.port=25
email.support=mt.broadcom.support@skyvera.com
db.driver=org.postgresql.Driver
#db.link=jdbc:postgresql://192.168.126.119:5432/broadcom_ae
db.link=jdbc:postgresql://192.168.126.119:5432/broadcom_ae_test
db.user=skyvera
db.pwd=Pa55w0rd
occ.rest.api.models=https://192.168.126.108/spectrum/restful/models
occ.msnodepath.mls=0x100004:0x1c3c2e
occ.rest.api.model=https://192.168.126.108/spectrum/restful/model
occ.msnode.mls=0x1c3c2e
occ.rest.api.association=https://192.168.126.108/spectrum/restful/associations/relation/0x10031/leftmodel/0x100017/rightmodel/
api.occ.user=paulsn
api.occ.pwd=Pa55w0rd
#oca.rest.http.groups=http://192.168.126.101:8181/pc/center/webservice/groups/false/true
oca.rest.https.groups=https://192.168.126.101:8182/pc/center/webservice/groups/false/true
#oca.rest.http.usersearch=http://192.168.126.101:8181/pc/center/webservice/users/userName/
oca.rest.https.usersearch=https://192.168.126.101:8182/pc/center/webservice/users/userName/
#oca.rest.http.usercreate=http://192.168.126.101:8181/pc/center/webservice/users/role/roleName/1-Customer
oca.rest.https.usercreate=https://192.168.126.101:8182/pc/center/webservice/users/role/roleName/1-Customer
#oca.rest.http.users=http://192.168.126.101:8181/pc/center/webservice/users/userId/
oca.rest.https.users=https://192.168.126.101:8182/pc/center/webservice/users/userId/
oca.user=admin 
oca.pwd=Pa55w0rd
```
* Place all imported external jars in the resource folder
* Place the eclipse src folder for the project in the source folder
## Eclipse Project
AE_AMOS_01_CREATE-CUSTOMER
![](D:\data\AppFlowyDataDoNotRename\images\8af6ffd2-a586-4398-9539-2df14ce94aaa.png)

