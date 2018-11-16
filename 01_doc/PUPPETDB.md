# PuppetDB
puppetdb whitelist: zugang zur puppetdb
siehe /etc/puppetlabs/puppetdb/conf.d/jetty.ini

# REST-API: Curl über Puppetserver (query catalog or node facts)
# permission through auth.conf
export CERT=$(puppet master --configprint hostcert)
export CACERT=$(puppet master --configprint localcacert)
export PRVKEY=$(puppet master --configprint hostprivkey)
export CERT_OPTIONS="--cert ${CERT} --cacert ${CACERT} --key ${PRVKEY}"
export MASTER="https://master.puppetlabs.vm:8140"

curl $CERT_OPTIONS ${MASTER}/puppet/v3/{node|catalog}/{CERTNAME}?environment=production | jq . | less
