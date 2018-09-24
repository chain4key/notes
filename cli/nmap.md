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
nmap -O -T4 -Pn -oG OSDetect.gnmap -iL LiveHosts.txt
```

+ 系统和服务检测

```sh
nmap -O -sV -T4 -Pn -p U:53,111,137,T:21-25,80,139,8080 -oG OS_Service_Detct.gnmap -iL LiveHosts.txt
```

### nmap 躲避防火墙

+ 分段 

```sh
nmap -f 
```

+ 修改默认 MTU 大小，但必须为 8 的倍数(8，16，24，32 等等)

```sh
nmap --mtu 24
```

+ 生成随机数量的欺骗

```sh
nmap -D RND:10 [Target IP]
```

+ 手动指定欺骗使用的 IP

```sh
nmap -D decy1,decoy2,decoy3 etc.
```

+ 僵尸网络扫描，首先要找到僵尸网络的 IP

```sh
nmap -sI [Zombie IP] [Target IP]
```

+ 指定源端口号

```sh
nmap --source-port 80 [Target IP]
```

+ 在每个扫描数据包后追加随机数量的数据

```sh
nmap --data-length 25 [Target IP]
```

+ MAC 地址欺骗，可以生成不同主机的 MAC 地址

```sh
nmap --spoof-mac Dell/Apple/3Com [Target IP]
```

### nmap 进行 WEB 扫描

```sh
cd /usr/share/nmap/scripts
wget http://www.computec.ch/projekte/vulscn/download/nmap_nse_vulsan-2.0.tar.gz
tar -xvf nmap_nse_vulsan-2.0.tar.gz
nmap -sS -sV --script=vulscan/vulscan.nse [Target IP]
nmap -sS -sV --script=vulscan/vulscan.nse -script-args vulscandb=scipvuldb.csv [Target IP]
nmap -sS -sV --script=vulscan/vulscan.nse -script-args vulscandb=scipvuldb.csv -p80 [Target IP]
nmap -PN -sS -sV --script=vulscan -script-args vulscancorrelation=1 -p80 [Target IP]
nmap -sV --script=vuln [Target IP]
nmap -PN -sS -sV --script=all -script-args vulscancorrelation=1 [Target IP]
```
