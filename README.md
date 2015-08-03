Role Name
=========

Install, configure and provision a Full Zimbra Server.

Requirements
------------

* CentOS 6, 7
* RHEL 6, 7
* Correctly configure DNS
* Correctly configued `/etc/hosts` file

Role Variables
--------------

* `zimbra_download_url`, URL to download Zimbra
* `zimbra_file`, name of the downloaded file
* `zimbra_256sum_file`, `SHA256SUM` of the file
* `zimbra_password`, password for admin and everything
* `zimbra_default_domain`, default domain to create

**zimbra_domains**

* `name`, name of a domain
* `accounts`, Array of accounts
* `distribution_lists`, Array of Distribution Lists

**accounts**

* `name`, email of the account
* `password`, if empty the default pass is `12345678`

**distribution_lists**

* `name`, email of the Distribution List
* `members`, Array of email addresses of members
* `authorized_senders`, Array of domain accounts who can send email to the list

Example Playbook
----------------

This is an example Playbook that I use for development using Vagrant:

```yaml
---
- hosts: all
  sudo: yes
  vars:
    zimbra_download_url: https://files.zimbra.com/downloads/8.6.0_GA/zcs-8.6.0_GA_1153.RHEL6_64.20141215151155.tgz
    zimbra_file: zcs-8.6.0_GA_1153.RHEL6_64.20141215151155
    zimbra_256sum_file: c2278e6632b9ca72487afdf24da2545238e325338090a9d8ad6e99b39593561c
    zimbra_password: 12345678
    zimbra_default_domain: 'zboxapp.dev'

    zimbra_domains:
      - name: 'itlinux.dev'
        o: 'ITLinux'
        accounts:
          - name: 'pbruna@itlinux.dev'
            zimbra_is_admin_account: TRUE
            password: 12345678

      - name: 'it-linux.dev'
        o: 'ITLinux'

      - name: 'zbox.dev'
        o: 'ZBOX'
        accounts:
          - name: 'zboxadmin@zbox.dev'
            password: 12345678
            zimbra_is_domain_admin_account: TRUE

        distribution_lists:
          - name: 'locked@zbox.dev'
            members:
              - '1@example.com'
              - '2@example.com'
              - 'zboxadmin@zbox.dev'
            authorized_senders:
              - 'zboxadmin@zbox.dev'

      - name: 'empty.com'
        o: 'ZBOX'

  roles:
    - role: pbruna.zimbradev
```

License
-------

MIT

Author Information
------------------

Developed with love by [Patricio Bruna](https://www.twitter.com/pbruna) at [ITLinux](http://www.itlinux.cl/)

If you need to contact me for help or anything please submit an `Issue`.
