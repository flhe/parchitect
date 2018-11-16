# PUPPET CLI CMDs

## remove ca if only compilerserver in multimaster ha environment
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

### i.e. auf dem compilerserver
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

validate_yaml() {
  ruby -ryaml -e "YAML.load_file '$1'"
}

validate_erb() {
  erb -P -x -T '-' $1 | ruby -c
}
```

## Docker
dockerproject.org

## Puppet Tasks (puppet pe)
## Puppet with bolt (open sw) (shell, python, perl, ...)
/opt/puppetlabs/puppet/bin/bolt task run service::linux action=stop name=ntp --nodes localhost --modulepath /etc/puppetlabs/code/modules --password puppet --user root
