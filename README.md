# onekey-tun2socks

Onekey tun2socks 一键脚本

这是一个为纯 IPv6 VPS 一键添加 IPv4 网络出口的工具，基于高性能 SOCKS5 隧道项目 hev-socks5-tunnel 实现。它通过创建虚拟网卡（TUN）并配置路由表，让所有 IPv4 流量经过 SOCKS5 代理转发，从而解决纯 IPv6 环境下无法访问 IPv4 资源的问题。

✨ 功能特点
自动下载并安装最新版 hev-socks5-tunnel 二进制文件

三种安装模式：

alice：预置公开的 Alice 节点，安装时只需选择出口端口

legend：预置 Legend 节点（需手动填写配置）

custom：使用你自己的 SOCKS5 代理服务器（支持 IPv4/IPv6）

自动配置 systemd 服务、IPv4/IPv6 双栈路由规则

支持 alice 模式一键切换 SOCKS5 端口

内置 DNS64 自动切换，确保在纯 IPv6 环境中能正常下载所需文件

🚀 快速开始
下载运行脚本
```bash
curl -6 -L -o onekey-tun2socks.sh https://raw.githubusercontent.com/everything-cd/onekey-tun2socks/refs/heads/main/onekey-tun2socks.sh && chmod +x onekey-tun2socks.sh && sudo ./onekey-tun2socks.sh -i alice
```
安装过程中会自动完成以下操作：

检测并解锁 /etc/resolv.conf（如果被锁定）

临时设置 DNS64 服务器，确保能访问 GitHub

从 GitHub API 获取 hev-socks5-tunnel 最新版本下载链接

下载适用于 linux-amd64 架构的二进制文件

提示你选择一个出口端口（输入数字 1-8 选择一个）

根据所选模式生成配置文件

创建 systemd 服务并启动

4. 验证安装
```
curl -4 ip.sb
```
若返回一个 IPv4 地址（非 127.0.0.1），说明隧道已成功建立，IPv4 出口正常工作。

🧰 命令参考
操作	命令
## 安装 alice 模式	
```
sudo ./onekey-tun2socks.sh -i alice
```
## 安装 legend 模式	
```
sudo ./onekey-tun2socks.sh -i legend
```
## 安装自定义模式	
```sudo ./onekey-tun2socks.sh -i custom
```
## 切换 alice 端口	
```
sudo ./onekey-tun2socks.sh -s
```
## 卸载 tun2socks	
```
sudo ./onekey-tun2socks.sh -r
```
## 更新脚本自身	
```
sudo ./onekey-tun2socks.sh -u
```
## 查看帮助	
```
sudo ./onekey-tun2socks.sh -h
```
# 🗑️ 完全卸载
执行卸载命令后，脚本会自动完成以下清理步骤：

```bash
sudo ./onekey-tun2socks.sh -r
```
清理内容
停止并禁用``` tun2socks.service```

删除 systemd 服务文件 (/etc/systemd/system/tun2socks.service)

删除配置文件目录 (/etc/tun2socks)

删除二进制文件 (/usr/local/bin/tun2socks)

清理所有相关的 IP 路由规则（fwmark、路由表、优先级规则等）

提示：卸载后建议重启服务器，确保所有路由规则彻底生效或消失。

⚙️ 手动管理服务
安装完成后，你可以使用 systemd 命令管理隧道服务：

bash
# 查看状态
```
systemctl status tun2socks.service
```
# 启动
```
sudo systemctl start tun2socks.service
```

# 停止
```
sudo systemctl stop tun2socks.service
```
# 重启
```
sudo systemctl restart tun2socks.service
```

# 查看实时日志
```
journalctl -u tun2socks.service -f
```
📁 文件与配置
类型	路径
二进制文件	/usr/local/bin/tun2socks
配置文件	/etc/tun2socks/config.yaml
systemd 服务	/etc/systemd/system/tun2socks.service
