# knife-lastrun

A plugin for Opscode's Knife command-line utility which displays Node 
metadata about the last chef-client run.

## Usage 

Supply a node name to get metrics from the last chef run

```bash
$ knife node lastrun www.example.com
Status                     success
Elapsed Time               74.198614
Start Time                 2012-02-22 00:39:04 +0000
End Time                   2012-02-22 00:40:18 +0000

Recipe                        Action                        Resource Type                 Resource
bigdata::default              run                           bash                          check for bashrc

Backtrace            none
Exception            none
Formatted Exception  none
```

## Installation

### Gem install

knife-lastrun is available on rubygems.org - if you have that source in your 
gemrc, you can simply use:

```bash
gem install knife-lastrun
```

## Configuring the Handler

### Using the chef_handler Resource

Create a Recipe with the following Resources:

```ruby
include_recipe 'chef_handler'

gem_package('knife-lastrun'){action :nothing}.run_action(:install)

chef_handler 'LastRunUpdateHandler' do
  action :enable
  source File.join(
    Gem.all_load_paths.grep(/lastrun_update/).first, 'lastrun_update.rb')
end
```

### Using client.rb

Add the following lines to your **client.rb**:

```ruby
require 'lastrun_update'
handler = LastRunUpdateHandler.new
report_handlers << handler
exception_handlers << handler 
```