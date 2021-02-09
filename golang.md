# GO

Copy dependency library to local project
```
# go mod init [project name] - create an index file
go mod init # create go.mod
go mod vendor
```

Update or .. - get the package again
```
# list all may need update
go list -u -m all

# go get github.com/..., -u for all sub-dependencies
go get -u github.com/vend/config
```

## AWS

[Dynamodb attribute value](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_AttributeValue.html)
