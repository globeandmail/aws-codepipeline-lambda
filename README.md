## aws-codepipeline-lambda
Creates a pipeline that generates a lambda zip archive and updates the existing function code

## v1.3 Note
The account that owns the guthub token must have admin access on the repo in order to generate a github webhook 

## Usage

```hcl
module "lambda_pipeline" {
  source = "github.com/globeandmail/aws-codepipeline-lambda?ref=1.3"

  name               = app-name
  function_name      = lambda-function-name
  github_repo_owner  = github-account-name
  github_repo_name   = github-repo-name
  github_oauth_token = data.aws_ssm_parameter.github_token.value
  tags = {
    Environment = var.environment
  }
}
```

## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|:----:|:-----:|:-----:|
| name | The name associated with the pipeline and assoicated resources. ie: app-name | string | n/a | yes |
| function\_name | The name of the Lambda function to update | string | n/a | yes |
| github\_repo\_owner | The owner of the GitHub repo | string | n/a | yes |
| github\_repo\_name | The name of the GitHub repository | string | n/a | yes |
| github\_oauth\_token | GitHub oauth token | string | n/a | yes |
| github\_branch\_name | The git branch name to use for the codebuild project | string | `"master"` | no |
| codebuild\_image | The codebuild image to use | string | `"null"` | no |
| function\_alias | The name of the Lambda function alias that gets passed to the UserParameters data in the deploy stage | string | `"live"` | no |
| deploy\_function\_name | The name of the Lambda function in the account that will update the function code | string | `"CodepipelineDeploy"` | no |
| tags | A mapping of tags to assign to the resource | map | `{}` | no |

## Outputs

| Name | Description |
|------|-------------|
| artifact\_bucket\_arn |  |
| artifact\_bucket\_id |  |
| codebuild\_project\_arn |  |
| codebuild\_project\_id |  |
| codepipeline\_arn |  |
| codepipeline\_id |  |

## Builspec example

```yml
version: 0.2

phases:
  install:
    runtime-versions:
      python: 3.7
  build:
    commands:
      - pip install --upgrade pip
      - pip install -r requirements.txt -t .
artifacts:
  type: zip
  files:
    - '**/*'
```