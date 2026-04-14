# nettop 🌐

> Real-time Network Traffic Monitor

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Shell](https://img.shields.io/badge/Shell-Bash-green.svg)](https://www.gnu.org/software/bash/)
[![Platform](https://img.shields.io/badge/Platform-macOS|Linux-blue.svg)](https://www.apple.com/macos)

`nettop` 是一个轻量级、类 htop 风格的实时网络流量监控工具。无需 root 权限，支持 macOS 和 Linux，提供实时 TUI 界面、连接状态监控和 Top-N 排行。

## ✨ 特性

- 📊 **实时 TUI 监控** — 类 htop 界面，带彩色输出和动态刷新
- 🌐 **双平台支持** — 完美支持 macOS 和 Linux（procfs）
- 🔗 **连接状态** — 实时显示 ESTABLISHED / LISTEN / UDP 连接数
- 📈 **Top-N 排行** — 按 RX/TX 带宽对接口排序
- 🔍 **连接详情** — 显示活动连接的 IP:Port，支持 IPv4/IPv6
- 🎨 **彩色终端** — 智能变色（绿色→黄色→红色按流量分级）
- ⚡ **零依赖** — 纯 Bash 脚本，仅需系统自带工具（awk, bc, sort）
- 🔧 **高度可配置** — 支持多种环境变量定制刷新间隔、排序、颜色等

## 🏃 快速开始

### 安装

```bash
# 克隆项目
git clone https://github.com/chensu1234/nettop.git
cd nettop

# 添加执行权限
chmod +x bin/nettop.sh
```

### 使用

```bash
# 实时 TUI 监控（默认）
./bin/nettop.sh watch

# Top 20 接口（按 RX 排序）
./bin/nettop.sh top 20

# 查看所有网络接口
./bin/nettop.sh interfaces

# 查看活动连接
./bin/nettop.sh connections 30

# 更快刷新 + 无颜色
NETTOP_REFRESH=1 NETTOP_COLOR=0 ./bin/nettop.sh watch

# 使用二进制单位（KiB/MiB）
NETTOP_BINARY=1 ./bin/nettop.sh watch
```

## ⚙️ 配置

### 环境变量

| 变量 | 说明 | 默认值 |
|------|------|--------|
| `NETTOP_REFRESH` | 刷新间隔（秒） | `2` |
| `NETTOP_MAX_ROWS` | Top/connections 最大行数 | `10` |
| `NETTOP_SORT` | 排序方式: `rx` \| `tx` | `rx` |
| `NETTOP_COLOR` | 启用颜色: `0` \| `1` | `1` |
| `NETTOP_BINARY` | 使用二进制单位（1024）: `0` \| `1` | `0` |

### watch 模式键盘快捷键

| 键 | 说明 |
|----|------|
| `q` / `Q` | 退出 |
| `r` / `R` | 切换排序（RX ↔ TX）|

## 📋 命令行选项

### `watch` — 实时 TUI 监控

```bash
./bin/nettop.sh watch
```

实时显示总带宽、连接数统计和按接口带宽排序表格。

### `top [N]` — 接口 Top-N 排行

```bash
./bin/nettop.sh top 10        # Top 10 默认
./bin/nettop.sh top 20        # Top 20
NETTOP_SORT=tx ./bin/nettop.sh top  # 按 TX 排序
```

### `interfaces` — 网络接口列表

```bash
./bin/nettop.sh interfaces
```

显示所有网络接口的状态、MTU 和 MAC 地址。

### `connections [N]` — 活动连接

```bash
./bin/nettop.sh connections          # 默认 10 条
./bin/nettop.sh connections 30       # 显示 30 条
```

显示活跃 TCP/UDP 连接的详细信息。

## 📁 项目结构

```
nettop/
├── bin/
│   └── nettop.sh              # 主脚本（入口）
├── config/
│   └── .gitkeep               # 配置目录（预留）
├── log/
│   └── .gitkeep               # 日志目录（预留）
├── README.md
├── CHANGELOG.md
└── LICENSE
```

## 📝 工作原理

### Linux

- 接口统计：`/proc/net/dev`（rx/tx bytes）
- 连接信息：`/proc/net/tcp` / `proc/net/udp`（hex IP 解析）
- 字节序：procfs 中的 IP 为 little-endian，脚本自动转换

### macOS

- 接口统计：`netstat -I <iface> -b -n`
- 连接信息：`netstat -a -n -p TCP/UDP`
- IPv6 支持：自动检测和格式化

### 增量计算

每次轮询后，脚本将当前统计保存到 `~/.state/nettop.prev`。下次轮询时计算差值，得到真实的 RX/TX 速率。

## 📝 CHANGELOG

### [1.0.0] - 2026-04-14

**初始版本**

- 实时 TUI 监控模式（watch）
- Top-N 接口排行（top）
- 网络接口列表（interfaces）
- 活动连接查看（connections）
- 双平台支持（macOS + Linux）
- IPv4 / IPv6 双栈连接解析
- 零外部依赖，纯 Bash 实现
- 智能彩色终端输出

## 📄 许可证

MIT License - 详见 [LICENSE](LICENSE) 文件。
