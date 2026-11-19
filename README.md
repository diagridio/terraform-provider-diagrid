# Terraform Provider for Diagrid

[![Terraform Registry](https://img.shields.io/badge/terraform-registry-623CE4?logo=terraform)](https://registry.terraform.io/providers/diagridio/diagrid/latest)
[![Release](https://img.shields.io/github/v/release/diagridio/terraform-provider-diagrid)](https://github.com/diagridio/terraform-provider-diagrid/releases)
[![Tests](https://github.com/diagridio/terraform-provider-diagrid/actions/workflows/test.yml/badge.svg)](https://github.com/diagridio/terraform-provider-diagrid/actions/workflows/test.yml)

The Terraform provider for Diagrid manages projects, App IDs, components, pub/sub, KV stores, resiliency policies and service accounts as code.

Diagrid is a managed platform for Dapr Workflows and durable execution, built on [Dapr](https://dapr.io).

- Provider docs: [Terraform Registry](https://registry.terraform.io/providers/diagridio/diagrid/latest/docs)
- Product docs: [docs.diagrid.io](https://docs.diagrid.io/)
- Changes: [CHANGELOG.md](CHANGELOG.md)

## Requirements

- [Terraform](https://developer.hashicorp.com/terraform/downloads) >= 1.0
- [Go](https://go.dev/doc/install) >= 1.26.9 (only to build the provider from source)

## Using the provider

Declare the provider and pass it an API key and the API endpoint (from [`examples/provider/provider.tf`](examples/provider/provider.tf)):

```terraform
# Specify required provider as maintained
terraform {
  required_providers {
    diagrid = {
      source = "diagridio/diagrid"
    }
  }
}

provider "diagrid" {
  api_key  = var.api_key
  endpoint = var.endpoint
}
```

The example declares `api_key` and `endpoint` in [`examples/provider/variables.tf`](examples/provider/variables.tf), without defaults. Set them with `TF_VAR_api_key` and `TF_VAR_endpoint`, `-var` flags, or a `.tfvars` file you do not commit.

Both settings are optional. To read them from the environment instead, leave them out of the provider block:

```terraform
provider "diagrid" {}
```

The provider then reads `DIAGRID_API_KEY` and `DIAGRID_API_ENDPOINT`, and the endpoint defaults to `https://api.r1.diagrid.io`:

```shell
export DIAGRID_API_KEY="<your-api-key>"
terraform init
terraform apply
```

To get an API key, sign in to the Diagrid Console and create one under your organization's API keys, or run `diagrid apikeys create` with the Diagrid CLI. Keep the key out of version control.

Diagrid has a permanent free tier. See [pricing](https://www.diagrid.io/pricing).

### Resources

- [`diagrid_appid`](https://registry.terraform.io/providers/diagridio/diagrid/latest/docs/resources/appid)
- [`diagrid_component`](https://registry.terraform.io/providers/diagridio/diagrid/latest/docs/resources/component)
- [`diagrid_configuration`](https://registry.terraform.io/providers/diagridio/diagrid/latest/docs/resources/configuration)
- [`diagrid_kvstore`](https://registry.terraform.io/providers/diagridio/diagrid/latest/docs/resources/kvstore)
- [`diagrid_project`](https://registry.terraform.io/providers/diagridio/diagrid/latest/docs/resources/project)
- [`diagrid_pubsub`](https://registry.terraform.io/providers/diagridio/diagrid/latest/docs/resources/pubsub)
- [`diagrid_region`](https://registry.terraform.io/providers/diagridio/diagrid/latest/docs/resources/region)
- [`diagrid_resiliency`](https://registry.terraform.io/providers/diagridio/diagrid/latest/docs/resources/resiliency)
- [`diagrid_service_account`](https://registry.terraform.io/providers/diagridio/diagrid/latest/docs/resources/service_account)
- [`diagrid_subscription`](https://registry.terraform.io/providers/diagridio/diagrid/latest/docs/resources/subscription)

### Data sources

- [`diagrid_appid`](https://registry.terraform.io/providers/diagridio/diagrid/latest/docs/data-sources/appid)
- [`diagrid_component`](https://registry.terraform.io/providers/diagridio/diagrid/latest/docs/data-sources/component)
- [`diagrid_configuration`](https://registry.terraform.io/providers/diagridio/diagrid/latest/docs/data-sources/configuration)
- [`diagrid_kvstore`](https://registry.terraform.io/providers/diagridio/diagrid/latest/docs/data-sources/kvstore)
- [`diagrid_organization`](https://registry.terraform.io/providers/diagridio/diagrid/latest/docs/data-sources/organization)
- [`diagrid_project`](https://registry.terraform.io/providers/diagridio/diagrid/latest/docs/data-sources/project)
- [`diagrid_pubsub`](https://registry.terraform.io/providers/diagridio/diagrid/latest/docs/data-sources/pubsub)
- [`diagrid_region`](https://registry.terraform.io/providers/diagridio/diagrid/latest/docs/data-sources/region)
- [`diagrid_resiliency`](https://registry.terraform.io/providers/diagridio/diagrid/latest/docs/data-sources/resiliency)
- [`diagrid_service_account`](https://registry.terraform.io/providers/diagridio/diagrid/latest/docs/data-sources/service_account)
- [`diagrid_subscription`](https://registry.terraform.io/providers/diagridio/diagrid/latest/docs/data-sources/subscription)

More examples, one per resource and data source, are in [`examples/`](examples/).

## Developing the provider

- Build and install the provider binary into `$GOPATH/bin`: `go install`
- Regenerate the docs in `docs/` from the schema and `examples/`: `go generate`
- Run the unit tests: `go test ./...`
- Run the acceptance tests: `make testacc`

`make testacc` sets `TF_ACC=1` and creates real resources in the Diagrid organization your API key belongs to, which may cost money. Point it at an organization you can clean up.
