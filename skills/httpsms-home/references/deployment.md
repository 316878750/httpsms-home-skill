# 部署流程

## 先定位修改版

确认仓库包含本地加固实现，核对以下文件；这些不是上游克隆即有的功能。

| 文件或模块 | 用途 |
|---|---|
| `compose.local.yml`、本地 Caddy 配置 | 网络绑定、HTTPS、数据库与服务 |
| `deploy/local/Prepare-Local.ps1`、`Start-Local.ps1` | 生成私有配置、初始化与启动 |
| Android 接收器、上传策略 | 持久任务、稳定 ID、合法响应与重试 |
| Go 消息服务、PostgreSQL 队列 | 去重、事务和事件补传 |
| `Build-Android.ps1`、`Build-Release.ps1` | Android 构建与正式签名 |
| `Enable-LoginRecovery.ps1`、`Resume-Local.ps1` | 登录后恢复与证据记录 |

实际路径以工作区为准，不能猜测脚本参数。读取脚本后再调用。

## 条件与工具

使用带真实 SIM、可正常收短信的专用 Android 手机和支持 Docker Desktop 的 Windows 电脑。开启 USB 调试并确认 adb 中状态为 device；授权弹窗需要用户在手机确认。

若用户指定 D 盘，分别配置 Docker 程序、Docker 数据盘、Android SDK、JDK、Gradle 缓存及构建临时目录。Windows 组件和少量系统配置仍可能位于系统盘；不要承诺 C 盘零占用。不得删除现有 WSL 数据或擅自改分页文件。

读取 Gradle、SDK 和 daemon JVM 配置核实版本。本次基线使用 SDK 37、Gradle 9.7、JetBrains JVM 25，不能仅安装 JDK 21 就认定环境完备。内存不足时降低并发、使用 Kotlin 进程内编译，并只终止当前项目的构建进程。

## Firebase

建立自己的项目；注册 Web 应用和与源码包名匹配的 Android 应用。启用邮件/密码登录；为本机网页添加 localhost 授权域。核对 FCM HTTP v1 已启用。

三类配置分别放置：Web 客户端配置、Android `google-services.json`、后端服务账号。禁止把后端私钥写入 APK 或网页。Analytics 不属于此收信流程的必要组件。删除 Analytics 依赖后，检查仍需要的 Google Play 基础依赖。

## 网络与服务

确认电脑家庭网卡实际为 Up，手机与电脑能在同一家庭 Wi-Fi 通信；USB 连接本身不代表局域网可达。建议路由器固定电脑地址。以下仅是占位示例，替换为实际家庭地址并核实脚本参数：

```powershell
.\deploy\local\Prepare-Local.ps1 -LanIP 192.168.1.50 -LanCidr 192.168.1.0/24
.\deploy\local\Start-Local.ps1
```

已有私有配置应复用并备份，不能覆盖生成新密钥。检查 `.local` 被 Git 忽略且 ACL 限定必要用户。填入自有 Firebase 配置，不在终端打印内容。

设计为本机网页 `https://localhost:8443`；手机入口示例 `https://192.168.1.50:8444`，仅提供注册、收信、心跳接口。数据库、Redis、内部事件与健康接口不得公开。

导出公开根证书并核对来源；Windows 当前用户信任库可使用：

```powershell
certutil -user -addstore Root .local/home-ca.crt
```

Android 用户 CA 的信任配置必须限定家庭地址。地址变化需要同步证书、应用配置及服务绑定。不能传输 CA 私钥。

Docker 源地址改写导致 403 时，先配置宿主机防火墙限制家庭子网，再检查代理观察到的真实来源。子网规则脚本若仅支持 /24，应明确限制或正确扩展，不能把不支持的网络强行当 /24。

## 手机与自动恢复

以私有方式配置手机用户账号、用户 API 密钥、服务器地址和 SIM 号码。只申请必要接收权限，按系统配置后台运行和电池策略。不要打印 provisioning 内容。

使用当前用户登录计划任务启动恢复脚本，等待家庭网卡及 Docker 就绪后启动容器。记录任务退出码、真实 OS 启动时间、TLS 健康检查及数据计数。注册表自启动条目存在或手工运行脚本成功不能替代真实重启验收。
