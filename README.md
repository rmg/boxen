== Vagrant Boxen

Vagrant is a handy command-line tool for spinning up and connecting to a
virtual machine.

By default, it supports VirtualBox VMs, but by installing some plugins you can
instead make use of various cloud providers, such as AWS, Digital Ocean, or
Rackspace.

 * `git clone git@github.com:rmg/boxen && cd boxen/centos32`
 * Download & install Vagrant [here](http://downloads.vagrantup.com/)
 * Install plugins for providers you wish to use
 * `vagrant up --provider=digital_ocean`
 * `vagrant ssh`
 * do whatever you needed to do in Linux...
 * `vagrant destroy`


=== Plugins

 * [vagrant-digitalocean](https://github.com/smdahlen/vagrant-digitalocean)
 * [vagrant-aws](https://github.com/mitchellh/vagrant-aws)
 * [vagrant-rackspace](https://github.com/mitchellh/vagrant-rackspace)
   * not used yet, patches welcome


=== Vagrantfile

Use the following examples to adjust `~/.vagrant.d/Vagrantfile` according to your preferneces:

```ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION ||= "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Personal preferences
  config.ssh.private_key_path = '~/.ssh/id_rsa'
  config.ssh.forward_agent = true

  # vagrant plugin install vagrant-digitalocean
  config.vm.provider :digital_ocean do |provider, override|
    provider.ca_path = '/usr/local/opt/curl-ca-bundle/share/ca-bundle.crt'
    provider.client_id = 'DIGITAL OCEAN CLIENT ID HERE'
    provider.api_key = 'DIGITAL OCEAN SECRET API KEY HERE'
    provider.region = 'San Francisco 1'
    provider.size = '512MB'

    # This should probably correspond with your config.ssh.private_key_path above
    provider.ssh_key_name = 'NAME OF EXISTING DIGITAL OCEAN KEY PAIR'
  end

  # vagrant plugin install vagrant-aws
  config.vm.provider :aws do |aws, override|
    aws.access_key_id = 'AWS KEY ID'
    aws.secret_access_key = 'AWS SECRET ACCESS KEY'

    # This should probably correspond with your config.ssh.private_key_path above
    aws.keypair_name = 'NAME OF EXISTING AWS KEY PAIR'

    aws.instance_type = 't1.micro'
    # For something larger: 'm1.small', 'm1.medium', etc..

    # Pick somewhere near you or whatever resources you plan on using
    aws.region = 'us-west-2'
    # us-east-1 - US East (Virginia)
    # us-west-2 - US West (Oregon)
    # us-west-1 - US West (Northern California)
    # eu-west-1 - EU West (Ireland)
    # ap-southeast-1 - Asia Pacific (Singapore)
    # ap-southeast-2 - Asia Pacific (Sydney)
    # ap-northeast-1 - Asia Pacific (Tokyo)
    # sa-east-1 - South America (Sao Paulo)
  end
end
```

