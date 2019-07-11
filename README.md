# dnspodapi
# This is simple python API for dnspod user.
THe simple feature


```
import dnspod
api = dnspod.Dnspodapi(domain='your.domain.com',serverip='your.server.ip.address',login_email='your@dnspod_login_email.com',login_password='your password', record_type='A',ttl='600',record_line='default')
```
- add record (Host record is domain name prefix.)
```
api.addrecord('Host record')
```
- remove record
```
api.removerecord('Host record')
```
- api version
```
api.version()
```
- check exist record
```
api.checkrecord('Host record')
```

python API for https://www.dnspod.com/docs/index.html

