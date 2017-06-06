---
RFC: unassigned
Title: User behaviour data from CLI tools
Author: Thom May <thom@chef.io>
Status: Draft
Type: Informational
---

# User behaviour data from CLI tools

In order to provide greater insight into how our users interact with our
tools, this RFC introduces a framework for defining the collection, storage
and deletion of anonymized usage data (telemetry) that can be applied to any Chef
tools.

## Motivation

    As an interaction designer,
    I want to understand how our users use our tools,
    so that I can improve their experience.

    As a product owner,
    I want to learn which tools and commands are most used,
    so I can prioritise development.

## Specification

This RFC defines the core of user behaviour insight and telemetry. It does not define
how individual apps would implement telemetry, and it is expected that
each app would create a follow up RFC that details their behaviour and
the types of data that would be tracked.

### Opt Out by default

Experience (with Habitat and Automate) has shown that opt-in responses to usage data result in very
small (and potentially biased) sample sets. In order to gather robust data,
our tools will send data by default. The tools will provide clear
information to allow the user to easily opt out, and any opt out will
apply to all Chef CLI tools. 

### Implementation

All Chef CLI tools will use a centralized configuration to provide
consistent behaviour data. If the configuration is not present, a
telemetry enabled application will generate a new UUID, and create a
well formatted configuration file. It will notify the user that data
collection and transmission is enabled, and permit the user to easily
opt out.

All telemetry enabled applications will provide simple ways for a user
to discover the status of data collection and transmission, and to opt
out.

### Privacy and Data Retention

All collected data will be subject to Chef's [privacy policy](https://www.chef.io/privacy-policy/),
which is Privacy Shield certified. Any collected data can be deleted upon request.

### Types of collected data

Some data we envisage collecting includes:

 - Community Cookbooks and versions (in policyfiles or berkshelf)
 - Kitchen plugins
 - Vagrant boxes
 - Commands and options attempted

### Sample message

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
