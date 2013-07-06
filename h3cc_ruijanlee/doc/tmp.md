4. Openwrt初始配置
======

###4.1 配置无线设置
接下来重新打开putty，地址依旧是192.168.1.1，但是协议选择ssh  

输入用户名root回车 然后输入你刚刚设定的密码  

接下来要修改配置文件，openwrt系统只有vi没有nano，  
各位如果从未使用过vi编辑器，  
个人觉得还是不使用的好，  
因为我也不知道如何教你们使用，  
各位如果稍微有点上进心的话，到下面这个链接学习vi的使用。  
http://vbird.dic.ksu.edu.tw/linux_basic/0310vi_1.php

vi编辑文件命令如下，这里给出来但是我们不需要用：  
输入vi /etc/config/wireless回车，就可以编辑无线网络的设置。  

这里我们使用一个初学者相对来说比较简单的方法，  
就是将配置文件下载下来，在win7中编辑好再上传上去。

使用winscp工具连接上路由器。  
填写好连接信息，使用scp协议。  

![](https://raw.github.com/ruijanlee/h3cc/master/h3cc_ruijanlee/_img/c4-01.jpg)  

连接上去之后，显示了右边显示的是路由器内部的文件。  
![](https://raw.github.com/ruijanlee/h3cc/master/h3cc_ruijanlee/_img/c4-02.jpg)  
如图显示了当前的目录是/root， 

进入上级目录，然后去到/etc/config/ 编辑wireless文件。  

![](https://raw.github.com/ruijanlee/h3cc/master/h3cc_ruijanlee/_img/c4-03.jpg)  

修改后的示例：  

```
config wifi-device  radio0
        option type     mac80211
        option channel  11
        option hwmode   11ng
        option path     'platform/ar933x_wmac'
        option htmode   HT40-
        list ht_capab   SHORT-GI-20
        list ht_capab   SHORT-GI-40
        list ht_capab   RX-STBC1
        list ht_capab   DSSS_CCK-40
        option noscan 1
        # REMOVE THIS LINE TO ENABLE WIFI:
        option disabled 0

config wifi-iface
        option device   radio0
        option network  lan
        option mode     ap
        option ssid     h3cc_wifi
        option encryption 'psk2'
        option key 'ruijanleezhbit'
```

无线的配置文件一般分为两块，上面那块是关于硬件的设置的，  
各位不要复制我的配置文件覆盖你的，原来是怎样就怎样， 
因为大家的硬件可能不同。  

上面那块最后一行默认是option disabled 1，意思是不开wifi，  
把1修改成0，即可打开wifi。  

可以看到上面有channel可以设置信道，  
也有hwmode可以设置工作模式 ，这里用默认就可以了。  

下面的那块按照我的写或者复制我的也行。  
填写的时候注意，那个对齐的空位不是按很多个空格，  
而是按键盘上的tab键，也就是Q键左边的那个。  
不过这个只影响排版的视觉效果，好像不影响系统的配置。  

你可以把上面的h3cc_wifi改成你想要的wifi名字。  
你可以把上面的ruijanleezhbit改成你想要的wifi密码   
（注意密码为数为9-12位）

###4.2 有线的配置

然后修改/etc/config/network文件。  

这一步中，由于703n和720n的网络接口数量不同，配置略有不同。  

720n修改后的示例：  

```
config interface 'loopback'
	option ifname 'lo'
	option proto 'static'
	option ipaddr '127.0.0.1'
	option netmask '255.0.0.0'

config interface 'lan'
	option ifname 'eth0'
	option type 'bridge'
	option proto 'static'
	option ipaddr '192.168.1.1'
	option netmask '255.255.255.0'
	option ip6addr 'fc00:0101:0101::1/64'

config interface 'wan'
	option ifname 'eth1'
	option _orig_ifname 'eth1'
	option _orig_bridge 'false'
	option proto 'static'
	option ipaddr '10.15.34.88'
	option netmask '255.255.254.0'
	option gateway '10.15.35.254'
	option broadcast '10.15.35.255'
	option dns '10.0.10.10'
	option macaddr '90:2B:34:66:88:05'
```

这里要用到你的网络参数了， 这里讲解一下：  
ipaddr 写你的IP地址  
netmask 子网掩码  
gateway 默认网关  
broadcast 广播地址，前三位同网关，后面改成255  
dns 就是DNS地址，10.0.10.10是我们学校的校内服务器，略坑  
macaddr mac地址  

以上信息确认填写正确，  
如果你不知道自己的这些信息，请看windows那章相关信息。 

###3.6 OpenWrt的恢复模式
如果你以上信息设置错误，很容易连接不上路由器或者出各种问题。  
这个时候可以清空路由器的设置，重新配置。  

方法就是
