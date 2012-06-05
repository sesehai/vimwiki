%toc
%title linux服务器每秒并发处理数的计算|海的心情

== linux服务器每秒并发处理数的计算 ==

日期：2012-05-13

==== 利用网络处理量计算。 ====

* 计算参考公式：

并发 = connection established / min(server keepalive, server timeout)

* 翻译一下：

并发 = 服务器传输链接数 除以 服务器软件的keepalive设置和服务器软件的超时设置之间的最小值

这个公式算出来的数字是keepalive时间段内的平均值，比真实平均值要小一点，如果想找最大值就要设定keepalive为0或1，然后多探测几次。

connection established是服务器当前正在传输的链接，但是keepalive打开时，新建立的传输链接会一直存在直到keepalive/timeout关闭链接;客户端主动关闭链接的话connection established也会关闭，不过这种链接一般比较少，多数浏览器都是支持keepalive并遵守服务器配置的。

* 在linux查看connection established数字的办法是在命令行执行：
{{{ class="brush:bash"
netstat -est|grep "connections established"|cut -d "c" -f 1
}}}

keepalive和timeout数字查看办法要查看web server软件的配置文件

注意:这个方法只能用于最前端的服务器或7层交换机，前端之后的服务器因为缓存或链接方式的原因往往是不准确的。

=== 利用服务器日志计算 ===

因为服务器每处理一个请求，都会在日志里留下一条信息，所以利用服务器软件的日志来计算是最准确的，但是是这种计算方式浮动也可能会比较大，需要取最大值计算。

首先在确定服务器软件有将所有请求写入一个日志文件里，并确保该日志文件正在不停记录。

为节省时间和服务器资源，把log文件的最后一万条记录拿出来统计，我就用nginx默认的main格式作个例子：

* 执行命令：
{{{ class="brush:bash"
tail -10000 nginx.log | awk '{print $4;}' | sort | uniq -c
}}}
命令的意思是取log文件的最后一万条记录，然后用awk取得日志文件中表示时间的一列($4)，接着再对该列进行一次排序，最后是用uniq把这一列相邻的重复行合并，并计算合并的条数。

其中先sort再uniq是一种安全的做法，以确保同一秒的日志先被归到一起，然后再合并，这样就不会有同一秒种的日志会被切成几段这样的现象。

* 可以得到输出：

23 [05/Jun/2012:20:26:02

26 [05/Jun/2012:20:26:03

17 [05/Jun/2012:20:26:04

20 [05/Jun/2012:20:26:05

...

70 [05/Jun/2012:20:29:43

61 [05/Jun/2012:20:29:44

45 [05/Jun/2012:20:29:45

37 [05/Jun/2012:20:29:46

2 [05/Jun/2012:20:29:47

在这些输出中，第一条记录和最后一条记录因为时间有可能被切断，所以是完全不可靠之信息，可以忽略。

如果输出少于10行的话，要扩大一下范围，修改tail -10000为tail -100000取最后十万条数据统计。

* 如果只需要看最大值，可以再用sort命令进行排序，并用head命令取出前10行记录：
{{{ class="brush:bash"
tail -10000 nginx.log | awk '{print $4;}' | sort | uniq -c | sort -nr | head
}}}

awk命令是一个功能比较强的命令，在这里只用到最简单的awk '{print $4;}'，意思是将日志每行按空格切分开，然后切出来的结果依次从左到右就是$1 $2 $3 ...，nginx默认的main日志时间字段刚好是$4，所以在这里拿$4来计算。如果是别的格式的日志，依照这个办法去找到列数：

就拿apache默认的日志来看，首先：

head -1 apache.log

得到类似以下的输出：

60.8.207.86 - - [05/Jun/2012:21:03:58 +0800] "GET / HTTP/1.0" 200 11141 "http://www.xxxxxxxx.com" "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1)"

用awk按空格来切分开之后，60.8.207.86就是$1，$2和$3都是-，[05/Jun/2012:21:03:58是$4，这就是需要拿出来统计的。嗯，怎么apache的日志和nginx的一样的?现在才发现。

* 那命令也基本没什么变化，执行一下：
{{{ class="brush:bash"
tail -10000 apache.log | awk '{print $4;}' | sort | uniq -c | sort -nr | head
}}}

注意，如果是在squid服务器后面的apache，则日志会变成这样：

60.8.207.86, 127.0.0.1 - - [05/Jun/2012:21:03:58 +0800] "GET / HTTP/1.0" 200 11141 "http://www.xxxxxxxxx.com" "Mozilla/4.0 (compatible; MSIE 6.0; Windows NT 5.1; SV1)"

因为日志的第一个段：x_forwarded_for中含有空格，所以时间的段会在$3、$4或$5之间变化，从而不能确定，可以先用一次awk或cut以[这个符号切分一下：
{{{ class="brush:bash"
tail -10000 apache.log | awk -F"[" '{print $2;}' | awk '{print $1;}' | sort | uniq -c | sort -nr | head
}}}
或
{{{ class="brush:bash"
tail -10000 apache.log | cut -d"[" -f 2 | awk '{print $1;}' | sort | uniq -c | sort -nr | head
}}}
这样统计就准确了。

--EOF--
