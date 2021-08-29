# Bug Bounty Tools

![alt text](https://pbs.twimg.com/profile_images/1384538705797554177/o0BURm0O_400x400.png)

Here are some of the tools that we use when we perform Live Recon Passive ONLY on Twitch:

1) Recon-ng
https://github.com/lanmaster53/recon-ng
2) httpx
https://github.com/projectdiscovery/httpx
3) isup.sh
https://github.com/gitnepal/isup
4) Arjun
https://github.com/s0md3v/Arjun
5) jSQL
https://github.com/ron190/jsql-injection
6) Smuggler
https://github.com/defparam/smuggler
7) Sn1per
https://github.com/1N3/Sn1per
8) Spiderfoot
https://github.com/smicallef/spiderfoot
9) Nuclei
https://github.com/projectdiscovery/nuclei
10) Jaeles
https://github.com/jaeles-project/jaeles
11) ChopChop
https://github.com/michelin/ChopChop
12) Inception
https://github.com/proabiral/inception
13) Eyewitness
https://github.com/FortyNorthSecurity/EyeWitness
14) Meg
https://github.com/tomnomnom/meg
15) Gau - Get All Urls
https://github.com/lc/gau
16) Snallygaster
https://github.com/hannob/snallygaster
17) NMAP
https://github.com/nmap/nmap
18) Waybackurls
https://github.com/tomnomnom/waybackurls
19) Gotty
https://github.com/yudai/gotty
20) GF
https://github.com/tomnomnom/gf
21) GF Patterns
https://github.com/1ndianl33t/Gf-Patterns
22) Paramspider
https://github.com/devanshbatham/ParamSpider
23) XSSER
https://github.com/epsylon/xsser
24) UPDOG
https://github.com/sc0tfree/updog
25) JSScanner
https://github.com/dark-warlord14/JSScanner
26) Takeover
https://github.com/m4ll0k/takeover
27) Keyhacks
https://github.com/streaak/keyhacks

Bounty Platform used:

Hexway

https://hexway.io/hive

We respect the privacy of clients we are working on Hackerone.com & use only passive techniques, we do not share anything related to security misconfigurations, and everything is taken from passive resources, including the techniques performed. The purpose is for Educational only!

We will update the list everytime we add/remove tools.

Some of the Methodologies we use during our Stream, since we had many requests to post it, here you go:

As seen on Hackerone.com Passive Bounty Focused for Quick Pwning:

Project Notes & Recon Approach Techniques:

Main Domain:

XXXX Enter from Scope XXXX



Secondary *.* Domains:

XXXX Enter from Scope XXXX



Single Sub-domains:

XXXX Enter from Scope XXXX

Passive Recon Techniques:

1. Create Folders (Subdomains, URLS, IPs)

2. Recon-ng - Recon Passively for subdomains/ips/ports/params/js

3. Export lists from recon-ng and use httpx to create urls/probing (urls/IPs/Subdomains)

4. Use isup.sh to filter ips 

**UPLOAD ALL RESULTS INTO PLATFORM**


Use updog to offer easier workflow when uploading/checking directories locally.

For example when using a raspberry pi, or VPS it helps uploading files locally on the machine.

********************************************************************************************************************

5. Use Nmap Aggressive Scan & Save to XML to Import into Bounty Platform:

nmap -iL ips.txt -sSV -A -T4 -O -Pn -v -F -oX nmap2.xml

**UPLOAD ALL RESULTS INTO PLATFORM**

OSINT: (Can be done on RPI)

Check for Domain TakeOver with Takeover by M4llok 

Takeover Tool: New!

takeover -l sub_domains.txt -v -t 10


**Check for open Amazon S3 buckets, Digital Ocean & Azure** 

spiderfoot -m sfp_azureblobstorage,sfp_s3bucket.sfp_digitaloceanspace -s DOMAIN.com -q


6. Use ParamSpider to Hunt for URLS with Parameters automatically from wayback machine - You can also use Arjun, we are switching to ParamSpider as part of building a workflow New!

