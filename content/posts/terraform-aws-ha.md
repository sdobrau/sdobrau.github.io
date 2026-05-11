+++
date = '2026-05-11T17:23:47+03:00'
draft = false
title = 'A highly available web application, setup in AWS+Terraform'
+++

![Terraform+AWS](../../images/terraform-aws.png)

# Introduction

To practice Terraform and working with AWS, I've set out to
architect a HA infrastructure for a sample web app, using [ALB](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/introduction.html), [CloudFront](https://aws.amazon.com/cloudfront/)
and [Auto Scaling Groups](https://docs.aws.amazon.com/autoscaling/ec2/userguide/auto-scaling-groups.html).

It could potentially evolve into a larger playground
with other ideas that would require Amazon Lambda, API Gateway
etc. For the moment the focus is VPC setup, IAM policies, ALBs, ASGs
and CloudFront.

The repository can be found [here](https://github.com/sdobrau/terraform-project).

The repository also provides a Jenkinsfile for running various tests on the output
of `terraform plan` and a `Dockerfile` for spinning up the CI environment.

# Features

Here are the main features. Details can be found in the repo's README:

- `ALB + Cloudfront setup`: A highly available `CloudFront` setup backed
  with an `origin group` of 2 internal ALBs (using `VPC origins`), each composed of:
  * [x] An SG allowing only ingress from and egress to =CloudFront=
  * [x] An `autoscaling group` and `target group`
  * [x] Own `private subnets` for the ALBs and private instances, `public
    subnets` for the NAT so instances can communicate with the internet
    behind the NAT
  * [x] Ingress/egress `SG` rules for the private instances to only allow
    ingress traffic from the ALBs egress everywhere
  * [x] `Autoscaling policy` on low/high CPU usage
  * [x] 'Recurring/Scheduled policy' to spin down to 0 at `2AM` and spinup at `6AM`
  * [x] `Launch template` of an `Amazon Linux AMI` coupled with a simple
    `httpd` hello web page as defined in the `user data` file
  * [x] 7 `EBS` snapshots at 24-hour intervals using a `DLM lifecycle policy`

# Jenkins pipeline

The Jenkinsfile provided passes the Terraform configuration
through various tools:

* [x] `Betterleaks` to check for committed secrets
* [x] `ClamAV` for antivirus scanning
* [x] `terraform validate` to validate the configs
* [x] `terraform plan` to further check for any validation errors
* [x] `tflint` to check for code smells, lack of best practices etc.
* [x] `checkov` to check for misconfigurations, no implementation of
  best practices etc.
* [x] `trivy` for further security checks
* [x] `Terratest` for testing that certain properties of the
  infrastructure are correct

# Dockerfile

All the tools mentioned above are run from a Jenkins pipeline, into a
bespoke `Docker` container based on `Alpine`, as specified in `build/Dockerfile`.

```
docker {
            image 'sdobrau/terraform-ci:2026_19_04'
            label 'worker-docker'
            registryUrl 'https://ghcr.io'
            registryCredentialsId 'ghcr_credentials'
            alwaysPull true
        }
```
