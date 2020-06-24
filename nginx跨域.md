nginx 也可以配置跨域
主要代码
	
	add_header 'Access-Control-Allow-Origin' '*';   
	add_header 'Access-Control-Allow-Credentials' 'true';   
	add_header 'Access-Control-Allow-Methods' 'OPTION, POST, GET';   
	add_header 'Access-Control-Allow-Headers' 'X-Requested-With, Content-Type';


下面完整例子
----------------------------------------
	
	server {
	        listen        80;
	        server_name  chatserver.nbsay.cn;
	        root   "/home/wwwroot/chatServerApi/public";
	
	    add_header 'Access-Control-Allow-Origin' '*';
	    add_header 'Access-Control-Allow-Credentials' 'true';
	    add_header 'Access-Control-Allow-Methods' 'OPTION, POST, GET';
	    add_header 'Access-Control-Allow-Headers' 'X-Requested-With, Content-Type';
	
	
	    location / {
	      index index.php index.html error/index.html;
	            if (!-e $request_filename)
	           {
	           rewrite ^/(.*)$ /index.php?/$1 last;        break;
	           }
	            autoindex  off;      
	 
	     }
	
	
	       location ~ \.php(.*)$ {
	            proxy_set_header Host $host;
	           proxy_set_header X-Real-IP $remote_addr;
	           proxy_set_header REMOTE-HOST $remote_addr;
	           proxy_set_header X-Forwarded-For
	           $proxy_add_x_forwarded_for;
	
	          
	
	             try_files $uri =404;
	            fastcgi_pass  unix:/tmp/php-cgi.sock;
	            fastcgi_index index.php;
	            include  /usr/local/nginx/conf/fastcgi.conf;
	            fastcgi_param  HTTP_X_FORWARDED_FOR $http_x_forwarded_for;
	            fastcgi_param HTTP_CLIENT_IP $http_client_ip; 
	     
	
	          
	
	           set_real_ip_from 0.0.0.0/0;
	           real_ip_header X-Forwarded-For;
	           real_ip_recursive on;  
	
	    }
	
	      access_log  /home/wwwlogs/chat_access.log;
	}