python3 paramspider.py --domain DOMAINNAME.com --exclude woff,png,svg,php,jpg --output /root/Desktop/Bounty/params.txt


Technique to Clean Params from XSS:

sed 's/unix/linux/g' reconfile.txt

 
7. Use Smuggler on URLs list to test for http requests that could desync, and posting multiple chunked requests to smuggle external sources so the backend server will forward the request with cookies, data to the front end server
   
(Can be done on RPI)

cat list_of_urls.txt | python3 smuggler.py -l /root/location.txt


**Bonus**

Tip 1: Add ‘tee’ to see realtime New!

Gau - for realtime URL extraction when performing manual search so you can have urls to attack.

Hunt for Links that have Parameters by using gau (Get all URLS) and displaying all links that have params: 

cat subdomains.txt | gau | tee /root/Desktop/urls.txt | lolcat

gau domains -o urls.txt
gau example.com
gau -o example-urls.txt example.com
gau -b png,jpg,gif example.com


Pattern Check Example for Results with gf & gf-patterns: New!

After you have the Parameters Gathered, we want to check for specific patterns and possible vulnerable URLs that can be attacked using Meg or other Fuzzing Tools.

cat /root/Desktop/Bounty/params.txt | gf xss | sed 's/FUZZ/ /g' >> /root/Desktop/Bounty/xss_params_forMeg.txt

Very Powerful One Liner - You can Pipe also directly to Meg.


8. Use Meg with Seclist fuzzing for Links: (Gathered from gau/arjun/paramspider/gf)

For Meg, we must remove the ‘FUZZ’ from paramspider and replace it with a null character:

sed 's/FUZZ//g' reconfile.txt

meg -v LFI-gracefulsecurity-linux.txt /root/Desktop/Bounty/urls.txt /root/Desktop/urls.txt -s 200

9. JSScanner: New!

Scanning Javascript Files for Endpoints, Secrets, Hardcoded credentials,IDOR, Openredirect and more

Paste URLS into alive.txt

Run script alive.txt - Examine the results using GF advanced patterns

Use tree command, cat into subdirectories:

cat * */*.txt
cat */*.js | gf api-keys
	

cat /*/*.txt | gf ssrf > /root/Desktop/ssrf.txt
 
10. Find XSS Vulnerabilities from Paramspider & xsser

Since we have params urls from paramspider, XSSER needs to know where to inject, and is defined with XSS instead of FUZZ, so here is a command to replace this from the result, and create a new list to be used on xsser:

sed 's/FUZZ/XSS/g' reconfile.txt

You are now ready for:

xsser -i params_xss.txt

11- After Recon: New!

When you find Keys/Tokens - Check from here:

https://github.com/streaak/keyhacks

********************************************************************************************************************

OSINT & Passive Amplified Attacks: (Raspberry Pi)

OSINT:

Perform OSINT using spiderfoot

One off 1337 Powerful Command Attacks with amass:

1. Nuclei:

amass enum -passive -d [subdomain] -v | httpx -verbose | nuclei -t /root/nuclei-templates/cves/ -o /root/Desktop/Bounty/location.txt

2. Jaeles:

amass enum -passive -d [Domain] -v | httpx -verbose | jaeles scan -s 'cves' -s 'sensitive' -s 'fuzz' -s ‘common' -s 'routines' report -o /root/Desktop/Bounty/reportname.txt --title "[Client] Jaeles Full Report"

3. Use Eyewitness to take screenshots from URLs

eyewitness -f /root/Desktop/Bounty/Client/urls.txt

More Tools:
chopchop / inception / jsql

./gochopchop scan --url-file /root/Desktop/Bounty/urls.txt --threads 4


Sn1per - Bounty Mode on Active Results

RPI Copy:
scp -P 7 /root/Desktop/test.txt root@192.168.0.12:/root

use Gotty 
https://github.com/yudai/gotty

gotty -p 1337 -w recon-ng 



You can watch us live on Twitch:

https://www.twitch.tv/bheh1337

**blackhatethicalhacking.com // 2021 // All Rights Reserved**
