# terraform

Move resource from one statefile to another statefile
```
terraform state mv -state=./ap-southeast-2/services/trav/terraform.tfstate -state-out=./terraform.tfstate aws_iam_policy_attachment.event-invoke-attach-role aws_iam_policy_attachment.event-invoke-attach-role
```
