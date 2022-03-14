# Chef Infra Client

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

## Links to Client Discussion Videos
