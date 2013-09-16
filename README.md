#My References 

##Applications

###Security

###HAProxy

  http://blog.exceliance.fr/2012/02/27/use-a-load-balancer-as-a-first-row-of-defense-against-ddos/

###Load Balancers


####Caching + Nosql


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


##Networking

###Security


##Storage
