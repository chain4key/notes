# 一些小技巧

## 1. http.server （一行 python 代码的 web 服务器）

http.server 是 python3 默认的一个模块，在 python2 里这个模块叫 SimpleHTTPServer ，使用这个模块，你可以通过如下命令在当前目录启动一个 web 服务，可以很方便的用来展示静态页面，及文件

```bash
cd /tmp/
sudo python3 -m http.server 80

sudo python2 -m SimpleHTTPServer 80
```
