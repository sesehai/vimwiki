== 有用的命令 ==
_2010-11-22_

=== wget请求播放地址： ===
{{{
wget -b -i url.txt 
http://sesehai.github.com/tech/2011-05-05-some-usefull-commands.html
取得地址：
wget -r [url]
}}}

=== 拷贝数据： ===
_2010-11-22_
{{{
scp * root@113.11.111.123:/path/
}}}

=== find 命令 ===
{{{
find . -name .svn -type d -exec rm -rf {} \;
}}}

=== 1.进程控制： ===
_2010-11-25_
{{{
  fork,exec
  http://learn.akae.cn/media/ch30s03.html
  僵尸进程是不能用kill命令清除掉的，因为kill命令只是用来终止进程的，而僵尸进程已经终止了。
  思考一下，用什么办法可以清除掉僵尸进程？
}}}

=== shell ===
{{{
  #!/bin/bash
  echo `date` >>/data/stat.log
  cat /data/web.log | grep found >> data/stat_csv.log
  tail  /data/stat_csv.log
}}}

=== php cgi: ===
{{{
Usage: php-cgi [-q] [-h] [-s] [-v] [-i] [-f <file>]
       php-cgi <file> [args...]
  -a               Run interactively
  -b <address:port>|<port> Bind Path for external FASTCGI Server mode
  -C               Do not chdir to the script's directory
  -c <path>|<file> Look for php.ini file in this directory
  -n               No php.ini file will be used
  -d foo[=bar]     Define INI entry foo with value 'bar'
  -e               Generate extended information for debugger/profiler
  -f <file>        Parse <file>.  Implies `-q'
  -h               This help
  -i               PHP information
  -l               Syntax check only (lint)
  -m               Show compiled in modules
  -q               Quiet-mode.  Suppress HTTP Header output.
  -s               Display colour syntax highlighted source.
  -v               Version number
  -w               Display source with stripped comments and whitespace.
  -z <file>        Load Zend extension <file>.
  -T <count>       Measure execution time of script repeated <count> times.
}}}
 
=== 查看系统状态： ===
{{{
  vmstat 1 100 
}}}
 
=== mysql 进程管理 ===
{{{
  mysqladmin -u name -ppwd processlist
  mysqladmin -u name -ppwd kill 

   show full processlist;

   netstat -ntp | grep 60691
}}}

=== Chrome 模拟 Agent 信息 ===
{{{
   chrome.exe --user-agent="Mozilla/5.0 (iPad; U; CPU OS 3_2_2 like Mac OS X; en-us) AppleWebKit/531.21.10 (KHTML, like Gecko) Version/4.0.4 Mobile/7B500 Safari/531.21.10"

   chrome.exe --user-agent="Mozilla/5.0 (Linux; U; Android 2.2; en-us; Nexus One Build/FRF91) AppleWebKit/533.1 (KHTML, like Gecko) Version/4.0 Mobile Safari/533.1"

   chrome.exe --user-agent="Mozilla/5.0 (iPhone; U; CPU OS 3_2_2 like Mac OS X; en-us) AppleWebKit/531.21.10 (KHTML, like Gecko) Version/4.0.4 Mobile/7B500 Safari/531.21.10"
}}}

=== nginx 管理命令 ===
{{{
   /usr/sbin/nginx -c /etc/nginx/nginx.conf
   /usr/sbin/nginx -t
   kill -HUP `cat /data/nginx/logs/nginx.pid`
   kill -HUP 977
}}}

=== nginx 开启ssi模块 ===
{{{
    ssi on;
    ssi_silent_errors on;
    ssi_types text/shtml;
}}}

=== 关闭在LINUX终端下的按键警报声 ===
{{{
     setterm -bleng 0
}}}     
