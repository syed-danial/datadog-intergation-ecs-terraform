# Terraform Operations GitHub Action Workflow

This GitHub Action Workflow automates the Terraform operations for your infrastructure. It handles the initialization, planning, and applying of Terraform configuration based on the provided inputs. Additionally, it triggers semantic release if the apply step is executed.

## Workflow Details

### Triggers

- **Workflow Call**: This workflow is designed to be called by other workflows, and it requires specific inputs and secrets.

### Inputs

- **tfvars-path**: Path to the Terraform variable file (required).
- **tf-apply**: Boolean flag to determine whether to run `terraform apply` or just `terraform plan` (required).
- **tf-workspace**: Name of the Terraform workspace to use (required).

### Secrets

- **token**: Required secret for the authentication.

### Jobs

1. **TerraformPlanOrApply**: Responsible for setting up Terraform and executing `terraform plan` and optionally `terraform apply` based on the `tf-apply` input.
2. **RunSemanticRelease**: Triggers the semantic release after Terraform operations are complete. This job depends on `TerraformPlanOrApply`.

## HOW TO

### How to Call this Workflow

You can call this workflow from another workflow within the same repository by using the `workflow_call` syntax. Here's an example:

```yaml
jobs:
  my_job:
    uses: ./.github/workflows/path-to-this-workflow.yml
    with:
      tfvars-path: 'path/to/your.tfvars'
      tf-apply: true
      tf-workspace: 'my_workspace'
    secrets:
      token: ${{ secrets.MY_TOKEN }}
```

### How to Modify the Workflow

- To change the AWS region or role, update the `aws-actions/configure-aws-credentials` step.
- To use a different Terraform version, modify the `terraform_version` in the `hashicorp/setup-terraform` step.
- To add additional Terraform commands or logic, you can include more steps in the `TerraformPlanOrApply` job.

### How to Debug the Workflow

- You can view the logs for each run of this workflow in the "Actions" tab of your GitHub repository.
- To troubleshoot issues with Terraform commands, you can refer to the output logs of the `Terraform Init`, `Terraform Plan`, and `Terraform Apply` steps.
- If you face issues with the semantic release, check the logs of the `RunSemanticRelease` job.

## Support & Contributions

For support or to contribute to this workflow, please [open an issue](link-to-your-repo-issues) or submit a pull request.