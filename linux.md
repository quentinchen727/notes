## netstat
netstat -tulpn | grep docker  // udp , tcp , listening, program, numeric
netstat is a command line tool for monitoring network connections both incoming
and outgoing as welll as viewing routing tables, interface stastics.

netstat -c // interactively
netstat -s  // stastics

## nmap (Network Mapper)
It is a security scanner, used to discover hosts and services on a computer network, to build
a "map" of the network.
nmap -sP 10.0.0.0/24  // Ping scans the network, listing machines that respond to ping.
nmap -p 1-65525 -sV -sS -T4 target // Full TCP port scan using service versio detection.
nmap -v -sS -A -Tr target // print verbose output, runs stealth sync scan, T4 timing, OS and versio detection+traceroute and scripts against target services
namp ip_address or hostname or 20.0.0.0-20 or 10.10.10.0/24 or -iL list-of-ips.txt
nmap -p 22 or 1-100 or -F (100 most common ports, fast) or -p-(all 65355 ports)
nmap -sT // using TCP connect
    -sS  // using TCP SYNC scan(default)
    -sU  // UDP ports
    -Pn  // ignore discovery as many firewalls or host will not respond to PING
nmap -A  // Detect OS and Services
    -sV  // standard service detection
    -sV --vresion-intensity 5  // aggressive service detection
nmap -oN file.name // save default to file
     -oX  //xml
     -oG  // grep
     -oA // all format
It support NSE scripts.
nmap -sV -sC ip_address // scan using default scripts
nmap --script-help=ssl-heartbleed // get help for a script
nmap -sV -sp 443 -script=ssl-heartbleed.nse 1.1.1.1 // scan using a NSE script
nmap -sV --script=smb* 1.1.1.1 // using a bunch of scripts
nmap --script=http-title 1.1.1.1/24 // gather page titles from HTTP services
            = http-headers
            = http-enum

## yum
yum --showduplicates list httpd | expand   // see what particular versions are available
expand  // convert tab to space
yum install <package-name>-<version info>
yum install httpd-2.4.6-6
repoquery --show-duplicates "httpd_2.4*"
yum --downloadonly <package>
yumdownloader <package>  // downloading 
yumdownload --resolve <package>
yum localinstall <path to rpm>
yum provides "*/brctl"

## apt-get (debian)
apt-get update
apt-get install iputils-ping
apt-get install net-tools // install ifconfi:w
