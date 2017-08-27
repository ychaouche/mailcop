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
## Requirements
 * You need to set auth_debug = yes in /etc/dovecot/conf.d/10-logging.conf

 * If you want to also monitor SASL attacks, you need to configure your SMTP server to use dovecot for SASL authentication.

 * You'll also need to install the geoiplookup command
 
 * If you're using fail2ban, the script also shows last banned IPs but you need to install shorewall and configure fail2ban actions to use `shorewall drop` or `shorewall logdrop`.

## Installation
No installation required, just download the scripts in some directory, then either execute `mailcop` for realtime analysis or `mailcop-gen` and `mailcop-static` for static analysis of past attacks. You can also call any of `mailcop-ips`, `mailcop-logins` or `mailcop-countries` for stats about IP, logins and countries that attacks originate from. 

## Stats

You can get stats with mailcop-ips, mailcop-logins and mailcop-countries

Here's how they work : You first generate stats with mailcop-gen, then call any of the scripts mailcop-ips, mailcop-logins or mailcop-countries.

## Example
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

mailcop-mailer is meant to be run as a cron job that would send daily statistics about the failed authentication attempts on your mail server. Here's an example mail it sends : 

```
Mailcop
=======
Statistiques des dernières attaques sur notre serveur de messagerie
Début : Aug 27 06:26:24
Fin   : Aug 27 16:37:47


Top 10 des pays
---------------
     10  Brazil
      9  China
      6  Mexico
      5  Colombia
      5  Chile
      4  United States
      3  Vietnam
      3  Taiwan
      3  South Africa
      2  Turkey



Top 10 des addresses IP
-----------------------
192.168.100.82    96 IP Address not found
10.10.10.19       38 IP Address not found
52.169.26.62       6 US, United States
41.73.125.74       5 ML, Mali  
93.149.107.116     4 IT, Italy  
80.147.168.27      4 DE, Germany  
59.100.16.223      4 AU, Australia  
37.211.146.162     4 QA, Qatar  
37.189.246.168     4 PT, Portugal  
31.149.66.65       4 NL, Netherlands  



Top 10 des logins utilisés
--------------------------
     96 application@domain.tld
     15 n.chabi@algrian-radio.dz
     10 radiobahdja16@gmail.com
      4 dafchaine1@algerien-radio.dz
      3 test
      3 postmaster
      3 dafchaine1@eprs.dz
      2 student
      2 spam
      2 sales



Dernières attaques
------------------
Aug 27 16:18:00 training                          190.147.156.214:
Aug 27 16:21:25 application@domain.tld            192.168.100.82:
Aug 27 16:21:25 student                           190.67.161.242:
Aug 27 16:24:28 ftpuser                           86.35.227.122:
Aug 27 16:27:43 application@domain.tld            192.168.100.82:
Aug 27 16:27:56 webmaster                         41.180.72.44:
Aug 27 16:31:28 audit                             201.131.240.134:
Aug 27 16:34:01 application@domain.tld            192.168.100.82:
Aug 27 16:34:27 internet                          52.174.4.62:
Aug 27 16:37:47 service                           80.147.168.27:



Les 10 dernières addresses bloquées
-----------------------------------
 pkts bytes target     prot opt in     out     source               destination         
    1    60 reject     all  --  *      *       222.162.70.249       0.0.0.0/0           
    0     0 reject     all  --  *      *       60.216.106.162       0.0.0.0/0           
    0     0 reject     all  --  *      *       118.163.58.117       0.0.0.0/0           
    0     0 reject     all  --  *      *       218.62.67.138        0.0.0.0/0           
    0     0 reject     all  --  *      *       122.146.88.186       0.0.0.0/0           
    0     0 reject     all  --  *      *       211.141.174.226      0.0.0.0/0           
    0     0 reject     all  --  *      *       61.163.231.213       0.0.0.0/0           
    0     0 reject     all  --  *      *       221.228.229.49       0.0.0.0/0           
   21  1064 reject     all  --  *      *       200.49.145.161       0.0.0.0/0           

```

# Contact

Questions / criticisme to @ychaouche
