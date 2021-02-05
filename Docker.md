# Docker

[docker mirror](https://iyt48kzn.mirror.aliyuncs.com)

## docker install dependencies

```bash
yum install -y yum-utils device-mapper-persistent-data lvm2
```

## docker repo

```bash
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

## docker install

```bash
yum install -y docker-ce docker-ce-cli containerd.io
```

## start docker service

```bash
systemctl start docker && systemctl enable docker
```

## checkout docker version

```bash
docker version
```

## get docker image methods

1. ​	

   ```bash
   docker pull [centos]
   ```

   change docker images download URL

   https://cr.console.aliyun.com it like this: https://iyt48kzn.mirror.aliyuncs.com

   change your file `/etc/docker/daemon.json`

   

   ```bash
   mkdir -p /etc/docker # if your machine have this direction,ignore this command
   sudo t ee /etc/docker/daemon.json <<-'EOF'
   {
   	"registry-mirrors":["https://iyt48kzn.mirror.aliyuncs.com"]
   }
   EOF
   systemctl daemon-reload
   systemctl restart docker
   ```

   ## change your router forward

   `vim /etc/sysctl.conf` or `vim /etc/sysctl.d` insert content of this

   ```bash
   net.ipv4.ip_forward = 1
   ```

   ## checkout router forward if work

   ```bash
   cat /proc/sys/net/ipv4/ip_forward
   1
   ```

   2.

   change docker service script `vim /usr/lib/systemd/system/docker.service`

   add `--registry-mirror=https://iyt48kzn.mirror.aliyuncs.com` at line 14 `fd://` back

   3.

   ```bash
   docker load -i /root/centos.tar
   ```

   4.

   ```bash
   docker pull xxx.xxx.com/library/xxx:lastest
   ```

   