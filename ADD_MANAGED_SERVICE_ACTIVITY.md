# AMOS - ADD MANAGED SERVICE Activity
## AMOS Portal
* Login to AMOS Portal
* Navigate to - DISCOVERY > ADD MANAGED SERVICE
* Fill the activity form
* Click on SUBMIT
![](D:\data\AppFlowyDataDoNotRename\images\c37f4fd9-e323-416f-9cd6-01dd5b36ba81.png)
### On Submit
* If selected service type in the form is SFBB then do - SFBB Actions
* If selected service type in the form is GINS 2.0 then do - GINS2 Actions
* Else, for all other service type do - GENERAL Actions
### On Submit - GINS2 Actions
* AMOS portal will create a string for the otherif entries as follows
	* Assuming that there are 3 ports entries in the otherif table
```
\"[{\\\"IFNAME\\\":\\\"ifName1\\\",\\\"CCT\\\":\\\"associated CCT ID\\\",\\\"TYPE\\\":\\\"port type\\\",\\\"BW\\\":\\\"bandwidth\\\"},{\\\"IFNAME\\\":\\\"ifName2\\\",\\\"CCT\\\":\\\"associated CCT ID\\\",\\\"TYPE\\\":\\\"port type\\\",\\\"BW\\\":\\\"bandwidth\\\"},{\\\"IFNAME\\\":\\\"ifName3\\\",\\\"CCT\\\":\\\"associated CCT ID\\\",\\\"TYPE\\\":\\\"port type\\\",\\\"BW\\\":\\\"bandwidth\\\"}]\"
```
* AMOS portal will create a JSON 
```json
{
  "object_name": "WF.ADD.MS.GINS2",
  "execution_option": "execute",
  "inputs": {
    "&OP#": "ADD MANAGED SERVICE - GINS2.0",
    "&REQ#": "requester name",
    "&REQMAIL#": "requester email",
    "&CUST#": "customer name",
    "&SRVC#": "SD-WAN",
    "&DEV#": "device type",
    "&CCT#": "circuit ID",
    "&BW#": "bandwidth",
    "&SITE#": "site name",
    "&SUBGRP#": "sub group name",
    "&PROACTIVE#": "proactive monitoring",
    "&NOTIFY#": "alert notifications",
    "&IP#": "device IP",
    "&RBS#": "RBS ID",
    "&BACKUP#": "NA",
    "&WANIF#": "WAN Interface ifName4",
    "&SNMPV#": "SNMP Version",
    "&SNMP#": "SNMP String",
    "&ICMP#": "ICMP Disabled",
    "&OTHERIF#":  "\"[{\\\"IFNAME\\\":\\\"ifName1\\\",\\\"CCT\\\":\\\"associated CCT ID\\\",\\\"TYPE\\\":\\\"port type\\\",\\\"BW\\\":\\\"bandwidth\\\"},{\\\"IFNAME\\\":\\\"ifName2\\\",\\\"CCT\\\":\\\"associated CCT ID\\\",\\\"TYPE\\\":\\\"port type\\\",\\\"BW\\\":\\\"bandwidth\\\"},{\\\"IFNAME\\\":\\\"ifName3\\\",\\\"CCT\\\":\\\"associated CCT ID\\\",\\\"TYPE\\\":\\\"port type\\\",\\\"BW\\\":\\\"bandwidth\\\"}]\""
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
  "object_name": "WF.ADD.MS.GINS2",
  "execution_option": "execute",
  "inputs": {
    "&OP#": "ADD MANAGED SERVICE - GINS2.0",
    "&REQ#": "requester name",
    "&REQMAIL#": "requester email",
    "&CUST#": "customer name",
    "&SRVC#": "SD-WAN",
    "&DEV#": "device type",
    "&CCT#": "circuit ID",
    "&BW#": "bandwidth",
    "&SITE#": "site name",
    "&SUBGRP#": "sub group name",
    "&PROACTIVE#": "proactive monitoring",
    "&NOTIFY#": "alert notifications",
    "&IP#": "device IP",
    "&RBS#": "RBS ID",
    "&BACKUP#": "NA",
    "&WANIF#": "WAN Interface ifName4",
    "&SNMPV#": "SNMP Version",
    "&SNMP#": "SNMP String",
    "&ICMP#": "ICMP Disabled",
    "&OTHERIF#":  "\"[{\\\"IFNAME\\\":\\\"ifName1\\\",\\\"CCT\\\":\\\"associated CCT ID\\\",\\\"TYPE\\\":\\\"port type\\\",\\\"BW\\\":\\\"bandwidth\\\"},{\\\"IFNAME\\\":\\\"ifName2\\\",\\\"CCT\\\":\\\"associated CCT ID\\\",\\\"TYPE\\\":\\\"port type\\\",\\\"BW\\\":\\\"bandwidth\\\"},{\\\"IFNAME\\\":\\\"ifName3\\\",\\\"CCT\\\":\\\"associated CCT ID\\\",\\\"TYPE\\\":\\\"port type\\\",\\\"BW\\\":\\\"bandwidth\\\"}]\""
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
INSERT INTO amos_user_activity (user_name, activity_date, activity, in_form_data, ae_wf_id, status) VALUES ('<Requester Name>', current_timestamp, 'ADD MANAGED SERVICE - GINS2.0', '<request JSON>', '<Run ID>', 'OPEN')
```
* If no exception then AMOS portal will display to requester that their request has been submitted.
![](D:\data\AppFlowyDataDoNotRename\images\d11fec94-de86-45fd-9512-1d50538c58c6.png)
* If there are exceptions then the failure will be displayed
![](D:\data\AppFlowyDataDoNotRename\images\29cfe5b8-f1f8-4f63-9c33-724ece3e70c4.png)
### On Submit - GENERAL Actions
* AMOS portal will create a string for the otherif entries as follows
	* Assuming that there are 3 ports entries in the otherif table
