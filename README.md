# DigitalOcean
[![Build Status](https://travis-ci.org/rmoriz/digital_ocean.png)](https://travis-ci.org/rmoriz/digital_ocean)

DigitalOcean provides simple cloud hosting. Create and delete
SSD-based virtual machines within seconds for a very affordable price.

Go to https://www.digitalocean.com/ for more information.

This gem is a simple ruby wrapper for the [HTTP API of DigitalOcean](https://api.digitalocean.com/) using
the great [Faraday](https://github.com/lostisland/faraday) library.


## Installation

Add this line to your application's Gemfile:

    gem 'digital_ocean'

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install digital_ocean

## Prerequisite: Create an account and API credentials

[![Create API credentials](http://i.imgur.com/1iYdSFF.png)](https://www.digitalocean.com/login)

## Usage

### Examples

```ruby
require 'digital_ocean'

# 1. create a client instance
#
dc = DigitalOcean::API.new :client_id => 'YOUR_CLIENT_ID', 
                           :api_key   => 'YOUR_API_KEY'

# 2. get all regions (server locations)
#
res = dc.regions.list
res.regions.size # => 2
res.regions.first.name # => "New York 1"

# 3. get all available machine types (sizes)
#
res = dc.sizes.list
res.sizes
# => [#<Hashie::Rash id=66 name="512MB">, 
#     #<Hashie::Rash id=63 name="1GB">, 
#     #<Hashie::Rash id=62 name="2GB">, 
#     #<Hashie::Rash id=64 name="4GB">, 
#     #<Hashie::Rash id=65 name="8GB">, 
#     #<Hashie::Rash id=61 name="16GB">, 
#     #<Hashie::Rash id=60 name="32GB">, 
#     #<Hashie::Rash id=70 name="48GB">, 
#     #<Hashie::Rash id=69 name="64GB">, 
#     #<Hashie::Rash id=68 name="96GB">]

# 4. get all available images (e.g. OS-Images, snapshots, backups)
#
res = dc.images.list
res.images
# => [#<Hashie::Rash distribution="CentOS" id=1601 name="CentOS 5.8 x64">, 
#     #<Hashie::Rash distribution="CentOS" id=1602 name="CentOS 5.8 x32">, 
#     ...
#     #<Hashie::Rash distribution="Ubuntu" id=2676 name="Ubuntu 12.04 x64 Server">, 
#     ...
#     #<Hashie::Rash distribution="Ubuntu" id=25306 name="Ubuntu 12.10 x32 Server">,
#     ...]

# 5. start a new instance (called droplet)
# 
res = dc.droplets.create :name      => 'new_hostname',
                         :size_id   => 64,
                         :image_id  => 2676,
                         :region_id => 1

# 6. query droplet status (e.g. to get the IP of a droplet)
#
res = dc.droplets.list
res.droplets
# => [#<Hashie::Rash backups_active=false id=123456 
#                    image_id=25306 ip_address="123.123.123.123" 
#                    name="new_hostname" region_id=1 size_id=64 
#                    status="active">]

```

see ```spec/api_spec.rb``` for more details and all implemented
resources.

## Features

### DigitalOcean::API
  - .new
    - should return an instance when called with the essential parameters
  - \#droplets
    - \#list
      - should be successful
      - should return a list of all droplets
    - \#show
      - should be successful
    - \#create
      - should be successful
    - \#reboot
      - should be successful
    - \#power\_cycle
      - should be successful
    - \#shutdown
      - should be successful
    - \#power\_on
      - should be successful
    - \#power\_off
      - should be successful
    - \#reset\_root\_password
      - should be successful (PENDING: does not work, ask digitalocean to fix)
    - \#resize
      - should be successful
    - \#snapshot
      - should be successful
    - \#restore
      - should be successful
    - \#rebuild
      - should be successful
    - \#enable\_backups
      - should be successful
    - \#disable\_backups
      - should be successful
    - \#delete
#<Hashie::Rash event_id=594739 status="OK">
      - should be successful
  - \#sizes
    - \#list
      - should be successful
      - should return a list of all droplet sizes
      - should return the correct ID for the 96GB size
  - \#regions
    - \#list
      - should be successful
      - should return a list of all regions
  - \#ssh\_keys
    - \#list
      - should be successful
      - should return a list of all SSH keys
    - \#show
      - valid
        - should be successful
        - should return the public keykey
      - invalid
        - should not be successful
    - \#add
      - does not work, ask digitalocean to fix (PENDING: No reason given)
      - should be successful (PENDING: Temporarily disabled with xit)
    - \#edit
      - does not work, ask digitalocean to fix (PENDING: No reason given)
    - \#delete
      - should be successful
  - \#images
    - \#list
      - without filter
        - should be successful
        - should return a list of all images
      - with filter: global
        - should be successful
        - should return global images
      - with filter: my\_images
        - should be successful
        - should return my\_images
    - \#show
      - valid
        - should be successful
        - should return the image
      - invalid
        - should not be successful
    - \#delete
      - valid
        - should be successful (PENDING: does not work, ask digitalocean to fix)
      - invalid
        - should not be successful

### DigitalOcean
  - should have a version number

## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push origin my-new-feature`)
5. Create new Pull Request

## License

MIT

## Copyright

2013 Moriz GmbH, https://moriz.de/

