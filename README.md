# Chef In Plain English

This is a set of technical enablement discussions around the Chef product that are designed to help inform new Chef's on the base architecture and functionalities of the technology as implemented.  Though the code can be complex in places, the fundamental concepts should be easy to understand and that understanding should allow any engineer (CA, SA, PS, Software, DevRel, etc..) to dig into the code to fix or improve it in a straightforward and expeditious manner.

## Chef Infra

Chef Infra is the bread and butter of Chef. 99% of our customers are using Chef Infra in one way or another to manage their fleet of servers from hundreds to hundreds of thousands of systems.

The active products that used in Chef Infra are:

- Chef Infra Client
- Chef Infra Server (standalone)
- Chef Infra Server (habitized - installed via Automate)
- Chef Backend

I'm going to dig into these products to try and explain some core concepts, impart some useful wisdom, and answer questions where possible.

### Chef Infra Client

Chef Infra Client is the workhorse of Chef Infra. It makes configuration management possible, and it is what implements every cookbook written.

Here's what I'll be covering:

- The chef-client run
  - read config.rb
  - report start to the chef server (unless in solo mode)
  - run ohai
  - read node object from chef server (if not in solo mode)
    - This is a checkout function. If something tries to modify the node object on the server after this point in the run, it'll be reverted when the run finishes.
  - merge in JSON attributes and override_run_list (or named_run_list in Policy)
  - Compile, Converge, Compliance phases
  - report finish to the chef server (completed or failed) && return the node object to the chef-server

- Notes about communications with the Chef Server
  - SSL has needs!
    - Full Cert Chain
    - Subject Alternative Name extension must have entry, even if no different than the cert subject name.
    - Trusting the Cert Chain
  - Object Sizes. Node objects and InSpec reports can only be so big.

- The phases of a client run
  - Compile
    - Sync cookbooks to local cache
    - Load Libraries from cookbooks
    - Load Attribute Mash from cookbooks (merge with node object)
    - Load Custom Resources
    - Load Profiles, Inputs, and Waivers for the Compliance phase (only if using the compliance phase)
    - Create Resource Collection
  - Converge
    - Run resource collection
    - Save the node object
  - Compliance (Or the Audit Cookbook)
    - Run InSpec Profiles with Waivers and Inputs

- Chef Resources - Custom and otherwise
  - The goal of resources
  - The differences between a built-in resource and a custom resource
  - Improving resources
  - Make resource writing easier

- Chef Helpers
  - Defining helpers
  - Making helpers Available to your cookbooks

- Ohai Plugins
  - Defining plugins
  - Where to put your plugins
  - Depending on other plugins

- Quick notes to make cookbook and client development easier
  - Chef-Shell
  - Monkeypatching

#### Links to Client Discussion Videos

### Chef Infra Server
