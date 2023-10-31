# AMOS - RESET CREDENTIALS Activity
## AMOS Portal
* Login to AMOS Portal
* Navigate to - CUSTOMER > RESET CREDENTIALS
* Fill the activity form
* Click on SUBMIT
![](D:\data\AppFlowyDataDoNotRename\images\10073358-5880-4122-a800-cec34a42c62f.png)
### On Submit
* AMOS portal will create a JSON 
```json
{
  "object_name": "WF.RESET.CREDS",
  "execution_option": "execute",
  "inputs": {
    "&OP#": "RESET CREDENTIALS",
    "&REQ#": "requester name",
    "&REQMAIL#": "requester email",
    "&CUSTUSER#": "customer username"
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
  "object_name": "WF.RESET.CREDS",
  "execution_option": "execute",
  "inputs": {
    "&OP#": "RESET CREDENTIALS",
    "&REQ#": "requester name",
    "&REQMAIL#": "requester email",
    "&CUSTUSER#": "customer username"
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
INSERT INTO amos_user_activity (user_name, activity_date, activity, in_form_data, ae_wf_id, status) VALUES ('<Requester Name>', current_timestamp, 'RESET CREDENTIALS', '<request JSON>', '<Run ID>', 'OPEN')
```
* If no exception then AMOS portal will display to requester that their request has been submitted.
![](D:\data\AppFlowyDataDoNotRename\images\987ddac5-8812-4ec6-a58a-21ef60dd3316.png)
* If there are exceptions then the failure will be displayed
![](D:\data\AppFlowyDataDoNotRename\images\6b1a47ba-e5b3-4420-a0d5-e7b5be245206.png)
## Automic
* When the workflow is executed - WF.RESET.CREDS
* Store the execution ID in variable - &RUNID#
* Store the execution path in variable &PATH#
* put a wait of 60 seconds
* Start the job on Agent 01 - RH-CA-ST-VM-10 192.168.126.86
* Create a variable &JSON#
```
"{" +
            "\"OP\":\"" + OP + "\"," +
            "\"REQ\":\"" + REQ + "\"," +
            "\"REQMAIL\":\"" + REQMAIL + "\"," +
            "\"CUSTUSER\":\"" + CUSTUSER + "\"" +
            "}"
```
* In the job execute the JAVA jar in path - skyvera\jobs\wf.reset.creds\deploy
> 📌
> java21path\java.exe -jar run.jar &PATH# &JSON# &RUNID#

## JAVA
Folder Structure
```
wf.reset.creds/
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
occ.rest.api.model=https://192.168.126.108/spectrum/restful/models
api.occ.user=paulsn
api.occ.pwd=Pa55w0rd
oca.rest.https.users=https://192.168.126.101:8182/pc/center/webservice/users/userId/
oca.user=admin
oca.pwd=Pa55w0rd
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
```
* Place all imported external jars in the resource folder
* Place the eclipse src folder for the project in the source folder
## Eclipse Project
AE_AMOS_02_RESET-CREDENTIALS
![](D:\data\AppFlowyDataDoNotRename\images\2e93c62c-616d-447b-9c41-88e432383d9f.png)

