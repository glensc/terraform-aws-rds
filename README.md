<!-- 














  ** DO NOT EDIT THIS FILE
  ** 
  ** This file was automatically generated by the `build-harness`. 
  ** 1) Make all changes to `README.yaml` 
  ** 2) Run `make init` (you only need to do this once)
  ** 3) Run`make readme` to rebuild this file. 
  **
  ** (We maintain HUNDREDS of open source projects. This is how we maintain our sanity.)
  **















  -->
[![README Header][readme_header_img]][readme_header_link]

[![Cloud Posse][logo]](https://cpco.io/homepage)

# terraform-aws-rds [![Codefresh Build Status](https://g.codefresh.io/api/badges/pipeline/cloudposse/terraform-modules%2Fterraform-aws-rds?type=cf-1)](https://g.codefresh.io/public/accounts/cloudposse/pipelines/5d33e2a97ff4a883c72e9fc0) [![Latest Release](https://img.shields.io/github/release/cloudposse/terraform-aws-rds.svg)](https://github.com/cloudposse/terraform-aws-rds/releases/latest) [![Slack Community](https://slack.cloudposse.com/badge.svg)](https://slack.cloudposse.com)


Terraform module to provision AWS [`RDS`](https://aws.amazon.com/rds/) instances


---

This project is part of our comprehensive ["SweetOps"](https://cpco.io/sweetops) approach towards DevOps. 
[<img align="right" title="Share via Email" src="https://docs.cloudposse.com/images/ionicons/ios-email-outline-2.0.1-16x16-999999.svg"/>][share_email]
[<img align="right" title="Share on Google+" src="https://docs.cloudposse.com/images/ionicons/social-googleplus-outline-2.0.1-16x16-999999.svg" />][share_googleplus]
[<img align="right" title="Share on Facebook" src="https://docs.cloudposse.com/images/ionicons/social-facebook-outline-2.0.1-16x16-999999.svg" />][share_facebook]
[<img align="right" title="Share on Reddit" src="https://docs.cloudposse.com/images/ionicons/social-reddit-outline-2.0.1-16x16-999999.svg" />][share_reddit]
[<img align="right" title="Share on LinkedIn" src="https://docs.cloudposse.com/images/ionicons/social-linkedin-outline-2.0.1-16x16-999999.svg" />][share_linkedin]
[<img align="right" title="Share on Twitter" src="https://docs.cloudposse.com/images/ionicons/social-twitter-outline-2.0.1-16x16-999999.svg" />][share_twitter]


[![Terraform Open Source Modules](https://docs.cloudposse.com/images/terraform-open-source-modules.svg)][terraform_modules]



It's 100% Open Source and licensed under the [APACHE2](LICENSE).







We literally have [*hundreds of terraform modules*][terraform_modules] that are Open Source and well-maintained. Check them out! 






## Introduction

The module will create:

* DB instance (MySQL, Postgres, SQL Server, Oracle)
* DB Option Group (will create a new one or you may use an existing)
* DB Parameter Group
* DB Subnet Group
* DB Security Group
* DNS Record in Route53 for the DB endpoint

## Usage


**IMPORTANT:** The `master` branch is used in `source` just as an example. In your code, do not pin to `master` because there may be breaking changes between releases.
Instead pin to the release tag (e.g. `?ref=tags/x.y.z`) of one of our [latest releases](https://github.com/cloudposse/terraform-aws-rds/releases).


```hcl
module "rds_instance" {
    source                      = "git::https://github.com/cloudposse/terraform-aws-rds.git?ref=master"
    namespace                   = "eg"
    stage                       = "prod"
    name                        = "app"
    dns_zone_id                 = "Z89FN1IW975KPE"
    host_name                   = "db"
    security_group_ids          = ["sg-xxxxxxxx"]
    ca_cert_identifier          = "rds-ca-2019"
    allowed_cidr_blocks         = ["XXX.XXX.XXX.XXX/32"]
    database_name               = "wordpress"
    database_user               = "admin"
    database_password           = "xxxxxxxxxxxx"
    database_port               = 3306
    multi_az                    = true
    storage_type                = "gp2"
    allocated_storage           = 100
    storage_encrypted           = true
    engine                      = "mysql"
    engine_version              = "5.7.17"
    major_engine_version        = "5.7"
    instance_class              = "db.t2.medium"
    db_parameter_group          = "mysql5.7"
    option_group_name           = "mysql-options"
    publicly_accessible         = false
    subnet_ids                  = ["sb-xxxxxxxxx", "sb-xxxxxxxxx"]
    vpc_id                      = "vpc-xxxxxxxx"
    snapshot_identifier         = "rds:production-2015-06-26-06-05"
    auto_minor_version_upgrade  = true
    allow_major_version_upgrade = false
    apply_immediately           = false
    maintenance_window          = "Mon:03:00-Mon:04:00"
    skip_final_snapshot         = false
    copy_tags_to_snapshot       = true
    backup_retention_period     = 7
    backup_window               = "22:00-03:00"

    db_parameter = [
      { name  = "myisam_sort_buffer_size"   value = "1048576" },
      { name  = "sort_buffer_size"          value = "2097152" }
    ]

    db_options = [
      { option_name = "MARIADB_AUDIT_PLUGIN"
          option_settings = [
            { name = "SERVER_AUDIT_EVENTS"           value = "CONNECT" },
            { name = "SERVER_AUDIT_FILE_ROTATIONS"   value = "37" }
          ]
      }
    ]
}
```






## Makefile Targets
```
Available targets:

  help                                Help screen
  help/all                            Display help for all targets
  help/short                          This help short screen
  lint                                Lint terraform code

```
## Inputs

| Name | Description | Type | Default | Required |
|------|-------------|:----:|:-----:|:-----:|
| allocated_storage | The allocated storage in GBs | number | - | yes |
| allow_major_version_upgrade | Allow major version upgrade | bool | `false` | no |
| allowed_cidr_blocks | The whitelisted CIDRs which to allow `ingress` traffic to the DB instance | list(string) | `<list>` | no |
| apply_immediately | Specifies whether any database modifications are applied immediately, or during the next maintenance window | bool | `false` | no |
| associate_security_group_ids | The IDs of the existing security groups to associate with the DB instance | list(string) | `<list>` | no |
| attributes | Additional attributes (e.g. `1`) | list(string) | `<list>` | no |
| auto_minor_version_upgrade | Allow automated minor version upgrade (e.g. from Postgres 9.5.3 to Postgres 9.5.4) | bool | `true` | no |
| backup_retention_period | Backup retention period in days. Must be > 0 to enable backups | number | `0` | no |
| backup_window | When AWS can perform DB snapshots, can't overlap with maintenance window | string | `22:00-03:00` | no |
| ca_cert_identifier | The identifier of the CA certificate for the DB instance | string | `rds-ca-2019` | no |
| copy_tags_to_snapshot | Copy tags from DB to a snapshot | bool | `true` | no |
| database_name | The name of the database to create when the DB instance is created | string | - | yes |
| database_password | (Required unless a snapshot_identifier or replicate_source_db is provided) Password for the master DB user | string | `` | no |
| database_port | Database port (_e.g._ `3306` for `MySQL`). Used in the DB Security Group to allow access to the DB instance from the provided `security_group_ids` | number | - | yes |
| database_user | (Required unless a `snapshot_identifier` or `replicate_source_db` is provided) Username for the master DB user | string | `` | no |
| db_options | A list of DB options to apply with an option group. Depends on DB engine | object | `<list>` | no |
| db_parameter | A list of DB parameters to apply. Note that parameters may differ from a DB family to another | object | `<list>` | no |
| db_parameter_group | Parameter group, depends on DB engine used | string | - | yes |
| deletion_protection | Set to true to enable deletion protection on the RDS instance | bool | `false` | no |
| delimiter | Delimiter to be used between `namespace`, `environment`, `stage`, `name` and `attributes` | string | `-` | no |
| dns_zone_id | The ID of the DNS Zone in Route53 where a new DNS record will be created for the DB host name | string | `` | no |
| enabled | Set to false to prevent the module from creating any resources | bool | `true` | no |
| enabled_cloudwatch_logs_exports | List of log types to enable for exporting to CloudWatch logs. If omitted, no logs will be exported. Valid values (depending on engine): alert, audit, error, general, listener, slowquery, trace, postgresql (PostgreSQL), upgrade (PostgreSQL). | list(string) | `<list>` | no |
| engine | Database engine type | string | - | yes |
| engine_version | Database engine version, depends on engine type | string | - | yes |
| environment | Environment, e.g. 'prod', 'staging', 'dev', 'pre-prod', 'UAT' | string | `` | no |
| final_snapshot_identifier | Final snapshot identifier e.g.: some-db-final-snapshot-2019-06-26-06-05 | string | `` | no |
| host_name | The DB host name created in Route53 | string | `db` | no |
| instance_class | Class of RDS instance | string | - | yes |
| iops | The amount of provisioned IOPS. Setting this implies a storage_type of 'io1'. Default is 0 if rds storage type is not 'io1' | number | `0` | no |
| kms_key_arn | The ARN of the existing KMS key to encrypt storage | string | `` | no |
| license_model | License model for this DB. Optional, but required for some DB Engines. Valid values: license-included | bring-your-own-license | general-public-license | string | `` | no |
| maintenance_window | The window to perform maintenance in. Syntax: 'ddd:hh24:mi-ddd:hh24:mi' UTC | string | `Mon:03:00-Mon:04:00` | no |
| major_engine_version | Database MAJOR engine version, depends on engine type | string | `` | no |
| max_allocated_storage | The upper limit to which RDS can automatically scale the storage in GBs | number | `0` | no |
| multi_az | Set to true if multi AZ deployment must be supported | bool | `false` | no |
| name | Solution name, e.g. 'app' or 'jenkins' | string | `` | no |
| namespace | Namespace, which could be your organization name or abbreviation, e.g. 'eg' or 'cp' | string | `` | no |
| option_group_name | Name of the DB option group to associate | string | `` | no |
| parameter_group_name | Name of the DB parameter group to associate | string | `` | no |
| performance_insights_enabled | Specifies whether Performance Insights are enabled. | bool | `false` | no |
| performance_insights_kms_key_id | The ARN for the KMS key to encrypt Performance Insights data. Once KMS key is set, it can never be changed. | string | `null` | no |
| performance_insights_retention_period | The amount of time in days to retain Performance Insights data. Either 7 (7 days) or 731 (2 years). | number | `7` | no |
| publicly_accessible | Determines if database can be publicly available (NOT recommended) | bool | `false` | no |
| security_group_ids | The IDs of the security groups from which to allow `ingress` traffic to the DB instance | list(string) | `<list>` | no |
| skip_final_snapshot | If true (default), no snapshot will be made before deleting DB | bool | `true` | no |
| snapshot_identifier | Snapshot identifier e.g: rds:production-2019-06-26-06-05. If specified, the module create cluster from the snapshot | string | `` | no |
| stage | Stage, e.g. 'prod', 'staging', 'dev', OR 'source', 'build', 'test', 'deploy', 'release' | string | `` | no |
| storage_encrypted | (Optional) Specifies whether the DB instance is encrypted. The default is false if not specified | bool | `false` | no |
| storage_type | One of 'standard' (magnetic), 'gp2' (general purpose SSD), or 'io1' (provisioned IOPS SSD) | string | `standard` | no |
| subnet_ids | List of subnets for the DB | list(string) | - | yes |
| tags | Additional tags (e.g. `map('BusinessUnit','XYZ')` | map(string) | `<map>` | no |
| vpc_id | VPC ID the DB instance will be created in | string | - | yes |

## Outputs

| Name | Description |
|------|-------------|
| hostname | DNS host name of the instance |
| instance_address | Address of the instance |
| instance_arn | ARN of the instance |
| instance_endpoint | DNS Endpoint of the instance |
| instance_id | ID of the instance |
| option_group_id | ID of the Option Group |
| parameter_group_id | ID of the Parameter Group |
| security_group_id | ID of the Security Group |
| subnet_group_id | ID of the Subnet Group |




## Share the Love 

Like this project? Please give it a ★ on [our GitHub](https://github.com/cloudposse/terraform-aws-rds)! (it helps us **a lot**) 

Are you using this project or any of our other projects? Consider [leaving a testimonial][testimonial]. =)


## Related Projects

Check out these related projects.

- [terraform-aws-rds-cluster](https://github.com/cloudposse/terraform-aws-rds-cluster) - Terraform module to provision an RDS Aurora cluster for MySQL or Postgres
- [terraform-aws-rds-cloudwatch-sns-alarms](https://github.com/cloudposse/terraform-aws-rds-cloudwatch-sns-alarms) - Terraform module that configures important RDS alerts using CloudWatch and sends them to an SNS topic



## Help

**Got a question?** We got answers. 

File a GitHub [issue](https://github.com/cloudposse/terraform-aws-rds/issues), send us an [email][email] or join our [Slack Community][slack].

[![README Commercial Support][readme_commercial_support_img]][readme_commercial_support_link]

## DevOps Accelerator for Startups


We are a [**DevOps Accelerator**][commercial_support]. We'll help you build your cloud infrastructure from the ground up so you can own it. Then we'll show you how to operate it and stick around for as long as you need us. 

[![Learn More](https://img.shields.io/badge/learn%20more-success.svg?style=for-the-badge)][commercial_support]

Work directly with our team of DevOps experts via email, slack, and video conferencing.

We deliver 10x the value for a fraction of the cost of a full-time engineer. Our track record is not even funny. If you want things done right and you need it done FAST, then we're your best bet.

- **Reference Architecture.** You'll get everything you need from the ground up built using 100% infrastructure as code.
- **Release Engineering.** You'll have end-to-end CI/CD with unlimited staging environments.
- **Site Reliability Engineering.** You'll have total visibility into your apps and microservices.
- **Security Baseline.** You'll have built-in governance with accountability and audit logs for all changes.
- **GitOps.** You'll be able to operate your infrastructure via Pull Requests.
- **Training.** You'll receive hands-on training so your team can operate what we build.
- **Questions.** You'll have a direct line of communication between our teams via a Shared Slack channel.
- **Troubleshooting.** You'll get help to triage when things aren't working.
- **Code Reviews.** You'll receive constructive feedback on Pull Requests.
- **Bug Fixes.** We'll rapidly work with you to fix any bugs in our projects.

## Slack Community

Join our [Open Source Community][slack] on Slack. It's **FREE** for everyone! Our "SweetOps" community is where you get to talk with others who share a similar vision for how to rollout and manage infrastructure. This is the best place to talk shop, ask questions, solicit feedback, and work together as a community to build totally *sweet* infrastructure.

## Newsletter

Sign up for [our newsletter][newsletter] that covers everything on our technology radar.  Receive updates on what we're up to on GitHub as well as awesome new projects we discover. 

## Office Hours

[Join us every Wednesday via Zoom][office_hours] for our weekly "Lunch & Learn" sessions. It's **FREE** for everyone! 

[![zoom](https://img.cloudposse.com/fit-in/200x200/https://cloudposse.com/wp-content/uploads/2019/08/Powered-by-Zoom.png")][office_hours]

## Contributing

### Bug Reports & Feature Requests

Please use the [issue tracker](https://github.com/cloudposse/terraform-aws-rds/issues) to report any bugs or file feature requests.

### Developing

If you are interested in being a contributor and want to get involved in developing this project or [help out](https://cpco.io/help-out) with our other projects, we would love to hear from you! Shoot us an [email][email].

In general, PRs are welcome. We follow the typical "fork-and-pull" Git workflow.

 1. **Fork** the repo on GitHub
 2. **Clone** the project to your own machine
 3. **Commit** changes to your own branch
 4. **Push** your work back up to your fork
 5. Submit a **Pull Request** so that we can review your changes

**NOTE:** Be sure to merge the latest changes from "upstream" before making a pull request!


## Copyright

Copyright © 2017-2020 [Cloud Posse, LLC](https://cpco.io/copyright)



## License 

[![License](https://img.shields.io/badge/License-Apache%202.0-blue.svg)](https://opensource.org/licenses/Apache-2.0) 

See [LICENSE](LICENSE) for full details.

    Licensed to the Apache Software Foundation (ASF) under one
    or more contributor license agreements.  See the NOTICE file
    distributed with this work for additional information
    regarding copyright ownership.  The ASF licenses this file
    to you under the Apache License, Version 2.0 (the
    "License"); you may not use this file except in compliance
    with the License.  You may obtain a copy of the License at

      https://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing,
    software distributed under the License is distributed on an
    "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
    KIND, either express or implied.  See the License for the
    specific language governing permissions and limitations
    under the License.









## Trademarks

All other trademarks referenced herein are the property of their respective owners.

## About

This project is maintained and funded by [Cloud Posse, LLC][website]. Like it? Please let us know by [leaving a testimonial][testimonial]!

[![Cloud Posse][logo]][website]

We're a [DevOps Professional Services][hire] company based in Los Angeles, CA. We ❤️  [Open Source Software][we_love_open_source].

We offer [paid support][commercial_support] on all of our projects.  

Check out [our other projects][github], [follow us on twitter][twitter], [apply for a job][jobs], or [hire us][hire] to help with your cloud strategy and implementation.



### Contributors

|  [![Erik Osterman][osterman_avatar]][osterman_homepage]<br/>[Erik Osterman][osterman_homepage] | [![Andriy Knysh][aknysh_avatar]][aknysh_homepage]<br/>[Andriy Knysh][aknysh_homepage] | [![Sergey Vasilyev][s2504s_avatar]][s2504s_homepage]<br/>[Sergey Vasilyev][s2504s_homepage] | [![Valeriy][drama17_avatar]][drama17_homepage]<br/>[Valeriy][drama17_homepage] | [![Konstantin B][comeanother_avatar]][comeanother_homepage]<br/>[Konstantin B][comeanother_homepage] | [![drmikecrowe][drmikecrowe_avatar]][drmikecrowe_homepage]<br/>[drmikecrowe][drmikecrowe_homepage] | [![Oscar Sullivan][osulli_avatar]][osulli_homepage]<br/>[Oscar Sullivan][osulli_homepage] | [![Federico Márquez][fedemzcor_avatar]][fedemzcor_homepage]<br/>[Federico Márquez][fedemzcor_homepage] |
|---|---|---|---|---|---|---|---|

  [osterman_homepage]: https://github.com/osterman
  [osterman_avatar]: https://img.cloudposse.com/150x150/https://github.com/osterman.png
  [aknysh_homepage]: https://github.com/aknysh
  [aknysh_avatar]: https://img.cloudposse.com/150x150/https://github.com/aknysh.png
  [s2504s_homepage]: https://github.com/s2504s
  [s2504s_avatar]: https://img.cloudposse.com/150x150/https://github.com/s2504s.png
  [drama17_homepage]: https://github.com/drama17
  [drama17_avatar]: https://img.cloudposse.com/150x150/https://github.com/drama17.png
  [comeanother_homepage]: https://github.com/comeanother
  [comeanother_avatar]: https://img.cloudposse.com/150x150/https://github.com/comeanother.png
  [drmikecrowe_homepage]: https://github.com/drmikecrowe
  [drmikecrowe_avatar]: https://img.cloudposse.com/150x150/https://github.com/drmikecrowe.png
  [osulli_homepage]: https://github.com/osulli
  [osulli_avatar]: https://img.cloudposse.com/150x150/https://github.com/osulli.png
  [fedemzcor_homepage]: https://github.com/fedemzcor
  [fedemzcor_avatar]: https://img.cloudposse.com/150x150/https://github.com/fedemzcor.png

[![README Footer][readme_footer_img]][readme_footer_link]
[![Beacon][beacon]][website]

  [logo]: https://cloudposse.com/logo-300x69.svg
  [docs]: https://cpco.io/docs?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-rds&utm_content=docs
  [website]: https://cpco.io/homepage?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-rds&utm_content=website
  [github]: https://cpco.io/github?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-rds&utm_content=github
  [jobs]: https://cpco.io/jobs?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-rds&utm_content=jobs
  [hire]: https://cpco.io/hire?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-rds&utm_content=hire
  [slack]: https://cpco.io/slack?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-rds&utm_content=slack
  [linkedin]: https://cpco.io/linkedin?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-rds&utm_content=linkedin
  [twitter]: https://cpco.io/twitter?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-rds&utm_content=twitter
  [testimonial]: https://cpco.io/leave-testimonial?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-rds&utm_content=testimonial
  [office_hours]: https://cloudposse.com/office-hours?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-rds&utm_content=office_hours
  [newsletter]: https://cpco.io/newsletter?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-rds&utm_content=newsletter
  [email]: https://cpco.io/email?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-rds&utm_content=email
  [commercial_support]: https://cpco.io/commercial-support?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-rds&utm_content=commercial_support
  [we_love_open_source]: https://cpco.io/we-love-open-source?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-rds&utm_content=we_love_open_source
  [terraform_modules]: https://cpco.io/terraform-modules?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-rds&utm_content=terraform_modules
  [readme_header_img]: https://cloudposse.com/readme/header/img
  [readme_header_link]: https://cloudposse.com/readme/header/link?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-rds&utm_content=readme_header_link
  [readme_footer_img]: https://cloudposse.com/readme/footer/img
  [readme_footer_link]: https://cloudposse.com/readme/footer/link?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-rds&utm_content=readme_footer_link
  [readme_commercial_support_img]: https://cloudposse.com/readme/commercial-support/img
  [readme_commercial_support_link]: https://cloudposse.com/readme/commercial-support/link?utm_source=github&utm_medium=readme&utm_campaign=cloudposse/terraform-aws-rds&utm_content=readme_commercial_support_link
  [share_twitter]: https://twitter.com/intent/tweet/?text=terraform-aws-rds&url=https://github.com/cloudposse/terraform-aws-rds
  [share_linkedin]: https://www.linkedin.com/shareArticle?mini=true&title=terraform-aws-rds&url=https://github.com/cloudposse/terraform-aws-rds
  [share_reddit]: https://reddit.com/submit/?url=https://github.com/cloudposse/terraform-aws-rds
  [share_facebook]: https://facebook.com/sharer/sharer.php?u=https://github.com/cloudposse/terraform-aws-rds
  [share_googleplus]: https://plus.google.com/share?url=https://github.com/cloudposse/terraform-aws-rds
  [share_email]: mailto:?subject=terraform-aws-rds&body=https://github.com/cloudposse/terraform-aws-rds
  [beacon]: https://ga-beacon.cloudposse.com/UA-76589703-4/cloudposse/terraform-aws-rds?pixel&cs=github&cm=readme&an=terraform-aws-rds
