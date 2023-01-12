# Linux 坑和小技巧

Data: 2023/1/11\
Description: Linux 坑和小技巧\
Category: Linux\
Tag: Linux

## Sysem Service
``` bash
sudo systemctl is-enabled ***.service #查看服务是否开机自动启动
sudo systemctl enable/disable ***.service
sudo systemctl start/stop/status/restart ***.service
```

## rm
``` bash
rm !(a.txt) # 删除当前目录下所有文件除了a.txt
```