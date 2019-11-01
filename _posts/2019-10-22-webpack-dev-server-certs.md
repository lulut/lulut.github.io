---
layout:     post
title:      webpack-dev-server ERR_CERT_INVALID on Chrome on macOS 10.15 (Catalina)
subtitle:   
date:       2019-10-22
header-img:
catalog: true
tags:
    - JavaScript
    - Chrome
    - macOS
    - webpack
---

## What
After upgrading my Mac to macOs 10.15 Catalina, I found the self-signed certification of webpack-dev-server cannot work on Chrome.
In the page of displaying error message: ERR_CERT_INVALID, you cannot proceed to the site by clicking Advanced button.  This issue casued me to be unable to debug front-end script in Chrome.  I tried Safari which works well as before.  But since Safari is not as easy as to use as Chrome, I have to find a way to solve this problem. 

## Why
Through the online search, finally I found that after upgrading macOS to 10.15, it has higher requirements for the website certificate which requires an extension of certificate.  You can find details here:
[Requirements for trusted certificates in iOS 13 and macOS 10.15](https://support.apple.com/en-us/HT210176)
Although this doesn't explain why works in Safari but not Chrome.

## How
There will be a patch of webpack-dev-server to resolve this but the expected time is: near future.  The temporary solution is:

1. Locate `node_modules` folder and find `webpack-dev-server`.
2. In lib/utils, find `createCertificate.js`
3. In the parameter of creating certificate, add the following attributes in `extensions` array:
``` JavaScript
      {
        name: 'extKeyUsage',
        serverAuth: true,
        clientAuth: true,
        codeSigning: true,
        timeStamping: true
      },
```
The modification is shown in the figure
![createCertificate](/img/webpack-dev-server-createCertificate.png)
4. Delete `server.pem` file under folder `ssl`
5. Restart your webpack-dev-server will generate a new server certificate

OK then, before the new release of webpack-dev-server, you can keep running your program on Chrome!