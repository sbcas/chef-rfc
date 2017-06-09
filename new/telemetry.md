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
that each app would create a follow up RFC that details individual behaviour,
and the types of data that will be shared with Chef Sofware.
All data will be available publicly, so that our users understand what
is being stored, and can review and explore the data.

### Opt Out by default

Experience (with Habitat and Automate) has shown that default opt-out for 
product usage data sharing provides sample sets that are too small (and 
potentially biased) to draw meaningful conclusions. In order to gather 
meaningful data, Chef tools will share anonymized usage data by default. The 
tools will provide clear information to allow the user to easily opt-out. 
An opt-out on an individual system will be honored by all Chef tools.

Server tools may each require their own separate opt-out.

### Implementation

All Chef tools will use a centralized configuration for product usage data
sharing, in order to provide a consistent experience. If
the configuration is not present, a data-sharing-enabled application will 
create a well formatted configuration file. It will notify the user that
product usage data sharing is enabled, and permit the user to easily opt out.

All product usage data sharing enabled applications will provide simple ways 
for a user to discover the status of product usage data sharing, and to opt-out.

### Privacy and Data Retention

To provide user privacy and protect from de-anonymization attacks while still
gathering data that is useful for understanding our tools, Chef tools
will choose a new UUID for each new user session, where a session is
defined as the user not having run a Chef tool for 10 minutes. Our tools
will not collect or store IP addresses or other data that could be used
to identify individuals or their employers.

To ensure that sensitive data is not collected, Chef tools should never
collect option data, and tools will be able to filter more aggressively
if required.

All collected data will be subject to Chef's [privacy policy](https://www.chef.io/privacy-policy/),
which is Privacy Shield certified. Any collected data can be deleted upon
request.

### Prior Art

Many open source projects collect data from their users to help allocate
resources. Both [Ubuntu](https://wiki.ubuntu.com/Apport) and
[Fedora](https://retrace.fedoraproject.org/) collect crash dumps from their
users, along with system information, to help developers better debug and
improve their systems. 
The Debian project runs [PopCon](https://popcon.debian.org/) to better
understand what packages are installed and which architectures are in
use.
Habitat, our sister project, collects [analytics](https://www.habitat.sh/docs/about-analytics/)
related to user interactions.

### Types of collected data

Some data we envisage collecting includes, but is not limited to:

 - Community Cookbooks used, and versions (in policyfiles or berkshelf)
 - Kitchen plugins
 - Vagrant boxes
 - Commands and options attempted

### Examples

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
     "event":"user-command",
	 "anonymousId":"2d5a4d61-d79a-4ff9-aa71-a403c2d5a001",
     "properties":{  
        "command": "chef generate cookbook",
        "timestamp": "2017-02-06T17:25:42   Z"
     }
  } 
}
```

## Copyright

This work is in the public domain. In jurisdictions that do not allow for this,
this work is available under CC0. To the extent possible under law, the person
who associated CC0 with this work has waived all copyright and related or
neighboring rights to this work.
