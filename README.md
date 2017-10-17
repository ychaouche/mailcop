# mailcop

 Monitor dovecot/postfix auth attacks in realtime with this simple script :
```
root@messagerie[10.10.10.19] ~ # /root/SCRIPTS/MAIL/mailcop
Oct 17 10:57:07 mantenimiento                            61.69.108.150:
Oct 17 11:01:20 marco                                    117.56.187.240:
Oct 17 11:03:37 margo                                    177.179.246.135:
Oct 17 11:08:10 marianne                                 177.179.98.220:
Oct 17 11:10:16 marion                                   115.75.0.178:
Oct 17 11:11:00 k.benismael@mydomain.tld                 10.10.10.19:
Oct 17 11:15:06 boultiour.mess@mydomain.tld              110.249.221.130:
Oct 17 11:15:39 boultiour.mess                           61.185.139.72:
Oct 17 11:17:02 mary                                     177.43.213.86:
Oct 17 11:23:41 matt                                     201.33.193.166:
Oct 17 11:28:01 i.chaouati@mydomain.tld                  10.10.10.19:
Oct 17 11:32:33 mb                                       91.237.124.222:
Oct 17 11:34:54 mcbride                                  187.94.111.100:
Oct 17 11:43:32 michel                                   179.185.95.114:
Oct 17 11:46:37 i.aitahmed                               10.10.10.19:
Oct 17 11:52:10 monica                                   200.49.145.161:
Oct 17 11:54:23 moreno                                   190.86.183.117:
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
154.235.28.12           3  IP Address not found
179.185.95.114          3  BR, Brazil
84.241.175.107          3  NL, Netherlands
91.237.124.222          3  UA, Ukraine
171.11.77.194           5  CN, China
10.10.10.19             8  IP Address not found
```

If one IP in particular catches your attention, you can list its attacks by supplying an argument : 

```
root@messagerie[10.10.10.19] ~/SCRIPTS/MAIL # ./mailcop-ips 84.241.175.107
Oct 17 06:27:47 dummy
Oct 17 07:48:37 gavin
Oct 17 09:42:42 kato
root@messagerie[10.10.10.19] ~/SCRIPTS/MAIL # 
```

You can also list attacks by country

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

By supplying an argument, I can also track when did attacks occure with a specific login : 

```
root@messagerie[10.10.10.19] ~/SCRIPTS/MAIL # ./mailcop-logins hamel
Oct 17 10:14:43 y.hamel@mydomain.tld
Oct 17 10:15:16 y.hamel
Oct 16 11:00:27 y.hamel@mydomain.tld
Oct 16 11:01:26 y.hamel
Oct 16 20:13:59 y.hamel@mydomain.tld
Oct 16 20:14:35 y.hamel
Oct 17 03:19:22 y.hamel@mydomain.tld
Oct 17 03:19:49 y.hamel
Oct 15 13:03:25 y.hamel@mydomain.tld
Oct 15 13:03:59 y.hamel
Oct 15 20:16:09 y.hamel@mydomain.tld
Oct 15 20:16:39 y.hamel
Oct 16 03:28:14 y.hamel@mydomain.tld
Oct 16 03:29:05 y.hamel
Oct 14 07:36:11 y.hamel@mydomain.tld
Oct 14 07:36:39 y.hamel
Oct 14 14:24:49 y.hamel@mydomain.tld
Oct 14 14:27:18 y.hamel
Oct 14 21:11:42 y.hamel@mydomain.tld
Oct 14 21:12:15 y.hamel
Oct 13 11:47:13 y.hamel@mydomain.tld
Oct 13 11:47:50 y.hamel
Oct 13 18:49:22 y.hamel@mydomain.tld
Oct 13 18:49:53 y.hamel
Oct 14 01:04:20 y.hamel@mydomain.tld
Oct 14 01:04:50 y.hamel
Oct 12 11:10:00 y.hamel@mydomain.tld
Oct 12 11:10:28 y.hamel
Oct 12 18:01:17 y.hamel@mydomain.tld
Oct 12 18:02:00 y.hamel
Oct 13 00:15:44 y.hamel@mydomain.tld
Oct 13 00:16:14 y.hamel
Oct 11 07:10:16 y.hamel@mydomain.tld
Oct 11 07:10:43 y.hamel
Oct 11 17:56:40 y.hamel@mydomain.tld
Oct 11 17:57:06 y.hamel
Oct 12 04:17:56 y.hamel@mydomain.tld
Oct 12 04:18:24 y.hamel
Oct 10 18:46:53 y.hamel@mydomain.tld
Oct 10 18:47:31 y.hamel
root@messagerie[10.10.10.19] ~/SCRIPTS/MAIL # 
```

The last command was operated on multiple files with ``mailcop-gen /var/log/dovecot.log*``


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