```
\"[{\\\"IFNAME\\\":\\\"ifName1\\\",\\\"CCT\\\":\\\"associated CCT ID\\\",\\\"TYPE\\\":\\\"port type\\\",\\\"BW\\\":\\\"bandwidth\\\"},{\\\"IFNAME\\\":\\\"ifName2\\\",\\\"CCT\\\":\\\"associated CCT ID\\\",\\\"TYPE\\\":\\\"port type\\\",\\\"BW\\\":\\\"bandwidth\\\"},{\\\"IFNAME\\\":\\\"ifName3\\\",\\\"CCT\\\":\\\"associated CCT ID\\\",\\\"TYPE\\\":\\\"port type\\\",\\\"BW\\\":\\\"bandwidth\\\"}]\"
```
* AMOS portal will create a JSON 
```json
{
  "object_name": "WF.ADD.MS.GEN",
  "execution_option": "execute",
  "inputs": {
    "&OP#": "ADD MANAGED SERVICE - GENERAL",
    "&REQ#": "requester name",
    "&REQMAIL#": "requester email",
    "&CUST#": "customer name",
    "&SRVC#": "service type",
    "&DEV#": "device type",
    "&CCT#": "circuit ID",
    "&BW#": "bandwidth",
    "&SITE#": "site name",
    "&SUBGRP#": "sub group name",
    "&PROACTIVE#": "proactive monitoring",
    "&NOTIFY#": "alert notifications",
    "&IP#": "device IP",
    "&RBS#": "RBS ID",
    "&BACKUP#": "NA",
    "&WANIF#": "WAN Interface ifName4",
    "&SNMPV#": "SNMP Version",
    "&SNMP#": "SNMP String",
    "&ICMP#": "ICMP Disabled",
    "&OTHERIF#":  "\"[{\\\"IFNAME\\\":\\\"ifName1\\\",\\\"CCT\\\":\\\"associated CCT ID\\\",\\\"TYPE\\\":\\\"port type\\\",\\\"BW\\\":\\\"bandwidth\\\"},{\\\"IFNAME\\\":\\\"ifName2\\\",\\\"CCT\\\":\\\"associated CCT ID\\\",\\\"TYPE\\\":\\\"port type\\\",\\\"BW\\\":\\\"bandwidth\\\"},{\\\"IFNAME\\\":\\\"ifName3\\\",\\\"CCT\\\":\\\"associated CCT ID\\\",\\\"TYPE\\\":\\\"port type\\\",\\\"BW\\\":\\\"bandwidth\\\"}]\""
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
  "object_name": "WF.ADD.MS.GEN",
  "execution_option": "execute",
  "inputs": {
    "&OP#": "ADD MANAGED SERVICE - GENERAL",
    "&REQ#": "requester name",
    "&REQMAIL#": "requester email",
    "&CUST#": "customer name",
    "&SRVC#": "service type",
    "&DEV#": "device type",
    "&CCT#": "circuit ID",
    "&BW#": "bandwidth",
    "&SITE#": "site name",
    "&SUBGRP#": "sub group name",
    "&PROACTIVE#": "proactive monitoring",
    "&NOTIFY#": "alert notifications",
    "&IP#": "device IP",
    "&RBS#": "RBS ID",
    "&BACKUP#": "NA",
    "&WANIF#": "WAN Interface ifName4",
    "&SNMPV#": "SNMP Version",
    "&SNMP#": "SNMP String",
    "&ICMP#": "ICMP Disabled",
    "&OTHERIF#":   "\"[{\\\"IFNAME\\\":\\\"ifName1\\\",\\\"CCT\\\":\\\"associated CCT ID\\\",\\\"TYPE\\\":\\\"port type\\\",\\\"BW\\\":\\\"bandwidth\\\"},{\\\"IFNAME\\\":\\\"ifName2\\\",\\\"CCT\\\":\\\"associated CCT ID\\\",\\\"TYPE\\\":\\\"port type\\\",\\\"BW\\\":\\\"bandwidth\\\"},{\\\"IFNAME\\\":\\\"ifName3\\\",\\\"CCT\\\":\\\"associated CCT ID\\\",\\\"TYPE\\\":\\\"port type\\\",\\\"BW\\\":\\\"bandwidth\\\"}]\""
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
INSERT INTO amos_user_activity (user_name, activity_date, activity, in_form_data, ae_wf_id, status) VALUES ('<Requester Name>', current_timestamp, 'ADD MANAGED SERVICE - GENERAL', '<request JSON>', '<Run ID>', 'OPEN')
```
* If no exception then AMOS portal will display to requester that their request has been submitted.
![](D:\data\AppFlowyDataDoNotRename\images\d11fec94-de86-45fd-9512-1d50538c58c6.png)
* If there are exceptions then the failure will be displayed
![](D:\data\AppFlowyDataDoNotRename\images\29cfe5b8-f1f8-4f63-9c33-724ece3e70c4.png)
### On Submit - SFBB Actions
* AMOS portal will create a string for the otherif entries as follows
	* Assuming that there are 3 ports entries in the otherif table
