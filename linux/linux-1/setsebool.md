# setsebool

修改SElinux策略内各项规则的布尔值

## 补充说明

**setsebool命令** 是用来修改SElinux策略内各项规则的布尔值。setsebool命令和getsebool命令是SELinux修改和查询布尔值的一套工具组。SELinux的策略与规则管理相关命令：seinfo命令、sesearch命令、getsebool命令、setsebool命令、semanage命令。

### 语法

```text
setsebool [-P] 布尔值=[0|1]
```

### 选项

```text
-P:直接将设置值写入配置文件，该设置数据将来会生效的。
```

### 实例

允许vsvtp匿名用户写入权限：

```text
setsebool -P allow_ftpd_anon_write=1
```

如果你希望你的ftp用户可以访问自己的家目录的话，需要开启：

```text
setsebool -P ftp_home_dir 1
```

如果你希望将vsftpd以daemon的方式运行的话，需要开启：

```text
setsebool -P ftpd_is_daemon 1
```

你可以让SElinux停止保护vsftpd的daemon方式动行：

```text
setsebool -P ftpd_disable_trans 1
```

HTTP被设置允许cgi的设置：

```text
setsebool -P httpd_enable_cgi 1
```

允许用户HHTP访问其家目录，该设定限仅于用户的家目录主页：

```text
setsebool -P httpd_enable_homedirs 1
chcon -R -t httpd_sys_content_t ~user/public_html
```

允许httpd访问终端：

```text
setsebool -P httpd_tty_comm 1
```

关闭Selinux的关于httpd进程守护的保护：

```text
setsebool -P httpd_disable_trans 1
service httpd restart
```

关于named、master更新selinux设定：

```text
setsebool -P named_write_master_zones 1
```

关闭named的进程守护保护：

```text
setsebool -P named_disable_trans 1
service named restart
```

Selinux将本机的NFS共享设置成只读：

```text
setsebool -P nfs_export_all_ro 1
```

SElinux将本机的NFS共享设置成可读可写：

```text
setsebool -P nfs_export_all_rw 1
```

如果你想要将远程NFS的家目录共享到本机，需要开启：

```text
setsebool -P use_nfs_home_dirs 1
```

如果samba服务器共享目录给多个域，则需要：

```text
setsebool -P allow_smbd_anon_write=1
```

samba服务器要共享家目录时：

```text
setsebool -P samba_enable_home_dirs 1
```

如果你需在本机上使用远程samba服务器的家目录：

```text
setsebool -P use_samba_home_dirs 1
```

关闭selinux关于samba的进程守护的保护：

```text
setsebool -P smbd_disable_trans 1
service smb restart
```

允许rsync其他用户写入时：

```text
setsebool -P allow_rsync_anon_write=1
```

停止rsync的进程保护

```text
setsebool -P rsync_disable_trans 1
```

允许系统使用kerberos：

```text
setsebool -P allow_kerberos 1
```

系统工作在nis环境时：

```text
setsebool -P allow_ypbind 1
```

