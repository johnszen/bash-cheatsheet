# AWS

## aws cli

### using profile
Set access key and secret in ~/.aws/credentials
```
...
[tiing]
aws_access_key_id = <access key>
aws_secret_access_key = <secret>
...
```

Run command by specifying the profile
```
aws --profile tiing --region ap-southeast-2 s3 ls
```

### using aws-vault

Set-up
```
#aws-vault add <profile-name>
aws-vault add tiing-assume
# prompt aws access key and secret access key as above
# this is stored in Keychains
# to view Keychains: Finder/Applications/Utilities/Keychain Access.app
```

in ~/.aws/config
```
...
[profile tiing-assume]
mfa_serial = arn:aws:iam::5678123456781234:mfa/john.zen

[profile tiing-admin]
output = json
region = us-east-1
mfa_serial = arn:aws:iam::5678123456781234:mfa/john.zen
source_profile = tiing-assume
role_arn = arn:aws:iam::5678123456781234:role/tiing-admin
role_session_name = john.zen
...
```

```
aws-vault exec <role to assume> -- <aws command>
```

## docker login
```
aws ecr get-login-password --region us-west-2 | docker login --username AWS --password-stdin 5678123456781234.dkr.ecr.us-west-2.amazonaws.com

docker logout 5678123456781234.dkr.ecr.us-west-2.amazonaws.com
```

## aws logs get assume-role usage
```
# get username and role assumed, output result result (queryId in json) to assumerole-query.json 
# get epoch time from https://www.epochconverter.com/
aws-vault exec privileged-admin -- aws logs start-query --start-time 1588291200 --end-time 1589748390 --log-group-name cloudtrail-cloudwatch-logs --query-string 'fields userIdentity.userName, resources.0.ARN | filter eventName = "AssumeRole" | filter resources.0.ARN  like "arn:aws:iam::5678123456781234:role/team" ' | tee assumerole-query.json

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
