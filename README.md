Sam's Chef Repo
===============

This is a chef-solo repo.  I haven't tried it with client/server.

Cookbooks
=========

Until I figure out now to get Roles working, we have one node.json that sets up a machine to be like itdoesnothing.com

Usage
=====
Create a new EC2 instance using AMI: ami-ad7e2ee8 (Ubuntu 10.10 Maverick EBS boot 32bit)

Log into the machine

sudo -s
apt-get update

# I had some issues with chef using the wrong rubygems when both the system version and REE were installed,
# so I'm going to forgo the system version and jump straight to REE.

apt-get install build-essential zlib1g-dev libssl-dev libreadline5-dev # All required for REE
cd /tmp
curl -O http://files.rubyforge.vm.bytemark.co.uk/emm-ruby/ruby-enterprise-1.8.7-2010.02.tar.gz
tar xvfz ruby-enterprise-1.8.7-2010.02.tar.gz
ruby-enterprise-1.8.7-2010.02/installer --no-dev-docs

ln -s ruby-enterprise-1.8.7-2010.02 /opt/ruby-enterprise
PATH=/opt/ruby-enterprise/bin:$PATH

gem install chef --no-rdoc --no-ri
git clone git@github.com:sampierson/chef-repo.git
cd chef-repo
chef-solo -c solo.rb -j node.json 

# ON NOTEBOOK
cd itdoesnothing
cap deploy:setup

# ON EC2 - until we fix capistrano to not clear setgid during deploy:setup
chmod -R g+s /u/apps

# ON NOTEBOOK
cap deploy
cap deploy:migrate
