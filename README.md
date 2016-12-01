### 一. NAT 穿透

## 1. 例如生产环境的机器可以允许给外网服务, 但是生产环境内部机器不允许外网下载数据, 环境的ssh入口点是2022, 用户值可以通过这个端口所有机器进行配置

   ssh 参数-fN使本命令在后台运行,不会占用终端
   
### 1.1 启动ssh方向反向代理命令
   例如 生产环境入口点是linux机器, 外网ip地址是173.173.173.173, ssh对外NAT绑定端口是2200

   ```
   ssh -fN  -R 37541:localhost:22 username@173.173.173.173 -p 2200
   ```

   执行以后通过ssh登录到远端服务器打印
```
   ssh username@173.173.173.173 'netstat -nptl'
  
   Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name   
   ... 
   tcp        0      0 127.0.0.1:37541         0.0.0.0:*               LISTEN      -                   
   ...
```


## 1.2 反向端口应用

   生产环境入口点有37541端口, 但是是绑定127.0.0.1, 所以只能本地使用. 本来此机器不能用ssh连接到外网机器,但是通过这个端口可以访问到执行命令的机器上`ssh -fN  -R 37541:localhost:22 username@173.173.173.173 -p 2200`

   命令如下:
```
   ssh -p 37541 username@127.0.0.1 
```



