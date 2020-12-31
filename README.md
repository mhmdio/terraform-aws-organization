Terraform AWS Organisation
==========================

Note: this is a small footprint Based on infrablocks work.
A Terraform module for managing an AWS Organisation.

The Organisation deployment consists of:
* an AWS organisation
* with a hierarchy of organisational units
* with a set of accounts placed in that hierarchy

Usage
-----

To use the module, include something like the following in your terraform
configuration:

```hcl-terraform
module "organisation" {
  source = "infrablocks/organisation/aws"
  version = "0.1.1"

  feature_set = "ALL"

  organizational_units = [
    {
      name = "Root",
      children = [
        {
          name = "Parent",
          children = [
            {
              name = "Dev",
            },
            {
              name = "Prod"
            }
          ]
        }
      ]
    }
  ]

  accounts = [
    {
      name = "Dev Gold"
      email = "me+dev.gold@example.com"
      organizational_unit = "Dev"
      allow_iam_users_access_to_billing = true
    },
    {
      name = "Dev Silver"
      email = "me+dev.silver@example.com"
      organizational_unit = "Dev"
      allow_iam_users_access_to_billing = true
    },
    {
      name = "Prod Platinum"
      email = "me+prod.platinum@example.com"
      organizational_unit = "Prod"
      allow_iam_users_access_to_billing = true
    }
  ]
}
```

Note: `organizational_units` can be nested up to 3 levels deep. Levels 1 & 2 
must include a `children` property although it can be an empty array. Level 3
must not include a `children` property.
