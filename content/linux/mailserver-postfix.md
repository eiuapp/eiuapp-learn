---
title: "Mailserver Postfix"
date: 2018-06-16T22:41:53+08:00
draft: true
tags: ["mailserver", "postfix"]
categories: ["server"]
subtitle: "ubuntu下用postfix搭建邮件服务器"
descriptions: ""
bigimg:
---

## Env

- os: ubuntu16

## Step

1666  apt install xview-clients
 1667  clear
 1668  date
 1669  clock -w
 1670  date --date 14:45:00
 1671  date
 1672  date -s 14:45:00
 1673  date
 1674  ls
 1675  crontab -e
 1676  ls
 1677  netstat -ntpl
 1678  cd /home/dev/trackable/
 1679  ls
 1680  tail -F nohup.out
 1681  ls
 1682  ./trackabled stop
 1683  nohup ./trackabled --load &
 1684  tail -F nohup.out
 1685  netstat -ntpl
 1686  cd /home/dev/trackable/
 1687  ls
 1688  nohup ./trackabled --load &
 1689  tail -F nohup.out
 1690  exit
 1691  cd ss-fly/
 1692  ls
 1693  cd ..
 1694  ls
 1695  cd ss-fly/
 1696  ls
 1697  cd ..
 1698  ls
 1699  rm -rf ss-fly/
 1700  ls
 1701  clear
 1702  sudo service postfix restart
 1703  sudo dpkg-reconfigure postfix
 1704  ls
 1705  mkdir ssl
 1706  cd ss
 1707  cd ssl
 1708  sudo openssl req -x509 -nodes -newkey rsa:2048 -keyout mailserver.key -out mailserver.crt -nodes -days 365
 1709  ls
 1710  sudo openssl req -new -x509 -extensions v3_ca -keyout cakey.pem -out cacert.pem -days 3650
 1711  ls
 1712  ls -a
 1713  sudo openssl req -new -x509 -extensions v3_ca -keyout cakey.pem -out cacert.pem -days 3650
 1714  ls
 1715  ls -a
 1716  ls
 1717  cd ..
 1718  ls
 1719  sudo openssl req -new -x509 -extensions v3_ca -keyout cakey.pem -out cacert.pem -days 3650
 1720  ls
 1721  mv ca* ssl/
 1722  cd ssl/
 1723  ls
 1724  ls /etc/postfix
 1725  ls /etc/postfix/sasl/
 1726  ls
 1727  cd ..
 1728  mv ssl/ /etc/postfix/
 1729  ls /etc/postfix/
 1730  ls /etc/postfix/ssl/
 1731  sudo postconf -e 'smtpd_sasl_local_domain ='
 1732  sudo postconf -e 'smtpd_sasl_auth_enable = yes'
 1733  sudo postconf -e 'smtpd_sasl_security_options = noanonymous'
 1734  sudo postconf -e 'broken_sasl_auth_clients = yes'
 1735  sudo postconf -e 'smtpd_recipient_restrictions =  permit_sasl_authenticated,permit_mynetworks,reject_unauth_destination'
 1736  sudo postconf -e 'inet_interfaces = all'
 1737  sudo postconf -e 'smtp_tls_security_level = may'
 1738  sudo postconf -e 'smtpd_tls_security_level = may'
 1739  sudo postconf -e 'smtpd_tls_auth_only = no'
 1740  sudo postconf -e 'smtp_tls_note_starttls_offer = yes'
 1741  sudo postconf -e 'smtpd_tls_key_file = /etc/postfix/ssl/mailserver.key'
 1742  sudo postconf -e 'smtpd_tls_cert_file = /etc/postfix/ssl/mailserver.crt'
 1743  sudo postconf -e 'smtpd_tls_CAfile = /etc/postfix/ssl/cacert.pem'
 1744  sudo postconf -e 'smtpd_tls_loglevel = 1'
 1745  sudo postconf -e 'smtpd_tls_received_header = yes'
 1746  sudo postconf -e 'smtpd_tls_session_cache_timeout = 3600s'
 1747  sudo postconf -e 'tls_random_source = dev:/dev/urandom'
 1748  vi /etc/hosts
 1749  cat /etc/hostname
 1750  sudo postconf -e 'myhostname = bitzone.zone'
 1751  vi /etc/postfix/sasl/smtpd.conf
 1752  sudo systemctl restart postfix
 1753  sudo apt-get install libsasl2-2 sasl2-bin libsasl2-modules -y
 1754  vi /etc/default/saslauthd
 1755  cat /etc/default/saslauthd
 1756  head /etc/default/saslauthd
 1757  vi /etc/default/saslauthd
 1758  sudo dpkg-statoverride --force --update --add root sasl 755 /var/spool/postfix/var/run/saslauthd
 1759  sudo ln -s /etc/default/saslauthd /etc/saslauthd
 1760  which saslauthd
 1761  sudo /etc/init.d/saslauthd start
 1762  which telnet
 1763  telnet localhost 25
 1764  ehlo localhost
 1765  cat /etc/lsb-release
 1766  telnet localhost 25
 1767  mail
 1768  ls
 1769  ls /root/Maildir/new
 1770  ls
 1771  telnet localhost 25
 1772  mail
 1773  ls
 1774  ls -a
 1775  ls
 1776  ls /var/mail/root
 1777  ls
 1778  vi /var/mail/root
 1779  ls
 1780  sudo apt-get install dovecot-core dovecot-imapd -y
 1781  sudo systemctl status dovecot
 1782  sudo chmod 777 /var/mail
 1783  sudo apt-get install squirrelmail -y
 1784  ls /etc/apache2/sites-available/example.com.conf
 1785  sudo apt-get install apache2 php5 -y
 1786  sudo apt-get install apache2
 1787  ls /etc/apache2/sites-available/example.com.conf
 1788  ls /etc/apache2/
 1789  ls /etc/apache2/sites-available/ -l
 1790  ls
 1791  cp -a /etc/apache2/sites-available/ .
 1792  ls
 1793  mv /etc/apache2/sites-available/default-ssl.conf /etc/apache2/sites-available/example.com.conf
 1794  vi /etc/apache2/sites-available/example.com.conf
 1795  cd /etc/apache2/
 1796  ls
 1797  grep 80 -rl ./
 1798  vi sites-available/000-default.conf
 1799  ls /usr/share/squirrelmail
 1800  sudo systemctl restart apache2
 1801  ls
 1802  cd sites-available/
 1803  ls
 1804  vi 000-default.conf
 1805  ls
 1806  ls /usr/share/squirrelmail
 1807  vi 000-default.conf
 1808  ls /var/www/html
 1809  vi /var/www/html/index.
 1810  vi /var/www/html/index.html
 1811  cd ..
 1812  grep 80 -rl ./
 1813  grep 80 -rn ./
 1814  vi ports.conf
 1815  ls
 1816  cd -
 1817  ls
 1818  vi 000-default.conf
 1819  sudo apt search php
 1820  sudo apt install php5
 1821  sudo apt update
 1822  sudo apt install php5
 1823  sudo apt-get install -y language-pack-en-base
 1824  作者：binvec
 1825  链接：https://www.zhihu.com/question/45999546/answer/100165171
 1826  来源：知乎
 1827  sudo apt update
 1828  sudo apt install php5
 1829  sudo apt-get install php5.5-common
 1830  sudo LC_ALL=en_US.UTF-8 add-apt-repository ppa:ondrej/php
 1831  sudo apt-get install -y language-pack-en-base
 1832  sudo  add-apt-repository ppa:ondrej/php
 1833  apt-get install software-properties-common
 1834  sudo  add-apt-repository ppa:ondrej/php
 1835  sudo LC_ALL=en_US.UTF-8 add-apt-repository ppa:ondrej/php
 1836  sudo add-apt-repository ppa:ondrej/php
 1837  sudo apt-get update
 1838  sudo apt-get install php5.5-common
 1839  sudo add-apt-repository ppa:ondrej/php
 1840  sudo apt-get update
 1841  apt-cache search php5
 1842  apt-cache search php5 | grep php5.6-common
 1843  sudo apt-get install php5.6-common
 1844  sudo apt-get install libapache2-mod-php5.6
 1845  sudo systemctl restart apache2
 1846  cd /var/www/html/
 1847  ls
 1848  vi a.html
 1849  curl http://127.0.0.1/a.html
 1850  netstat -tlnp | grep 80
 1851  sudo systemctl stop nginx
 1852  sudo systemctl restart apache2
 1853  cd ..
 1854  ls
 1855  cd
 1856  ls
 1857  history
