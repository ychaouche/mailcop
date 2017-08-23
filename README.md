# mailcop

 Monitor dovecot/postfix auth attacks in realtime with this simple script :
```
root@messagerie[10.10.10.19] ~/SCRIPTS/MAIL # ./mailcop
Aug 22 18:06:02 a.chaouche@mydomain.tld             221.7.96.91:
Aug 22 18:06:07 adiomitidjann@mydomain.tld          58.20.55.71:
Aug 22 18:06:08 adioelbahdja@mydomain.tld           218.107.46.228:
Aug 22 18:06:24 a.chaouche                          124.207.250.89:
Aug 22 18:06:30 adioelbahdja                        63.227.77.160:
Aug 22 18:06:31 adiomitidjann                       221.215.106.218:
Aug 22 18:07:08 jon                                 105.226.222.70:
Aug 22 18:07:21 aisonradio@mydomain.tld             111.1.89.230:
Aug 22 18:07:31 aine3@mydomain.tld                  36.34.121.162:
Aug 22 18:07:44 aisonradio                          222.137.252.18:
Aug 22 18:07:44 a.chaouche@mydomain.tld             183.234.60.38:   
```
##Requirements
You need to set auth_debug = yes in /etc/dovecot/conf.d/10-logging.conf
If you want to also monitor SASL attacks, you need to configure your SMTP server to use dovecot for SASL authentication.
You'll also need to install the geoiplookup command

##Installation
No installation required, just download the scripts in some directory, then either execute `mailcop` for realtime analysis or `mailcop-gen` and `mailcop-static` for static analysis of past attacks. You can also call any of `mailcop-ips`, `mailcop-logins` or `mailcop-countries` for stats about IP, logins and countries that attacks originate from. 

##Stats

You can get stats with mailcop-ips, mailcop-logins and mailcop-countries

Here's how they work : You first generate stats with mailcop-gen, then call any of the scripts mailcop-ips, mailcop-logins or mailcop-countries.

##Example
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

## The daily cronjob

In the etc directory you'll find a mailcop cron job that would send daily statistics about the failed authentication attempts on your mail server. Here's an example mail it sends : 

```
Mailcop
=======
Statistiques des dernières attaques sur notre serveur de messagerie
Début : Aug 22 16:31:09
Fin   : Aug 23 11:59:28


Top 10 des pays
---------------
    308  China
     33  Brazil
     26  Russian Federation
     19  United States
     18  Taiwan
     15  Mexico
     14  India
      9  Korea
      8  Vietnam
      8  Argentina



Top 10 des addresses IP
-----------------------
192.168.100.82   184 IP Address not found
80.82.78.104      15 NL, Netherlands  
80.82.65.56       12 NL, Netherlands  
81.45.170.67      10 ES, Spain  
181.209.223.108    9 GT, Guatemala  
94.102.51.31       8 NL, Netherlands  
88.220.44.47       8 PL, Poland  
218.107.46.228     8 CN, China  
191.96.249.65      8 CL, Chile  
59.125.182.141     7 TW, Taiwan  



Top 10 des logins utilisés
--------------------------
    184 application@mydomain.tld
     29 blanc_antenne@mydomain.tld
     28 blank_control@mydomain.tld
     27 blank_control
     27 blanc_antenne
     20 digi
     20 a.chaouche
     19 info@mydomain.tld
     19 info
     19 chaine3@mydomain.tld



Dernières attaques
------------------
Aug 23 11:56:27 application@mydomain.tld            192.168.100.82:
Aug 23 11:56:41 ray                                 187.94.111.100:
Aug 23 11:57:03 blank_control                       115.114.40.105:
Aug 23 11:58:32 chaine2@mydomain.tld                122.255.115.242:
Aug 23 11:58:32 chaine3@mydomain.tld                59.127.65.34:
Aug 23 11:58:32 chaine1@mydomain.tld                221.207.29.94:
Aug 23 11:58:59 chaine2                             179.185.174.73:
Aug 23 11:59:12 veritas                             12.130.172.232:
Aug 23 11:59:13 chaine1                             5.228.18.204:
Aug 23 11:59:28 chaine3                             58.22.194.44:

```


Questions / criticisme to @ychaouche
