# 普希达一键安装

没有 curl 时先复制执行（Debian / Ubuntu / CentOS / Rocky / Alma / Fedora / Alpine）：

```bash
command -v curl >/dev/null 2>&1 || { apt-get update && apt-get install -y curl; } || yum install -y curl || dnf install -y curl || apk add --no-cache curl
```

$\textcolor{#2563eb}{\textbf{完整版}}$：

```bash
curl -fsSL https://raw.githubusercontent.com/puxida/puxida/main/install-full.sh | bash
```

$\textcolor{#16a34a}{\textbf{精简版}}$：

```bash
curl -fsSL https://raw.githubusercontent.com/puxida/puxida/main/install-concise.sh | bash
```

$\textcolor{#2563eb}{\textbf{完整版}}$（全自动安装）：

```bash
curl -fsSL https://raw.githubusercontent.com/puxida/puxida/main/install-full.sh | \
  bash -s -- --port 端口 --username 账号 --password 密码
```

$\textcolor{#16a34a}{\textbf{精简版}}$（全自动安装）：

```bash
curl -fsSL https://raw.githubusercontent.com/puxida/puxida/main/install-concise.sh | \
  bash -s -- --port 端口 --username 账号 --password 密码
```

仓库只放安装脚本。完整版 / 精简版二进制包超过 GitHub 普通文件 100MB 限制，必须作为 **Release 附件** 上传。当前发行版：**v20260923**。

安装脚本会在 AlmaLinux / Alpine / CentOS / Debian / Fedora / Rocky Linux / Ubuntu 上自动结束占用软件包锁的进程、补装缺失依赖，并在没有 systemd 时直接拉起面板进程。不会主动开启未运行的防火墙，以免把 SSH 22 端口挡掉。

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

## v20260923 面板更新

- 修复「系统」菜单 `/hosts` 跳到无效路径导致 404
- 容器入口与概览页对齐；已安装节点「重新安装」不再显示「开始安装」
- 顶栏菜单窄屏折行、子菜单高亮、日志子页间距、RouterButton 白底

## v20260922 面板更新

- 顶栏菜单在窄屏下折行，各页面主内容区随导航高度让位
- 工具箱 / 日志 / 设置子菜单正确高亮
- 日志审计、登录日志、系统日志子页顶部间距单独调整
- 带 RouterButton 的导航条使用白底
- 受控端子面板顶距收紧；安装受控端走精简版一键安装（`/dev/tcp` 拉 bootstrap）
