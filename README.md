# 普希达一键安装

仓库只放安装脚本。完整版 / 精简版二进制包超过 GitHub 普通文件 100MB 限制，必须作为 **Release 附件** 上传，不要 `git add *.tar.gz`。

当前发行版：**v20260922**（由最新源码重新编译面板与安装包）。

安装脚本会在 AlmaLinux / Alpine / CentOS / Debian / Fedora / Rocky Linux / Ubuntu 上自动结束占用软件包锁的进程、补装缺失依赖，并在没有 systemd 时直接拉起面板进程。不会主动开启未运行的防火墙，以免把 SSH 22 端口挡掉。

## 其它服务器一键安装（x86_64 Linux，root）

完整版（主控，默认端口 **41275**）：

```bash
curl -fsSL https://raw.githubusercontent.com/y648394245-tech/puxida/main/install-full.sh | bash
```

精简版（子节点，默认端口 **20999**）：

```bash
curl -fsSL https://raw.githubusercontent.com/y648394245-tech/puxida/main/install-concise.sh | bash
```

Alpine 若还没有 bash / curl：

```bash
apk add --no-cache bash curl
curl -fsSL https://raw.githubusercontent.com/y648394245-tech/puxida/main/install-concise.sh | bash
```

指定端口和账号（不要用默认弱口令）：

完整版：

```bash
curl -fsSL https://raw.githubusercontent.com/y648394245-tech/puxida/main/install-full.sh | \
  bash -s -- --port 端口 --username 账号 --password 密码
```

精简版：

```bash
curl -fsSL https://raw.githubusercontent.com/y648394245-tech/puxida/main/install-concise.sh | \
  bash -s -- --port 端口 --username 账号 --password 密码
```

环境变量（可选）：

| 变量 | 说明 |
| --- | --- |
| `PANEL_PORT` | 面板端口 |
| `PANEL_USERNAME` / `PANEL_PASSWORD` | 登录账号密码 |
| `PANEL_INSTALL_DOCKER` | `y`/`n`，默认安装 Docker |
| `PXD_RELEASE_TAG` | 指定 Release 标签，默认 `latest` |
| `PXD_PACKAGE_URL` | 自定义安装包地址 |

脚本会从本仓库最新 Release 下载：

- `pxd-full-father.tar.gz`
- `pxd-concise-son.tar.gz`

本机 80 端口分发（与面板「安装受控端」同一套包）：

```bash
curl -fsSL http://204.194.52.45/install-full-father | bash
curl -fsSL http://204.194.52.45/install-concise-son | bash
```

## v20260922 面板更新

- 顶栏菜单在窄屏下折行，各页面主内容区随导航高度让位
- 工具箱 / 日志 / 设置子菜单正确高亮
- 日志审计、登录日志、系统日志子页顶部间距单独调整
- 带 RouterButton 的导航条使用白底
- 受控端子面板顶距收紧；安装受控端走精简版一键安装（`/dev/tcp` 拉 bootstrap）

## 发布新版本（本机构建机上）

1. 只推送脚本到 `main`（本目录这几个文件）。
2. 创建或覆盖 Release 附件，把 `/opt` 里两个 tar 当附件上传。

```bash
cd /root/copy_code/puxida-github
git add README.md LICENSE .gitignore install-full.sh install-concise.sh
git commit -m "Release v20260922 one-click packages"
git push origin main

gh release create v20260922 \
  /opt/pxd_full_father/pxd-full-father.tar.gz \
  /opt/pxd_concise_son/pxd-concise-son.tar.gz \
  --repo y648394245-tech/puxida \
  --title "v20260922" \
  --notes "最新源码一键安装包（完整版 + 精简版）"
```
