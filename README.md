# kittchen_aws_ec2_extensive
This repo contains extensive tests with kitchen fro more the one instance createdin AWS specificaly

# What this repo does

This repo contains terraform code that will create three instances in AWS of the same OS and test if the OS is as expected and if two of the instances are able to reach the third one. Not only that, this ca be applied for two different OS types at the same time. Terraform will create two separate workspaces for each OS so depending on which you would like to test you can specify it in the kitchen test command.There are also tests for the local system's workspac if the `terraform.tfstate` file was created proeprly during test and if terraform outputs are as expected.

# Why use this repo

This repository provides a hands on  experience with kitchen test of terraform. You can use the configuration in this repo as an example to create your own tests with the test-kitchen utility.

# Prerequisites

* AWS Account - must be able to create resources in all regions
* AWS Access Key ID
* AWS Secret Key

  Put both AWS Access Key ID and AWS Secret Key as environment variables for the shel you are using like so
  ```
  export AWS_ACCESS_KEY_ID=XXXXXXXXXXXXXXXX
  export AWS_SECRET_ACCESS_KEY=XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX
  ```
  check if they are now present by typing in `env` command
  
* An SSH key pair - this is actually provided in this repo allready to save some manual work.

**_Use it only during testing out this repo and make sure to delete you resources in AWS when done._**

* Terraform installed

