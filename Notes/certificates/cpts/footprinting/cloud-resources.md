---
description: (Infrastructure Based Enumeration)
---

# Cloud Resources

Even though cloud providers secure their infrastructure centrally, this does not mean that companies are free from vulnerabilities. The configurations made by the administrators may nevertheless make the company's cloud resources vulnerable. This often starts with the `S3 buckets` (AWS), `blobs` (Azure), `cloud storage` (GCP), which can be accessed without authentication if configured incorrectly.

## Cloud Resources

```bash
for i in $(cat subdomainlist);do host $i | grep "has address" | grep inlanefreight.com | cut -d" " -f1,4;done
```

Often cloud storage is added to the DNS list when used for administrative purposes by other employees. This step makes it much easier for the employees to reach and manage them.



## Find cloud storage on google

```
intext:{term} inurl:{amazonaws.com or blob.core.windows.net or }
```

{% hint style="warning" %}
This info can also be found sometimes in source code.
{% endhint %}



Third-party providers such as [domain.glass](https://domain.glass/) and [GrayHatWarfare](https://buckets.grayhatwarfare.com/) can also tell us a lot about the company's infrastructure.
