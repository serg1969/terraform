# Work with TLS via terraform

A terraform module for making TLS resources.

## Usage
----------------------
Import the module and retrieve with ```terraform get``` or ```terraform get --update```. Adding a module resource to your template, e.g. `main.tf`:

```
#
# MAINTAINER Vitaliy Natarov "vitaliy.natarov@yahoo.com"
#
terraform {
    required_version = "~> 0.12.12"
    backend "remote" {
        organization = "captain"

        workspaces {
            name = "captain-tls-nonprod"
        }
    }
}


module "tls_private_key" {
    source                                          = "../../modules/tls"

    enable_tls_private_key                          = true
    tls_private_key_algorithm                       = "RSA"
    tls_private_key_ecdsa_curve                     = "P224"
    tls_private_key_rsa_bits                        = 2048
}

module "tls_self_signed_cert" {
    source                                          = "../../modules/tls"

    enable_self_signed_cert                         = true
    tls_self_signed_cert_key_algorithm              = "RSA"
    #tls_self_signed_cert_private_key_pem           = "${file("file.pem")}"
    tls_self_signed_cert_private_key_pem            = "${module.tls_private_key.tls_private_key_private_key_pem}"
    tls_self_signed_cert_subject_common_name        = "linux-notes.org"
    tls_self_signed_cert_subject_organization       = "Organisation"
}

module "tls_locally_signed_cert" {
    source                                          = "../../modules/tls"

    enable_locally_signed_cert                      = false
}

module "tls_cert_request" {
    source                                          = "../../modules/tls"

    enable_tls_cert_request                         = false

}
```

## Module Input Variables
----------------------
- `name` - The name for newrelic_alert resources (`default = test`)
- `enable_tls_private_key` - Enable tls_private_key usage (`default = ""`)
- `tls_private_key_algorithm` - (Required) The name of the algorithm to use for the key. Currently-supported values are 'RSA' and 'ECDSA'. (`default = RSA`)
- `tls_private_key_ecdsa_curve` - (Optional) When algorithm is 'ECDSA', the name of the elliptic curve to use. May be any one of 'P224', 'P256', 'P384' or 'P521', with 'P224' as the default. (`default = P224`)
- `tls_private_key_rsa_bits` - (Optional) When algorithm is 'RSA', the size of the generated RSA key in bits. Defaults to 2048. (`default = 2048`)
- `enable_self_signed_cert` - Enable self_signed_cert usage (`default = ""`)
- `tls_self_signed_cert_key_algorithm` - (Required) The name of the algorithm for the key provided in private_key_pem. (`default = ECDSA`)
- `tls_self_signed_cert_private_key_pem` - (Required) PEM-encoded private key data. This can be read from a separate file using the file interpolation function. If the certificate is being generated to be used for a throwaway development environment or other non-critical application, the tls_private_key resource can be used to generate a TLS private key from within Terraform. Only an irreversable secure hash of the private key will be stored in the Terraform state. (`default = ""`)
- `tls_self_signed_cert_subject_common_name` - Set common name (`default = ""`)
- `tls_self_signed_cert_subject_organization` - Set organization (`default = ""`)
- `tls_self_signed_cert_subject_organizational_unit` - Set organizational unit (`default = ""`)
- `tls_self_signed_cert_subject_street_address` - Set street address (`default = ""`)
- `tls_self_signed_cert_subject_locality` - Set locality (`default = ""`)
- `tls_self_signed_cert_subject_province` - Set province (`default = ""`)
- `tls_self_signed_cert_subject_country` - Set country (`default = ""`)
- `tls_self_signed_cert_subject_postal_code` - Set postal code (`default = ""`)
- `tls_self_signed_cert_validity_period_hours` - (Required) The number of hours after initial issuing that the certificate will become invalid. (`default = 12`)
- `tls_self_signed_cert_allowed_uses` - (Required) List of keywords each describing a use that is permitted for the issued certificate. The valid keywords are listed below. (`default = ['key_encipherment', 'digital_signature', 'server_auth']`)
- `tls_self_signed_cert_dns_names` - (Optional) List of DNS names for which a certificate is being requested. (`default = ""`)
- `tls_self_signed_cert_ip_addresses` - (Optional) List of IP addresses for which a certificate is being requested (`default = ""`)
- `tls_self_signed_cert_early_renewal_hours` - (Optional) If set, the resource will consider the certificate to have expired the given number of hours before its actual expiry time. This can be useful to deploy an updated certificate in advance of the expiration of the current certificate. Note however that the old certificate remains valid until its true expiration time, since this resource does not (and cannot) support certificate revocation. Note also that this advance update can only be performed should the Terraform configuration be applied during the early renewal period. (`default = 12`)
- `tls_self_signed_cert_is_ca_certificate` - (Optional) Boolean controlling whether the CA flag will be set in the generated certificate. Defaults to false, meaning that the certificate does not represent a certificate authority. (`default = ""`)
- `enable_locally_signed_cert` - Enable tls_locally_signed_cert usage (`default = ""`)
- `cert_request_pem` - (Required) PEM-encoded request certificate data. (`default = ""`)
- `ca_key_algorithm` - (Required) The name of the algorithm for the key provided in ca_private_key_pem. (`default = ECDSA`)
- `ca_private_key_pem` - (Required) PEM-encoded private key data for the CA. This can be read from a separate file using the file interpolation function. (`default = ""`)
- `ca_cert_pem` - (Required) PEM-encoded certificate data for the CA. (`default = ""`)
- `locally_validity_period_hours` - (Required) The number of hours after initial issuing that the certificate will become invalid. (`default = 12`)
- `locally_allowed_uses` - (Required) List of keywords each describing a use that is permitted for the issued certificate. The valid keywords are listed below. (`default = ['key_encipherment', 'digital_signature', 'server_auth']`)
- `locally_early_renewal_hours` - (Optional) If set, the resource will consider the certificate to have expired the given number of hours before its actual expiry time. This can be useful to deploy an updated certificate in advance of the expiration of the current certificate. Note however that the old certificate remains valid until its true expiration time, since this resource does not (and cannot) support certificate revocation. Note also that this advance update can only be performed should the Terraform configuration be applied during the early renewal period. (`default = 12`)
- `locally_is_ca_certificate` - (Optional) Boolean controlling whether the CA flag will be set in the generated certificate. Defaults to false, meaning that the certificate does not represent a certificate authority. (`default = ""`)
- `enable_tls_cert_request` - Enable cert_request usage (`default = ""`)
- `tls_cert_request_key_algorithm` - (Required) The name of the algorithm for the key provided in private_key_pem. (`default = ECDSA`)
- `tls_cert_request_private_key_pem` - (Required) PEM-encoded private key data. This can be read from a separate file using the file interpolation function. Only an irreversable secure hash of the private key will be stored in the Terraform state. (`default = ""`)
- `tls_cert_request_dns_names` - (Optional) List of DNS names for which a certificate is being requested. (`default = ""`)
- `tls_cert_request_ip_addresses` - (Optional) List of IP addresses for which a certificate is being requested. (`default = ""`)
- `tls_cert_request_subject_common_name` - Set common name. Ex: linux-notes.org (`default = ""`)
- `tls_cert_request_subject_organization` - Set organization name (`default = ""`)
- `tls_cert_request_subject_organizational_unit` - Set organization unit name (`default = ""`)
- `tls_cert_request_subject_street_address` - Set street name (`default = ""`)
- `tls_cert_request_subject_locality` - Set location (`default = ""`)
- `tls_cert_request_subject_province` - Set province name (`default = ""`)
- `tls_cert_request_subject_country` - Set country name (`default = ""`)
- `tls_cert_request_subject_postal_code` - Set postal code (`default = ""`)
- `tls_cert_request_subject_postal_serial_number` - Set postal serial number (`default = ""`)

