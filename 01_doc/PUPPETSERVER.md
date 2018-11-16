# PUPPET CLI CMDs

# important: tuning java heap, network parameter

## compiler server: remove ca if only compilerserver in multimaster ha environment
```
cat /etc/puppetlabs/puppetserver/bootstrap.cfg
```

## in jedem environment
```
puppet type generate
```

```
puppet agent --test --summarize
cat /opt/puppetlabs/puppet/cache/state/last_run_summary.yaml
```

### i.e. auf dem compilerserver (multi-master)
https://github.com/puppetlabs/puppetlabs-haproxy
```
puppet agent --configdir statedir
puppet agent --configprint server
puppet agent --configprint all | nl -ba
```

```
cat $HOME/.bashrc << EOF
# load customizations, such as the pretty git prompt.
# comment this out to use your own customizations.
[[ -f ~/.bashrc.puppet ]] && source ~/.bashrc.puppet
EOF
```

```
cat $HOME/.gemrc << EOF
:backtrace: false
:benchmark: false
:bulk_threshold: 1000
:sources:
- file:///var/cache/rubygems/
- https://rubygems.org
:update_sources: true
:verbose: true
EOF
```

```
# User specific aliases and functions
alias rm='rm -i'
alias cp='cp -i'
alias mv='mv -i'
alias vi=vim
#export TERM=ansi
```

## Docker
dockerproject.org

## Puppet Tasks (puppet pe)
## Puppet with bolt (open sw) (shell, python, perl, ...)
/opt/puppetlabs/puppet/bin/bolt task run service::linux action=stop name=ntp --nodes localhost --modulepath /etc/puppetlabs/code/modules --password puppet --user root


puppet tasks in google gibt auch bolt output
github tasks-playground

puppet docs: Welcome to bolt
puppet docs: writing plans
example42: plans_and_tasks


## Status der Pupppet Infrastructure (puppet pe only)
puppet infrastructure status

cat >> /etc/puppetlabs/puppet/puppet.conf << EOF
[main]
server = master.puppetlabs.vm
certname = host.puppetlabs.vm
user = pe-puppet
group = pe-puppet
mkusers = true
module_groups = base+pe_only
environment_timeout = 0
dns_alt_names = host, proxy.puppetlabs.vm

[master]
node_terminus = classifier
storeconfigs = true
storeconfigs_backend = puppetdb
reports = puppetdb
certname = host.puppetlabs.vm
always_retry_plugins = false
disable_i18n = false
EOF
