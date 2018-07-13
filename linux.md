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
nmap -sV --script=smb\* 1.1.1.1 // using a bunch of scripts
nmap --script=http-title 1.1.1.1/24 // gather page titles from HTTP services
            = http-headers
            = http-enum

## yum
yum --showduplicates list httpd | expand   // see what particular versions are available
expand  // convert tab to space
yum install <package-name>-<version info>
yum install httpd-2.4.6-6
repoquery --show-duplicates "httpd_2.4\*"
yum --downloadonly <package>
yumdownloader <package>  // downloading 
yumdownload --resolve <package> // also download dependency
yum localinstall <path to rpm>
yum provides "\*/brctl"

To enable all yum operation to use a proxy server, setting it in /etc/yum.conf.
for a specific use, set it in shell script and export it.

## apt-get (debian)
apt-get update
apt-get install iputils-ping
apt-get install net-tools // install ifconfi:w

## tcpdump
tcpdump -i enp3s0f0 port 80 -nn -A -s 15000
curl -v -X PUT -H "Authorization: Basic LnJpb3VzZXI6cm9vdHJvb3Q=" -H "X-Rio-CopyCount:1" -T tt1 http://20.0.52.47/rio/kyle/tt1

## firewalld and ip(6)tables
firewalld is a frontend of linux kenel ip table service.

## name lookup
dig(more information) > host > nslookup(expire)

## Bash
set -o vi // to turn on vi mode in bash, where readline will simulate some of the vi function
bind -P //check binding mode
fc // bring on your $EDITOR to fix your recent command
ctrl-s // command history forward search
ctrl-r // command history backward search

## mmap
mmap is a POSIX-compliant unix system call that maps files or devices into
memory, a method of memory-mapped file I/O. It naturally implements demand
paging. It can be used as IPC, as opposed to System V IPC "shared memory
facility".

## cscope
cscope -d ## open command line interface

## Gflags
The flag definitions can be scattered around the source code, not just listed
in one place such as main().
1. dEclare dependency on gflags with CMake/Bazel.
2. DEFINE: defining flags in program:
    #include <gflags/gflags.h>
    DEFINE_bool // defining a boolean flag
    and other types: int32, int64, uint64, double, string.
    DEFINE_bool(name, default_value, help_string) // --help flag
You can define a flag anywhere in your source code. If u want to use it across
files, define it in .cc, and declare it in .h.
3. Accessing the Flag: FLAGS_foo
4. accessing the flag in a different file: DECLARE_bool(foo);
