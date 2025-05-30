# Reusable Workflows for NuGet Push

This repository contains reusable workflows for deploying NuGet packages from within your CI/CD pipeline.

> These workflows is part of the Codebelt umbrella and ensures a consistent way of: 
> 
> - Defining your CI/CD pipeline 
> - Structuring your repository
> - Keeping your codebase small and feasible
> - Writing clean and maintainable code
> - Deploying your code to different environments
> - Automating as much as possible
>
> A paved path to excel as a DevSecOps Engineer.

## Available Workflows

- [default.yml](.github/workflows/default.yml) - the default workflow that:
  - determines the job to invoke based on the environment input,
  - [installs the Mono SDK](https://github.com/codebeltnet/install-mono),
  - [installs the NuGet CLI](https://github.com/codebeltnet/install-nuget),
  - downloads the NuGet build artifact,
  - [deployes the NuGet package](https://github.com/codebeltnet/nuget-push).

### Usage

To call this workflow in your GitHub repository, you can follow these steps:

```yaml
nuget-call:
    uses: codebeltnet/jobs-nuget-push/.github/workflows/default.yml@v2
```

### Inputs

```yaml
with:
  # The version of your project, e.g., 1.0.0.
  version:
  # When specified, the environment secret will be used, and not the secret passed from the caller workflow.
  environment:
  # Defines the build configuration. Default is Release.
  configuration: Release
  # The source to push the package to. Default is https://api.nuget.org/v3/index.json.
  source: https://api.nuget.org/v3/index.json
# The name of the downloaded build artifact. Default, when left empty, is 'format('NuGet-{0}', inputs.configuration)'.
  download-build-artifact-name:
  # Sets the verbosity level of the command. Allowed values are quiet, normal and detailed. The default is quiet.
  verbosity-level:
  # The maximum time in minutes to allow the job to run. Default is 15 minutes.
  timeout-minutes: 15
```

### Secrets

```yaml
secrets:
  NUGET_TOKEN: ${{ secrets.NUGET_TOKEN }}
```

### Outputs

This workflow has no outputs.

### Example

```yaml
jobs:
  deploy:
    if: github.event_name != 'pull_request'
    name: call-nuget
    needs: [build, pack, test, sonarcloud, codecov, codeql]
    uses: codebeltnet/jobs-nuget-push/.github/workflows/default.yml@v2
    with:
      version: ${{ needs.build.outputs.version }}
      environment: Production
      configuration: ${{ inputs.configuration == '' && 'Release' || inputs.configuration }}
    secrets: inherit
```

## Caller workflows to showcase the Codebelt experience

### Basic CI/CD Pipeline

- Bootstrapper API - https://github.com/codebeltnet/bootstrapper/blob/main/.github/workflows/pipelines.yml
- Extensions for Asp.Versioning API - https://github.com/codebeltnet/asp-versioning/blob/main/.github/workflows/pipelines.yml
- Extensions for AWS Signature Version 4 API - https://github.com/codebeltnet/aws-signature-v4/blob/main/.github/workflows/pipelines.yml
- Extensions for Globalization API - https://github.com/codebeltnet/globalization/blob/main/.github/workflows/pipelines.yml
- Extensions for Newtonsoft.Json API - https://github.com/codebeltnet/newtonsoft-json/blob/main/.github/workflows/pipelines.yml
- Extensions for Swashbuckle.AspNetCore API - https://github.com/codebeltnet/swashbuckle-aspnetcore/blob/main/.github/workflows/pipelines.yml
- Extensions for xUnit API - https://github.com/codebeltnet/xunit/blob/main/.github/workflows/pipelines.yml
- Extensions for YamlDotNet API - https://github.com/codebeltnet/yamldotnet/blob/main/.github/workflows/pipelines.yml
- Shared Kernel API - https://github.com/codebeltnet/shared-kernel/blob/main/.github/workflows/pipelines.yml
- Unitify API - https://github.com/codebeltnet/unitify/blob/main/.github/workflows/pipelines.yml

### Intermediate CI/CD Pipeline

- Savvy I/O - https://github.com/codebeltnet/savvyio/blob/main/.github/workflows/pipelines.yml

### Advanced CI/CD Pipeline

- Cuemon for .NET - https://github.com/gimlichael/Cuemon/blob/main/.github/workflows/pipelines.yml

## Contributing to Reusable Workflows for NuGet Push

Contributions are welcome! 
Feel free to submit issues, feature requests, or pull requests to help improve these workflows.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
