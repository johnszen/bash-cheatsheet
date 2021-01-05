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
```json
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

## make two elements value become {value1: value2}
`.TagList |  map ( { (."Key") : .Value } ) | add`

Input
``` json
{
"TagList": [
        {
            "Key": "Cost Center",
            "Value": "Solutions"
        },
        {
            "Key": "Project Team",
            "Value": "Internal Systems"
        },
        {
            "Key": "Project Service",
            "Value": "captain"
        },
        {
            "Key": "MakeSnapshot-KeepCount",
            "Value": "7"
        },
        {
            "Key": "MakeSnapshot",
            "Value": "True"
        }
    ]
}
```

```json
{
  "Cost Center": "Solutions",
  "Project Team": "Internal Systems",
  "Project Service": "captain",
  "MakeSnapshot-KeepCount": "7",
  "MakeSnapshot": "True"
}
```


