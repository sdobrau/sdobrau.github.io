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
- Spain: 34
- Gibraltar: 350
- Portugal: 351
- Luxembourg: 352
- Ireland: 353
- Iceland: 354
- Albania: 355
- Malta: 356
- Cyprus: 357
- Bulgaria: 359
- Hungary: 36
- Lithuania: 370
- Latvia: 371
- Estonia: 372
- Armenia: 374
- Belarus: 375
- Andorra: 376
- Monaco: 377
- San Marino: 378
- Vatican City: 379
- Ukraine: 380
- Serbia: 381
- Montenegro: 382
- Kosovo: 383
- Croatia: 385
- Slovenia: 386
- Bosnia and Herzegovina: 387
- North Macedonia: 389
- Romania: 40
- Czech Republic: 420
- Slovakia: 421
- Liechtenstein: 423
- Austria: 43
- Denmark: 45
- Sweden: 46
- Poland: 48
- Germany: 49
- Falkland Islands: 500
- Belize: 501
- Guatemala: 502
- El Salvador: 503
- Honduras: 504
- Nicaragua: 505
- Costa Rica: 506
- Panama: 507
- Saint-Pierre and Miquelon: 508
- Haiti: 509
- Peru: 51
- Mexico: 52
- Cuba: 53
- Argentina: 54
- Brazil: 55
- Chile: 56
- Colombia: 57
- Venezuela: 58
- Guadeloupe: 590
- Bolivia: 591
- Guyana: 592
- Ecuador: 593
- French Guiana: 594
- Paraguay: 595
- Martinique: 596
- Suriname: 597
- Uruguay: 598
- Sint Eustatius: Netherlands Antilles: 5993
-  Saba: Netherlands Antilles: 5994
-  Bonaire: Netherlands Antilles: 5997
-  Curaçao: Netherlands Antilles: 5999
- Malaysia: 60
- Indonesia: 62
- Philippines: 63
- New Zealand: 64
- Singapore: 65
- Thailand: 66
- East Timor: 670
- Brunei: 673
- Nauru: 674
- Papua New Guinea: 675
- Tonga: 676
- Solomon Islands: 677
- Vanuatu: 678
- Fiji: 679
- Palau: 680
- Wallis and Futuna: 681
- Cook Islands: 682
- Niue: 683
- Samoa: 685
- Kiribati: 686
- New Caledonia: 687
- Tuvalu: 688
- French Polynesia: 689
- Tokelau: 690
- Federated States of Micronesia: 691
- Marshall Islands: 692
- Japan: 81
- South Korea: 82
- Vietnam: 84
- North Korea: 850
- Hong Kong: 852
- Macau: 853
- Cambodia: 855
- Laos: 856
- China: 86
- Bangladesh: 880
- Taiwan: 886
- Afghanistan: 93
- Sri Lanka: 94
- Myanmar: 95
- Maldives: 960
- Lebanon: 961
- Jordan: 962
- Syria: 963
- Iraq: 964
- Kuwait: 965
- Saudi Arabia: 966
- Yemen: 967
- Oman: 968
- Palestine: 970
- United Arab Emirates: 971
- Israel: 972
- Bahrain: 973
- Qatar: 974
- Bhutan: 975
- Mongolia: 976
- Nepal: 977
- Iran: 98
- Tajikistan: 992
- Turkmenistan: 993
- Azerbaijan: 994
- State of Georgia: 995
- Kyrgyzstan: 996
- Uzbekistan: 998
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
 - Norfolk Island: Australian External Territories: 672383214
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
