# Docker Registry with existing LDAP Integration

## Introduction

This is a docker application to run Docker Registry behind an Nginx in order to obtain an authentication method using a company Active Directory.

## Before Running
- Rename conf/nginx.conf.template in conf/nginx.conf replacing value of you Active Directory.
```
    ldap_server ldapserver {
	    url ldap://<active_directory_ip>:<active_directory_port>/OU=Users,DC=<domain>,DC=it?samaccountname?sub?(objectClass=user);
        binddn <ldap_user>@<domain>;
        binddn_passwd <ldap_user_password>
        group_attribute uniquemember;
        group_attribute_is_dn on;
    }
```
and
```
  ...
  server_name <fully_domain_qualified_name_server>;
  ...
  location / {
    return 301 https://<fully_domain_qualified_name_server>/v2;
  }
```
- Put right SSL certificate for your domain in certs folder (The certificate present are self-signed)

## Running

Run in this directory:

```
$ docker-compose up -d
```

Now you can point your docker registry with its fdqn with https protocol:

```
docker login https://<fully_domain_qualified_name_server>
```

## License

Copyright 2016 Tierra SpA

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

[http://www.apache.org/licenses/LICENSE-2.0](http://www.apache.org/licenses/LICENSE-2.0)

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
