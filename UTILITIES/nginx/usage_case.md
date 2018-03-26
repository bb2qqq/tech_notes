我决定在云服务器上使用nginx作为receptionist，来处理各种请求。

我会在服务器上开一个或多个gitbook服务（用不同的端口），然后开一个flaskr的进程。  

gitbook服务用来作为文档的展现和索引（最好是每一本book对应一个directory)  

现在第一步，我要在本地的nginx上，试一下重定向功能。  

我的第一个实验步骤，是把对本地gitbook首页的访问，重定向到一个html页面上。  

我需要先找到本地的nginx配置的地址，我决定先使用`which nginx`看一下  

通过google，我找到了OSX系统上nginx配置的位置
https://gist.github.com/jimothyGator/5436538

进一步搜索，我找到了一个教nginx redirect的教程  
https://www.nginx.com/blog/creating-nginx-rewrite-rules/


我对比了教程里的写法和我的默认Nginx配置里的写法，发现默认配置里有个location语句块在教程中不存在，所以我去搜了下关于location的用法教学
https://www.digitalocean.com/community/tutorials/understanding-nginx-server-and-location-block-selection-algorithms

看完之后，我终于做了第一个初始的redirection处理，code block 如下:

```
location  /gitbook {
    return 301 http://localhost:4000;
}
```

这样一个跳转，会直接将对servername/gitbook及其子目录的访问跳转到指定的新端口。

关于更多的match手段，请看[location tutorial](https://www.digitalocean.com/community/tutorials/understanding-nginx-server-and-location-block-selection-algorithms)里此部分：

```
* =: If an equal sign is used, this block will be considered a match if the request URI exactly matches the location given.

* ~: If a tilde modifier is present, this location will be interpreted as a case-sensitive regular expression match.
* ~*: If a tilde and asterisk modifier is used, the location block will be interpreted as a case-insensitive regular expression match.

* ^~: If a carat and tilde modifier is present, and if this block is selected as the best non-regular expression match, regular expression matching will not take place.
```

至此，目前我对nginx的需求已满足，进入下一个事项
