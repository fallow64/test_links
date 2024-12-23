Jekyll Website Link Checker

This action is used to check links on OSG, CHTC, PATh and HTCondor. 

It uses [gjtorikian/html-proofer](https://github.com/gjtorikian/html-proofer) to do this. To update the image you can 
run the line below, you might need to upgrade the ruby image as well. 

### To build a new image with the latest version of html-proofer. 

```shell
docker build --platform linux/amd64 -t hub.opensciencegrid.org/opensciencegrid/html-proofer:latest .
docker push hub.opensciencegrid.org/opensciencegrid/html-proofer:latest
```

### Proof Locally

```shell
FILE_IGNORE="/./classad/.*/,/./instructions/.*/,/./slides/.*/,/./slides/.*/,/./uw-support/support-agreement.html/,/./doc/.*/,/./wiki-archive/.*/,/./news-archive.html/"
URL_IGNORE=$(curl -s https://raw.githubusercontent.com/CHTC/test_links/refs/heads/master/links-action/ignore_urls.json | jq -r 'map("/" + . + "/") | join(",")' | sed 's/ //g')

docker run -v ${PWD}/_site:/_site hub.opensciencegrid.org/opensciencegrid/html-proofer:latest --ignore-empty-alt --ignore-missing-alt --no-enforce-https --no-check-external-hash --ignore-status-codes 302,401 --ignore-files $FILE_IGNORE --ignore-urls $URL_IGNORE /_site
```
