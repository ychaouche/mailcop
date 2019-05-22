# mailcop

Monitor dovecot/postfix auth attacks in realtime with this simple script :

```
root@messagerie[10.10.10.19] ~ # /root/SCRIPTS/MAIL/mailcop -f
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

## Demo
https://vimeo.com/337750376

## Requirements
 * You need to set auth_debug = yes in /etc/dovecot/conf.d/10-logging.conf

 * If you want to also monitor SASL attacks, you need to configure your SMTP server to use dovecot for SASL authentication.

 * You'll also need to install the geoiplookup command
 
 * If you're using fail2ban, the script also shows last banned IPs but you need to install shorewall and configure fail2ban actions to use `shorewall drop` or `shorewall logdrop`.

## Installation
No installation required, just download the scripts in some directory, then either execute `mailcop -f` (follow) for realtime reporting or `mailcop` and `mailcop -a` to report past attacks (-a for all attacks). You can also call any of `mailcop-ips`, `mailcop-logins` or `mailcop-countries` for stats about IP, logins and countries that attacks originate from. `mailcop-info` on any IP, country or login to get corresponding attacks. The `-a` switch works with all mailcop* commands and will scan all dovecot log files instead of just dovecot.log (usually 24h). 

## Stats

You can get stats with mailcop-ips, mailcop-logins and mailcop-countries

Here's how they look

```
root@messagerie[10.10.10.19] ~/SCRIPTS/MAIL # ./mailcop-ips 
[...]
154.235.28.12           3  IP Address not found
179.185.95.114          3  BR, Brazil
84.241.175.107          3  NL, Netherlands
91.237.124.222          3  UA, Ukraine
171.11.77.194           5  CN, China
10.10.10.19             8  IP Address not found
```

If one IP in particular catches your attention, you can list its attacks by supplying an argument to mailcop-info: 

```
root@messagerie[10.10.10.19] ~/SCRIPTS/MAIL # ./mailcop-info 84.241.175.107
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
And here I can see what accounts are trying to get hacked the most :
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

By supplying an argument to `mailcop-info`, I can also track when did attacks occure with a specific login : 

```
root@messagerie[10.10.10.19] ~/SCRIPTS/MAIL # ./mailcop-info hamel
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


## The daily cronjob

mailcop-mailer is meant to be run as a cron job that would send daily statistics about failed authentication attempts on your mail server. It simply calls the different `mailcop-*` commands and concatenates the output in a single e-mail. Here's an example mail it sends : 

```            _ _             
 _ __  __ _(_) |__ ___ _ __
| '  \/ _` | | / _/ _ \ '_ \
|_|_|_\__,_|_|_\__\___/ .__/
                      |_|   

Statistiques des dernières attaques sur notre serveur de messagerie
Début             : Oct 16 06:26:23
Fin               : Oct 17 06:12:26
Nombre d'attaques : 360


Top 10 des pays
---------------
     29  China
     16  Brazil
      7  Argentina
      6  Australia
      4  Colombia
      3  Vietnam
      3  Taiwan
      2  South Africa
      2  Peru
      2  Malaysia



Top 10 des addresses IP
-----------------------
10.10.10.19       19 IP Address not found
41.226.11.226     10 TN, Tunisia  
200.77.217.106    10 MX, Mexico  
181.143.94.74     10 CO, Colombia  
181.143.83.140     9 CO, Colombia  
118.163.4.1        8 TW, Taiwan  
103.6.12.229       8 AU, Australia  
93.207.49.135      7 DE, Germany  
61.69.110.138      7 AU, Australia  
190.2.52.53        7 AR, Argentina  



Top 10 des logins utilisés
--------------------------
      8 info
      6 radiomitidja
      5 sde
      4 t.meziane@radioalgerian-gmail.dz
      4 relex@mydomain.tld
      4 it
      4 badmin@mydomain.tld
      4 arch-admin@mydomain.tld
      3 y.hamen@mydomain.tld
      3 y.hamen



Dernières attaques
------------------
Oct 17 05:33:30 culture                                  91.183.212.186:
Oct 17 05:37:51 custodia                                 123.56.151.114:
Oct 17 05:42:08 cynthia                                  41.191.224.5:
Oct 17 05:44:20 dalton                                   91.183.212.186:
Oct 17 05:50:54 daniele                                  118.163.4.1:
Oct 17 05:59:29 db                                       115.75.0.178:
Oct 17 06:01:39 dealer                                   123.56.151.114:
Oct 17 06:05:58 diane                                    181.143.83.140:
Oct 17 06:08:10 dimas                                    118.163.4.1:
Oct 17 06:12:26 direccion                                93.207.49.135:



Les 10 dernières addresses bloquées
-----------------------------------
 pkts bytes target     prot opt in     out     source               destination         
    6   304 reject     all  --  *      *       123.52.76.74         0.0.0.0/0           
    0     0 reject     all  --  *      *       60.167.113.119       0.0.0.0/0           
   21  1064 reject     all  --  *      *       103.6.12.229         0.0.0.0/0           
    3   144 reject     all  --  *      *       118.113.45.212       0.0.0.0/0           
    7   420 reject     all  --  *      *       200.77.217.106       0.0.0.0/0           
    9   456 reject     all  --  *      *       181.143.94.74        0.0.0.0/0           
    6   304 reject     all  --  *      *       182.38.33.36         0.0.0.0/0           
    6   304 reject     all  --  *      *       59.25.144.187        0.0.0.0/0           
    3   152 reject     all  --  *      *       41.162.66.100        0.0.0.0/0           
```

The last listing is that of the `shorewall show dyanmic` command (shorewall needed)

# Contact

Questions / criticisme to @ychaouche
