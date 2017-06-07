---
RFC: unassigned
Title: User behaviour data collection
Author: Thom May <thom@chef.io>, Charles Johnson <charles@chef.io>
Status: Draft
Type: Informational
---

# User behaviour data collection

In order to provide greater insight into how our users interact with our
tools, this RFC introduces a framework for defining the policies for the
collection (sharing), storage, and deletion of anonymized product usage data
that can be applied to any Chef project.

## Motivation

    As a UX designer at Chef,
    I want to understand how people use Chef tools,
    so that I can improve their experience.

    As a product owner at Chef,
    I want to learn which Chef tools and commands are most used,
    so I can prioritise development.

## Specification

This RFC defines the core of product usage data sharing. It does not define how
individual apps will implement product usage data sharing, and it is expected
that each app would create a follow up RFC that details individaul behaviour,
and the types of data that will be shared with Chef Sofware.

### Opt Out by default

Experience (with Habitat and Automate) has shown that default opt-out for 
product usage data sharing provides sample sets that are too small (and 
potentially biased) to draw meaningful conclusions. In order to gather 
meaningful data, Chef tools will share anonymized usage data by default. The 
tools will provide clear information to allow the user to easily opt-out. 
Any workstation or client opt-out will be honored by all Chef workstation or
client tools.

Server tools may each require their own separate opt-out.

### Implementation

All Chef Workstation and client tools will use a centralized configuration for
product usage data sharing, in order to provide a consistent experience. If 
the configuration is not present, a data-sharing-enabled application will 
generate a new UUID, and create a well formatted configuration file. It will
notify the user that product usage data sharing is enabled, and permit the user
to easily opt out.

All product usage data sharing enabled applications will provide simple ways 
for a user to discover the status of product usage data sharing, and to opt-out.

### Privacy and Data Retention

All collected data will be subject to Chef's [privacy policy](https://www.chef.io/privacy-policy/),
which is Privacy Shield certified. Any collected data can be deleted upon
request.

### Types of collected data

Some data we envisage collecting includes, but is not limited to:

 - Community Cookbooks used, and versions (in policyfiles or berkshelf)
 - Kitchen plugins
 - Vagrant boxes
 - Commands and options attempted

### Example sample message

```json
{
  "instance_id":"00000000-0000-0000-0000-000000000000",
  "message_version":1.0,
  "payload_version":1.0,
  "license_id":"00000000-0000-0000-0000-000000000000",
  "origin": "command-line",
  "type": "track",
  "product": "chefdk",
  "timestamp": "2017-02-06T17:25:42   Z",
  "payload":{  
     "event":"install-time",
	 "anonymousId":"2d5a4d61-d79a-4ff9-aa71-a403c2d5a001",
     "properties":{  
        "install_time":1500,
        "package_installed": "chefdk-2.0.1-1_amd64.deb",
        "operating_system": "ubuntu 14.04"
     }
  } 
}
```

## Copyright

This work is in the public domain. In jurisdictions that do not allow for this,
this work is available under CC0. To the extent possible under law, the person
who associated CC0 with this work has waived all copyright and related or
neighboring rights to this work.
