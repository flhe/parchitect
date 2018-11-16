beaker: vms nach test abreissen und neu deployen

curl queries to cmdb (insert)

KEINE expliziten lookups!!
(kein hiera und kein lookup mehr verwenden. Puppet macht automated databinding.)
Abfragen aus Profilen! Hardcoded Data in Profilen! Keine Daten in Modulen!


in hieradata yaml files:
ldap::ldap_binddn: 'dc=domain,dc=de'
ldap_default_binddn: "%{lookup('ldap::ldap_binddn')}"
ohne doppelpunkt: abfrage von hier per internem lookup

.pp files:
String[1] $ldap_binddn.
Optional[String[1]] $ldap_binddn = undef,

please see: https://puppet.com/docs/hiera/3.3/variables.html


provider: apt, rpm, gem, pip
ls /opt/puppetlabs/puppet/lib/ruby/vendory_ruby/puppet/provider/package/*.rb
grep defaultfor /opt/puppetlabs/puppet/lib/ruby/vendory_ruby/puppet/provider/package/*.rb

ls /opt/puppetlabs/puppet/lib/ruby/vendory_ruby/puppet/provider/


puppetserver: intermediate CA (greift auf CA zu, darf aber signieren!)
