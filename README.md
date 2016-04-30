<!-- language: lang-none -->
      ______              _____            _        _ _           _             
     |  ____|            / ____|          | |      (_) |         | |            
     | |__ _ __ ___  ___| |     ___  _ __ | |_ _ __ _| |__  _   _| |_ ___  _ __ 
     |  __| '__/ _ \/ _ \ |    / _ \| '_ \| __| '__| | '_ \| | | | __/ _ \| '__|
     | |  | | |  __/  __/ |___| (_) | | | | |_| |  | | |_) | |_| | || (_) | |   
     |_|  |_|  \___|\___|\_____\___/|_| |_|\__|_|  |_|_.__/ \__,_|\__\___/|_|   
                                                                                                                                                        

Enjoy a safe and faster web experience

TL:DR `cat domains-{ads,tracking,malware} > /dev/null`

## Intro

This bash script intends to extract domains lists from various sources.
It is a replacement for ad blocking extensions in your browser.
It [blocks ads, malware, trackers at DNS level](https://en.wikipedia.org/wiki/DNSBL).

## Why

 - [Major sites including New York Times and BBC hit by 'ransomware' malvertising](http://www.theguardian.com/technology/2016/mar/16/major-sites-new-york-times-bbc-ransomware-malvertising)
 - [Adblocking: advertising 'accounts for half of data used to read articles'](http://www.theguardian.com/media/2016/mar/16/ad-blocking-advertising-half-of-data-used-articles)
 - [The Verge's web sucks](http://blog.lmorchard.com/2015/07/22/the-verge-web-sucks/) and [The web is Doom](https://mobiforge.com/research-analysis/the-web-is-doom)

## What the scripts does?

 - Backup the original configuration file
 - Download and merge domains lists from various sources.
 - Create a cron job to automaticly update the hosts file, default every week (optional)

## Benefits and Features

 - Low CPU and RAM usage.
 - **Speeds up your Internet** use since the local file is checked first, before send a DNS request.
 - **Data savings** since the ad content is never downloaded.
 - Stops ad tracking.
 - Blocks spyware and malware. That increases the safety of your networking experience.
 - Not just for browsers, it blocks ads and malware across the entire operative system.


## Dependencies

 - [GNU bash](http://www.gnu.org/software/bash/bash.html)
 - [GNU sed](http://www.gnu.org/software/sed)
 - [GNU grep](http://www.gnu.org/software/grep/grep.html)
 - [GNU coreutils](http://www.gnu.org/software/coreutils)
 - [GNU wget](https://www.gnu.org/software/wget/) or [cURL](http://curl.haxx.se/) (default)
 - DNS cacher: [Dnsmasq](http://www.thekelleys.org.uk/dnsmasq/doc.html) (default), [Unbound](https://unbound.net/) or [Pdnsd](http://members.home.nl/p.a.rombouts/pdnsd/index.html)


## Install

```sh
git clone https://github.com/tbds/FreeContibutor
cd FreeContibutor/src
sudo ./installer.sh
```

FreeContributor has some scripts, such as, exporting uBlock/uMatrix rules to various formats: hosts, dnsmasq, pdnsd and unbound.

## Sources

FreeContributor downloads external files; each has its own license, detailed in the list below.


| URL                                                                              | License |
| -------                                                                          | ------- |
|[Adaway list](https://adaway.org/hosts.txt)                                       | CC Attribution 3.0|
|[MVPS Hosts](http://winhelp2002.mvps.org/hosts.htm)                               | CC Attribution-NonCommercial-ShareAlike 4.0 |
|[hpHosts’s ad and tracking servers‎](http://www.hosts-file.net/)                   | *Read [Terms of Use](http://www.hosts-file.net/)* |
|[Peter Lowe’s Ad server list](http://pgl.yoyo.org/adservers/)                     | ? |
|[Dan Pollock’s hosts file](http://someonewhocares.org/hosts/)                     | non-commercial |
|[CAMELEON](http://sysctl.org/cameleon/)                                           | ? |
|[StevenBlack/hosts](https://github.com/StevenBlack/hosts/)                        | ? |
|[Quidsup/notrack](https://github.com/quidsup/notrack)                             | ? |
|[Gorhill's uMatrix Blocklist](https://github.com/gorhill/uMatrix)                 | ? |
|[Malware Domain List](http://www.malwaredomainlist.com/hostslist/hosts.txt)       | |
|[AdBlock Manager](http://adblock.gjtech.net/?format=unix-hosts)                   | CC Attribution 3.0 |
|[Hostfile project](http://hostsfile.org/hosts.html)                               | LGPL as GPLv2 |
|[Airelle's host file](http://rlwpx.free.fr/WPFF/hosts.htm)                        | CC Attribution 3.0 |
|[The Hosts File Project](http://hostsfile.mine.nu)                                | LGPL |
|[Mahakala](http://adblock.mahakala.is/)                                           | ? |
|[Secure Mecca](http://securemecca.com/)                                           | LGPL as GPLv2 |
|[Spam404scamlist](http://spam404bl.com/)                                          | |
|[Malwaredomains](http://malwaredomains.lehigh.edu/)                               | |
|[Adzhosts](https://sourceforge.net/projects/adzhosts/)                            | |
|[Zeustracker](hhttps://zeustracker.abuse.ch/blocklist.php)                        | |
|[hosts.eladkarako.com](http://hosts.eladkarako.com/)                              | |
|[Malekal](http://www.malekal.com/)                                                | |

## DNS 101

Without an custom DNS Server

<!-- language: lang-none -->
    +----+      +------------+      +------------------+      +------------------------+
    | PC | <==> | DNS Server | <==> | Other DNS Server | <==> | example.tld = ip adress|
    +----+      +------------+      +------------------+      +------------------------+

    +----+      +-------------------------- + 
    | PC | <==> | ip adress of example.tld  |
    +----+      +---------------------------+ 


With a local DNS resolver

<!-- language: lang-none -->
    +----+      +----------------+      +------------------+      +------------------+
    | PC |      | DNS Server     | <==> | Other DNS Server | <==> | goodwebsite.tld  |
    +----+      +----------------+      +------------------+      +------------------+
      ^^             ^^                                                    ||
      ||             ||                                                    || 
      vv             ||                                                    ||
    +--------------------+      +----------------------------------------------------+
    | local DNS resolver | <==> | ads.example.tld = 127.0.0.1 or 0.0.0.0 or NXDOMAIN | 
    +--------------------+      +----------------------------------------------------+
                                                         +------------+    ||
                                                         | DNS cache  |  <= /
                                                         +------------+
    future requests of goodwebsite.tld

    +----+      +--------------------+      +------------------------------------------+
    | PC | <==> | local DNS resolver | <==> | DNS cache of goodwebsite.tld = ip adress |
    +----+      +--------------------+      +------------------------------------------+



## Hosts vs DNS resolver

The hosts blocking method can not use wildcards (*) and and therefore someone must keep track 
of each subdomain that should be blocked. Some DNS caching servers can block the domain and
subdomains with just one rule. For example `/etc/hosts`

    127.0.0.1 example.tld
    0.0.0.0 example.tld


Will redirect example.tld to the localhost, but not ads.example.tld. With a dns caching server,
such as Dnsmasq, for example `/etc/dnsmasq.conf`

    address=/example.tld/127.0.0.1
    address=/example.tld/0.0.0.0
    server=/example.tld/


Will redirect example.tld and all subdomains to 127.0.0.1 or 0.0.0.0. Better yet, it can 
return NXDOMAIN.


## Comparasion


| Program              | Language      | Adblocking Method                              |
| :-------------       | :-------------| :----------------------------------------------|
| FreeContributor      | Bash          | DNS caching server (Dnsmasq, Unbound or Pdnsd) |
| Pi-Hole              | Bash, Php     | Hosts with Dnsmasq (for cache only)            |
| NoTrack              | Bash, Php     | Hosts with Dnsmasq (for cache only)            |
| Hostsblock           | Bash          | Hosts with Dnsmasq (for cache only)            |
| dnsgate              | Python        | Hosts or Dnsmasq                               |
| StevenBlack/hosts    | Python        | Hosts                                          |
| adsuck               | C             | DNS server                                     |
| pfBlockerNG          | Sh, PHP       | DNS caching server: Unbound                    |


## License

FreeContributor is licensed under the [General Public License (GPL) version 3](https://www.gnu.org/licenses/gpl.html)

The ASCII art logo at the top was made using [FIGlet](http://www.figlet.org/).