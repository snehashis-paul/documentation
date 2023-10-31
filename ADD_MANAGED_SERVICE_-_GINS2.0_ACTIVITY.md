# AMOS - ADD MANAGED SERVICE - GINS2.0 Activity
## Automic
* When the workflow is executed - WF.ADD.MS.GINS2.0
* Store the execution ID in variable - &RUNID#
* Store the execution path in variable &PATH#
* put a wait of 60 seconds
* start the job on communication test for servers
	* SS2
	* DC01
	* DC02
* Start the job on Agent 03 - RH-CA-VM-32 192.168.126.119
* Create a variable &JSON#
```
{
  \"OP\": \"ADD MANAGED SERVICE - GINS2.0\",
  \"REQ\": \""+&REQ#+"\",
  \"REQMAIL\": \""+&REQMAIL#+"\",
  \"CUST\": \""+&CUST#+"\",
  \"SRVC\": \""+&SRVC#+"\",
  \"DEV\": \""+&DEV#+"\",
  \"CCT\": \""+&CCT#+"\",
  \"BW\": \""+&BW#+"\",
  \"SITE\": \""+&SITE#+"\",
  \"SUBGRP\": \""+&SUBGRP#+"\",
  \"PROACTIVE\": \""+&PROACTIVE#+"\",
 \"NOTIFY\": \""+&NOTIFY#+"\",
  \"IP\": \""+&IP#+"\",
  \"RBS\": \""+&RBS#+"\",
  \"BACKUP\": \""+&BACKUP#+"\",
  \"WANIF\": \""+&WANIF#+"\",
  \"SNMPV\": \""+&SNMPV#+"\",
  \"SNMP\": \""+&SNMP#+"\",
  \"ICMP\": \""+&ICMP#+"\",
  \"OTHERIF\": "+&OTHERIF#+"
}
```
* In the job execute the JAVA jar in path - skyvera\jobs\wf.add.ms.sfbb\deploy
> 📌
> java21path\java.exe -jar run.jar "&PATH#" "&JSON#" "&RUNID#"

SAMPLE &JSON#
```
{
  \"OP\": \"ADD MANAGED SERVICE - GINS2.0\",
  \"REQ\": \"requester name\",
  \"REQMAIL\": \"requester email\",
  \"CUST\": \"customer name\",
  \"SRVC\": \"SD-WAN\",
  \"DEV\": \"device type\",
  \"CCT\": \"circuit ID\",
  \"BW\": \"bandwidth\",
  \"SITE\": \"site name\",
  \"SUBGRP\": \"sub group name\",
  \"PROACTIVE\": \"proactive monitoring\",
 \"NOTIFY\": \"alert notifications\",
  \"IP\": \"device IP\",
  \"RBS\": \"RBS ID\",
  \"BACKUP\": \"NA\",
  \"WANIF\": \"WAN Interface ifName4\",
  \"SNMPV\": \"SNMP Version\",
  \"SNMP\": \"SNMP String\",
  \"ICMP\": \"ICMP Disabled\",
  \"OTHERIF\": [{\"IFNAME\":\"ifName1\",\"CCT\":\"associated CCT ID\",\"TYPE\":\"port type\",\"BW\":\"bandwidth\"},{\"IFNAME\":\"ifName2\",\"CCT\":\"associated CCT ID\",\"TYPE\":\"port type\",\"BW\":\"bandwidth\"},{\"IFNAME\":\"ifName3\",\"CCT\":\"associated CCT ID\",\"TYPE\":\"port type\",\"BW\":\"bandwidth\"}]
}
```
## JAVA
Folder Structure
```
wf.add.ms.gins2/
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
api.da.device.attr=http://192.168.126.100:8581/rest/devices/customattributes/
api.da.devices=http://192.168.126.100:8581/rest/devices/
api.da.discovery=http://192.168.126.100:8581/rest/tenant/1/discoveryprofiles
api.da.instance=http://192.168.126.100:8581/rest/discoveryinstances/
api.da.port=http://192.168.126.100:8581/rest/ports/
api.da.port.attr=http://192.168.126.100:8581/rest/ports/customattributes/
api.da.port.filter=http://192.168.126.100:8581/rest/ports/filtered
api.da.port.poll=http://192.168.126.100:8581/rest/pollable/
api.da.snmp=http://192.168.126.100:8581/rest/profiles/snmpv1/filtered
api.occ.action=https://192.168.126.108/spectrum/restful/action/0x1000e?mh=
api.occ.association=https://192.168.126.108/spectrum/restful/associations/relation/0x10002/model/
api.occ.model=https://192.168.126.108/spectrum/restful/model
api.pc.addgrp=https://192.168.126.101:8182/pc/center/webservice/groups/groupPath
api.pc.devices=https://192.168.126.101:8182/pc/center/webservice/devices/deviceItemId
api.pc.groups=https://192.168.126.101:8182/pc/center/webservice/groups/false/true
api.pc.snmp=https://192.168.126.101:8182/pc/center/webservice/profiles/saveProfile/true
api.soap.sd=http://192.168.126.114:8080/axis/services/USD_R11_WebService
sd.user=servicedesk
sd.pwd=sdAdmin@#123
sd.soap.create=http://www.ca.com/UnicenterServicePlus/ServiceDesk/USD_WebServiceSoap/createObjectRequest
sd.soap.doselect=http://www.ca.com/UnicenterServicePlus/ServiceDesk/USD_WebServiceSoap/doSelectRequest
sd.soap.login=http://www.ca.com/UnicenterServicePlus/ServiceDesk/USD_WebServiceSoap/loginRequest
sd.soap.logout=http://www.ca.com/UnicenterServicePlus/ServiceDesk/USD_WebServiceSoap/logoutRequest
oca.user=admin
oca.pwd=Pa55w0rd
occ.user=paulsn
occ.pwd=Pa55w0rd
email.signature=MT Automation
email.sender=automation@telecom.mu
email.smtp=email.smtp
email.port=email.port
email.support=email.support
db.driver=db.driver
db.link=db.link
db.user=db.user
db.pwd=db.pwd
```
* Place all imported external jars in the resource folder
* Place the eclipse src folder for the project in the source folder
## Eclipse Project
AE_AMOS_06_ADD-MANAGED-SERVICE-GINS2
![](D:\data\AppFlowyDataDoNotRename\images\346a9092-41fb-40c4-a23c-c55e516974ac.png)

