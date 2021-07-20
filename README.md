#My References 

##Applications

Centos 5.x

Cronjob monthly first week "30 23 * * 2   [ `date +\%d` -le 7 ] && /var/lib/haproxy/haproxy-geoip"

wget http://dl.fedoraproject.org/pub/epel/5/x86_64/epel-release-5-4.noarch.rpm

wget http://rpms.famillecollet.com/enterprise/remi-release-5.rpm

sudo rpm -Uvh remi-release-5*.rpm epel-release-5*.rpm

Centos 6.x

wget http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm

wget http://rpms.famillecollet.com/enterprise/remi-release-6.rpm

sudo rpm -Uvh remi-release-6*.rpm epel-release-6*.rpm

###Security

###Load Balancers

###HAProxy

  http://blog.exceliance.fr/2012/02/27/use-a-load-balancer-as-a-first-row-of-defense-against-ddos/
  
  http://www.severalnines.com/resources/clustercontrol-mysql-haproxy-load-balancing-tutorial

  http://cbonte.github.io/haproxy-dconv/configuration-1.5.html

####Caching + Nosql

## check_mk

PNP4NAGIOS graphs location

/opt/omd/versions/0.52/share/pnp4nagios/htdocs/templates.dist/

Check_mk autocheck commands

    #check_mk -I Hostname ( Takes inventory )
    #check_mk -U ( Updates configs )
    #check_mk -u ( Arrange autocheck files as hostname )
    

###Apps
#### App Performance Tests

