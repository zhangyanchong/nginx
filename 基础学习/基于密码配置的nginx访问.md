<pre><code>
基于密码配置的nginx的访问权限  配置文件 chong.conf  目录在/www/nginx/conf/vhosts/chong.conf
server {
        listen 80;
        server_name docker.chong.com;
        index index.php;
        access_log /logzyc/log/chong_access.log;
        error_log /logzyc/chong_error.log;

        root /www/web/chong/;

      location ~ .*\.php?$
      {
          #密码访问 ***********  主要是这2句话  且后面auth_basic_user_file地址一定写对
          auth_basic "please input username and password";
          auth_basic_user_file /www/nginx/conf/vhosts/chongpwd.db;

          client_max_body_size    20m;
          fastcgi_pass 127.0.0.1:9000;
          try_files $uri $uri/ /index.php?$query_string;
          fastcgi_index index.php;
          fastcgi_param  SCRIPT_FILENAME    $document_root$fastcgi_script_name;
          include fastcgi_params;
      }

}

第一步
安装htpasswd
htpasswd是Apache密码生成工具，Nginx支持auth_basic认证，因此我门可以将生成的密码用于Nginx中，输入一行命令即可安装：yum -y install httpd-tools ，参数如下：
-c 创建passwdfile.如果passwdfile 已经存在,那么它会重新写入并删去原有内容.
-n 不更新passwordfile，直接显示密码
-m 使用MD5加密（默认）
-d 使用CRYPT加密（默认）
-p 使用普通文本格式的密码
-s 使用SHA加密
-b 命令行中一并输入用户名和密码而不是根据提示输入密码，可以看见明文，不需要交互
-D 删除指定的用户

第二步
############ 添加chong用户名  会提示输入密码 输入2次即可 （现在的密码默认都加密，暂时无法不加密）
htpasswd  -m  /www/nginx/conf/vhosts/chongpwd.db   chong    

第三步 访问站点
 
![1](https://github.com/zhangyanchong/nginx/blob/master/img/nginx_basic_1.png)



</pre></code>