root@ripple01:~#


root@ripple01:~# sudo useradd myusername
root@ripple01:~# sudo passwd myusername
Enter new UNIX password:
Retype new UNIX password:
passwd: password updated successfully
root@ripple01:~# sudo mkdir -p /var/www/html/myusername
root@ripple01:~# usermod -m -d /var/www/html/myusername myusername
usermod: directory /var/www/html/myusername exists
root@ripple01:~# sudo chown -R myusername:myusername /var/www/html/myusername
root@ripple01:~# su - test2
test2@ripple01:~$ mail
s-nail version v14.8.6.  Type ? for help.
"/var/mail/test2": 1 message 1 new
>N  1 myusername@bitzone Sat Jun 16 10:40   26/886   &#26089;&#28857;&#30561;&#35273;
? 1
[-- Message  1 -- 26 lines, 886 bytes --]:
From myusername@bitzone.zone  Sat Jun 16 10:40:58 2018
Message-ID: <a78f9dea944b68cfe221e531d9b3c27f.squirrel@47.52.242.6>
Date: Sat, 16 Jun 2018 10:40:58 +0800
Subject: &#26089;&#28857;&#30561;&#35273;
From: myusername@bitzone.zone
To: test2@localhost

&#26089;&#28857;&#30561;&#35273;


? q
Held 1 message in /var/mail/test2
test2@ripple01:~$


## Ref

- https://www.1and1.com/cloud-community/learn/application/e-mail/set-up-a-postfix-mail-server-with-dovecot-and-squirrelmail-on-ubuntu-1604/
- https://www.tecmint.com/setup-postfix-mail-server-in-ubuntu-debian/