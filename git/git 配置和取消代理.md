# git 配置和取消代理

### 为何要配置git代理

在使用了ShadowSocks代理系统之后，git如不设置代理可能会造成通信障碍。可以由以下方法设置或取消代理

### git 配置代理

```
git config --global http.proxy 'socks5://127.0.0.1:1080'
git config --global https.proxy 'socks5://127.0.0.1:1080'
```

### git 取消代理

```
git config --global --unset http.proxy
git config --global --unset https.proxy
```
