## 设置备份路径
```
容器路径：/var/opt/gitlab/backups
主机路径： /srv/gitlab/data/backups
```
## 备份命令(容器中执行)
```
创建备份
gitlab-rake gitlab:backup:create
恢复备份
gitlab-rake gitlab:backup:restore BACKUP=1502357536_2017_08_10_9.4.3

```

## 重启生效配置文件

```
gitlab-ctl reconfigure
```

参考资料
https://blog.csdn.net/ouyang_peng/article/details/77070977
