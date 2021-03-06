# Work with Google Cloud  Platform (endpoints service) via terraform

A terraform module for making google endpoints service.

## Usage
--------

Import the module and retrieve with ```terraform get``` or ```terraform get --update```. Adding a module resource to your template, e.g. `main.tf`:

```
#
# MAINTAINER Vitaliy Natarov "vitaliy.natarov@yahoo.com"
#
terraform {
  required_version = "> 0.9.0"
}
provider "google" {
    credentials = "${file("/Users/captain/.config/gcloud/creds/terraform_creds.json")}"
    project     = "terraform-2018"
    region      = "us-east1"
}
module "endpoints_service" {
    source                              = "../../modules/endpoints_service"

    # Use openapi
    enable_endpoints_service_openapi    = true
    openapi_config                      = "files/openapi_spec.yml"
    #
    # Use grpc
    #enable_endpoints_service_grpc        = true
    #grpc_config                         = "files/service_spec.yml"
    #protoc_output_base64                = "files/compiled_descriptor_file.pb"

   service_name                         = "api-name.endpoints.terraform-2018.cloud.goog"
}

```

Module Input Variables
----------------------
- `project` - "(Optional) The project ID that the service belongs to. If not provided, provider project is used." (`  default     = ""    `)
- `enable_endpoints_service_openapi` - "Enable endpoints service openapi usage" (`    default     = "false"`)
- `openapi_config` - "(Optional) The full text of the OpenAPI YAML configuration as described. Set path to openapi config file. Ex: openapi_spec.yml." (`    default     = ""`)
- `enable_endpoints_service_grpc` - "Enable endpoints service grpc usage" (`    default     = "false"`)
- `grpc_config` - "(Optional) The full text of the Service Config YAML file (Example located here). If provided, must also provide protoc_output. open_api config must not be provided. Set path to grpc config file. Ex: service_spec.yml" (`    default     = ""`)
- `protoc_output_base64` - "(Optional) The full contents of the Service Descriptor File generated by protoc. This should be a compiled .pb file, base64-encoded. Set path to protoc output file. Ex: compiled_descriptor_file.pb" (`    default     = ""`)
- `service_name` - "(Required) The name of the service. Usually of the form $apiname.endpoints.$projectid.cloud.goog." (`    default     = ""`)


Authors
=======

Created and maintained by [Vitaliy Natarov](https://github.com/SebastianUA)
(vitaliy.natarov@yahoo.com).

License
=======

Apache 2 Licensed. See [LICENSE](https://github.com/SebastianUA/terraform/blob/master/LICENSE) for full details.
