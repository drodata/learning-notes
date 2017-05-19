# 服务器返回的时间戳不准确的问题

有一天，我意外发现自己购买的 Amazon AWS 服务器显示的时间不准确。抛开时区不说，服务器上返回的 UNIX 时间戳比本地主机上的时间慢了近五分钟。我又在国内阿里云 ECS 上进行了测试，发现 ECS 上显示是准确的，这表明是 AWS 的设置出现了问题。

搜到 Amazon [官方的一个解决方案][ntp-offical-solution], 但并不实用。在 ServerFault 上找到[匹配的问题][exact-question]，下面是步骤：

首先安装 `ntpdate` package:


```
apt-get install ntpdate
```
  
> :bell: 安装是可能会遇到 Local 缺失的提示信息，执行 `sudo locale-gen UTF-8` 可解决

然后使用 root 用户执行 `ntpdate -u pool.ntp.org`, 输出大致内容如下：

```
# ntpdate -u pool.ntp.org
19 May 03:48:38 ntpdate[26339]: adjust time server 203.122.222.149 offset 0.025985 sec
```

"offset 0.0..." 表示日期已同步。


[ntp-offical-solution]: http://docs.aws.amazon.com/zh_cn/AWSEC2/latest/UserGuide/set-time.html#configure_ntp "配置网络时间协议 (NTP)"
[exact-question]: https://serverfault.com/questions/513326/wrong-time-on-my-ubuntu-amazon-ec2-instances

