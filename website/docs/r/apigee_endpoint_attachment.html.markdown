---
# ----------------------------------------------------------------------------
#
#     ***     AUTO GENERATED CODE    ***    Type: MMv1     ***
#
# ----------------------------------------------------------------------------
#
#     This file is automatically generated by Magic Modules and manual
#     changes will be clobbered when the file is regenerated.
#
#     Please read more about how to change this file in
#     .github/CONTRIBUTING.md.
#
# ----------------------------------------------------------------------------
subcategory: "Apigee"
page_title: "Google: google_apigee_endpoint_attachment"
description: |-
  Apigee Endpoint Attachment.
---

# google\_apigee\_endpoint\_attachment

Apigee Endpoint Attachment.


To get more information about EndpointAttachment, see:

* [API documentation](https://cloud.google.com/apigee/docs/reference/apis/apigee/rest/v1/organizations.endpointAttachments/create)
* How-to Guides
    * [Creating an environment](https://cloud.google.com/apigee/docs/api-platform/get-started/create-environment)

## Example Usage - Apigee Endpoint Attachment Basic


```hcl
data "google_client_config" "current" {}

resource "google_compute_network" "apigee_network" {
  name = "apigee-network"
}

resource "google_compute_global_address" "apigee_range" {
  name          = "apigee-range"
  purpose       = "VPC_PEERING"
  address_type  = "INTERNAL"
  prefix_length = 16
  network       = google_compute_network.apigee_network.id
}

resource "google_service_networking_connection" "apigee_vpc_connection" {
  network                 = google_compute_network.apigee_network.id
  service                 = "servicenetworking.googleapis.com"
  reserved_peering_ranges = [google_compute_global_address.apigee_range.name]
}

resource "google_apigee_organization" "apigee_org" {
  analytics_region   = "us-central1"
  project_id         = data.google_client_config.current.project
  authorized_network = google_compute_network.apigee_network.id
  depends_on         = [google_service_networking_connection.apigee_vpc_connection]
}

resource "google_apigee_endpoint_attachment" "apigee_endpoint_attachment" {
  org_id                 = google_apigee_organization.apigee_org.id
  endpoint_attachment_id = "test1"
  location               = "{google_compute_service_attachment location}"
  service_attachment     = "{google_compute_service_attachment id}"
}
```

## Argument Reference

The following arguments are supported:


* `location` -
  (Required)
  Location of the endpoint attachment.

* `service_attachment` -
  (Required)
  Format: projects/*/regions/*/serviceAttachments/*

* `org_id` -
  (Required)
  The Apigee Organization associated with the Apigee instance,
  in the format `organizations/{{org_name}}`.

* `endpoint_attachment_id` -
  (Required)
  ID of the endpoint attachment.


- - -



## Attributes Reference

In addition to the arguments listed above, the following computed attributes are exported:

* `id` - an identifier for the resource with format `{{org_id}}/endpointAttachments/{{endpoint_attachment_id}}`

* `name` -
  Name of the Endpoint Attachment in the following format:
  organizations/{organization}/endpointAttachments/{endpointAttachment}.

* `host` -
  Host that can be used in either HTTP Target Endpoint directly, or as the host in Target Server.

* `connection_state` -
  State of the endpoint attachment connection to the service attachment.


## Timeouts

This resource provides the following
[Timeouts](/docs/configuration/resources.html#timeouts) configuration options:

- `create` - Default is 30 minutes.
- `delete` - Default is 30 minutes.

## Import


EndpointAttachment can be imported using any of these accepted formats:

```
$ terraform import google_apigee_endpoint_attachment.default {{org_id}}/endpointAttachments/{{endpoint_attachment_id}}
$ terraform import google_apigee_endpoint_attachment.default {{org_id}}/{{endpoint_attachment_id}}
```