You can download Terraform from this [link](https://www.terraform.io/downloads.html)

* Ruby >= 2.4, < 2.7

Ruby can be installed with `brew install ruby`

* Bundler installed

Bundler can be installed with `gem install bundler`


# How to use this repo

* Download this repository to you local workstation

```
git clone git@github.com:yordanivh/kitchen_aws_ec2_extensive.git
```

* Change to the downloaded repo

```
cd kitchen_aws_ec2_extensive
```

* In the Gemfile you can see that we will use `kitchen-terraform` gem, there we need to download it for this environment with the following command

```
bundle install
```

* Now we should be good to go. We have two platforms set up for testing `redhat` and `ubuntu`. You can see that in the `.kitchen.yml` file. We will test them both at the same time. Terraform will work in separate worskapce so there wont be any problem with that.

* The `kitchen create` command will prepare the terraform workspace and tests for the specified platform, here is and example output below.


```
bundle exec kitchen create redhat
```
```
❯ bundle exec kitchen create redhat
-----> Starting Test Kitchen (v2.4.0)
-----> Creating <extensive-suite-redhat>...
$$$$$$ Verifying the Terraform client version is in the supported interval of >= 0.11.4, < 0.13.0...
$$$$$$ Reading the Terraform client version...
       Terraform v0.11.14
       + provider.aws v1.60.0
       + provider.random v1.3.1
       
       Your version of Terraform is out of date! The latest version
       is 0.12.23. You can update by downloading from www.terraform.io/downloads.html
$$$$$$ Finished reading the Terraform client version.
$$$$$$ Finished verifying the Terraform client version.
$$$$$$ Initializing the Terraform working directory...
       Upgrading modules...
       - module.extensive_kitchen_terraform
         Updating source "../../../"
       
       Initializing provider plugins...
       - Checking for available provider plugins on https://releases.hashicorp.com...
       - Downloading plugin for provider "aws" (1.60.0)...
       - Downloading plugin for provider "random" (1.3.1)...
       
       Terraform has been successfully initialized!
$$$$$$ Finished initializing the Terraform working directory.
$$$$$$ Creating the kitchen-terraform-extensive-suite-redhat Terraform workspace...
       Created and switched to workspace "kitchen-terraform-extensive-suite-redhat"!
       
       You're now on a new, empty workspace. Workspaces isolate their state,
       so if you run "terraform plan" Terraform will not see any existing state
       for this configuration.
$$$$$$ Finished creating the kitchen-terraform-extensive-suite-redhat Terraform workspace.
       Finished creating <extensive-suite-redhat> (0m15.87s).
-----> Test Kitchen is finished. (0m16.76s)
```
#
```
bundle exec kitchen create ubuntu
```
```
❯ bundle exec kitchen create ubuntu
-----> Starting Test Kitchen (v2.4.0)
-----> Creating <extensive-suite-ubuntu>...
$$$$$$ Verifying the Terraform client version is in the supported interval of >= 0.11.4, < 0.13.0...
$$$$$$ Reading the Terraform client version...
       Terraform v0.11.14
       + provider.aws v1.60.0
       + provider.random v1.3.1
       
       Your version of Terraform is out of date! The latest version
       is 0.12.23. You can update by downloading from www.terraform.io/downloads.html
$$$$$$ Finished reading the Terraform client version.
$$$$$$ Finished verifying the Terraform client version.
$$$$$$ Initializing the Terraform working directory...
       Upgrading modules...
       - module.extensive_kitchen_terraform
         Updating source "../../../"
       
       Initializing provider plugins...
       - Checking for available provider plugins on https://releases.hashicorp.com...
       - Downloading plugin for provider "aws" (1.60.0)...
       - Downloading plugin for provider "random" (1.3.1)...
       
       Terraform has been successfully initialized!
$$$$$$ Finished initializing the Terraform working directory.
$$$$$$ Creating the kitchen-terraform-extensive-suite-ubuntu Terraform workspace...
       Created and switched to workspace "kitchen-terraform-extensive-suite-ubuntu"!
       
       You're now on a new, empty workspace. Workspaces isolate their state,
       so if you run "terraform plan" Terraform will not see any existing state
       for this configuration.
$$$$$$ Finished creating the kitchen-terraform-extensive-suite-ubuntu Terraform workspace.
       Finished creating <extensive-suite-ubuntu> (0m9.39s).
-----> Test Kitchen is finished. (0m10.06s)
```

* The `kitchen converge` command will create the resources and leave them running so that they can be continuely tested after any changes made to them. Here is a sample output to these commands.



```
bundle exec kitchen converge redhat
```
```
❯ bundle exec kitchen converge redhat
-----> Starting Test Kitchen (v2.4.0)
-----> Converging <extensive-suite-redhat>...
$$$$$$ Verifying the Terraform client version is in the supported interval of >= 0.11.4, < 0.13.0...
$$$$$$ Reading the Terraform client version...
       Terraform v0.11.14
       + provider.aws v1.60.0
       + provider.random v1.3.1
       
       Your version of Terraform is out of date! The latest version
       is 0.12.23. You can update by downloading from www.terraform.io/downloads.html
$$$$$$ Finished reading the Terraform client version.
$$$$$$ Finished verifying the Terraform client version.
$$$$$$ Selecting the kitchen-terraform-extensive-suite-redhat Terraform workspace...
       Switched to workspace "kitchen-terraform-extensive-suite-redhat".
$$$$$$ Finished selecting the kitchen-terraform-extensive-suite-redhat Terraform workspace.
$$$$$$ Downloading the modules needed for the Terraform configuration...
       - module.extensive_kitchen_terraform
         Updating source "../../../"
$$$$$$ Finished downloading the modules needed for the Terraform configuration.
$$$$$$ Validating the Terraform configuration files...
$$$$$$ Finished validating the Terraform configuration files.
$$$$$$ Building the infrastructure based on the Terraform configuration...
       module.extensive_kitchen_terraform.random_string.key_name: Creating...
         length:      "" => "9"
         lower:       "" => "true"
         min_lower:   "" => "0"
         min_numeric: "" => "0"
         min_special: "" => "0"
         min_upper:   "" => "0"
         number:      "" => "true"
         result:      "" => "<computed>"
         special:     "" => "false"
         upper:       "" => "true"
       module.extensive_kitchen_terraform.random_string.key_name: Creation complete after 0s (ID: hervjBaOn)
       module.extensive_kitchen_terraform.aws_key_pair.extensive_tutorial: Creating...
         fingerprint: "" => "<computed>"
         key_name:    "" => "kitchen-terraform-hervjBaOn"
         public_key:  "" => "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQChW+NcPRrHMNdh9EekgS9Z1wXl0nxz2ZMmcsyrhxfHcCjJQkGvgDcjTZmz8s90feZUXsjvDyqa60Ie2oW8T6LtzSAy1Qv0aJ5ki2t0MUBMl5XVSRIuQxZb23M/SM8E60WX06jmBswqM8wTVMsgmpG/8KVuhR9T4cBJnMsW7iSZu8IElYekdwyv7bnTNPyDfuXnJnWAZN4RCimJD6dE/wygst/k1RlyLdruh504txUAjqtnmtHlQxfqz7SGjiBK1W/TxCqYSy5rXAtL9AQGQnSwcaIa/g5Eha6V+yO88dn49alhC8KNTZhF7z7WqpMIWrTVc+sRQ+7NDCpEMsLVaajSekJhuEpcmIr59UEqYBmOGcdWjmrMq9NbhEG3QmmpHj01ShrwtlqZKEuBzWseWIUY7mRVEkv0flUnqfQ4rUxYVUfyA28bZ79oZM5CAhGIIiKDmTWZwGO9bzP5TxtUp96q5QFB4wfq+aiMUjY2NrfB3oSbRWawVMdYcxegDJmc8tmueYoTG938xKJxHErqQaDPgZxSjc72yEkg7sGYjOs9DhApm8uhfg0ez1xeAaaXWuFJethOkHQXckno/yjlB23hhLz/xJbfkqkTu4dGsYNAt7kMTPMJt0e1xd5ZWoX85Eb03lOst4sLglADAyFKHdR5dxj/cbxWXKQOtqxuzjMspw== Kitchen-Terraform AWS provider tutorial"
       module.extensive_kitchen_terraform.aws_vpc.extensive_tutorial: Creating...
         arn:                              "" => "<computed>"
         assign_generated_ipv6_cidr_block: "" => "false"
         cidr_block:                       "" => "192.168.0.0/16"
         default_network_acl_id:           "" => "<computed>"
         default_route_table_id:           "" => "<computed>"
         default_security_group_id:        "" => "<computed>"
         dhcp_options_id:                  "" => "<computed>"
         enable_classiclink:               "" => "<computed>"
         enable_classiclink_dns_support:   "" => "<computed>"
         enable_dns_hostnames:             "" => "true"
         enable_dns_support:               "" => "true"
         instance_tenancy:                 "" => "default"
         ipv6_association_id:              "" => "<computed>"
         ipv6_cidr_block:                  "" => "<computed>"
         main_route_table_id:              "" => "<computed>"
         owner_id:                         "" => "<computed>"
         tags.%:                           "" => "1"
         tags.Name:                        "" => "kitchen_terraform_extensive_tutorial"
       module.extensive_kitchen_terraform.aws_key_pair.extensive_tutorial: Creation complete after 1s (ID: kitchen-terraform-hervjBaOn)
       module.extensive_kitchen_terraform.aws_vpc.extensive_tutorial: Creation complete after 9s (ID: vpc-0db64b659a3096c14)
       module.extensive_kitchen_terraform.aws_internet_gateway.extensive_tutorial: Creating...
         owner_id:  "" => "<computed>"
         tags.%:    "0" => "1"
         tags.Name: "" => "kitchen_terraform_extensive_tutorial"
         vpc_id:    "" => "vpc-0db64b659a3096c14"
       module.extensive_kitchen_terraform.aws_subnet.extensive_tutorial: Creating...
         arn:                             "" => "<computed>"
         assign_ipv6_address_on_creation: "" => "false"
         availability_zone:               "" => "us-east-2a"
         availability_zone_id:            "" => "<computed>"
         cidr_block:                      "" => "192.168.1.0/24"
         ipv6_cidr_block:                 "" => "<computed>"
         ipv6_cidr_block_association_id:  "" => "<computed>"
         map_public_ip_on_launch:         "" => "true"
         owner_id:                        "" => "<computed>"
         tags.%:                          "" => "1"
         tags.Name:                       "" => "kitchen_terraform_extensive_tutorial"
         vpc_id:                          "" => "vpc-0db64b659a3096c14"
       module.extensive_kitchen_terraform.aws_security_group.extensive_tutorial: Creating...
         arn:                                  "" => "<computed>"
         description:                          "" => "Allow all inbound traffic"
         egress.#:                             "" => "1"
         egress.482069346.cidr_blocks.#:       "" => "1"
         egress.482069346.cidr_blocks.0:       "" => "0.0.0.0/0"
         egress.482069346.description:         "" => ""
         egress.482069346.from_port:           "" => "0"
         egress.482069346.ipv6_cidr_blocks.#:  "" => "0"
         egress.482069346.prefix_list_ids.#:   "" => "0"
         egress.482069346.protocol:            "" => "-1"
         egress.482069346.security_groups.#:   "" => "0"
         egress.482069346.self:                "" => "false"
         egress.482069346.to_port:             "" => "0"
         ingress.#:                            "" => "1"
         ingress.482069346.cidr_blocks.#:      "" => "1"
         ingress.482069346.cidr_blocks.0:      "" => "0.0.0.0/0"
         ingress.482069346.description:        "" => ""
         ingress.482069346.from_port:          "" => "0"
         ingress.482069346.ipv6_cidr_blocks.#: "" => "0"
         ingress.482069346.prefix_list_ids.#:  "" => "0"
         ingress.482069346.protocol:           "" => "-1"
         ingress.482069346.security_groups.#:  "" => "0"
         ingress.482069346.self:               "" => "false"
         ingress.482069346.to_port:            "" => "0"
         name:                                 "" => "kitchen-terraform-extensive_tutorial"
         owner_id:                             "" => "<computed>"
         revoke_rules_on_delete:               "" => "false"
         tags.%:                               "" => "2"
         tags.Name:                            "" => "kitchen-terraform-extensive_tutorial"
         tags.Terraform:                       "" => "true"
         vpc_id:                               "" => "vpc-0db64b659a3096c14"
       module.extensive_kitchen_terraform.aws_subnet.extensive_tutorial: Creation complete after 3s (ID: subnet-0c74e85c0c034db15)
       module.extensive_kitchen_terraform.aws_internet_gateway.extensive_tutorial: Creation complete after 5s (ID: igw-0a2c8280d58a0e31d)
       module.extensive_kitchen_terraform.aws_route_table.extensive_tutorial: Creating...
         owner_id:                                   "" => "<computed>"
         propagating_vgws.#:                         "" => "<computed>"
         route.#:                                    "" => "1"
         route.1601353840.cidr_block:                "" => "0.0.0.0/0"
         route.1601353840.egress_only_gateway_id:    "" => ""
         route.1601353840.gateway_id:                "" => "igw-0a2c8280d58a0e31d"
         route.1601353840.instance_id:               "" => ""
         route.1601353840.ipv6_cidr_block:           "" => ""
         route.1601353840.nat_gateway_id:            "" => ""
         route.1601353840.network_interface_id:      "" => ""
         route.1601353840.transit_gateway_id:        "" => ""
         route.1601353840.vpc_peering_connection_id: "" => ""
         tags.%:                                     "" => "1"
         tags.Name:                                  "" => "kitchen_terraform_extensive_tutorial"
         vpc_id:                                     "" => "vpc-0db64b659a3096c14"
       module.extensive_kitchen_terraform.aws_security_group.extensive_tutorial: Creation complete after 6s (ID: sg-0fce61c168b7b64e3)
       module.extensive_kitchen_terraform.aws_instance.reachable_other_host: Creating...
         ami:                              "" => "ami-0520e698dd500b1d1"
         arn:                              "" => "<computed>"
         associate_public_ip_address:      "" => "true"
         availability_zone:                "" => "<computed>"
         cpu_core_count:                   "" => "<computed>"
         cpu_threads_per_core:             "" => "<computed>"
         ebs_block_device.#:               "" => "<computed>"
         ephemeral_block_device.#:         "" => "<computed>"
         get_password_data:                "" => "false"
         host_id:                          "" => "<computed>"
         instance_state:                   "" => "<computed>"
         instance_type:                    "" => "t2.micro"
         ipv6_address_count:               "" => "<computed>"
         ipv6_addresses.#:                 "" => "<computed>"
         key_name:                         "" => "kitchen-terraform-hervjBaOn"
         network_interface.#:              "" => "<computed>"
         network_interface_id:             "" => "<computed>"
         password_data:                    "" => "<computed>"
         placement_group:                  "" => "<computed>"
         primary_network_interface_id:     "" => "<computed>"
         private_dns:                      "" => "<computed>"
         private_ip:                       "" => "<computed>"
         public_dns:                       "" => "<computed>"
         public_ip:                        "" => "<computed>"
         root_block_device.#:              "" => "<computed>"
         security_groups.#:                "" => "<computed>"
         source_dest_check:                "" => "true"
         subnet_id:                        "" => "subnet-0c74e85c0c034db15"
         tags.%:                           "" => "2"
         tags.Name:                        "" => "kitchen-terraform-reachable-other-host"
         tags.Terraform:                   "" => "true"
         tenancy:                          "" => "<computed>"
         volume_tags.%:                    "" => "<computed>"
         vpc_security_group_ids.#:         "" => "1"
         vpc_security_group_ids.842436846: "" => "sg-0fce61c168b7b64e3"
       module.extensive_kitchen_terraform.aws_instance.remote_group[0]: Creating...
         ami:                              "" => "ami-0520e698dd500b1d1"
         arn:                              "" => "<computed>"
         associate_public_ip_address:      "" => "<computed>"
         availability_zone:                "" => "<computed>"
         cpu_core_count:                   "" => "<computed>"
         cpu_threads_per_core:             "" => "<computed>"
         ebs_block_device.#:               "" => "<computed>"
         ephemeral_block_device.#:         "" => "<computed>"
         get_password_data:                "" => "false"
         host_id:                          "" => "<computed>"
         instance_state:                   "" => "<computed>"
         instance_type:                    "" => "t2.micro"
         ipv6_address_count:               "" => "<computed>"
         ipv6_addresses.#:                 "" => "<computed>"
         key_name:                         "" => "kitchen-terraform-hervjBaOn"
         network_interface.#:              "" => "<computed>"
         network_interface_id:             "" => "<computed>"
         password_data:                    "" => "<computed>"
         placement_group:                  "" => "<computed>"
         primary_network_interface_id:     "" => "<computed>"
         private_dns:                      "" => "<computed>"
         private_ip:                       "" => "<computed>"
         public_dns:                       "" => "<computed>"
         public_ip:                        "" => "<computed>"
         root_block_device.#:              "" => "<computed>"
         security_groups.#:                "" => "<computed>"
         source_dest_check:                "" => "true"
         subnet_id:                        "" => "subnet-0c74e85c0c034db15"
         tags.%:                           "" => "2"
         tags.Name:                        "" => "kitchen-terraform-test-target-0"
         tags.Terraform:                   "" => "true"
         tenancy:                          "" => "<computed>"
         volume_tags.%:                    "" => "<computed>"
         vpc_security_group_ids.#:         "" => "1"
         vpc_security_group_ids.842436846: "" => "sg-0fce61c168b7b64e3"
       module.extensive_kitchen_terraform.aws_instance.remote_group[1]: Creating...
         ami:                              "" => "ami-0520e698dd500b1d1"
         arn:                              "" => "<computed>"
         associate_public_ip_address:      "" => "<computed>"
         availability_zone:                "" => "<computed>"
         cpu_core_count:                   "" => "<computed>"
         cpu_threads_per_core:             "" => "<computed>"
         ebs_block_device.#:               "" => "<computed>"
         ephemeral_block_device.#:         "" => "<computed>"
         get_password_data:                "" => "false"
         host_id:                          "" => "<computed>"
         instance_state:                   "" => "<computed>"
         instance_type:                    "" => "t2.micro"
         ipv6_address_count:               "" => "<computed>"
         ipv6_addresses.#:                 "" => "<computed>"
         key_name:                         "" => "kitchen-terraform-hervjBaOn"
         network_interface.#:              "" => "<computed>"
         network_interface_id:             "" => "<computed>"
         password_data:                    "" => "<computed>"
         placement_group:                  "" => "<computed>"
         primary_network_interface_id:     "" => "<computed>"
         private_dns:                      "" => "<computed>"
         private_ip:                       "" => "<computed>"
         public_dns:                       "" => "<computed>"
         public_ip:                        "" => "<computed>"
         root_block_device.#:              "" => "<computed>"
         security_groups.#:                "" => "<computed>"
         source_dest_check:                "" => "true"
         subnet_id:                        "" => "subnet-0c74e85c0c034db15"
         tags.%:                           "" => "2"
         tags.Name:                        "" => "kitchen-terraform-test-target-1"
         tags.Terraform:                   "" => "true"
         tenancy:                          "" => "<computed>"
         volume_tags.%:                    "" => "<computed>"
         vpc_security_group_ids.#:         "" => "1"
         vpc_security_group_ids.842436846: "" => "sg-0fce61c168b7b64e3"
       module.extensive_kitchen_terraform.aws_route_table.extensive_tutorial: Creation complete after 3s (ID: rtb-057ca26cfc76d3a2b)
       module.extensive_kitchen_terraform.aws_route_table_association.extensive_tutorial: Creating...
         route_table_id: "" => "rtb-057ca26cfc76d3a2b"
         subnet_id:      "" => "subnet-0c74e85c0c034db15"
       module.extensive_kitchen_terraform.aws_route_table_association.extensive_tutorial: Creation complete after 0s (ID: rtbassoc-01c46e2eccb0184bd)
       module.extensive_kitchen_terraform.aws_instance.reachable_other_host: Still creating... (10s elapsed)
       module.extensive_kitchen_terraform.aws_instance.remote_group.0: Still creating... (10s elapsed)
       module.extensive_kitchen_terraform.aws_instance.remote_group.1: Still creating... (10s elapsed)
       module.extensive_kitchen_terraform.aws_instance.remote_group.0: Still creating... (20s elapsed)
       module.extensive_kitchen_terraform.aws_instance.reachable_other_host: Still creating... (20s elapsed)
       module.extensive_kitchen_terraform.aws_instance.remote_group.1: Still creating... (20s elapsed)
       module.extensive_kitchen_terraform.aws_instance.remote_group[0]: Creation complete after 27s (ID: i-0d2d8c6c0ce8abb1d)
       module.extensive_kitchen_terraform.aws_instance.reachable_other_host: Creation complete after 27s (ID: i-0e0f9f895128da043)
       module.extensive_kitchen_terraform.aws_instance.remote_group[1]: Creation complete after 28s (ID: i-03067f4757e5fcea1)
       
       Apply complete! Resources: 11 added, 0 changed, 0 destroyed.
       
       Outputs:
       
       reachable_other_host_ip_address = 18.191.24.108
       
       remote_group_public_dns = [
           ec2-3-135-216-41.us-east-2.compute.amazonaws.com,
           ec2-18-189-195-55.us-east-2.compute.amazonaws.com
       ]
       static_terraform_output = static terraform output
       terraform_state = /Users/yhalachev/repos/kittchen_aws_ec2_extensive/test/fixtures/wrapper/terraform.tfstate.d/kitchen-terraform-extensive-suite-redhat/terraform.tfstate
$$$$$$ Finished building the infrastructure based on the Terraform configuration.
$$$$$$ Reading the output variables from the Terraform state...
$$$$$$ Finished reading the output variables from the Terraform state.
$$$$$$ Parsing the Terraform output variables as JSON...
$$$$$$ Finished parsing the Terraform output variables as JSON.
$$$$$$ Writing the output variables to the Kitchen instance state...
$$$$$$ Finished writing the output varibales to the Kitchen instance state.
$$$$$$ Writing the input variables to the Kitchen instance state...
$$$$$$ Finished writing the input variables to the Kitchen instance state.
       Finished converging <extensive-suite-redhat> (0m50.53s).
-----> Test Kitchen is finished. (0m51.19s)
```
#
```
bundle exec kitchen converge ubuntu
```

```
❯ bundle exec kitchen converge ubuntu
-----> Starting Test Kitchen (v2.4.0)
-----> Converging <extensive-suite-ubuntu>...
$$$$$$ Verifying the Terraform client version is in the supported interval of >= 0.11.4, < 0.13.0...
$$$$$$ Reading the Terraform client version...
       Terraform v0.11.14
       + provider.aws v1.60.0
       + provider.random v1.3.1
       
       Your version of Terraform is out of date! The latest version
       is 0.12.23. You can update by downloading from www.terraform.io/downloads.html
$$$$$$ Finished reading the Terraform client version.
$$$$$$ Finished verifying the Terraform client version.
$$$$$$ Selecting the kitchen-terraform-extensive-suite-ubuntu Terraform workspace...
       Switched to workspace "kitchen-terraform-extensive-suite-ubuntu".
$$$$$$ Finished selecting the kitchen-terraform-extensive-suite-ubuntu Terraform workspace.
$$$$$$ Downloading the modules needed for the Terraform configuration...
       - module.extensive_kitchen_terraform
         Updating source "../../../"
$$$$$$ Finished downloading the modules needed for the Terraform configuration.
$$$$$$ Validating the Terraform configuration files...
$$$$$$ Finished validating the Terraform configuration files.
$$$$$$ Building the infrastructure based on the Terraform configuration...
       module.extensive_kitchen_terraform.random_string.key_name: Creating...
         length:      "" => "9"
         lower:       "" => "true"
         min_lower:   "" => "0"
         min_numeric: "" => "0"
         min_special: "" => "0"
         min_upper:   "" => "0"
         number:      "" => "true"
         result:      "" => "<computed>"
         special:     "" => "false"
         upper:       "" => "true"
       module.extensive_kitchen_terraform.random_string.key_name: Creation complete after 0s (ID: rgor5r65n)
       module.extensive_kitchen_terraform.aws_key_pair.extensive_tutorial: Creating...
         fingerprint: "" => "<computed>"
         key_name:    "" => "kitchen-terraform-rgor5r65n"
         public_key:  "" => "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAACAQChW+NcPRrHMNdh9EekgS9Z1wXl0nxz2ZMmcsyrhxfHcCjJQkGvgDcjTZmz8s90feZUXsjvDyqa60Ie2oW8T6LtzSAy1Qv0aJ5ki2t0MUBMl5XVSRIuQxZb23M/SM8E60WX06jmBswqM8wTVMsgmpG/8KVuhR9T4cBJnMsW7iSZu8IElYekdwyv7bnTNPyDfuXnJnWAZN4RCimJD6dE/wygst/k1RlyLdruh504txUAjqtnmtHlQxfqz7SGjiBK1W/TxCqYSy5rXAtL9AQGQnSwcaIa/g5Eha6V+yO88dn49alhC8KNTZhF7z7WqpMIWrTVc+sRQ+7NDCpEMsLVaajSekJhuEpcmIr59UEqYBmOGcdWjmrMq9NbhEG3QmmpHj01ShrwtlqZKEuBzWseWIUY7mRVEkv0flUnqfQ4rUxYVUfyA28bZ79oZM5CAhGIIiKDmTWZwGO9bzP5TxtUp96q5QFB4wfq+aiMUjY2NrfB3oSbRWawVMdYcxegDJmc8tmueYoTG938xKJxHErqQaDPgZxSjc72yEkg7sGYjOs9DhApm8uhfg0ez1xeAaaXWuFJethOkHQXckno/yjlB23hhLz/xJbfkqkTu4dGsYNAt7kMTPMJt0e1xd5ZWoX85Eb03lOst4sLglADAyFKHdR5dxj/cbxWXKQOtqxuzjMspw== Kitchen-Terraform AWS provider tutorial"
       module.extensive_kitchen_terraform.aws_vpc.extensive_tutorial: Creating...
         arn:                              "" => "<computed>"
         assign_generated_ipv6_cidr_block: "" => "false"
         cidr_block:                       "" => "192.168.0.0/16"
         default_network_acl_id:           "" => "<computed>"
         default_route_table_id:           "" => "<computed>"
         default_security_group_id:        "" => "<computed>"
         dhcp_options_id:                  "" => "<computed>"
         enable_classiclink:               "" => "<computed>"
         enable_classiclink_dns_support:   "" => "<computed>"
         enable_dns_hostnames:             "" => "true"
         enable_dns_support:               "" => "true"
         instance_tenancy:                 "" => "default"
         ipv6_association_id:              "" => "<computed>"
         ipv6_cidr_block:                  "" => "<computed>"
         main_route_table_id:              "" => "<computed>"
         owner_id:                         "" => "<computed>"
         tags.%:                           "" => "1"
         tags.Name:                        "" => "kitchen_terraform_extensive_tutorial"
       module.extensive_kitchen_terraform.aws_key_pair.extensive_tutorial: Creation complete after 1s (ID: kitchen-terraform-rgor5r65n)
       module.extensive_kitchen_terraform.aws_vpc.extensive_tutorial: Still creating... (10s elapsed)
       module.extensive_kitchen_terraform.aws_vpc.extensive_tutorial: Creation complete after 12s (ID: vpc-0252c9ec94116fa98)
       module.extensive_kitchen_terraform.aws_internet_gateway.extensive_tutorial: Creating...
         owner_id:  "" => "<computed>"
         tags.%:    "0" => "1"
         tags.Name: "" => "kitchen_terraform_extensive_tutorial"
         vpc_id:    "" => "vpc-0252c9ec94116fa98"
       module.extensive_kitchen_terraform.aws_subnet.extensive_tutorial: Creating...
         arn:                             "" => "<computed>"
         assign_ipv6_address_on_creation: "" => "false"
         availability_zone:               "" => "us-west-2b"
         availability_zone_id:            "" => "<computed>"
         cidr_block:                      "" => "192.168.1.0/24"
         ipv6_cidr_block:                 "" => "<computed>"
         ipv6_cidr_block_association_id:  "" => "<computed>"
         map_public_ip_on_launch:         "" => "true"
         owner_id:                        "" => "<computed>"
         tags.%:                          "" => "1"
         tags.Name:                       "" => "kitchen_terraform_extensive_tutorial"
         vpc_id:                          "" => "vpc-0252c9ec94116fa98"
       module.extensive_kitchen_terraform.aws_security_group.extensive_tutorial: Creating...
         arn:                                  "" => "<computed>"
         description:                          "" => "Allow all inbound traffic"
         egress.#:                             "" => "1"
         egress.482069346.cidr_blocks.#:       "" => "1"
         egress.482069346.cidr_blocks.0:       "" => "0.0.0.0/0"
         egress.482069346.description:         "" => ""
         egress.482069346.from_port:           "" => "0"
         egress.482069346.ipv6_cidr_blocks.#:  "" => "0"
         egress.482069346.prefix_list_ids.#:   "" => "0"
         egress.482069346.protocol:            "" => "-1"
         egress.482069346.security_groups.#:   "" => "0"
         egress.482069346.self:                "" => "false"
         egress.482069346.to_port:             "" => "0"
         ingress.#:                            "" => "1"
         ingress.482069346.cidr_blocks.#:      "" => "1"
         ingress.482069346.cidr_blocks.0:      "" => "0.0.0.0/0"
         ingress.482069346.description:        "" => ""
         ingress.482069346.from_port:          "" => "0"
         ingress.482069346.ipv6_cidr_blocks.#: "" => "0"
         ingress.482069346.prefix_list_ids.#:  "" => "0"
         ingress.482069346.protocol:           "" => "-1"
         ingress.482069346.security_groups.#:  "" => "0"
         ingress.482069346.self:               "" => "false"
         ingress.482069346.to_port:            "" => "0"
         name:                                 "" => "kitchen-terraform-extensive_tutorial"
         owner_id:                             "" => "<computed>"
         revoke_rules_on_delete:               "" => "false"
         tags.%:                               "" => "2"
         tags.Name:                            "" => "kitchen-terraform-extensive_tutorial"
         tags.Terraform:                       "" => "true"
         vpc_id:                               "" => "vpc-0252c9ec94116fa98"
       module.extensive_kitchen_terraform.aws_subnet.extensive_tutorial: Creation complete after 5s (ID: subnet-0732780af08f2b7eb)
       module.extensive_kitchen_terraform.aws_internet_gateway.extensive_tutorial: Creation complete after 6s (ID: igw-09bb93f51bf6be275)
       module.extensive_kitchen_terraform.aws_route_table.extensive_tutorial: Creating...
         owner_id:                                   "" => "<computed>"
         propagating_vgws.#:                         "" => "<computed>"
         route.#:                                    "" => "1"
         route.2262668096.cidr_block:                "" => "0.0.0.0/0"
         route.2262668096.egress_only_gateway_id:    "" => ""
         route.2262668096.gateway_id:                "" => "igw-09bb93f51bf6be275"
         route.2262668096.instance_id:               "" => ""
         route.2262668096.ipv6_cidr_block:           "" => ""
         route.2262668096.nat_gateway_id:            "" => ""
         route.2262668096.network_interface_id:      "" => ""
         route.2262668096.transit_gateway_id:        "" => ""
         route.2262668096.vpc_peering_connection_id: "" => ""
         tags.%:                                     "" => "1"
         tags.Name:                                  "" => "kitchen_terraform_extensive_tutorial"
         vpc_id:                                     "" => "vpc-0252c9ec94116fa98"
       module.extensive_kitchen_terraform.aws_security_group.extensive_tutorial: Creation complete after 9s (ID: sg-0fc29d53757c811cb)
       module.extensive_kitchen_terraform.aws_instance.remote_group[1]: Creating...
         ami:                               "" => "ami-0d1cd67c26f5fca19"
         arn:                               "" => "<computed>"
         associate_public_ip_address:       "" => "<computed>"
         availability_zone:                 "" => "<computed>"
         cpu_core_count:                    "" => "<computed>"
         cpu_threads_per_core:              "" => "<computed>"
         ebs_block_device.#:                "" => "<computed>"
         ephemeral_block_device.#:          "" => "<computed>"
         get_password_data:                 "" => "false"
         host_id:                           "" => "<computed>"
         instance_state:                    "" => "<computed>"
         instance_type:                     "" => "t2.micro"
         ipv6_address_count:                "" => "<computed>"
         ipv6_addresses.#:                  "" => "<computed>"
         key_name:                          "" => "kitchen-terraform-rgor5r65n"
         network_interface.#:               "" => "<computed>"
         network_interface_id:              "" => "<computed>"
         password_data:                     "" => "<computed>"
         placement_group:                   "" => "<computed>"
         primary_network_interface_id:      "" => "<computed>"
         private_dns:                       "" => "<computed>"
         private_ip:                        "" => "<computed>"
         public_dns:                        "" => "<computed>"
         public_ip:                         "" => "<computed>"
         root_block_device.#:               "" => "<computed>"
         security_groups.#:                 "" => "<computed>"
         source_dest_check:                 "" => "true"
         subnet_id:                         "" => "subnet-0732780af08f2b7eb"
         tags.%:                            "" => "2"
         tags.Name:                         "" => "kitchen-terraform-test-target-1"
         tags.Terraform:                    "" => "true"
         tenancy:                           "" => "<computed>"
         volume_tags.%:                     "" => "<computed>"
         vpc_security_group_ids.#:          "" => "1"
         vpc_security_group_ids.2929961087: "" => "sg-0fc29d53757c811cb"
       module.extensive_kitchen_terraform.aws_instance.reachable_other_host: Creating...
         ami:                               "" => "ami-0d1cd67c26f5fca19"
         arn:                               "" => "<computed>"
         associate_public_ip_address:       "" => "true"
         availability_zone:                 "" => "<computed>"
         cpu_core_count:                    "" => "<computed>"
         cpu_threads_per_core:              "" => "<computed>"
         ebs_block_device.#:                "" => "<computed>"
         ephemeral_block_device.#:          "" => "<computed>"
         get_password_data:                 "" => "false"
         host_id:                           "" => "<computed>"
         instance_state:                    "" => "<computed>"
         instance_type:                     "" => "t2.micro"
         ipv6_address_count:                "" => "<computed>"
         ipv6_addresses.#:                  "" => "<computed>"
         key_name:                          "" => "kitchen-terraform-rgor5r65n"
         network_interface.#:               "" => "<computed>"
         network_interface_id:              "" => "<computed>"
         password_data:                     "" => "<computed>"
         placement_group:                   "" => "<computed>"
         primary_network_interface_id:      "" => "<computed>"
         private_dns:                       "" => "<computed>"
         private_ip:                        "" => "<computed>"
         public_dns:                        "" => "<computed>"
         public_ip:                         "" => "<computed>"
         root_block_device.#:               "" => "<computed>"
         security_groups.#:                 "" => "<computed>"
         source_dest_check:                 "" => "true"
         subnet_id:                         "" => "subnet-0732780af08f2b7eb"
         tags.%:                            "" => "2"
         tags.Name:                         "" => "kitchen-terraform-reachable-other-host"
         tags.Terraform:                    "" => "true"
         tenancy:                           "" => "<computed>"
         volume_tags.%:                     "" => "<computed>"
         vpc_security_group_ids.#:          "" => "1"
         vpc_security_group_ids.2929961087: "" => "sg-0fc29d53757c811cb"
       module.extensive_kitchen_terraform.aws_instance.remote_group[0]: Creating...
         ami:                               "" => "ami-0d1cd67c26f5fca19"
         arn:                               "" => "<computed>"
         associate_public_ip_address:       "" => "<computed>"
         availability_zone:                 "" => "<computed>"
         cpu_core_count:                    "" => "<computed>"
         cpu_threads_per_core:              "" => "<computed>"
         ebs_block_device.#:                "" => "<computed>"
         ephemeral_block_device.#:          "" => "<computed>"
         get_password_data:                 "" => "false"
         host_id:                           "" => "<computed>"
         instance_state:                    "" => "<computed>"
         instance_type:                     "" => "t2.micro"
         ipv6_address_count:                "" => "<computed>"
         ipv6_addresses.#:                  "" => "<computed>"
         key_name:                          "" => "kitchen-terraform-rgor5r65n"
         network_interface.#:               "" => "<computed>"
         network_interface_id:              "" => "<computed>"
         password_data:                     "" => "<computed>"
         placement_group:                   "" => "<computed>"
         primary_network_interface_id:      "" => "<computed>"
         private_dns:                       "" => "<computed>"
         private_ip:                        "" => "<computed>"
         public_dns:                        "" => "<computed>"
         public_ip:                         "" => "<computed>"
         root_block_device.#:               "" => "<computed>"
         security_groups.#:                 "" => "<computed>"
         source_dest_check:                 "" => "true"
         subnet_id:                         "" => "subnet-0732780af08f2b7eb"
         tags.%:                            "" => "2"
         tags.Name:                         "" => "kitchen-terraform-test-target-0"
         tags.Terraform:                    "" => "true"
         tenancy:                           "" => "<computed>"
         volume_tags.%:                     "" => "<computed>"
         vpc_security_group_ids.#:          "" => "1"
         vpc_security_group_ids.2929961087: "" => "sg-0fc29d53757c811cb"
       module.extensive_kitchen_terraform.aws_route_table.extensive_tutorial: Creation complete after 4s (ID: rtb-0d289f8d57dfb68b0)
       module.extensive_kitchen_terraform.aws_route_table_association.extensive_tutorial: Creating...
         route_table_id: "" => "rtb-0d289f8d57dfb68b0"
         subnet_id:      "" => "subnet-0732780af08f2b7eb"
       module.extensive_kitchen_terraform.aws_route_table_association.extensive_tutorial: Creation complete after 1s (ID: rtbassoc-0ff5392b10fcfc152)
       module.extensive_kitchen_terraform.aws_instance.remote_group.0: Still creating... (10s elapsed)
       module.extensive_kitchen_terraform.aws_instance.reachable_other_host: Still creating... (10s elapsed)
       module.extensive_kitchen_terraform.aws_instance.remote_group.1: Still creating... (10s elapsed)
       module.extensive_kitchen_terraform.aws_instance.reachable_other_host: Still creating... (20s elapsed)
       module.extensive_kitchen_terraform.aws_instance.remote_group.0: Still creating... (20s elapsed)
       module.extensive_kitchen_terraform.aws_instance.remote_group.1: Still creating... (20s elapsed)
       module.extensive_kitchen_terraform.aws_instance.remote_group.0: Still creating... (30s elapsed)
       module.extensive_kitchen_terraform.aws_instance.remote_group.1: Still creating... (30s elapsed)
       module.extensive_kitchen_terraform.aws_instance.reachable_other_host: Still creating... (30s elapsed)
       module.extensive_kitchen_terraform.aws_instance.remote_group[0]: Creation complete after 31s (ID: i-06013c9a50403d4f7)
       module.extensive_kitchen_terraform.aws_instance.reachable_other_host: Creation complete after 31s (ID: i-01855959085e277ff)
       module.extensive_kitchen_terraform.aws_instance.remote_group[1]: Creation complete after 31s (ID: i-0f902c2d2e0081e2c)
       
       Apply complete! Resources: 11 added, 0 changed, 0 destroyed.
       
       Outputs:
       
       reachable_other_host_ip_address = 34.218.236.119
       
       remote_group_public_dns = [
           ec2-52-25-121-40.us-west-2.compute.amazonaws.com,
           ec2-54-191-164-76.us-west-2.compute.amazonaws.com
       ]
       static_terraform_output = static terraform output
       terraform_state = /Users/yhalachev/repos/kittchen_aws_ec2_extensive/test/fixtures/wrapper/terraform.tfstate.d/kitchen-terraform-extensive-suite-ubuntu/terraform.tfstate
$$$$$$ Finished building the infrastructure based on the Terraform configuration.
$$$$$$ Reading the output variables from the Terraform state...
$$$$$$ Finished reading the output variables from the Terraform state.
$$$$$$ Parsing the Terraform output variables as JSON...
$$$$$$ Finished parsing the Terraform output variables as JSON.
$$$$$$ Writing the output variables to the Kitchen instance state...
$$$$$$ Finished writing the output varibales to the Kitchen instance state.
$$$$$$ Writing the input variables to the Kitchen instance state...
$$$$$$ Finished writing the input variables to the Kitchen instance state.
       Finished converging <extensive-suite-ubuntu> (0m58.89s).
-----> Test Kitchen is finished. (0m59.55s)
```

* The `kitchen verify` command will carry out the tests and will show the results on screen.Here is an example output.



```
bundle exec kitchen verify redhat
```
```
❯ bundle exec kitchen verify redhat
-----> Starting Test Kitchen (v2.4.0)
-----> Setting up <extensive-suite-redhat>...
       Finished setting up <extensive-suite-redhat> (0m0.00s).
-----> Verifying <extensive-suite-redhat>...
$$$$$$ Reading the Terraform input variables from the Kitchen instance state...
$$$$$$ Finished reading the Terraform input variables from the Kitchen instance state.
$$$$$$ Reading the Terraform output variables from the Kitchen instance state...
$$$$$$ Finished reading the Terraform output varibales from the Kitchen instance state.
$$$$$$ Verifying the systems...
$$$$$$ Verifying the 'local' system...

Profile: Extensive Kitchen-Terraform (extensive_suite)
Version: 0.1.0
Target:  local://

  ✔  state_file: 0.11.14
     ✔  0.11.14 is expected to match /\d+\.\d+\.\d+/
  ✔  inspec_attributes: static terraform output
     ✔  static terraform output is expected to eq "static terraform output"
     ✔  static terraform output is expected to eq "static terraform output"


Profile Summary: 2 successful controls, 0 control failures, 0 controls skipped
Test Summary: 3 successful, 0 failures, 0 skipped
$$$$$$ Finished verifying the 'local' system.
$$$$$$ Verifying the 'remote' system...

Profile: Extensive Kitchen-Terraform (extensive_suite)
Version: 0.1.0
Target:  ssh://ec2-user@ec2-3-135-216-41.us-east-2.compute.amazonaws.com:22

  ✔  operating_system: redhat
     ✔  redhat is expected to eq "redhat"
  ✔  reachable_other_host: Host 18.191.24.108

     ✔  Host 18.191.24.108
      is expected to be reachable


Profile Summary: 2 successful controls, 0 control failures, 0 controls skipped
Test Summary: 2 successful, 0 failures, 0 skipped

Profile: Extensive Kitchen-Terraform (extensive_suite)
Version: 0.1.0
Target:  ssh://ec2-user@ec2-18-189-195-55.us-east-2.compute.amazonaws.com:22

  ✔  operating_system: redhat
     ✔  redhat is expected to eq "redhat"
  ✔  reachable_other_host: Host 18.191.24.108

     ✔  Host 18.191.24.108
      is expected to be reachable


Profile Summary: 2 successful controls, 0 control failures, 0 controls skipped
Test Summary: 2 successful, 0 failures, 0 skipped
$$$$$$ Finished verifying the 'remote' system.
$$$$$$ Finished verifying the systems.
       Finished verifying <extensive-suite-redhat> (0m22.58s).
-----> Test Kitchen is finished. (0m23.24s)
```
#
```
bundle exec kitchen verify ubuntu
```

```
❯ bundle exec kitchen verify ubuntu
-----> Starting Test Kitchen (v2.4.0)
-----> Setting up <extensive-suite-ubuntu>...
       Finished setting up <extensive-suite-ubuntu> (0m0.00s).
-----> Verifying <extensive-suite-ubuntu>...
$$$$$$ Reading the Terraform input variables from the Kitchen instance state...
$$$$$$ Finished reading the Terraform input variables from the Kitchen instance state.
$$$$$$ Reading the Terraform output variables from the Kitchen instance state...
$$$$$$ Finished reading the Terraform output varibales from the Kitchen instance state.
$$$$$$ Verifying the systems...
$$$$$$ Verifying the 'local' system...

Profile: Extensive Kitchen-Terraform (extensive_suite)
Version: 0.1.0
Target:  local://

  ✔  state_file: 0.11.14
     ✔  0.11.14 is expected to match /\d+\.\d+\.\d+/
  ✔  inspec_attributes: static terraform output
     ✔  static terraform output is expected to eq "static terraform output"
     ✔  static terraform output is expected to eq "static terraform output"


Profile Summary: 2 successful controls, 0 control failures, 0 controls skipped
Test Summary: 3 successful, 0 failures, 0 skipped
$$$$$$ Finished verifying the 'local' system.
$$$$$$ Verifying the 'remote' system...

Profile: Extensive Kitchen-Terraform (extensive_suite)
Version: 0.1.0
Target:  ssh://ubuntu@ec2-52-25-121-40.us-west-2.compute.amazonaws.com:22

  ✔  operating_system: ubuntu
     ✔  ubuntu is expected to eq "ubuntu"
  ✔  reachable_other_host: Host 34.218.236.119

     ✔  Host 34.218.236.119
      is expected to be reachable


Profile Summary: 2 successful controls, 0 control failures, 0 controls skipped
Test Summary: 2 successful, 0 failures, 0 skipped

Profile: Extensive Kitchen-Terraform (extensive_suite)
Version: 0.1.0
Target:  ssh://ubuntu@ec2-54-191-164-76.us-west-2.compute.amazonaws.com:22

  ✔  operating_system: ubuntu
     ✔  ubuntu is expected to eq "ubuntu"
  ✔  reachable_other_host: Host 34.218.236.119

     ✔  Host 34.218.236.119
      is expected to be reachable


Profile Summary: 2 successful controls, 0 control failures, 0 controls skipped
Test Summary: 2 successful, 0 failures, 0 skipped
$$$$$$ Finished verifying the 'remote' system.
$$$$$$ Finished verifying the systems.
       Finished verifying <extensive-suite-ubuntu> (0m21.43s).
-----> Test Kitchen is finished. (0m22.09s)
```

* The `kitchen destroy` command will conclude the test by destroying the resources in AWS. The step is necessary to avoid any unnecessary billing and resource allocation.



```
bundle exec kitchen destroy redhat
```

```
❯ bundle exec kitchen destroy redhat
-----> Starting Test Kitchen (v2.4.0)
-----> Destroying <extensive-suite-redhat>...
$$$$$$ Verifying the Terraform client version is in the supported interval of >= 0.11.4, < 0.13.0...
$$$$$$ Reading the Terraform client version...
       Terraform v0.11.14
       + provider.aws v1.60.0
       + provider.random v1.3.1
       
       Your version of Terraform is out of date! The latest version
       is 0.12.23. You can update by downloading from www.terraform.io/downloads.html
$$$$$$ Finished reading the Terraform client version.
$$$$$$ Finished verifying the Terraform client version.
$$$$$$ Initializing the Terraform working directory...
       Initializing modules...
       - module.extensive_kitchen_terraform
       
       Initializing provider plugins...
       
       Terraform has been successfully initialized!
$$$$$$ Finished initializing the Terraform working directory.
$$$$$$ Selecting the kitchen-terraform-extensive-suite-redhat Terraform workspace...
       Switched to workspace "kitchen-terraform-extensive-suite-redhat".
$$$$$$ Finished selecting the kitchen-terraform-extensive-suite-redhat Terraform workspace.
$$$$$$ Destroying the Terraform-managed infrastructure...
       random_string.key_name: Refreshing state... (ID: hervjBaOn)
       aws_vpc.extensive_tutorial: Refreshing state... (ID: vpc-0db64b659a3096c14)
       aws_key_pair.extensive_tutorial: Refreshing state... (ID: kitchen-terraform-hervjBaOn)
       aws_subnet.extensive_tutorial: Refreshing state... (ID: subnet-0c74e85c0c034db15)
       aws_internet_gateway.extensive_tutorial: Refreshing state... (ID: igw-0a2c8280d58a0e31d)
       aws_security_group.extensive_tutorial: Refreshing state... (ID: sg-0fce61c168b7b64e3)
       aws_route_table.extensive_tutorial: Refreshing state... (ID: rtb-057ca26cfc76d3a2b)
       aws_instance.reachable_other_host: Refreshing state... (ID: i-0e0f9f895128da043)
       aws_instance.remote_group[0]: Refreshing state... (ID: i-0d2d8c6c0ce8abb1d)
       aws_instance.remote_group[1]: Refreshing state... (ID: i-03067f4757e5fcea1)
       aws_route_table_association.extensive_tutorial: Refreshing state... (ID: rtbassoc-01c46e2eccb0184bd)
       module.extensive_kitchen_terraform.aws_instance.reachable_other_host: Destroying... (ID: i-0e0f9f895128da043)
       module.extensive_kitchen_terraform.aws_route_table_association.extensive_tutorial: Destroying... (ID: rtbassoc-01c46e2eccb0184bd)
       module.extensive_kitchen_terraform.aws_instance.remote_group[0]: Destroying... (ID: i-0d2d8c6c0ce8abb1d)
       module.extensive_kitchen_terraform.aws_instance.remote_group[1]: Destroying... (ID: i-03067f4757e5fcea1)
       module.extensive_kitchen_terraform.aws_route_table_association.extensive_tutorial: Destruction complete after 1s
       module.extensive_kitchen_terraform.aws_route_table.extensive_tutorial: Destroying... (ID: rtb-057ca26cfc76d3a2b)
       module.extensive_kitchen_terraform.aws_route_table.extensive_tutorial: Destruction complete after 1s
       module.extensive_kitchen_terraform.aws_internet_gateway.extensive_tutorial: Destroying... (ID: igw-0a2c8280d58a0e31d)
       module.extensive_kitchen_terraform.aws_instance.remote_group.1: Still destroying... (ID: i-03067f4757e5fcea1, 10s elapsed)
       module.extensive_kitchen_terraform.aws_instance.remote_group.0: Still destroying... (ID: i-0d2d8c6c0ce8abb1d, 10s elapsed)
       module.extensive_kitchen_terraform.aws_instance.reachable_other_host: Still destroying... (ID: i-0e0f9f895128da043, 10s elapsed)
       module.extensive_kitchen_terraform.aws_internet_gateway.extensive_tutorial: Still destroying... (ID: igw-0a2c8280d58a0e31d, 10s elapsed)
       module.extensive_kitchen_terraform.aws_instance.reachable_other_host: Still destroying... (ID: i-0e0f9f895128da043, 20s elapsed)
       module.extensive_kitchen_terraform.aws_instance.remote_group.0: Still destroying... (ID: i-0d2d8c6c0ce8abb1d, 20s elapsed)
       module.extensive_kitchen_terraform.aws_instance.remote_group.1: Still destroying... (ID: i-03067f4757e5fcea1, 20s elapsed)
       module.extensive_kitchen_terraform.aws_internet_gateway.extensive_tutorial: Destruction complete after 20s
       module.extensive_kitchen_terraform.aws_instance.reachable_other_host: Destruction complete after 22s
       module.extensive_kitchen_terraform.aws_instance.remote_group[1]: Destruction complete after 22s
       module.extensive_kitchen_terraform.aws_instance.remote_group.0: Still destroying... (ID: i-0d2d8c6c0ce8abb1d, 30s elapsed)
       module.extensive_kitchen_terraform.aws_instance.remote_group[0]: Destruction complete after 32s
       module.extensive_kitchen_terraform.aws_key_pair.extensive_tutorial: Destroying... (ID: kitchen-terraform-hervjBaOn)
       module.extensive_kitchen_terraform.aws_security_group.extensive_tutorial: Destroying... (ID: sg-0fce61c168b7b64e3)
       module.extensive_kitchen_terraform.aws_subnet.extensive_tutorial: Destroying... (ID: subnet-0c74e85c0c034db15)
       module.extensive_kitchen_terraform.aws_key_pair.extensive_tutorial: Destruction complete after 1s
       module.extensive_kitchen_terraform.random_string.key_name: Destroying... (ID: hervjBaOn)
       module.extensive_kitchen_terraform.random_string.key_name: Destruction complete after 0s
       module.extensive_kitchen_terraform.aws_security_group.extensive_tutorial: Destruction complete after 2s
       module.extensive_kitchen_terraform.aws_subnet.extensive_tutorial: Destruction complete after 2s
       module.extensive_kitchen_terraform.aws_vpc.extensive_tutorial: Destroying... (ID: vpc-0db64b659a3096c14)
       module.extensive_kitchen_terraform.aws_vpc.extensive_tutorial: Destruction complete after 0s
       
       Destroy complete! Resources: 11 destroyed.
$$$$$$ Finished destroying the Terraform-managed infrastructure.
$$$$$$ Selecting the default Terraform workspace...
       Switched to workspace "default".
$$$$$$ Finished selecting the default Terraform workspace.
$$$$$$ Deleting the kitchen-terraform-extensive-suite-redhat Terraform workspace...
       Deleted workspace "kitchen-terraform-extensive-suite-redhat"!
$$$$$$ Finished deleting the kitchen-terraform-extensive-suite-redhat Terraform workspace.
       Finished destroying <extensive-suite-redhat> (0m50.07s).
-----> Test Kitchen is finished. (0m50.72s)
```
#

```
bundle exec kitchen destroy ubuntu
```
```
❯ bundle exec kitchen destroy ubuntu
-----> Starting Test Kitchen (v2.4.0)
-----> Destroying <extensive-suite-ubuntu>...
$$$$$$ Verifying the Terraform client version is in the supported interval of >= 0.11.4, < 0.13.0...
$$$$$$ Reading the Terraform client version...
       Terraform v0.11.14
       + provider.aws v1.60.0
       + provider.random v1.3.1
       
       Your version of Terraform is out of date! The latest version
       is 0.12.23. You can update by downloading from www.terraform.io/downloads.html
$$$$$$ Finished reading the Terraform client version.
$$$$$$ Finished verifying the Terraform client version.
$$$$$$ Initializing the Terraform working directory...
       Initializing modules...
       - module.extensive_kitchen_terraform
       
       Initializing provider plugins...
       
       Terraform has been successfully initialized!
$$$$$$ Finished initializing the Terraform working directory.
$$$$$$ Selecting the kitchen-terraform-extensive-suite-ubuntu Terraform workspace...
       Switched to workspace "kitchen-terraform-extensive-suite-ubuntu".
$$$$$$ Finished selecting the kitchen-terraform-extensive-suite-ubuntu Terraform workspace.
$$$$$$ Destroying the Terraform-managed infrastructure...
       random_string.key_name: Refreshing state... (ID: rgor5r65n)
       aws_key_pair.extensive_tutorial: Refreshing state... (ID: kitchen-terraform-rgor5r65n)
       aws_vpc.extensive_tutorial: Refreshing state... (ID: vpc-0252c9ec94116fa98)
       aws_internet_gateway.extensive_tutorial: Refreshing state... (ID: igw-09bb93f51bf6be275)
       aws_security_group.extensive_tutorial: Refreshing state... (ID: sg-0fc29d53757c811cb)
       aws_subnet.extensive_tutorial: Refreshing state... (ID: subnet-0732780af08f2b7eb)
       aws_route_table.extensive_tutorial: Refreshing state... (ID: rtb-0d289f8d57dfb68b0)
       aws_instance.reachable_other_host: Refreshing state... (ID: i-01855959085e277ff)
       aws_instance.remote_group[0]: Refreshing state... (ID: i-06013c9a50403d4f7)
       aws_instance.remote_group[1]: Refreshing state... (ID: i-0f902c2d2e0081e2c)
       aws_route_table_association.extensive_tutorial: Refreshing state... (ID: rtbassoc-0ff5392b10fcfc152)
       module.extensive_kitchen_terraform.aws_route_table_association.extensive_tutorial: Destroying... (ID: rtbassoc-0ff5392b10fcfc152)
       module.extensive_kitchen_terraform.aws_instance.remote_group[0]: Destroying... (ID: i-06013c9a50403d4f7)
       module.extensive_kitchen_terraform.aws_instance.remote_group[1]: Destroying... (ID: i-0f902c2d2e0081e2c)
       module.extensive_kitchen_terraform.aws_instance.reachable_other_host: Destroying... (ID: i-01855959085e277ff)
       module.extensive_kitchen_terraform.aws_route_table_association.extensive_tutorial: Destruction complete after 1s
       module.extensive_kitchen_terraform.aws_route_table.extensive_tutorial: Destroying... (ID: rtb-0d289f8d57dfb68b0)
       module.extensive_kitchen_terraform.aws_route_table.extensive_tutorial: Destruction complete after 2s
       module.extensive_kitchen_terraform.aws_internet_gateway.extensive_tutorial: Destroying... (ID: igw-09bb93f51bf6be275)
       module.extensive_kitchen_terraform.aws_instance.remote_group.1: Still destroying... (ID: i-0f902c2d2e0081e2c, 10s elapsed)
       module.extensive_kitchen_terraform.aws_instance.reachable_other_host: Still destroying... (ID: i-01855959085e277ff, 10s elapsed)
       module.extensive_kitchen_terraform.aws_instance.remote_group.0: Still destroying... (ID: i-06013c9a50403d4f7, 10s elapsed)
       module.extensive_kitchen_terraform.aws_internet_gateway.extensive_tutorial: Still destroying... (ID: igw-09bb93f51bf6be275, 10s elapsed)
       module.extensive_kitchen_terraform.aws_instance.remote_group.0: Still destroying... (ID: i-06013c9a50403d4f7, 20s elapsed)
       module.extensive_kitchen_terraform.aws_instance.remote_group.1: Still destroying... (ID: i-0f902c2d2e0081e2c, 20s elapsed)
       module.extensive_kitchen_terraform.aws_instance.reachable_other_host: Still destroying... (ID: i-01855959085e277ff, 20s elapsed)
       module.extensive_kitchen_terraform.aws_internet_gateway.extensive_tutorial: Destruction complete after 19s
       module.extensive_kitchen_terraform.aws_instance.reachable_other_host: Destruction complete after 23s
       module.extensive_kitchen_terraform.aws_instance.remote_group[1]: Destruction complete after 23s
       module.extensive_kitchen_terraform.aws_instance.remote_group.0: Still destroying... (ID: i-06013c9a50403d4f7, 30s elapsed)
       module.extensive_kitchen_terraform.aws_instance.remote_group[0]: Destruction complete after 33s
       module.extensive_kitchen_terraform.aws_security_group.extensive_tutorial: Destroying... (ID: sg-0fc29d53757c811cb)
       module.extensive_kitchen_terraform.aws_key_pair.extensive_tutorial: Destroying... (ID: kitchen-terraform-rgor5r65n)
       module.extensive_kitchen_terraform.aws_subnet.extensive_tutorial: Destroying... (ID: subnet-0732780af08f2b7eb)
       module.extensive_kitchen_terraform.aws_key_pair.extensive_tutorial: Destruction complete after 1s
       module.extensive_kitchen_terraform.random_string.key_name: Destroying... (ID: rgor5r65n)
       module.extensive_kitchen_terraform.random_string.key_name: Destruction complete after 0s
       module.extensive_kitchen_terraform.aws_subnet.extensive_tutorial: Destruction complete after 2s
       module.extensive_kitchen_terraform.aws_security_group.extensive_tutorial: Destruction complete after 2s
       module.extensive_kitchen_terraform.aws_vpc.extensive_tutorial: Destroying... (ID: vpc-0252c9ec94116fa98)
       module.extensive_kitchen_terraform.aws_vpc.extensive_tutorial: Destruction complete after 1s
       
       Destroy complete! Resources: 11 destroyed.
$$$$$$ Finished destroying the Terraform-managed infrastructure.
$$$$$$ Selecting the default Terraform workspace...
       Switched to workspace "default".
$$$$$$ Finished selecting the default Terraform workspace.
$$$$$$ Deleting the kitchen-terraform-extensive-suite-ubuntu Terraform workspace...
       Deleted workspace "kitchen-terraform-extensive-suite-ubuntu"!
$$$$$$ Finished deleting the kitchen-terraform-extensive-suite-ubuntu Terraform workspace.
       Finished destroying <extensive-suite-ubuntu> (0m57.07s).
-----> Test Kitchen is finished. (0m57.73s)
```
