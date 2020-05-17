# AWS

## aws logs get assume-role usage
```
# get username and role assumed, output result result (queryId in json) to assumerole-query.json 
# get epoch time from https://www.epochconverter.com/
aws-vault exec privileged-admin -- aws logs start-query --start-time 1588291200 --end-time 1589748390 --log-group-name cloudtrail-cloudwatch-logs --query-string 'fields userIdentity.userName, resources.0.ARN | filter eventName = "AssumeRole" | filter resources.0.ARN  like "arn:aws:iam::542640492856:role/team" ' | tee assumerole-query.json

#{
#    "queryId": "040e3285-6366-431e-9c5f-ee8e22a3eb9a"
#}


# get the query result
cat assumerole-query.json | jq '.queryId' | xargs -I {} aws-vault exec privileged-admin -- aws logs get-query-results --query-id {} | tee assumerole-output.json
{
    "results": [],
    "statistics": {
        "recordsMatched": 0.0,
        "recordsScanned": 32574940.0,
        "bytesScanned": 46341262983.0
    },
    "status": "Running"
}

# to get a record details pointed to by @ptr
aws-vault exec privileged-admin -- aws logs get-log-record --log-record-pointer "CmgKKwonNTQyNjQwNDkyODU2OmNsb3VkdHJhaWwtY2xvdWR3YXRjaC1sb2dzEAYSORoYAgXmCKMIAAAAAgmFLHcABed8NPAAAAKCIAEolObMnpAuMPe01J6QLjjuCECo72NIq8snUOCWJxCwAhgB"

# on complete run
cat assumerole-output.json | jq '.results[]' | jq '.[0].value + " " + .[1].value ' | sort | uniq

```
