# jq

## return empty instead of null
`.executionRoleArn | select (.!= null)`

## if then else
`.Parameters[] | if .Name == \"${p_key}\" then .Name else empty end `

## range of an array
`.Parameters[5:10][]`

## get an element in an array with the array possibility not exist
`.containerDefinitions[] | select (.environment != null)  | .environment[] | select ( .name == \"${p_key}\") | .name `

## filter an array
`.Parameters | map(select(any(.Type; . == "String"))) `

Pipe to length to get match count
`.Parameters | map(select(any(.Type; . == "String"))) | length`
Input
```
{
  "Parameters": [
    {
      "Name": "/global/HONEYCOMB_API_KEY",
      "Type": "String"
    },
    {
      "Name": "/global/consumer-production/KAFKA_BROKER",
      "Type": "String"
    }
    ]
}
```

Output
```
# not matched
[]
# matched
[
    {
      "Name": "/global/HONEYCOMB_API_KEY",
      "Type": "String"
    },
    {
      "Name": "/global/consumer-production/KAFKA_BROKER",
      "Type": "String"
    }
 ]
```
## add element to an array
`.containerDefinitions[].secrets |= .+ [{\"key\":\"${p_key}\", \"valueFrom\":\"${p_value_from}\"}]`

## combine files
`jq -s '{ Parameters: map(.Parameters[]) }' .work/tmp_"${p_filename}"*.json`
