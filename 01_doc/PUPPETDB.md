# PuppetDB
puppetdb whitelist: zugang zur puppetdb
siehe /etc/puppetlabs/puppetdb/conf.d/jetty.ini

# REST-API: Curl über Puppetserver (query catalog or node facts)
# permission through auth.conf
/etc/puppetlabs/puppetserver/conf.d/auth.conf
DEPRECATED: /etc/puppetlabs/puppet/auth.conf

https://github.com/puppetlabs/puppetlabs-puppet_authorization.git
https://github.com/puppetlabs/puppetlabs-hocon.git
puppet_authorization::rule { 'catalog_request':
  match_request_path         => '^/puppet/v3/catalog/([^/]+)$',
  match_request_type         => 'regex',
  match_request_method       => ['get','post'],
  match_request_query_params => {'environment' => [ 'production', 'test' ]},
  allow                      => ['$1', 'adminhost.mydomain.com'],
  sort_order                 => 200,
  path                       => '/etc/puppetlabs/puppetserver/conf.d/auth.conf',
}

export CERT=$(puppet master --configprint hostcert)
export CACERT=$(puppet master --configprint localcacert)
export PRVKEY=$(puppet master --configprint hostprivkey)
export CERT_OPTIONS="--cert ${CERT} --cacert ${CACERT} --key ${PRVKEY}"
export MASTER="https://master.puppetlabs.vm:8140"

# Zertificate revoke, clean and sign through REST API
# can become automatised
curl $CERT_OPTIONS ${MASTER}/puppet/v3/{node|catalog}/{CERTNAME}?environment=production | jq . | less
# Auf dem CA Master:
curl -X DELETE $CERT_OPTIONS ${MASTER}/puppet-ca/v1/certificate_status/{CERTNAME}?environment=production

curl -X GET $CERT_OPTIONS ${MASTER}/status/v1/simple
curl -X GET $CERT_OPTIONS ${MASTER}/status/v1/services
curl -X POST $CERT_OPTIONS ${MASTER}/puppet-ca/v1/certificate_status/{CERTNAME}?environment=production --data '{"desired_state":"revoked"}'

