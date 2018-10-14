# openssh

### centos 7 升级到 openssh-7.7p1

+ 环境准备

```sh
yum groupinstall Development tools -y
yum install openssl-devel glibc-devel krb5-devel pam-devel -y
yum install rpm-build -y
```

+ 创建目录

```sh
mkdir -pv rpmbuild/{BUILD,BUILDROOT,RPMS,SOURCES,SPECS,SRPMS}
```

+ 下载源码，修改 openssh.spec 文件

```sh
cd rpmbuild/SOURCES
curl -O https://cdn.openbsd.org/pub/OpenBSD/OpenSSH/portable/openssh-7.7p1.tar.gz
tar -xvf openssh-7.7p1.tar.gz
```

openssh.spec 做如下修改

```sh
12 %define no_x11_askpass 1
...
15 %define no_gnome_askpass 1
...
103 # BuildRequires: openssl-devel < 1.1
```

+ 生成 rpm 包

```sh
cd rpmbuild/SPECS
rpmbuild -bb openssh.spec
```

+ 安装 rpm 包

```sh
cd rpmbuild/RPMS/x86_64
rm openssh-debuginfo
yum install ./
```
