OnlyTransfer 是一个专为Minecraft 1.21+服务器设计的轻量级transfer跨服管理插件，兼容Paper/Spigot、Java 21环境，完全重写了原版transfer跨服指令的逻辑，通过令牌验证机制，OnlyTransfer确保只有玩家可以通过指定方式从登录服进入您的主服务器，特别适合登录和主服务器分离的环境。

你需要在双方服务器上同时安装本插件，配置相同的token和允许跨服的服务器IP列表。支持配置是否允许通过服务器列表直接加入。若禁用，只有通过/transfer的玩家可进入；若启用，玩家可从服务器列表直接加入。仅允许传送配置文件中列出的目标服务器。


使用方法

将插件放入plugins文件夹，编辑config.yml，设置令牌（transferToken）、允许的服务器列表（allowedServers）和服务器列表加入选项（allowServerList）


配置文件

```yml
# 是否允许通过服务器列表直接进入服务器
allow-server-list: false

# 跨服传送的令牌，两台服务器必须配置相同的令牌
transfer-token: "1145141919810"

# 允许的服务器IP和端口
allowed-servers:
  - "127.0.0.1:25565"
```

下载插件

https://enanetdisk.pages.dev/?file=%2Fdisk%2FMinecraftPlugins%2FOnlyTransfer.jar

转载请注明作者：ENA