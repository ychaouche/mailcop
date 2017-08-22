# mailcop

 Monitor dovecot/postfix auth attacks in realtime with this simple script :
```
root@messagerie[10.10.10.19] ~/SCRIPTS/MAIL # ./mailcop
Aug 22 18:06:02 a.chaouche@mydomain.tld             221.7.96.91:
Aug 22 18:06:07 adiomitidjann@mydomain.tld          58.20.55.71:
Aug 22 18:06:08 adioelbahdja@mydomain.tld           218.107.46.228:
Aug 22 18:06:24 a.chaouche                               124.207.250.89:
Aug 22 18:06:30 adioelbahdja                             63.227.77.160:
Aug 22 18:06:31 adiomitidjann                            221.215.106.218:
Aug 22 18:07:08 jon                                      105.226.222.70:
Aug 22 18:07:21 aisonradio@mydomain.tld             111.1.89.230:
Aug 22 18:07:31 aine3@mydomain.tld                  36.34.121.162:
Aug 22 18:07:44 aisonradio                               222.137.252.18:
Aug 22 18:07:44 a.chaouche@mydomain.tld             183.234.60.38:   
```
Requirements:

    You need to set auth_debug = yes in /etc/dovecot/conf.d/10-logging.conf
    If you want to also monitor SASL attacks, you need to configure your SMTP server to use dovecot for SASL authentication.

Stats :

You can get stats with mailcop-ips, mailcop-logins and mailcop-countries

Here's how they work : You first generate stats with mailcop-gen, then call any of the scripts mailcop-ips, mailcop-logins or mailcop-countries.

Example :
```
root@messagerie[10.10.10.19] ~/SCRIPTS/MAIL # ./mailcop-gen
Generating /tmp/failures
/tmp/failures generated.
now run mailcop
root@messagerie[10.10.10.19] ~/SCRIPTS/MAIL # ./mailcop-ips 
[...]
218.107.46.228          2  CN, China
218.207.74.9            2  CN, China
218.60.28.126           2  CN, China
221.1.107.142           2  CN, China
36.34.121.162           2  CN, China
60.13.230.163           2  CN, China
80.82.65.56             2  NL, Netherlands
94.102.51.31            2  NL, Netherlands
192.168.100.82          17  IP Address not found
```
Most attacks seem to come from China
```
root@messagerie[10.10.10.19] ~/SCRIPTS/MAIL # ./mailcop-countries 
[...]
      2  Taiwan
      2  Turkey
      3  India
      4  Netherlands
      4  Vietnam
      6  Brazil
      7  Russian Federation
      7  United States
     59  China
     
```
And here I can see what accounts are trying to be hacked the most :
```
root@messagerie[10.10.10.19] ~/SCRIPTS/MAIL # ./mailcop-logins 
[...]
      4 aine3
      4 aine3@mydomain.tld
      4 aisonradio
      4 aisonradio@mydomain.tld
      4 blanc_antenne
      4 blanc_antenne@mydomain.tld
      4 blank_control@mydomain.tld
      5 a.chaouche
     17 application@mydomain.tld
root@messagerie[10.10.10.19] ~/SCRIPTS/MAIL # 
```

Questions / criticisme to @ychaouche
