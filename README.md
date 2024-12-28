# terraform-aws-lambda-auto-start-stop-ec2-instances

A terraform module to start or stop EC2 instances on a periodic basis.

In order to save some AWS costs, it is usual to stop Non-Production assets out of business hours.
This module enables to stop or start instances at the time you need.
Please note that EC2 instances managed by an Auto Scaling Group are excluded. See [julb/terraform-aws-lambda-auto-start-stop-ec2-autoscalinggroups](https://github.com/julb/terraform-aws-lambda-auto-start-stop-ec2-autoscalinggroups) module to manage these ones.

See related modules:

- [julb/terraform-aws-lambda-auto-start-stop-ec2-instances](https://github.com/julb/terraform-aws-lambda-auto-start-stop-ec2-instances)
- [julb/terraform-aws-lambda-auto-start-stop-ec2-autoscalinggroups](https://github.com/julb/terraform-aws-lambda-auto-start-stop-ec2-autoscalinggroups)
- [julb/terraform-aws-lambda-auto-start-stop-rds-instances](https://github.com/julb/terraform-aws-lambda-auto-start-stop-rds-instances)
- [julb/terraform-aws-lambda-auto-start-stop-cloudwatch-alarms](https://github.com/julb/terraform-aws-lambda-auto-start-stop-cloudwatch-alarms)


## Usage

- Stop EC2 instances

```hcl
module "stop_ec2_instances" {
  source              = "github.com/julb/terraform-aws-lambda-auto-start-stop-ec2-instances"
  name                = "StopEc2Instances"
  schedule_expression = "cron(0 0 ? * FRI *)"
  action              = "stop"
  tags                = { "custom:tag" : "someValue" }
  lookup_resource_tag = {
    key   = "ops:env"
    value = "non-prod"
  }
}
```

- Start EC2 instances

```hcl
module "start_ec2_instances" {
  source              = "github.com/julb/terraform-aws-lambda-auto-start-stop-ec2-instances"
  name                = "StartEc2Instances"
  schedule_expression = "cron(0 8 ? * MON *)"
  action              = "start"
  tags                = { "custom:tag" : "someValue" }
  lookup_resource_tag = {
    key   = "ops:env"
    value = "non-prod"
  }
}
```
<!-- BEGIN_TF_DOCS -->
## Requirements

| Name | Version |
|------|---------|
| <a name="requirement_terraform"></a> [terraform](#requirement\_terraform) | >= 0.13.0 |

## Providers

| Name | Version |
|------|---------|
| <a name="provider_archive"></a> [archive](#provider\_archive) | 2.7.0 |
| <a name="provider_aws"></a> [aws](#provider\_aws) | 5.82.2 |

## Modules

No modules.

## Resources

| Name | Type |
|------|------|
| [archive_file.this](https://registry.terraform.io/providers/hashicorp/archive/latest/docs/resources/file) | resource |
| [aws_cloudwatch_event_rule.this](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/cloudwatch_event_rule) | resource |
| [aws_cloudwatch_event_target.this](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/cloudwatch_event_target) | resource |
| [aws_cloudwatch_log_group.this](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/cloudwatch_log_group) | resource |
| [aws_iam_role.this](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role) | resource |
| [aws_iam_role_policy.this_autoscaling_describe_instances](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role_policy) | resource |
| [aws_iam_role_policy.this_ec2_start_instances](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role_policy) | resource |
| [aws_iam_role_policy.this_ec2_stop_instances](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role_policy) | resource |
| [aws_iam_role_policy.this_logs_put_events](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/iam_role_policy) | resource |
| [aws_lambda_function.this](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/lambda_function) | resource |
| [aws_lambda_permission.this](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/lambda_permission) | resource |
| [aws_region.this](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/data-sources/region) | data source |

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|------|---------|:--------:|
| <a name="input_action"></a> [action](#input\_action) | The action which the lambda needs to perform. | `string` | n/a | yes |
| <a name="input_custom_iam_role_arn"></a> [custom\_iam\_role\_arn](#input\_custom\_iam\_role\_arn) | A custom IAM role to execute the lambda. If not specified, a role will be created. | `string` | `null` | no |
| <a name="input_kms_key_id"></a> [kms\_key\_id](#input\_kms\_key\_id) | Optional KMS key ID used to encrypt the logs. | `string` | `null` | no |
| <a name="input_lookup_resource_regions"></a> [lookup\_resource\_regions](#input\_lookup\_resource\_regions) | A list of regions in which the resources will be looked for. By default, use the current region. | `list(string)` | `null` | no |
| <a name="input_lookup_resource_tag"></a> [lookup\_resource\_tag](#input\_lookup\_resource\_tag) | The tag to use to search for EC2 instances. | `object({ key = string, value = string })` | n/a | yes |
| <a name="input_name"></a> [name](#input\_name) | The name of the lambda to create. | `string` | n/a | yes |
| <a name="input_retention_in_days"></a> [retention\_in\_days](#input\_retention\_in\_days) | The number of days to retain the logs. | `number` | `7` | no |
| <a name="input_schedule_expression"></a> [schedule\_expression](#input\_schedule\_expression) | Define the schedule expression to trigger the lambda. | `string` | n/a | yes |
| <a name="input_tags"></a> [tags](#input\_tags) | Tags to associate to the lambda. | `map(string)` | `{}` | no |

## Outputs

| Name | Description |
|------|-------------|
| <a name="output_lambda_arn"></a> [lambda\_arn](#output\_lambda\_arn) | The Lambda ARN. |
| <a name="output_lambda_function_name"></a> [lambda\_function\_name](#output\_lambda\_function\_name) | The Lambda Function name. |
| <a name="output_lambda_iam_role_arn"></a> [lambda\_iam\_role\_arn](#output\_lambda\_iam\_role\_arn) | The Lambda IAM role ARN. |
| <a name="output_lambda_log_group_arn"></a> [lambda\_log\_group\_arn](#output\_lambda\_log\_group\_arn) | The Lambda Log Group ARN. |
| <a name="output_lambda_log_group_name"></a> [lambda\_log\_group\_name](#output\_lambda\_log\_group\_name) | The name of the Lambda Log Group. |
<!-- END_TF_DOCS -->

## Contributing

This project is totally open source and contributors are welcome.

When you submit a PR, please ensure that the python code is well formatted and linted.

```
$ make format
$ make lint
$ make test
```
