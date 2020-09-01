# terraform

Import task-definition into statefile
```
aws-vault exec an_assume-role -- terraform import aws_ecs_task_definition.a_task_def arn:aws:ecs:my-region-1:1234567890:task-definition/actual_task_def_name:101
```

Move resource from one statefile to another statefile
```
terraform state mv -state=./ap-southeast-2/services/trav/terraform.tfstate -state-out=./terraform.tfstate aws_iam_policy_attachment.event-invoke-attach-role aws_iam_policy_attachment.event-invoke-attach-role
```
