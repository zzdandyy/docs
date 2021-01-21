# ssh-copy-id

把本地的ssh公钥文件安装到远程主机对应的账户下

## 补充说明

**ssh-copy-id命令** 可以把本地主机的公钥复制到远程主机的authorized\_keys文件上，ssh-copy-id命令也会给远程主机的用户主目录（home）和`~/.ssh`, 和`~/.ssh/authorized_keys`设置合适的权限。

### 语法

```text
ssh-copy-id [-i [identity_file]] [user@]machine
```

### 选项

```text
-i：指定公钥文件
```

### 实例

1、把本地的ssh公钥文件安装到远程主机对应的账户下：

```text
ssh-copy-id user@server
ssh-copy-id -i ~/.ssh/id_rsa.pub user@server
```

