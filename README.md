![npm package](https://img.shields.io/badge/centos-7.9.2009-purple.svg)
![npm package](https://img.shields.io/badge/postfix-2.10.1-grey.svg)
![npm package](https://img.shields.io/badge/dovecot-2.2.36-cyan.svg)
![npm package](https://img.shields.io/badge/mariadb-5.5.68-brown.svg)
![npm package](https://img.shields.io/badge/spamassassin-3.4.0-pink.svg)
![npm package](https://img.shields.io/badge/opendkim-2.11.0-yellow.svg)
![npm package](https://img.shields.io/badge/clamav-0.103.4-red.svg)

<h2>Prerequisites</h2>
  - Create a public domain if not exists yet
  - with an A and MX and TXT records
  - root privileges on the workstation (server)

<h2>Install and configure mysql</h2>
We need a place to store our client's informations (email addresses etc.). I have chosen mariadb for that purpose, since its fast, easy to setup
and secure.
The commands below represents how to setup and configure:
<h1>Let's install first among with the other packages:</h1>
```bash
yum update && yum install postfix dovecot dovecot-mysql spamassassin clamav clamav-scanner-systemd clamav-data clamav-update mariadb-server
```