## Module Output Variables
----------------------
- `tls_private_key_algorithm` - The algorithm that was selected for the key.
- `tls_private_key_private_key_pem` - The private key data in PEM format.
- `tls_private_key_public_key_pem` - The public key data in PEM format.
- `tls_private_key_public_key_openssh` - The public key data in OpenSSH authorized_keys format, if the selected private key format is compatible. All RSA keys are supported, and ECDSA keys with curves 'P256', 'P384' and 'P521' are supported. This attribute is empty if an incompatible ECDSA curve is selected.
- `tls_private_key_public_key_fingerprint_md5` - The md5 hash of the public key data in OpenSSH MD5 hash format, e.g. aa:bb:cc:.... Only available if the selected private key format is compatible, as per the rules for public_key_openssh.
- `tls_self_signed_cert_cert_pem` - The certificate data in PEM format.
- `tls_self_signed_cert_validity_start_time` - The time after which the certificate is valid, as an RFC3339 timestamp.
- `tls_self_signed_cert_validity_end_time` - The time until which the certificate is invalid, as an RFC3339 timestamp.
- `tls_locally_signed_cert_cert_pem` - The certificate data in PEM format.
- `tls_locally_signed_cert_validity_start_time` - The time after which the certificate is valid, as an RFC3339 timestamp.
- `tls_locally_signed_cert_validity_end_time` - The time until which the certificate is invalid, as an RFC3339 timestamp.
- `tls_cert_request_cert_request_pem` - The certificate request data in PEM format.


## Authors

Created and maintained by [Vitaliy Natarov](https://github.com/SebastianUA). An email: [vitaliy.natarov@yahoo.com](vitaliy.natarov@yahoo.com).

## License

Apache 2 Licensed. See [LICENSE](https://github.com/SebastianUA/terraform/blob/master/LICENSE) for full details.
