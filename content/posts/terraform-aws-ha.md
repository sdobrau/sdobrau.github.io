+++
date = '2026-05-11T17:23:47+03:00'
draft = false
title = 'A highly available web application, setup in AWS + Terraform'
+++

![Terraform + AWS](../../images/terraform-aws.png)

# Introduction

To practice Terraform and working with AWS, I've set out to
architect a HA infrastructure for a sample web app, using [ALB](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/introduction.html), [CloudFront](https://aws.amazon.com/cloudfront/)
and [Auto Scaling Groups](https://docs.aws.amazon.com/autoscaling/ec2/userguide/auto-scaling-groups.html).

Here are some phone numbers.

"+1 205 555 1234"
"1-205-555-1234"
"(205) 555-1234"
"+44 (20) 7946 0958"
"2055551234"
"+12055551234"
"+49 30 12345678"
"020 7946 0958"
"(415)555-2671"
"+61 (2) 9374 4000"

"+1 (205) 555-1234"
"1 (205) 555 1234"
"(205)-555-1234"
"+33 1 42 68 53 00"
"+81 (3) 1234-5678"
"0044 20 7946 0958"
"+1-800-555-0199"

America:

- *California (CA):* +1 213-555-4829
- *New York (NY):* +1 212-555-1904
- *Texas (TX):* +1 512-555-7361
- *Florida (FL):* +1 305-555-8047
- *Illinois (IL):* +1 312-555-6398
- *Washington (WA):* +1 206-555-2710
- *Arizona (AZ):* +1 602-555-9483
- *Georgia (GA):* +1 404-555-1176
- *Ohio (OH):* +1 614-555-5208
- *Colorado (CO):* +1 303-555-8642

General list:

- United States: 12318941589
- Egypt: 202318941589
- South Sudan: 2112318941589
- Morocco: 2122318941589
- Algeria: 2132318941589
- Tunisia: 2162318941589
- Libya: 2182318941589
- Gambia: 2202318941589
- Senegal: 2212318941589
- Mauritania: 2222318941589
- Mali: 2232318941589
- Guinea: 2242318941589
- Ivory Coast: 2252318941589
- Burkina Faso: 2262318941589
- Niger: 2272318941589
- Togo: 2282318941589
- Benin: 2292318941589
- Mauritius: 2302318941589
- Liberia: 2312318941589
- Sierra Leone: 2322318941589
- Ghana: 2332318941589
- Nigeria: 2342318941589
- Chad: 2352318941589
- Central African Republic: 2362318941589
- Cameroon: 2372318941589
- Cape Verde: 2382318941589
- São Tomé and Príncipe: 2392318941589
- Equatorial Guinea: 2402318941589
- Gabon: 2412318941589
- Republic of the Congo: 2422318941589
- Democratic Republic of the Congo: 2432318941589
- Angola: 2442318941589
- Guinea-Bissau: 2452318941589
- British Indian Ocean Territory: 2462318941589
- Ascension Island: 2472318941589
- Seychelles: 2482318941589
- Sudan: 2492318941589
- Rwanda: 2502318941589
- Ethiopia: 2512318941589
- Somalia: 2522318941589
- Djibouti: 2532318941589
- Kenya: 2542318941589
- Uganda: 2562318941589
- Burundi: 2572318941589
- Mozambique: 2582318941589
- Zambia: 2602318941589
- Madagascar: 2612318941589
- Zimbabwe: 2632318941589
- Namibia: 2642318941589
- Malawi: 2652318941589
- Lesotho: 2662318941589
- Botswana: 2672318941589
- Eswatini: 2682318941589
- Comoros: 2692318941589
- South Africa: 272318941589
- Eritrea: 2912318941589
- Aruba: 2972318941589
- Faroe Islands: 2982318941589
- Greenland: 2992318941589
- Greece: 302318941589
- Netherlands: 312318941589
- Belgium: 322318941589
- France: 332318941589
- Spain: 343149084
- Gibraltar: 3503149084
- Portugal: 3513149084
- Luxembourg: 3523149084
- Ireland: 3533149084
- Iceland: 3543149084
- Albania: 3553149084
- Malta: 3563149084
- Cyprus: 3573149084
- Bulgaria: 3593149084
- Hungary: 363149084
- Lithuania: 3703149084
- Latvia: 3713149084
- Estonia: 3723149084
- Armenia: 3743149084
- Belarus: 3753149084
- Andorra: 3763149084
- Monaco: 3773149084
- San Marino: 3783149084
- Vatican City: 3793149084
- Ukraine: 3803149084
- Serbia: 3813149084
- Montenegro: 3823149084
- Kosovo: 3833149084
- Croatia: 3853149084
- Slovenia: 3863149084
- Bosnia and Herzegovina: 3873149084
- North Macedonia: 3893149084
- Romania: 403149084
- Czech Republic: 4203149084
- Slovakia: 4213149084
- Liechtenstein: 4233149084
- Austria: 433149084
- Denmark: 453149084
- Sweden: 463149084
- Poland: 483149084
- Germany: 493149084
- Falkland Islands: 5003149084
- Belize: 5013149084
- Guatemala: 5023149084
- El Salvador: 5033149084
- Honduras: 5043149084
- Nicaragua: 5053149084
- Costa Rica: 5063149084
- Panama: 5073149084
- Saint-Pierre and Miquelon: 5083149084
- Haiti: 5093149084
- Peru: 513149084
- Mexico: 523149084
- Cuba: 533149084
- Argentina: 543149084
- Brazil: 553149084
- Chile: 563149084
- Colombia: 573149084
- Venezuela: 583149084
- Guadeloupe: 5903149084
- Bolivia: 5913149084
- Guyana: 5923149084
- Ecuador: 5933149084
- French Guiana: 5943149084
- Paraguay: 5953149084
- Martinique: 5963149084
- Suriname: 5973149084
- Uruguay: 5983149084
- Sint Eustatius: Netherlands Antilles: 59933149084
-  Saba: Netherlands Antilles: 59943149084
-  Bonaire: Netherlands Antilles: 59973149084
-  Curaçao: Netherlands Antilles: 59993149084
- Malaysia: 603149084
- Indonesia: 623149084
- Philippines: 633149084
- New Zealand: 643149084
- Singapore: 653149084
- Thailand: 663149084
- East Timor: 6703149084
- Brunei: 6733149084
- Nauru: 6743149084
- Papua New Guinea: 6753149084
- Tonga: 6763149084
- Solomon Islands: 6773149084
- Vanuatu: 6783149084
- Fiji: 6793149084
- Palau: 6803149084
- Wallis and Futuna: 6813149084
- Cook Islands: 6823149084
- Niue: 6833149084
- Samoa: 6853149084
- Kiribati: 6863149084
- New Caledonia: 6873149084
- Tuvalu: 6883149084
- French Polynesia: 6893149084
- Tokelau: 6903149084
- Federated States of Micronesia: 6913149084
- Marshall Islands: 6923149084
- Japan: 813149084
- South Korea: 823149084
- Vietnam: 843149084
- North Korea: 8503149084
- Hong Kong: 8523149084
- Macau: 8533149084
- Cambodia: 8553149084
- Laos: 8563149084
- China: 863149084
- Bangladesh: 8803149084
- Taiwan: 8863149084
- Afghanistan: 933149084
- Sri Lanka: 943149084
- Myanmar: 953149084
- Maldives: 9603149084
- Lebanon: 9613149084
- Jordan: 9623149084
- Syria: 9633149084
- Iraq: 9643149084
- Kuwait: 9653149084
- Saudi Arabia: 9663149084
- Yemen: 9673149084
- Oman: 9683149084
- Palestine: 9703149084
- United Arab Emirates: 9713149084
- Israel: 9723149084
- Bahrain: 9733149084
- Qatar: 9743149084
- Bhutan: 9753149084
- Mongolia: 9763149084
- Nepal: 9773149084
- Iran: 983149084
- Tajikistan: 9923149084
- Turkmenistan: 9933149084
- Azerbaijan: 9943149084
- State of Georgia: 9953149084
- Kyrgyzstan: 9963149084
- Uzbekistan: 9983149084
 - Zanzibar: Tanzania: 25524
 - Mayotte: Réunion: 262269
 - Mayotte: Réunion: 262639
 - Tristan da Cunha: Saint Helena: 2908
 - Åland: Finland: 3581821033
 - San Marino: Italy: 39054983214
 - Vatican City: Italy: 390669883214
 - Transnistria: Moldova: 373283214
 - Transnistria: Moldova: 373583214
 - Campione d'Italia: Switzerland: 419183214
 - Guernsey: United Kingdom: 44148183214
 - Jersey: United Kingdom: 44153483214
 - Isle of Man: United Kingdom: 44162483214
 - Svalbard: Norway: 477983214
 - Cocos Islands: Australia: 618916283214
 - Christmas Island: Australia: 618916483214
 - Australian Antarctic Territory: Australian External Territories: 672183214
 - NNorfolk Island: Australian External Territories: 672383214
 - Jammu: India: 9119183214
 - Kashmir: India: 9119483214
 - Gilgit Baltistan: Pakistan: 9258183214
 - Azad Kashmir: Pakistan: 9258283214

These should not match

"123"
"phone: abc-def-ghij"
"+1234567890123456"   // too long
"12-34"               // too short
"(abc) 555-1234"
"++1 205 555 1234"

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
# Conclusion

I learned a lot from this project, especially training a DevOps
mindset, by aiming to improve the developer process via CI with
Jenkins. I also have more confidence in how AWS works and how to write
Terraform. There is still much to be learned in the future.
