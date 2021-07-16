# Chef
### Common commands
```bash
# generate a cookbook dir
chef generate cookbook <name>

# generate a repo
chef generate repo <name>

# list nodes
kitchen list

# update nodes to those specified in cookbook / recipes
kitchen converge

kitchen login <node>

```

### Using AWS EC2 driver

```yml
driver:
  name: ec2
```

```bash
aws-vault exec <assume-role> -- kitchen create
aws-vault exec <assume-role> -- kitchen destroy
```
