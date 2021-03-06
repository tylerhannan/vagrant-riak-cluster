#!/usr/bin/env ruby
# vim: set ft=ruby :
# vim: set syntax=ruby :

#--- BEGIN config vars ---#

# Select the number of riak nodes in the cluster.
# There is a reason to use 5 (in production) that is explained in detail on the
# Basho blog.

num_nodes = 5

# Select the name of the box you prefer.
#
# This will only work with Ubuntu Precise (12.04 AMD 64) base boxes, but if the name
# you used when you added the box is different from 'ubuntu' 
# then change this value.

base_box = "ubuntu"

# Base IP value.
#
# Modify if desired.

base_ip = "33.33.33."

# Select the storage backend you want for Riak.
# To take advantage of 2i we must use eLevelDB.
#
# Valid options are:
#
#   bitcask
#   eleveldb
#   innostore
#   memory
#   multi

riak_backend = "eleveldb"

# IP addresses can't start at one (i.e X.X.X.1) or complaints will
# come your way. This is the IP increment for the ip4 value.

ip_inc = 10

#--- END config vars ---#

Vagrant::Config.run do |cluster|

  (1..num_nodes).each do |index|

    ip4       = index * ip_inc
    ip_addr   = "#{base_ip}#{ip4}"
    hostname  = "riak#{index}"
    prov_args = {
      :facter => {
        "ip_addr"      => ip_addr,
        "join_ip"      => "#{base_ip}#{ip_inc}",
	      "riak_backend" => riak_backend,
      }
    }

    cluster.vm.define hostname do |node|
      node.vm.box       = base_box
      node.vm.host_name = hostname
      node.vm.boot_mode = :headless

      node.vm.network   :hostonly, ip_addr
      node.vm.provision :puppet, prov_args do |puppet|
        puppet.manifests_path = "puppet"
	      puppet.module_path    = "puppet"
        puppet.manifest_file  = "init.pp"
        # Uncomment for verbose debugging output
        # quite helpful if something goes awry
        ## puppet.options = "--verbose --debug"
      end
    end
  end
end
