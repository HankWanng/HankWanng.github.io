# Flask 部署

## gunicorn部署（不支持windows）

gunicorn作为服务器，安装gunicorn

pip3 install gunicorn

启动

==gunicorn -w 3 -b 0.0.0.0:8000 app:app==

-w 处理进程数

-b 运⾏主机ip端⼝

dpj.wsgi 项⽬的wsgi

gunicorn常⽤配置

-c CONFIG : CONFIG,配置⽂件的路径，通过配置⽂件启动；⽣产环境使⽤； 

-b ADDRESS : ADDRESS，ip加端⼝，绑定运⾏的主机； 

-w INT, --workers INT：⽤于处理⼯作进程的数量，为正整数，默认为1； 

-k STRTING, --worker-class STRTING：要使⽤的⼯作模式，默认为sync异步，可以下载 

eventlet和gevent并指定 

--threads INT：处理请求的⼯作线程数，使⽤指定数量的线程运⾏每个worker。为正整数，默认为1。 

--worker-connections INT：最⼤客户端并发数量，默认情况下这个值为1000。 

--backlog int：未决连接的最⼤数量，即等待服务的客户的数量。默认2048个，⼀般不修改； 

-p FILE, --pid FILE：设置pid⽂件的⽂件名，如果不设置将不会创建pid⽂件 

--access-logfile FILE ： 要写⼊的访问⽇志⽬录--access-logformat STRING：要写⼊的访问⽇志格式 

--error-logfile FILE, --log-file FILE ： 要写⼊错误⽇志的⽂件⽬录。 

--log-level LEVEL ： 错误⽇志输出等级。 

--limit-request-line INT ： HTTP请求头的⾏数的最⼤⼤⼩，此参数⽤于限制HTTP请求⾏的允 

许⼤⼩，默认情况下，这个值为4094。值是0~8190的数字。 

--limit-request-fields INT ： 限制HTTP请求中请求头字段的数量。此字段⽤于限制请求头字 

段的数量以防⽌DDOS攻击，默认情况下，这个值为100，这个值不能超过32768 

--limit-request-field-size INT ： 限制HTTP请求中请求头的⼤⼩，默认情况下这个值为8190 

字节。值是⼀个整数或者0，当该值为0时，表示将对请求头⼤⼩不做限制 

-t INT, --timeout INT：超过这么多秒后⼯作将被杀掉，并重新启动。⼀般设定为30秒； 

--daemon： 是否以守护进程启动，默认false； 

--chdir： 在加载应⽤程序之前切换⽬录； 

--graceful-timeout INT：默认情况下，这个值为30，在超时(从接收到重启信号开始)之后仍然活着 

的⼯作将被强⾏杀死；⼀般使⽤默认； 

--keep-alive INT：在keep-alive连接上等待请求的秒数，默认情况下值为2。⼀般设定在1~5秒之 

间。 

--reload：默认为False。此设置⽤于开发，每当应⽤程序发⽣更改时，都会导致⼯作重新启动。 

--spew：打印服务器执⾏过的每⼀条语句，默认False。此选择为原⼦性的，即要么全部打印，要么全部 

不打印； 

--check-config ：显示现在的配置，默认值为False，即显示。 

-e ENV, --env ENV： 设置环境变量



## Tornado部署(windows方案)

### 制作完成一个flask框架 启动入口名为app.py

### app.py文件夹内新建一个tornado server的启动程式

![image-20210425130409936](C:\Users\C3939\AppData\Roaming\Typora\typora-user-images\image-20210425130409936.png)

.py脚本的内容：

```python
# coding=utf-8
from tornado.wsgi import WSGIContainer
from tornado.httpserver import HTTPServer
from tornado.ioloop import IOLoop
import app				#入口文件app.py


if __name__ == '__main__':
    http_server = HTTPServer(WSGIContainer(app.app)) #app文件里的app函数
    http_server.listen(5000) #端口号自定义
    IOLoop.instance().start()

```



### 若要开机启动则新建一只BAT脚本启动server文件

隐藏运行：

```bat
@echo off
if "%1"=="h" goto begin
start mshta vbscript:createobject("wscript.shell").run("""%~nx0"" h",0)(window.close)&&exit
:begin

cd\
D:
cd "GIT\test"
start /b python tornado_server.py
exit
```

将脚本放入开机启动文件夹内：

shell：startup

![image-20210425130851701](C:\Users\C3939\AppData\Roaming\Typora\typora-user-images\image-20210425130851701.png)