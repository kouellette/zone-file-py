# DNS Zone File Library

[![CircleCI](https://img.shields.io/circleci/project/blockstack/dns-zone-file-py.svg)](https://circleci.com/gh/blockstack/dns-zone-file-py)
[![PyPI](https://img.shields.io/pypi/v/zonefile.svg)](https://pypi.python.org/pypi/zonefile/)
[![PyPI](https://img.shields.io/pypi/dm/zonefile.svg)](https://pypi.python.org/pypi/zonefile/)
[![PyPI](https://img.shields.io/pypi/l/zonefile.svg)](https://pypi.python.org/pypi/zonefile/)
[![Slack](http://slack.blockstack.org/badge.svg)](http://slack.blockstack.org/)

*A library for creating and parsing DNS zone files*

#### Zone File Example

```
$ORIGIN example.com
$TTL 86400

server1      IN     A       10.0.1.5
server2      IN     A       10.0.1.7
dns1         IN     A       10.0.1.2
dns2         IN     A       10.0.1.3

ftp          IN     CNAME   server1
mail         IN     CNAME   server1
mail2        IN     CNAME   server2
www          IN     CNAME   server2
```

#### Parsing Zone Files

```python
>>> zone_file_object = parse_zone_file(zone_file)
>>> print json.dumps(zone_file_object, indent=4, sort_keys=True)
{
    "$ORIGIN": "EXAMPLE.COM", 
    "$TTL": 86400, 
    "A": [
        {
            "ip": "10.0.1.5", 
            "name": "SERVER1"
        }, 
        {
            "ip": "10.0.1.7", 
            "name": "SERVER2"
        }, 
        {
            "ip": "10.0.1.2", 
            "name": "DNS1"
        }, 
        {
            "ip": "10.0.1.3", 
            "name": "DNS2"
        }
    ], 
    "CNAME": [
        {
            "alias": "SERVER1", 
            "name": "FTP"
        }, 
        {
            "alias": "SERVER1", 
            "name": "MAIL"
        }, 
        {
            "alias": "SERVER2", 
            "name": "MAIL2"
        }, 
        {
            "alias": "SERVER2", 
            "name": "WWW"
        }
    ]
}
```

#### Making Zone Files

```python
>>> records = {'URI': [{'priority': 1, 'target': 'https://mq9.s3.amazonaws.com/naval.id/profile.json', 'name': '@', 'weight': 10, 'ttl': '1D'}]}
>>> zone_file = make_zone_file(records, origin="ryan.id", ttl="3600")
>>> print zone_file
$ORIGIN ryan.id
$TTL 3600
@ 1D URI 1 10 "https://mq9.s3.amazonaws.com/naval.id/profile.json"
```
