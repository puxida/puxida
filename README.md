# 普希达一键安装

仓库只放安装脚本。完整版 / 精简版二进制包约 145MB，超过 GitHub 普通文件 100MB 限制，必须作为 **Release 附件** 上传，不要 `git add *.tar.gz`。

安装脚本会在 AlmaLinux / Alpine / CentOS / Debian / Fedora / Rocky Linux / Ubuntu 上自动结束占用软件包锁的进程、补装缺失依赖，并在没有 systemd 时直接拉起面板进程。不会主动开启未运行的防火墙，以免把 SSH 22 端口挡掉。

## 其它服务器一键安装（x86_64 Linux，root）

完整版（主控，默认端口 41275）：

```bash
curl -fsSL https://raw.githubusercontent.com/y648394245-tech/puxida/main/install-full.sh | bash
```

精简版（子节点，默认端口 20999）：

```bash
curl -fsSL https://raw.githubusercontent.com/y648394245-tech/puxida/main/install-concise.sh | bash
```

Alpine 若还没有 bash / curl：

```bash
apk add --no-cache bash curl
curl -fsSL https://raw.githubusercontent.com/y648394245-tech/puxida/main/install-concise.sh | bash
```

指定端口和账号（不要用默认弱口令）：

```bash
curl -fsSL https://raw.githubusercontent.com/y648394245-tech/puxida/main/install-full.sh | \
  bash -s -- --port 41275 --username admin --password '你的强密码'
```

脚本会从本仓库最新 Release 下载：

- `pxd-full-father.tar.gz`
- `pxd-concise-son.tar.gz`

本机 80 端口分发（与面板受控端安装同一套包）：

```bash
curl -fsSL http://204.194.52.45/install-full-father | bash
curl -fsSL http://204.194.52.45/install-concise-son | bash
```

## 发布新版本（本机构建机上）

1. 只推送脚本到 `main`（本目录这几个文件）。
2. 创建或覆盖 Release 附件，把 `/opt` 里两个 tar 当附件上传。

```bash
cd /root/copy_code/puxida-github
git add README.md LICENSE .gitignore install-full.sh install-concise.sh
git commit -m "Sync multi-distro one-click installers"
git push origin main

gh release upload v20260921 \
  /opt/pxd_full_father/pxd-full-father.tar.gz \
  /opt/pxd_concise_son/pxd-concise-son.tar.gz \
  --repo y648394245-tech/puxida \
  --clobber
```
