#nginx  反向代理解决跨域

服务器介绍
   web服务器：web.nbsay.cn     http://web.nbsay.cn/1.html
   接口服务器：api.nbsay.cn    (http://api.nbsay.cn/1.php  正常连接返回数据)
1.html 代码如下
```
<html>
<body>
    qweqwewqe
<script src="https://code.jquery.com/jquery-3.1.1.min.js"></script>
<script>
      /*
     * 提交
     * **/
    function  tijiao() {
        var data={};
        $.ajax({
            type: "GET",
            url: "http://api.nbsay.cn/api/1.php",
            dataType:'json',
            data: data,
            success: function(info){
                   console.log(info);
            }
        });

    }
    tijiao();
</script>
</body>
</html>
```
web服务器每次请求接口，需要加上api域名和api目录 如：http://api.nbsay.cn/api/
其他的路径跟接口服务器是一样的  

接口服务器不用任何操作nginx配置，只需要操作web服务器的nginx配置 
 第一种  
 web服务器nginx配置加入
```  location ^~ /api/
   {
    proxy_pass http://api.nbsay.cn/;
   } 
```  
  宝塔的完成的配置文件
```      location ^~ /api/
    {
        proxy_pass http://xs.nbsay.cn/;
        proxy_set_header Host xs.nbsay.cn;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header REMOTE-HOST $remote_addr;
        
        add_header X-Cache $upstream_cache_status;
        
        #Set Nginx Cache
        
        
        set $static_file0m5DsEmF 0;
        if ( $uri ~* "\.(gif|png|jpg|css|js|woff|woff2)$" )
        {
        	set $static_file0m5DsEmF 1;
        	expires 12h;
            }
        if ( $static_file0m5DsEmF = 0 )
        {
        add_header Cache-Control no-cache;
        }
    }
```
  
  
```  
 如果是宝塔可以开启反向代理 操作如下
    开启代理->高级功能
    代码名称  daili  （随便写）
    代理目录  /api/
    目标url  http://api.nbsay.cn  发送域名   api.nbsay.cn

     
   
   
