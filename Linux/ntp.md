
## 安装ntp
## 修改配置文件:
    
restrict :配置运行内网中其他机器同步时间和允许上层时间服务器主动修改本机时间
    
server :配置层时间服务器

## 开始同步
```
ntpdate -u 192.168.0.102  
```

## 主要说明
| 选项 | 意义 |
| --- | --- |
| nomodify | 客户端不能修改服务器时间，但是可以从服务器获取时间 |
| notrap | 客户端不能使用trap（远端事件登录功能remote event logging） |
| noquery | 其他客户端不能从本机获取时间 |

>[使用ntp服务同步时间](https://blog.csdn.net/u013712826/article/details/61938675)