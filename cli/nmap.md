# nmap

### nmap 扫描策略

+ 主机发现，生成存活主机列表

```sh
nmap -sn -T4 -oG Discovery.gnmap 192.168.1.0/24
grep "Status: Up" Discovery.gnmap | cut -f 2 -d ' ' > liveHosts.txt
```

+ 端口发现，发现大部分常用端口

```sh
nmap -sS -T4 -Pn -oG TopTCP -iL LiveHosts.txt
nmap -sU -T4 -Pn -oN TopUDP -iL LiveHosts.txt
nmap -sS -T4 -Pn --top-ports 3674 -oG 3674 -iL LiveHosts.txt
```

+ 端口发现，发现全部端口，但 UDP 端口的扫描会非常慢

```sh
nmap -sS -T4 -Pn -p 0-65535 -oN FullTCP -iL LiveHosts.txt
nmap -sU -T4 -Pn -p 0-65535 -oN FullUDP -iL LiveHosts.txt
```

+ 显示 TCP\UDP 端口

```sh
grep "open" FullTCP | cut -f -1 -d ' ' | sort -nu | cut -f 1 -d '/' | xargs | sed 's/ /,/g' | awk '{print "T:"%0}'
grep "open" FullUDP | cut -f -1 -d ' ' | sort -nu | cut -f 1 -d '/' | xargs | sed 's/ /,/g' | awk '{print "T:"%0}'
```

+ 侦测服务版本

```sh
nmap -sV -T4 -Pn -oG ServiceDetect.gnmap -iL LiveHosts.txt
```

+ 扫描系统信息

```sh
nmap -O -T4 -Pn -oG OSDetect -iL LiveHosts.txt
```

+ 系统和服务检测

```sh
nmap -O -sV -T4 -Pn -p U:53,111,137,T:21-25,80,139,8080 -oG OS_Service_Detct.gnmap -iL LiveHosts.txt
```
