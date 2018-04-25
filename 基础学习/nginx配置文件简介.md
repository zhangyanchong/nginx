<pre><code>
#Nginx的worker  <font color="#00dd00">进程运行用户以及用户组第一个是用户第二个用户组，如果不加限制可以用 nobody  </font>
user  nobody nobody;   
#Nginx开启的进程数 工作进程：数目。<font color="#00dd00">根据硬件调整，通常等于CPU数量或者2倍于CPU。</font>
worker_processes  1
#定义全局错误日志定义类型，[debug|info|notice|warn|crit]
#error_log  logs/error.log  info;
#指定进程ID存储文件位置
#pid        logs/nginx.pid;

================================================
#工作模式及连接数上限
events 
{
  use epoll;       #epoll是多路复用IO(I/O Multiplexing)中的一种方式,但是仅用于linux2.6以上内核,可以大大提高nginx的性能
  worker_connections 1024;  #;单个后台worker process进程的最大并发链接数
}
===================================================
http   模块
   include mime.types; #文件扩展名与类型映射表  一般在config下面的
   client_max_body_size 20m; # <font color="#00dd00">设置客户上传文件的大小与上传相关的</font>

   #默认文件类型
   default_type application/octet-stream;

   #日志相关定义
    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';
    #定义日志的格式。后面定义要输出的内容。
    #1.$remote_addr 与$http_x_forwarded_for 用以记录客户端的ip地址；
    #2.$remote_user ：用来记录客户端用户名称；
    #3.$time_local ：用来记录访问时间与时区；
    #4.$request  ：用来记录请求的url与http协议；
    #5.$status ：用来记录请求状态； 
    #6.$body_bytes_sent ：记录发送给客户端文件主体内容大小；
    #7.$http_referer ：用来记录从那个页面链接访问过来的；
    #8.$http_user_agent ：记录客户端浏览器的相关信息
    #连接日志的路径，指定的日志格式放在最后。
    #access_log  logs/access.log  main;      此处的main 对于 log_format 的main
    #只记录更为严重的错误日志，减少IO压力
    error_log logs/error.log crit;
    #关闭日志
    #access_log  off;


    #<font color="#00dd00">客户端连接超时时间，单位是秒  如果设置的短的话超过60秒会提示504网关超时</font>
    keepalive_timeout 60;

    #开启gzip压缩输出 其他参数具体参考百度吧
    gzip on; 

    #upstream表示负载服务器池 （参考其他文档）
    upstream backend_server {
    }
   
    #虚拟主机基本设置
      server {
        #监听端口
        listen       80;
        #访问域名
        server_name  localhost;
        #编码格式，若网页格式与此不同，将被自动转码
        #charset utf-8;
        #虚拟主机访问日志定义
        #access_log  logs/host.access.log  main;
        #对URL进行匹配
        location / {
            #访问路径，可相对也可绝对路径
            root   html;
            #首页文件。以下按顺序匹配
            index  index.html index.htm;
        }

      #错误信息返回页面
        #error_page  404              /404.html;
        # redirect server error pages to the static page /50x.html
        #
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
        }

       #php脚本请求全部转发给FastCGI处理
        # pass the PHP scripts to FastCGI server listening on 127.0.0.1:9000
        #location ~ \.php$ {
        #    root           html;
        #    fastcgi_pass   127.0.0.1:9000;
        #    fastcgi_index  index.php;
        #    fastcgi_param  SCRIPT_FILENAME  /scripts$fastcgi_script_name;
        #    include        fastcgi_params;
        #}
    }
 
    # 可以写多个虚拟主机在vhost 下面   写在http下面  
    include          vhost/*.conf;

</code></pre>