```
\"[{\\\"IFNAME\\\":\\\"ifName1\\\",\\\"CCT\\\":\\\"associated CCT ID\\\",\\\"TYPE\\\":\\\"port type\\\",\\\"BW\\\":\\\"bandwidth\\\"},{\\\"IFNAME\\\":\\\"ifName2\\\",\\\"CCT\\\":\\\"associated CCT ID\\\",\\\"TYPE\\\":\\\"port type\\\",\\\"BW\\\":\\\"bandwidth\\\"},{\\\"IFNAME\\\":\\\"ifName3\\\",\\\"CCT\\\":\\\"associated CCT ID\\\",\\\"TYPE\\\":\\\"port type\\\",\\\"BW\\\":\\\"bandwidth\\\"}]\"
```
* AMOS portal will create a JSON 
```json
{
  "object_name": "WF.ADD.MS.SFBB",
  "execution_option": "execute",
  "inputs": {
    "&OP#": "ADD MANAGED SERVICE - SFBB",
    "&REQ#": "requester name",
    "&REQMAIL#": "requester email",
    "&CUST#": "customer name",
    "&SRVC#": "SFBB",
    "&DEV#": "device type",
    "&CCT#": "circuit ID",
    "&BW#": "bandwidth",
    "&SITE#": "site name",
    "&SUBGRP#": "sub group name",
    "&PROACTIVE#": "proactive monitoring",
    "&NOTIFY#": "alert notifications",
    "&IP#": "device IP",
    "&RBS#": "RBS ID",
    "&BACKUP#": "NA",
    "&WANIF#": "WAN Interface ifName4",
    "&SNMPV#": "SNMP Version",
    "&SNMP#": "SNMP String",
    "&ICMP#": "ICMP Disabled",
    "&OTHERIF#":   "\"[{\\\"IFNAME\\\":\\\"ifName1\\\",\\\"CCT\\\":\\\"associated CCT ID\\\",\\\"TYPE\\\":\\\"port type\\\",\\\"BW\\\":\\\"bandwidth\\\"},{\\\"IFNAME\\\":\\\"ifName2\\\",\\\"CCT\\\":\\\"associated CCT ID\\\",\\\"TYPE\\\":\\\"port type\\\",\\\"BW\\\":\\\"bandwidth\\\"},{\\\"IFNAME\\\":\\\"ifName3\\\",\\\"CCT\\\":\\\"associated CCT ID\\\",\\\"TYPE\\\":\\\"port type\\\",\\\"BW\\\":\\\"bandwidth\\\"}]\""
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
  "object_name": "WF.ADD.MS.SFBB",
  "execution_option": "execute",
  "inputs": {
    "&OP#": "ADD MANAGED SERVICE - SFBB",
    "&REQ#": "requester name",
    "&REQMAIL#": "requester email",
    "&CUST#": "customer name",
    "&SRVC#": "SFBB",
    "&DEV#": "device type",
    "&CCT#": "circuit ID",
    "&BW#": "bandwidth",
    "&SITE#": "site name",
    "&SUBGRP#": "sub group name",
    "&PROACTIVE#": "proactive monitoring",
    "&NOTIFY#": "alert notifications",
    "&IP#": "device IP",
    "&RBS#": "RBS ID",
    "&BACKUP#": "NA",
    "&WANIF#": "WAN Interface ifName4",
    "&SNMPV#": "SNMP Version",
    "&SNMP#": "SNMP String",
    "&ICMP#": "ICMP Disabled",
    "&OTHERIF#": "\"[{\\\"IFNAME\\\":\\\"ifName1\\\",\\\"CCT\\\":\\\"associated CCT ID\\\",\\\"TYPE\\\":\\\"port type\\\",\\\"BW\\\":\\\"bandwidth\\\"},{\\\"IFNAME\\\":\\\"ifName2\\\",\\\"CCT\\\":\\\"associated CCT ID\\\",\\\"TYPE\\\":\\\"port type\\\",\\\"BW\\\":\\\"bandwidth\\\"},{\\\"IFNAME\\\":\\\"ifName3\\\",\\\"CCT\\\":\\\"associated CCT ID\\\",\\\"TYPE\\\":\\\"port type\\\",\\\"BW\\\":\\\"bandwidth\\\"}]\""
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
INSERT INTO amos_user_activity (user_name, activity_date, activity, in_form_data, ae_wf_id, status) VALUES ('<Requester Name>', current_timestamp, 'ADD MANAGED SERVICE - SFBB', '<request JSON>', '<Run ID>', 'OPEN')
```
* If no exception then AMOS portal will display to requester that their request has been submitted.
![](D:\data\AppFlowyDataDoNotRename\images\d11fec94-de86-45fd-9512-1d50538c58c6.png)
* If there are exceptions then the failure will be displayed
![](D:\data\AppFlowyDataDoNotRename\images\29cfe5b8-f1f8-4f63-9c33-724ece3e70c4.png)

