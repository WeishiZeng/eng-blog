---
layout: post
title: CORS, XSS and CSRF
date: '2019-11-14T17:49:02.002-07:00'
author: Weishi Zeng
tags: engineering security
---

# Security Related Acronyms in Web Applications

## CORS

For a web application, typical scenario is the following:
1. User makes a http request to a server
2. Server responds with some resources. I.e. html, css and js code running under user's browser.
3. Client side javascript code makes more requests based on user interaction. E.g. user click next button.
4. Client side javascript may also request resources from another domain, e.g. to display ads sent by google. This is a cross origin request.

Cross-Origin Resource Sharing (CORS) is a protocol that HTTP headers and preflight requests to secure the cross origin requests.


![CORS Diagram](https://mdn.mozillademos.org/files/14295/CORS_principle.png "Cors Diagram")
### Reference
https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS


## XSS
Cross-site scripting (XSS) enables attackers to inject client-side scripts into web pages. It's a server side vulnerability. Considering following scenario.
1. Web page has a search box. Server usually echoes back whatever you just searched for.
2. User enter js code in search box. If server doesn't escape it properly, the script will run.

### XSS Examples
Checkout examples [here](http://www.insecurelabs.org/task/Rule1?query=%3Cscript+type%3D%27application%2Fjavascript%27%3Ealert%28%27xss%27%29%3B%3C%2Fscript%3E).

![image](/assets/images/blog/xss-example.png)

### XSS Exploits
1. Attacker crafts a link that injects script to a vulnerable website.
2. Victim click on the link. The injected javascript code is executed with the level of permission of the user.
3. It can read cookies, make requests to the same origin, replace the html element, or send something to attacker's server. [Youtube video](https://youtu.be/t161cahMAZc?t=158) explained it with a little more details.

## CSRF

Cross-site request forgery (CSRF) victims are tricked by attacker to submit a web request they did not intend. However attacker cannot read the response.

### CSRF Exploits
1. Craft a malicious request. The request usually change some states on the server. E.g. change password.
2. The request could be a GET, POST etc. If GET, attacker tricks user to click it. If POST, attacker needs to [embed](https://stackoverflow.com/a/17953761/2797254) it into some page victim may open. This could be a link, a hidden form, a img tag or etc.
3. The request would have the same level of permission as a legit request. E.g. if victim is logged in, the malicious request would have the same cookie attached.

[CSRF token](https://stackoverflow.com/a/49435939/2797254) is used to prevent above attack. Server embed a unique token for every state-changing request per user session. Attacker couldn't guess the token thus cannot send the request on behalf of the user.
