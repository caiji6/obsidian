# 👌一台 Linux 系统初始化环境后需要做一些什么安全工作？

### 1. 更新系统
**更新操作系统和软件包**：确保系统和所有已安装的软件包都是最新的，并应用所有安全补丁。

```plain
sudo apt update && sudo apt upgrade -y   # Debian/Ubuntu
sudo yum update -y                       # CentOS/RHEL
sudo dnf update -y                       # Fedora
```

### 2. 配置用户和权限
**禁用或删除不必要的用户**：删除或禁用默认的、不必要的用户账户。

```plain
sudo userdel username
sudo passwd -l username   # 锁定用户
```

**设置强密码策略**：配置密码策略，如密码复杂度、有效期等。

```plain
sudo vi /etc/security/pwquality.conf
```

**创建和配置用户组**：创建必要的用户组，并将用户分配到相应的组。

```plain
sudo groupadd groupname
sudo usermod -aG groupname username
```

### 3. 配置 SSH
**禁用 root 登录**：编辑 SSH 配置文件，禁用 root 用户通过 SSH 登录。

```plain
sudo vi /etc/ssh/sshd_config# 设置 PermitRootLogin no
```

**使用密钥认证**：配置 SSH 使用密钥认证而不是密码认证。

```plain
sudo vi /etc/ssh/sshd_config# 设置 PasswordAuthentication no
```

**更改默认 SSH 端口**：更改 SSH 服务的默认端口（22），以增加安全性。

```plain
sudo vi /etc/ssh/sshd_config# 设置 Port 新端口号
```

### 4. 配置防火墙
**启用并配置防火墙**：使用`ufw`、`firewalld`或`iptables`配置防火墙规则。

```plain
sudo ufw enable
sudo ufw allow ssh
sudo ufw allow http
sudo ufw allow https
sudo ufw status
```

或者使用`firewalld`：

```plain
sudo systemctl start firewalld
sudo firewall-cmd --permanent --add-service=ssh
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
sudo firewall-cmd --reload
```

### 5. 安装和配置安全工具
**安装 Fail2ban**：安装并配置 Fail2ban，以防止暴力破解攻击。

```plain
sudo apt install fail2ban   # Debian/Ubuntu
sudo yum install fail2ban   # CentOS/RHEL
sudo systemctl enable fail2ban
sudo systemctl start fail2ban
```

**安装和配置防病毒软件**：安装防病毒软件（如 ClamAV）并定期扫描系统。

```plain
sudo apt install clamav
sudo freshclam
sudo clamscan -r /home
```

### 6. 配置日志和监控
**配置日志审计**：确保系统日志记录正常，并配置日志轮转。

```plain
sudo vi /etc/rsyslog.conf
sudo systemctl restart rsyslog
```

**安装和配置监控工具**：安装系统监控工具，如`Nagios`、`Zabbix`或`Prometheus`，以监控系统性能和安全事件。



> 原文: <https://www.yuque.com/jingdianjichi/xyxdsi/iydsdzgsx7ugxg29>