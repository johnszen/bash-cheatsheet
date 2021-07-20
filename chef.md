# Chef
### Common commands
```bash
# generate a cookbook dir; -P to generate Policyfile.rb also
# Policyfile.rb specifies how cookbooks to be run
chef generate cookbook <dir-name> [-P]

# generate attributes for a cookbook
chef generate attribute <dir-name> <filename>

# generate a repo
chef generate repo <dir-name>

# compile a Policyfile into a lock file e.g. cookbooks/base/Policyfile.lock.json 
chef install cookbooks/base/Policyfile.rb
# list all the version to be used
chef describe-cookbook cookbooks/base


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
