---
title: Role DSL
nav_text: Role
categories: dsl
nav_order: 13
---

Cody can create the IAM service role associated with the codebuild project. Here's an example:

.cody/role.rb:

```ruby
iam_policy("logs", "ssm")
```

For more control, here's a longer form:

```ruby
iam_policy(
  action: [
    "logs:CreateLogGroup",
    "logs:CreateLogStream",
    "logs:PutLogEvents",
    "ssm:*",
  ],
  effect: "Allow",
  resource: "*"
)
```

You can also create managed IAM policy.

```ruby
managed_iam_policy("AmazonS3ReadOnlyAccess")
```

You can also add multiple managed IAM policies:

```ruby
managed_iam_policy("AmazonS3ReadOnlyAccess", "AmazonEC2ReadOnlyAccess")
```

## Full DSL

The convenience methods merely wrap properties of the [AWS::IAM::Role
 CloudFormation Resource](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-iam-role.html).  If you wanted to set the CloudFormation properties more directly, here's an example of using the "Full" DSL.

.cody/role.rb:

```ruby
assume_role_policy_document(
  statement: [{
    action: ["sts:AssumeRole"],
    effect: "Allow",
    principal: {
      service: ["codebuild.amazonaws.com"]
    }
  }],
  version: "2012-10-17"
)
path("/")
policies([{
  policy_name: "CodeBuildAccess",
  policy_document: {
    version: "2012-10-17",
    statement: [{
      action: [
        "logs:CreateLogGroup",
        "logs:CreateLogStream",
        "logs:PutLogEvents",
      ],
      effect: "Allow",
      resource: "*"
    }]
  }
}])
```

{% include prev_next.md %}