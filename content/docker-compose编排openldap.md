---
title: "docker-compose编排openldap"
date: 2021-03-29T23:52:59+08:00
draft: flase
---

统一认证 openldap
gitlab cicd


ldapadminphp

gitlab

yapi

zentao/tapd


宝塔

docker-compose.yaml 
```

version: '2'
services:
  openldap:
    image: osixia/openldap:1.5.0
    container_name: openldap
    environment:
      LDAP_LOG_LEVEL: "256"
      LDAP_ORGANISATION: "abc Inc."
      LDAP_DOMAIN: "abc.com"
      LDAP_BASE_DN: ""
      LDAP_ADMIN_PASSWORD: "xxxxxxxxxxxxxxxx"
      LDAP_CONFIG_PASSWORD: "config"
      LDAP_READONLY_USER: "false"
      #LDAP_READONLY_USER_USERNAME: "readonly"
      #LDAP_READONLY_USER_PASSWORD: "readonly"
      LDAP_RFC2307BIS_SCHEMA: "false"
      LDAP_BACKEND: "mdb"
      #LDAP_TLS: "false"
      #LDAP_TLS_CRT_FILENAME: "ldap.crt"
      #LDAP_TLS_KEY_FILENAME: "ldap.key"
      #LDAP_TLS_DH_PARAM_FILENAME: "dhparam.pem"
      #LDAP_TLS_CA_CRT_FILENAME: "ca.crt"
      #LDAP_TLS_ENFORCE: "false"
      #LDAP_TLS_CIPHER_SUITE: "SECURE256:-VERS-SSL3.0"
      #LDAP_TLS_VERIFY_CLIENT: "demand"
      LDAP_REPLICATION: "false"
      #LDAP_REPLICATION_CONFIG_SYNCPROV: 'binddn="cn=admin,cn=config" bindmethod=simple credentials="$$LDAP_CONFIG_PASSWORD" searchbase="cn=config" type=refreshAndPersist retry="60 +" timeout=1 starttls=critical'
      #LDAP_REPLICATION_DB_SYNCPROV: 'binddn="cn=admin,$$LDAP_BASE_DN" bindmethod=simple credentials="$$LDAP_ADMIN_PASSWORD" searchbase="$$LDAP_BASE_DN" type=refreshAndPersist interval=00:00:00:10 retry="60 +" timeout=1 starttls=critical'
      #LDAP_REPLICATION_HOSTS: "#PYTHON2BASH:['ldap://ldap.example.org','ldap://ldap2.example.org']"
      KEEP_EXISTING_CONFIG: "false"
      LDAP_REMOVE_CONFIG_AFTER_SETUP: "true"
      #LDAP_SSL_HELPER_PREFIX: "ldap"
    tty: true
    stdin_open: true
    volumes:
      - ./data/ldap:/var/lib/ldap
      - ./data/slapd.d:/etc/ldap/slapd.d
      - ./data/certs:/container/service/slapd/assets/certs/
    ports:
      - "389:389"
      - "636:636"
    # For replication to work correctly, domainname and hostname must be
    # set correctly so that "hostname"."domainname" equates to the
    # fully-qualified domain name for the host.
    domainname: "abc.com"
    hostname: "ldap-server"
  phpldapadmin:
    image: osixia/phpldapadmin:latest
    container_name: phpldapadmin
    environment:
      PHPLDAPADMIN_LDAP_HOSTS: "openldap"
      PHPLDAPADMIN_HTTPS: "false"
    ports:
      - "8080:80"
    depends_on:
      - openldap

        #volumes:
        #  ldap_storge:
        #    driver: local       

```
