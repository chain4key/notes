# 配置本地 github 环境

## gitconfig 配置

```bash
git config --global user.name "chain4key"
git config --global user.email "JulioBAyres2018@gmail.com"
git config --global color.ui true
git config --global core.autocrlf false
git config --global gui.encoding utf-8
git config --global core.quotepath off
git config --global push.default matching
git config --global push.default simple
```

## 设置 ssh 密钥认证

+ 生成密钥对

```bash
ssh-keygen -t rsa -b 4096 -C "JulioBAyres2018@gmail.com" -f ~/.ssh/chain4key.pem
```

+ 通过 config 指定密钥

```bash
vim ~/.ssh/config

Host github.com
     HostName github.com
     User git
     IdentityFile ~/.ssh/chain4key.pem

chmod 0600 ~/.ssh/config
```

+ 检验

```bash
ssh -T git@github.com
```  

