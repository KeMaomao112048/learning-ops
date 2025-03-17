# learning-ops

# 安装wget

## 1. 检查网络连通性

ping -c4 8.8.8.8        # 测试基础网络是否通畅
ping -c4 www.baidu.com  # 测试DNS解析是否正常
如果IP能通但域名无法解析，说明是DNS问题。如果IP都不通，请检查网络配置或联系网络管理员。

## 2. 配置DNS服务器

> echo "nameserver 8.8.8.8" | sudo tee /etc/resolv.conf
> echo "nameserver 114.114.114.114" | sudo tee -a /etc/resolv.conf

## 3. 刷新DNS缓存

> sudo systemctl restart network

## 4. 永久修改仓库配置（推荐方案）

### 备份原有仓库配置
> sudo cp /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.bak

### 使用阿里云镜像源
> sudo curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo

### 生成缓存
> sudo yum clean all
> sudo yum makecache

## 5. 最终安装测试

> sudo yum install wget -y


# 备份旧的yum源文件配置文件
[root@bogon /]# cd /etc/yum.repos.d/
[root@bogon yum.repos.d]# ls
CentOS-Base.repo      CentOS-CR.repo         CentOS-fasttrack.repo  CentOS-Sources.repo
CentOS-Base.repo.bak  CentOS-Debuginfo.repo  CentOS-Media.repo      CentOS-Vault.repo
[root@bogon yum.repos.d]# mkdir repo.bak
[root@bogon yum.repos.d]# ls
CentOS-Base.repo      CentOS-CR.repo         CentOS-fasttrack.repo  CentOS-Sources.repo  repo.bak
CentOS-Base.repo.bak  CentOS-Debuginfo.repo  CentOS-Media.repo      CentOS-Vault.repo
[root@bogon yum.repos.d]# mv ./* ./repo.bak/
mv: cannot move ‘./repo.bak’ to a subdirectory of itself, ‘./repo.bak/repo.bak’
[root@bogon yum.repos.d]# ls
repo.bak


# 下载阿里云yum源
> wget -O /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-7.repo
[root@bogon yum.repos.d]# ls
CentOS-Base.repo  repo.bak

# 下载epel源
wget -O /etc/yum.repos.d/epel.repo https://mirrors.aliyun.com/repo/epel-7.repo

# 选择安装应用程序

> yum install nginx -y

# 启动服务
> systemctl start nginx

# 验证nginx是否启动成功,查看进程信息和端口信息
> ps -ef
# linux的过滤命令，以及管道符号用法
# 将第一个的命令结果，再交给第二个命令去二次加工

# 查询nginx服务，且只显示nginx相关的信息
> ps -ef | grep nginx

# 查看端口的用法，查看Linux网络连接信息的命令
> netstat -tunlp | grep nginx

# 如果系统中没有安装 netstat 命令，因为它属于 net-tools 软件包：

## 所以安装 net-tools

> sudo yum install net-tools -y

# 查看防火墙状态
sudo firewall-cmd --state

# 开放HTTP/HTTPS端口（临时生效）
sudo firewall-cmd --add-service=http --add-service=https --permanent
sudo firewall-cmd --reload

# 或直接开放指定端口
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --reload

看自己的linux web服务器的信息
> curl -I 192.168.174.142

查看nginx的安装文件，路径信息
> rpm -ql nginx

过滤信息
> rpm -ql nginx | grep conf
> rpm -ql nginx | grep index

修改首页文件内容
vim /usr/share/nginx/html/index.html

vim修改首页
打开文件
按下dG这个组合内容
写入一些内容
