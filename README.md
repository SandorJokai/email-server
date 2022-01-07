![npm package](https://img.shields.io/badge/centos-7.9.2009-purple.svg)
![npm package](https://img.shields.io/badge/postfix-2.10.1-silver.svg)
![npm package](https://img.shields.io/badge/dovecot-2.2.36-cyan.svg)
![npm package](https://img.shields.io/badge/mariadb-5.5.68-orange.svg)
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
<h4>Let's install first among with the other packages:</h4>

```bash
yum update && yum install postfix dovecot dovecot-mysql spamassassin clamav clamav-scanner-systemd clamav-data clamav-update mariadb-server
```
Once installed, start it and setup for start automatically from the next reboot.
Run the 'mysql_secure_installation' command and set everything yes (disallow) except "Disallow root login remotely?", otherwise we'd need to create another
user account.

<h4>Create the database and the tables</h4>

```bash
MariaDB [(none)]> create database EmailServer_db;
```

```bash
MariaDB [(none)]> CREATE TABLE `EmailServer_db`.`Domains_tbl` ( `DomainId` INT NOT NULL AUTO_INCREMENT , `DomainName` VARCHAR(50) NOT NULL , PRIMARY KEY (`DomainId`)) ENGINE = InnoDB;
```

```bash
MariaDB [(none)]> CREATE TABLE `EmailServer_db`.`Users_tbl` ( `UserId` INT NOT NULL AUTO_INCREMENT, `DomainId` INT NOT NULL, `password` VARCHAR(106) NOT NULL, `Email` VARCHAR(100) NOT NULL, PRIMARY KEY (`UserId`), UNIQUE KEY `Email` (`Email`), FOREIGN KEY (DomainId) REFERENCES Domains_tbl(DomainId) ON DELETE CASCADE ) ENGINE = InnoDB;
```
(Note that using varchar(!106) caused password mismatch as the return hash value is 106, needs to be set to 106 at least!)

```bash
MariaDB [(none)]> CREATE TABLE `EmailServer_db`.`Alias_tbl` ( `AliasId` INT NOT NULL AUTO_INCREMENT, `DomainId` INT NOT NULL, `Source` varchar(100) NOT NULL, `Destination` varchar(100) NOT NULL, PRIMARY KEY (`AliasId`), FOREIGN KEY (DomainId) REFERENCES Domains_tbl(DomainId) ON DELETE CASCADE ) ENGINE = InnoDB;
```


<h4>Once the tables has created, let's upload some details:</h4>

```bash
MariaDB [(none)]> use database EmailServer_db;
MariaDB [(none)]> INSERT INTO Domains_tbl (DomainName) VALUES ('domain.tld');
```

```bash
MariaDB [(none)]> INSERT INTO Alias_tbl (DomainId, Source, Destination) VALUES (1, 'info@domain.tld', 'administrator@domain.tld');
```

(The role of alias if somebody send an email to 'info@domain.tld', it'll be received in 'administrator@domain.tld' mailbox eventually.)

```bash
MariaDB [(none)]> INSERT INTO Users_tbl (DomainId, password, Email) VALUES (1, ENCRYPT('secretPassword', CONCAT('$6$', SUBSTRING(SHA(RAND()), -16))), 'user@domain.tld');
```

![npm package](https://img.shields.io/badge/centos-7.9.2009-purple.svg)
![npm package](https://img.shields.io/badge/postfix-2.10.1-silver.svg)
![npm package](https://img.shields.io/badge/dovecot-2.2.36-cyan.svg)
![npm package](https://img.shields.io/badge/mariadb-5.5.68-orange.svg)
![npm package](https://img.shields.io/badge/spamassassin-3.4.0-pink.svg)
![npm package](https://img.shields.io/badge/opendkim-2.11.0-yellow.svg)
![npm package](https://img.shields.io/badge/clamav-0.103.4-red.svg)
