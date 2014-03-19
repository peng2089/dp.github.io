---
layout: default
title: shell语句
---

**系统安全**

查看登录系统的用户

	cat /etc/passwd | grep -v "/sbin/nologin" 

查看登录日志

	last -n 20

端口检查

	nmap localhost

列出ssh登录成功的记录

	cat /var/log/secure | grep 'Accepted' | awk '{print $1 " " $2 " " $3 " User: " $9 " " }'
	cat /var/log/secure.2 | sort | grep 'Accepted' | awk '{print $1 " " $2 " " $3 " User: " $9 " IP:" $11 }'

列出sudo用户ssh登录成功的记录

	cat /var/log/secure | grep 'session opened for user root' | awk '{print $1 " " $2 " " $3 " Sudo User: " $13 " " }'


列出不存在跟没有认证的用户的ssh登录尝试

	cat /var/log/secure | grep 'Invalid user'

列出授权过的但是密码不对ssh登录尝试

	cat /var/log/secure | grep -v invalid | grep 'Failed password'

**web安全**

查看一天只能变动过的php文件

	find /www/ -name "*.php" -mtime -1 -exec ls -l {} \; 

查看php文件中含有eval关键词的文件

	find /www -name "*.php*" |xargs grep "eval"

另注:需要检查的关键词除了eval,还有shell_exec,base64_encode,move_uploaded_file,exec,passthru,system,iframe

web目录权限设置(设定目录:/var/www/test)
	
	chown apache.apache -R /var/www/test/
	chmod 555 -R /var/www/test
	cd /var/www/test
	find -type d -exec chmod 750 {} \;			# 有创建目录的权限
	find -not -type d -exec chmod 640 {} \;		#设定文件权限
	find upload -type d -exec chmod 770 {} \;	#如果存在需要写入的目录,则需要单独设置


**apache安全**

计算httpd占用内存的平均数(单位kb):

	ps aux|grep -v grep|awk '/httpd/{sum+=$6;n++};END{print sum/n}'


查看apache进程数:

	ps aux | grep httpd | grep -v grep | wc -l

查看80端口的tcp连接:

	netstat -tan | grep "ESTABLISHED" | grep ":80" | wc -l

查看80端口连接数最多的20个IP

	netstat -anlp|grep 80|grep tcp|awk '{print $5}'|awk -F: '{print $1}'|sort|uniq -c|sort -nr|head -n20
	netstat -ant |awk '/:80/{split($5,ip,":");++A[ip[1]]}END{for(i in A) print A,i}' |sort -rn|head -n20

用tcpdump嗅探80端口的访问看看谁最高

	tcpdump -i eth0 -tnn dst port 80 -c 1000 | awk -F"." '{print $1"."$2"."$3"."$4}' | sort | uniq -c | sort -nr |head -20

查找较多time_wait连接

	netstat -n|grep TIME_WAIT|awk '{print $5}'|sort|uniq -c|sort -rn|head -n20

查找较多的SYN连接

	netstat -an | grep SYN | awk '{print $5}' | awk -F: '{print $1}' | sort | uniq -c | sort -nr | more


**apache日志**

通过日志查看当天ip连接数，过滤重复:

	cat /var/log/httpd/access_log | grep "4/Dec/2013" | awk '{print $2}' | sort | uniq -c | sort -nr

当天ip连接数最高的ip :

	cat /var/log/httpd/access_log | grep "4/Dec/2013:00" | grep "122.102.7.212" | awk '{print $8}' | sort | uniq -c | sort -nr | head -n 10

当天访问页面排前10的url:

	cat /var/log/httpd/access_log | grep "20/Oct/2008:00" | awk '{print $8}' | sort | uniq -c | sort -nr | head -n 10

用tcpdump嗅探80端口的访问看看谁最高

	tcpdump -i eth0 -tnn dst port 80 -c 1000 | awk -F"." '{print $1"."$2"."$3"."$4}' | sort | uniq -c | sort -nr

接着从日志里查看该ip在干嘛:

	cat /var/log/httpd/access_log | grep ***.***.***.*** | awk '{print $1"\t"$8}' | sort | uniq -c | sort -nr 

查看某一时间段的ip连接数:

	grep "2013:0[7-9]" /var/log/httpd/access_log_2013_12_04 | awk '{print $2}' | sort | uniq -c| sort -nr | wc


查看当天有多少个IP访问：

	awk '{print $1}' /var/log/httpd/access_log|sort|uniq|wc -l

查看某一个页面被访问的次数：

	grep "/index.php" /var/log/httpd/access_log | wc -l

查看每一个IP访问了多少个页面：

	awk '{++S[$1]} END {for (a in S) print a,S[a]}' /var/log/httpd/access_log

将每个IP访问的页面数进行从小到大排序：

	awk '{++S[$1]} END {for (a in S) print S[a],a}' /var/log/httpd/access_log | sort -n

查看某一个IP访问了哪些页面：

	grep ^***.***.***.*** /var/log/httpd/access_log| awk '{print $1,$7}'

去掉搜索引擎统计当天的页面：

	awk '{print $12,$1}' /var/log/httpd/access_log | grep ^\"Mozilla | awk '{print $2}' |sort | uniq | wc -l

查看2014年2月22日11时这一个小时内有多少IP访问：

	awk '{print $4,$1}' /var/log/httpd/access_log | grep 22/Feb/2014:11 | awk '{print $2}'| sort | uniq | wc -l


查看了一下系统的失败登陆记录

	grep -o '[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}\.[0-9]\{1,3\}' /var/log/secure | sort | uniq -c


	



