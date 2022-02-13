# Email-Server

Server
```
sudo hostnamectl set-hostname name.domain
sudo su -
apt update && apt upgrade -y
apt -y install postfix postfix-doc dovecot-imapd dovecot-pop3d sasl2-bin
 - enta.pt
dpkg-reconfigure postfix
 - domain
 - ubuntu (in these case is ubuntu)
 - Skip
 - No
 - Clear the line
 - 0
 - +
 - ipv4
nano /etc/postfix/main.cf
 - Change the crt and the key
nano /etc/postfix/master.cf
 - uncomment on 1º Group:
   submission inet n       -       y       -       -       smtpd
 -o syslog_name=postfix/submission
 -o smtpd_tls_security_level=encrypt
 -o smtpd_sasl_auth_enable=yes
 -o smtpd_client_restrictions=permit_sasl_authenticated,reject
 change the relay to client on that line.
          
Do the same thing on the 2º Group

 - uncomment on 2º Group:
 smtps     inet  n       -       y       -       -       smtpd
 -o syslog_name=postfix/smtps
 -o smtpd_tls_wrappermode=yes
 -o smtpd_sasl_auth_enable=yes
 -o smtpd_client_restrictions=permit_sasl_authenticated,reject
 change the relay to client on that line.

nano /etc/postfix/sasl/smtpd.conf
  - Add
  pwcheck_method: saslauthd
  mech_list: PLAIN LOGIN
 
nano /etc/default/saslauthd
  - Change
  START=yes
  OPTIONS="-c -m /var/spool/postfix/var/run/saslauthd"
    
postconf -e "home_mailbox = Maildir/"

nano /etc/dovecot/conf.d/10-auth.conf
  - Uncomment and chango to no
  disable_plaintext_auth = no
 
nano /etc/dovecot/conf.d/10-mail.conf
  - Uncomment
  mail_location = maildir:~/Maildir
  - comment
  #mail_location = mbox:~/mail:INBOX=/var/mail/%u
  
nano /etc/dovecot/conf.d/10-ssl.conf
  change ssl = yes
  change your crts and keys

adduser postfix sasl
adduser dovecot sasl

systemctl restart saslauthd.service postfix dovecot

sudo su -
cd /etc/skel
  - maildirmake.dovecot Maildir
  
adduser (your user)
```
Client
```
sudo hostnamectl set-hostname (your name and the server domain)
apt update && apt upgrade -y
adduser (your users)
cp /home/ubuntu/.xsession /home/youruser/.xsession
chown youruser:youruser /home/youruser/.xsession
```
You need to install the GUI
