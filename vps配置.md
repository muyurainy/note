## 安装配置shadowsocks ##
### 1.安装pip ###
**ubuntu**:  
    sudo apt-get install python-pip  
**Centos**  
    yum install python-setuptools && easy_install pip  
之后运行  
    pip install shadowsocks  
### 2.配置Shadowsocks ###
安装完成后，在/etc/下新建一个叫shadowsocks.json的配置文件，内容如下：
```
{
	"server" : "::",
	"server_port" : 8388,
	"local_address" : "127.0.0.1",
	"local_port" : 1080,
	"password" : "PASSWORD",
	"timeout" : 300,
	"method" : "rc4-md5"
}
```
### 3.启动运行 ###
执行以下开启：  
    ssserver -c /etc/shadowsocks.json --fast-open -d start
关闭：  
    ssserver -d stop
