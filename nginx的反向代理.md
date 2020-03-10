
例子 基本用法


	location ^~ /advApi {   
	     proxy_pass http://part-admin.abc.cn;  
	     proxy_set_header X-Real-IP $remote_addr;  
	     proxy_set_header REMOTE-HOST $remote_addr;  
	     proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;  
	 }

或者

	location /advApi {
	  proxy_pass http://part-admin.bcd.cn;
	  proxy_set_header X-Real-IP $remote_addr;
	  proxy_set_header REMOTE-HOST $remote_addr;
	  proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
	
	}