* Apache strees test using ab
		Performance Testing your Web Server

		To benchmark the performance of your web server applications we recommend the Apache "ab" tool.  The ab tool will 
		show how many requests per second your Apache installation is capable of serving.  The ab tool is a part of the 
		Apache httpd package in CentOS and Red Hat distributions and the "apache2-utils" package in Debian.

		Below is the basic ab command and its output.  The -c parameter specifies the number of connections; the -k stands
		for HTTP Keep-Alive; and the -t parameter sets the time in seconds for which each connection is alive.  The 
		application is then hammered through those connections.

		# ab -kc 20 -t 60 http://8.19.73.87/index.html

		Benchmarking 8.19.73.87 (be patient)
		Finished 130 requests


		Server Software:        Apache/2.2.3
		Server Hostname:        8.19.73.87
		Server Port:            80

		Document Path:          /index.html
		Document Length:        283 bytes

		Concurrency Level:      20
		Time taken for tests:   62.269650 seconds
		Complete requests:      130
		Failed requests:        0
		Write errors:           0
		Non-2xx responses:      130
		Keep-Alive requests:    0
		Total transferred:      60060 bytes
		HTML transferred:       36790 bytes
		Requests per second:    2.09 [#/sec] (mean)
		Time per request:       9579.946 [ms] (mean)
		Time per request:       478.997 [ms] (mean, across all concurrent requests)
		Transfer rate:          0.93 [Kbytes/sec] received

		Connection Times (ms)
					  min  mean[+/-sd] median   max
		Connect:      206  392 637.2    250    3325
		Processing:  4523 8222 3030.7   8016   13982
		Waiting:      208 4798 2958.5   4212   10838
		Total:       4813 8614 3120.1   8329   14269

		Percentage of the requests served within a certain time (ms)
		  50%   8329
		  66%  10851
		  75%  10998
		  80%  11128
		  90%  13933
		  95%  14056
		  98%  14189
		  99%  14223
		 100%  14269 (longest request)

		To perform a "flood" test we set the number of requests (-n) to, say, 5000, and assign the number of concurrent
		connections{{ (-c}}) to something like 200:

		# ab -n 5000 -c 200 http://8.19.73.87/index.html

		Benchmarking 8.19.73.87 (be patient)
		Finished 316 requests


		Server Software:        Apache/2.2.3
		Server Hostname:        8.19.73.87
		Server Port:            80

		Document Path:          /index.html
		Document Length:        283 bytes

		Concurrency Level:      1
		Time taken for tests:   203.610963 seconds
		Complete requests:      316
		Failed requests:        0
		Write errors:           0
		Non-2xx responses:      316
		Total transferred:      145992 bytes
		HTML transferred:       89428 bytes
		Requests per second:    1.55 [#/sec] (mean)
		Time per request:       644.338 [ms] (mean)
		Time per request:       644.338 [ms] (mean, across all concurrent requests)
		Transfer rate:          0.70 [Kbytes/sec] received

		Connection Times (ms)
					  min  mean[+/-sd] median   max
		Connect:      206  340 509.5    250    3324
		Processing:   207  302 450.1    250    7830
		Waiting:      206  285 201.5    250    2693
		Total:        414  643 683.4    501    8081

		Percentage of the requests served within a certain time (ms)
		  50%    501
		  66%    505
		  75%    579
		  80%    645
		  90%    651
		  95%   1313
		  98%   3648
		  99%   3649
		 100%   8081 (longest request)

		If the ab output makes you suspect issues, it is useful to look into any replies using tcpdump.  In particular,
		tcp-rst replies could appear.  To catch them, 
		use:

		# tcpdump -nn 'tcp[tcpflags] == tcp-rst' and port 80

		tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
		listening on eth0, link-type EN10MB (Ethernet), capture size 96 bytes
		10:59:06.036411 IP 89.253.250.50.53261 > 8.19.73.87.80: R 179261015:179261015(0) win 0
		10:59:06.036521 IP 89.253.250.50.53261 > 8.19.73.87.80: R 179261015:179261015(0) win 0
		10:59:06.036553 IP 89.253.250.50.53261 > 8.19.73.87.80: R 179261016:179261016(0) win 0

		We are interested mostly in tcp-rst server replies, as they point to misconfiguration or performance issues.
		To catch server-side tcp-rst replies use:

		# tcpdump -nn 'tcp[tcpflags] == tcp-rst' and port 80 and src host 89.253.250.50

		where 89.253.250.50 is the server hosting your tests.



###Database

watch --differences=cumulative -n 0  "mysql -e 'SHOW SLAVE STATUS\G' | grep Seconds_Behind_Master"

Fix Replication issue on mysql

	Master_Log_File (Line 6) : Log File on the Master whose position was last read
    	Read_Master_Log_Pos (Line 7) : Last position read on the Slave from the Master
    	Relay_Master_Log_File (Line 10) : Log File on the Master whose position was last executed
    	Exec_Master_Log_Pos (Line 22) : Last position executed on the Slave from the Master
    	Relay_Log_Space (Line 23) : Sum of bytes from all relay logs

	STOP SLAVE;
	CHANGE MASTER TO MASTER_LOG_FILE='mysql-bin.002814',MASTER_LOG_POS=823078734;
	START SLAVE;

##Networking

Network timeout test

netstat -on | grep TIME_WAIT | less

netstat -ant | grep 554 | awk '{print $6}' | sort | uniq -c | sort -n

TCPDUMP

tcpdump -i any port 80 -s 4096 -w trace.pcap

SYNC Attack

Log in messages : possible SYN flooding on port 80. Sending cookies

URL : http://forum.ovh.co.uk/showthread.php?t=6455

net.ipv4.tcp_syncookies = 1

net.ipv4.tcp_max_syn_backlog = 2048

net.ipv4.tcp_synack_retries = 3

	# Kernel sysctl configuration file for Red Hat Linux
	#
	# For binary values, 0 is disabled, 1 is enabled.  See sysctl(8) and
	# sysctl.conf(5) for more details.

	# Controls IP packet forwarding
	net.ipv4.ip_forward = 0

	# Controls source route verification
	net.ipv4.conf.default.rp_filter = 1

	# Do not accept source routing
	net.ipv4.conf.default.accept_source_route = 0

	# Controls the System Request debugging functionality of the kernel
	kernel.sysrq = 0

	# Controls whether core dumps will append the PID to the core filename.
	# Useful for debugging multi-threaded applications.
	kernel.core_uses_pid = 1

	# Controls the use of TCP syncookies
	net.ipv4.tcp_syncookies = 1

	# Disable netfilter on bridges.
	net.bridge.bridge-nf-call-ip6tables = 0
	net.bridge.bridge-nf-call-iptables = 0
	net.bridge.bridge-nf-call-arptables = 0

	# Controls the default maxmimum size of a mesage queue
	kernel.msgmnb = 65536

	# Controls the maximum size of a message, in bytes
	kernel.msgmax = 65536

	# Controls the maximum shared segment size, in bytes
	kernel.shmmax = 68719476736

	# Controls the maximum number of shared memory segments, in pages
	kernel.shmall = 4294967296

	# Minimize time_wait
	net.ipv4.tcp_tw_recycle = 1
	net.ipv4.tcp_tw_reuse = 1

	# open local ports for clients
	net.ipv4.ip_local_port_range = 1024 65535

	# settings for syn floods 
	net.ipv4.tcp_max_syn_backlog = 662144
	# Increase number of incoming connections backlog
	net.core.netdev_max_backlog = 40000
	# Increase the tcp-time-wait buckets pool size
	net.ipv4.tcp_max_tw_buckets = 400000
	# Increase number of max incoming connections
	net.core.somaxconn = 40000
	# Increase TCP performance
	net.ipv4.neigh.default.unres_qlen = 6
	net.ipv4.neigh.default.proxy_qlen = 96
	# Increase size of socket buffers
	net.ipv4.tcp_mem = 776352       1035136 16777216
	# Set the minimum, initial, and maximum sizes for the write buffer. Note that this maximum should be less than or equal to the value set in net.core.wmem_max.
	net.ipv4.tcp_wmem = 776352        1035136   16777216
	# Set the minimum, initial, and maximum sizes for the read buffer. Note that this maximum should be less than or equal to the value set in net.core.rmem_max.
	net.ipv4.tcp_rmem = 776352        1035136  16777216

	net.core.rmem_max = 16777216
	net.core.rmem_max = 16777216
	net.ipv4.tcp_max_orphans = 256000
	net.ipv4.tcp_timestamps = 0

	# Use system memory to calculate this setting eg for 8GB.
	net.netfilter.nf_conntrack_max = 656000
	net.nf_conntrack_max = 656000

	vm.swappiness = 0

	net.ipv4.tcp_fin_timeout = 10
	net.ipv4.tcp_no_metrics_save = 1

Fin/Syn
        
        net.ipv4.tcp_fin_timeout = 30
	net.ipv4.tcp_keepalive_time = 600
	net.ipv4.tcp_keepalive_probes = 5
	net.ipv4.tcp_keepalive_intvl = 15
	
	net.netfilter.nf_conntrack_tcp_timeout_established = 40

	net.netfilter.nf_conntrack_tcp_timeout_close = 	10
	net.netfilter.nf_conntrack_tcp_timeout_close_wait = 10
	net.netfilter.nf_conntrack_tcp_timeout_fin_wait = 10
	net.netfilter.nf_conntrack_tcp_timeout_last_ack = 10
	net.netfilter.nf_conntrack_tcp_timeout_syn_recv = 10
	net.netfilter.nf_conntrack_tcp_timeout_syn_sent = 10
	net.netfilter.nf_conntrack_tcp_timeout_time_wait = 10


Ubuntu 18.4


	# Kernel sysctl configuration file for Linux
	#
	# Version 1.14 - 2019-04-05
	# Michiel Klaver - IT Professional
	# http://klaver.it/linux/ for the latest version - http://klaver.it/bsd/ for a BSD variant
	#
	# This file should be saved as /etc/sysctl.conf and can be activated using the command:
	# sysctl -e -p /etc/sysctl.conf
	#
	# For binary values, 0 is disabled, 1 is enabled.  See sysctl(8) and sysctl.conf(5) for more details.
	#
	# Tested with: Ubuntu 14.04 LTS kernel version 3.13
	#              Debian 7 kernel version 3.2
	#              CentOS 7 kernel version 3.10

	#
	# Intended use for dedicated server systems at high-speed networks with loads of RAM and bandwidth available
	# Optimised and tuned for high-performance web/ftp/mail/dns servers with high connection-rates
	# DO NOT USE at busy networks or xDSL/Cable connections where packetloss can be expected
	# ----------

	# Credits:
	# http://www.enigma.id.au/linux_tuning.txt
	# http://www.securityfocus.com/infocus/1729
	# http://fasterdata.es.net/TCP-tuning/linux.html
	# http://fedorahosted.org/ktune/browser/sysctl.ktune
	# http://www.cymru.com/Documents/ip-stack-tuning.html
	# http://www.kernel.org/doc/Documentation/networking/ip-sysctl.txt
	# http://www.frozentux.net/ipsysctl-tutorial/chunkyhtml/index.html
	# http://knol.google.com/k/linux-performance-tuning-and-measurement
	# http://www.cyberciti.biz/faq/linux-kernel-tuning-virtual-memory-subsystem/
	# http://www.redbooks.ibm.com/abstracts/REDP4285.html
	# http://www.speedguide.net/read_articles.php?id=121
	# http://lartc.org/howto/lartc.kernel.obscure.html
	# http://en.wikipedia.org/wiki/Sysctl
	# https://blog.cloudflare.com/http-2-prioritization-with-nginx/



	###
	### GENERAL SYSTEM SECURITY OPTIONS ###
	###

	# Controls the System Request debugging functionality of the kernel
	kernel.sysrq = 0

	# Controls whether core dumps will append the PID to the core filename.
	# Useful for debugging multi-threaded applications.
	kernel.core_uses_pid = 1

	#Allow for more PIDs
	kernel.pid_max = 65535

	# The contents of /proc/<pid>/maps and smaps files are only visible to
	# readers that are allowed to ptrace() the process
	kernel.maps_protect = 1

	#Enable ExecShield protection
	kernel.exec-shield = 1
	kernel.randomize_va_space = 2

	# Controls the maximum size of a message, in bytes
	kernel.msgmnb = 65535

	# Controls the default maxmimum size of a mesage queue
	kernel.msgmax = 65535

	# Restrict core dumps
	fs.suid_dumpable = 0

	# Hide exposed kernel pointers
	kernel.kptr_restrict = 1



	###
	### IMPROVE SYSTEM MEMORY MANAGEMENT ###
	###

	# Increase size of file handles and inode cache
	fs.file-max = 209708

	# Do less swapping
	vm.swappiness = 30
	vm.dirty_ratio = 30
	vm.dirty_background_ratio = 5

	# specifies the minimum virtual address that a process is allowed to mmap
	vm.mmap_min_addr = 4096

	# 50% overcommitment of available memory
	vm.overcommit_ratio = 50
	vm.overcommit_memory = 0

	# Set maximum amount of memory allocated to shm to 256MB
	kernel.shmmax = 268435456
	kernel.shmall = 268435456

	# Keep at least 64MB of free RAM space available
	vm.min_free_kbytes = 65535



	###
	### GENERAL NETWORK SECURITY OPTIONS ###
	###

	#Prevent SYN attack, enable SYNcookies (they will kick-in when the max_syn_backlog reached)
	net.ipv4.tcp_syncookies = 1
	net.ipv4.tcp_syn_retries = 2
	net.ipv4.tcp_synack_retries = 2
	net.ipv4.tcp_max_syn_backlog = 4096

	# Disables packet forwarding
	net.ipv4.ip_forward = 0
	net.ipv4.conf.all.forwarding = 0
	net.ipv4.conf.default.forwarding = 0
	net.ipv6.conf.all.forwarding = 0
	net.ipv6.conf.default.forwarding = 0

	# Disables IP source routing
	net.ipv4.conf.all.send_redirects = 0
	net.ipv4.conf.default.send_redirects = 0
	net.ipv4.conf.all.accept_source_route = 0
	net.ipv4.conf.default.accept_source_route = 0
	net.ipv6.conf.all.accept_source_route = 0
	net.ipv6.conf.default.accept_source_route = 0

	# Enable IP spoofing protection, turn on source route verification
	net.ipv4.conf.all.rp_filter = 1
	net.ipv4.conf.default.rp_filter = 1

	# Disable ICMP Redirect Acceptance
	net.ipv4.conf.all.accept_redirects = 0
	net.ipv4.conf.default.accept_redirects = 0
	net.ipv4.conf.all.secure_redirects = 0
	net.ipv4.conf.default.secure_redirects = 0
	net.ipv6.conf.all.accept_redirects = 0
	net.ipv6.conf.default.accept_redirects = 0

	# Enable Log Spoofed Packets, Source Routed Packets, Redirect Packets
	net.ipv4.conf.all.log_martians = 1
	net.ipv4.conf.default.log_martians = 1

	# Decrease the time default value for tcp_fin_timeout connection
	net.ipv4.tcp_fin_timeout = 7

	# Decrease the time default value for connections to keep alive
	net.ipv4.tcp_keepalive_time = 300
	net.ipv4.tcp_keepalive_probes = 5
	net.ipv4.tcp_keepalive_intvl = 15

	# Don't relay bootp
	net.ipv4.conf.all.bootp_relay = 0

	# Don't proxy arp for anyone
	net.ipv4.conf.all.proxy_arp = 0

	# Turn on the tcp_timestamps, accurate timestamp make TCP congestion control algorithms work better
	net.ipv4.tcp_timestamps = 1

	# Don't ignore directed pings
	net.ipv4.icmp_echo_ignore_all = 0

	# Enable ignoring broadcasts request
	net.ipv4.icmp_echo_ignore_broadcasts = 1

	# Enable bad error message Protection
	net.ipv4.icmp_ignore_bogus_error_responses = 1

	# Allowed local port range
	net.ipv4.ip_local_port_range = 16384 65535

	# Enable a fix for RFC1337 - time-wait assassination hazards in TCP
	net.ipv4.tcp_rfc1337 = 1

	# Do not auto-configure IPv6
	net.ipv6.conf.all.autoconf=0
	net.ipv6.conf.all.accept_ra=0
	net.ipv6.conf.default.autoconf=0
	net.ipv6.conf.default.accept_ra=0
	net.ipv6.conf.eth0.autoconf=0
	net.ipv6.conf.eth0.accept_ra=0



	###
	### TUNING NETWORK PERFORMANCE ###
	###

	# Use BBR TCP congestion control and set tcp_notsent_lowat to 16384 to ensure HTTP/2 prioritization works optimally
	# Do a 'modprobe tcp_bbr' first (kernel > 4.9)
	# Fall-back to htcp if bbr is unavailable (older kernels)
	net.ipv4.tcp_congestion_control = htcp
	net.ipv4.tcp_congestion_control = bbr
	net.ipv4.tcp_notsent_lowat = 16384

	# For servers with tcp-heavy workloads, enable 'fq' queue management scheduler (kernel > 3.12)
	net.core.default_qdisc = fq

	# Turn on the tcp_window_scaling
	net.ipv4.tcp_window_scaling = 1

	# Increase the read-buffer space allocatable
	net.ipv4.tcp_rmem = 8192 87380 16777216
	net.ipv4.udp_rmem_min = 16384
	net.core.rmem_default = 262144
	net.core.rmem_max = 16777216

	# Increase the write-buffer-space allocatable
	net.ipv4.tcp_wmem = 8192 65536 16777216
	net.ipv4.udp_wmem_min = 16384
	net.core.wmem_default = 262144
	net.core.wmem_max = 16777216

	# Increase number of incoming connections
	net.core.somaxconn = 32768

	# Increase number of incoming connections backlog
	net.core.netdev_max_backlog = 16384
	net.core.dev_weight = 64

	# Increase the maximum amount of option memory buffers
	net.core.optmem_max = 65535

	# Increase the tcp-time-wait buckets pool size to prevent simple DOS attacks
	net.ipv4.tcp_max_tw_buckets = 1440000

	# try to reuse time-wait connections, but don't recycle them (recycle can break clients behind NAT)
	net.ipv4.tcp_tw_recycle = 0
	net.ipv4.tcp_tw_reuse = 1

	# Limit number of orphans, each orphan can eat up to 16M (max wmem) of unswappable memory
	net.ipv4.tcp_max_orphans = 16384
	net.ipv4.tcp_orphan_retries = 0

	# Limit the maximum memory used to reassemble IP fragments (CVE-2018-5391)
	net.ipv4.ipfrag_low_thresh = 196608
	net.ipv6.ip6frag_low_thresh = 196608
	net.ipv4.ipfrag_high_thresh = 262144
	net.ipv6.ip6frag_high_thresh = 262144


	# don't cache ssthresh from previous connection
	net.ipv4.tcp_no_metrics_save = 1
	net.ipv4.tcp_moderate_rcvbuf = 1

	# Increase size of RPC datagram queue length
	net.unix.max_dgram_qlen = 50

	# Don't allow the arp table to become bigger than this
	net.ipv4.neigh.default.gc_thresh3 = 2048

	# Tell the gc when to become aggressive with arp table cleaning.
	# Adjust this based on size of the LAN. 1024 is suitable for most /24 networks
	net.ipv4.neigh.default.gc_thresh2 = 1024

	# Adjust where the gc will leave arp table alone - set to 32.
	net.ipv4.neigh.default.gc_thresh1 = 32

	# Adjust to arp table gc to clean-up more often
	net.ipv4.neigh.default.gc_interval = 30

	# Increase TCP queue length
	net.ipv4.neigh.default.proxy_qlen = 96
	net.ipv4.neigh.default.unres_qlen = 6

	# Enable Explicit Congestion Notification (RFC 3168), disable it if it doesn't work for you
	net.ipv4.tcp_ecn = 1
	net.ipv4.tcp_reordering = 3

	# How many times to retry killing an alive TCP connection
	net.ipv4.tcp_retries2 = 15
	net.ipv4.tcp_retries1 = 3

	# Avoid falling back to slow start after a connection goes idle
	# keeps our cwnd large with the keep alive connections (kernel > 3.6)
	net.ipv4.tcp_slow_start_after_idle = 0

	# Allow the TCP fastopen flag to be used, beware some firewalls do not like TFO! (kernel > 3.7)
	net.ipv4.tcp_fastopen = 3

	# This will enusre that immediatly subsequent connections use the new values
	net.ipv4.route.flush = 1
	net.ipv6.route.flush = 1
###Security


##Storage

####Find out inode utilization on server

echo "Detailed Inode usage for: $(pwd)" ; for d in `find -maxdepth 1 -type d |cut -d\/ -f2 |grep -xv . |sort`; \
do c=$(find $d |wc -l) ; printf "$c\t\t- $d\n" ; done ; printf "Total: \t\t$(find $(pwd) | wc -l)\n"


### Must Read

http://ben.goodacre.name/tech/Possible_SYN_flooding_on_port_xxx._Sending_cookies_%28Linux%29

http://sphinxsearch.com/docs/1.10/conf-max-iops.html

http://sphinxsearch.com/forum/view.html?id=5136

http://sphinxsearch.com/forum/view.html?id=8361

http://www.ivinco.com/blog/sphinx-replication/

http://sphinxsearch.com/blog/2012/08/14/indexing-tips-tricks/

http://www.linuxforu.com/2012/03/web-acceleration-varnish-3-wordpress-wptouch/

http://www.linuxforu.com/2012/04/how-to-lock-down-wordpress-admin-access-using-socks5-proxy/

http://www.linuxforu.com/2009/06/puppet-show-automating-unix-administration/
 
http://www.linuxforu.com/2010/11/data-centre-automation-puppet-master-client-setup/
 
http://www.linuxforu.com/2010/12/data-centre-automation-puppet-user-group-management/
 
http://www.linuxforu.com/2011/01/data-centre-automation-puppet-resources-types-examples/
 
http://www.linuxforu.com/2011/02/data-centre-automation-puppet-classes-modules/
 
http://www.linuxforu.com/2011/10/syn-flooding-using-scapy-and-prevention-using-iptables/
