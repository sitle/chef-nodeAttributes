# chef-haclusters-cookbook

 This chef cookbook allows to simulate node environments in nodes definition by the use of data bags...

## Supported Platforms

 ubuntu/debian

## Attributes

<table>
  <tr>
    <th>Key</th>
    <th>Type</th>
    <th>Description</th>
    <th>Default</th>
  </tr>
  No default attribute
</table>

## Usage

 Default attributes can be definied in a data bag whose item is the fqdn of the node. Then, an other cookbook can be applied...

 (1): Dots are not allowed (only alphanumeric), substitute by underscores

eg:
<pre>
{
  "id": "ldap2_toriki_dmz_srv_gov_pf",
  "haproxy":{
    "services":{
      "ldap_cluster":{
        "app_server_role": "toriki.dmz.srv",
        "pool_members":[{
          "hostname":"ldap2",
          "ipaddress":"ldap2.toriki.dmz.srv.gov.pf",
          "member_port":"390",
          "member_options":"check port 5667 inter 2s fall 5 rise 1"
        }]
      }
    }
  },
  "iproute2":{}
}
{
  "id": "ldap_toriki_dmz_srv_gov_pf",
  "haproxy": {
    "httpchk": "HEAD",
    "services": {
      "ldap_cluster": {
        "app_server_role": "toriki.dmz.srv",
        "httpchk": "HEAD",
        "mode": "tcp",
        "balance": "leastconn",
        "incoming_address": "0.0.0.0",
        "incoming_port": "389"
      }
    }
  },
  "iproute2": {}
}
{
  "id": "ldap1_toriki_dmz_srv_gov_pf",
  "haproxy":{
    "services":{
      "ldap_cluster":{
        "app_server_role": "toriki.dmz.srv",
        "pool_members":[{
          "hostname":"ldap1",
          "ipaddress":"ldap1.toriki.dmz.srv.gov.pf",
          "member_port":"390",
          "member_options":"check port 5667 inter 2s fall 5 rise 1"
        }]
      }
    }
  },
  "iproute2":{}
}
{
  "id": "loadbalancer_dev_gov_pf",
  "iproute2":{
  },
  "haproxy":{
    "services": {
      "ldap_cluster": {
        "app_server_role": "",
        "incoming_address": "0.0.0.0",
        "incoming_port": "389",
        "mode": "tcp",
        "httpchk": "HEAD",
        "balance": "leastconn",
        "pool_members": [{
          "hostname": "ldapwrite",
          "ipaddress": "ldapwrite.srv.gov.pf",
          "member_port": "390",
          "member_options": "check port 5667 inter 2s fall 5 rise 1"
        },{
          "hostname": "ldapsecond",
          "ipaddress": "ldapsecond.srv.gov.pf",
          "member_port": "390",
          "member_options": "check port 5667 inter 2s fall 5 rise 1"
        },{
          "hostname": "ldapdmz",
          "ipaddress": "ldapdmz.srv.gov.pf",
          "member_port": "389",
          "member_options": "check addr localhost port 5667 inter 2s fall 5 rise 1 backup"
        }]
      }
    }
  }
}
</pre>

### chef-haclusters::default

Include `chef-haclusters` in your node's `run_list`:

```json
{
  "run_list": [
    "recipe[chef-nodeAttributes::default]"...
  ]
}
```

## License and Authors

Author:: PE, pf. (<philippe.eychart@mail.pf>)