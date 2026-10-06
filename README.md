# httpSMS 家庭收信部署 Skill

为 Codex 提供专用 Android 手机 + 家庭 Windows 电脑收信系统的部署、加固、排障和验收指南。使用自有真实 SIM，默认仅接收新短信。

## 安装与调用

在 Codex 中发送：

```text
使用 skill-installer，从 https://github.com/316878750/httpsms-home-skill/tree/main/skills/httpsms-home 安装 Skill。
```

安装后在下一轮对话调用：

```text
$httpsms-home 请检查我的 Windows 和 Android 环境，准备一个只在家庭网络使用的收信系统。
```

也可以将本仓库的 `skills/httpsms-home` 整个文件夹复制到自己的 Codex Skills 目录（默认 `~/.codex/skills/`）。已有同名 Skill 时先备份，不要直接覆盖个人修改。

## 包含什么

- `SKILL.md`：任务范围、流程与设计约束。
- `references/deployment.md`：工具、Firebase、局域网 HTTPS 与恢复机制。
- `references/troubleshooting.md`：实际部署中常见故障和处理方式。
- `references/acceptance.md`：签名升级、断网补传及真实重启验收。
- `agents/openai.yaml`：Codex 显示信息与默认调用提示。

## 使用边界

本仓库是工作流程指南，**不是完整修改版源码，也不是一键部署软件**。文档引用的本地 PowerShell 脚本属于加固修改版；若实际工作区没有这些文件，需先取得或实现对应功能，不能在未经修改的上游仓库直接执行示例。

原项目：[NdoleStudio/httpsms](https://github.com/NdoleStudio/httpsms)。本仓库未复制其应用源码，未分发 APK，也不是上游官方项目。

需要自己的 SIM、Firebase 项目、Android 设备和家庭电脑。仍依赖 Firebase 等外部服务；默认短信数据库为明文，未实现自动过期清理。工具版本与网络参数需要按实际源码和设备重新核实。

不要将真实号码、短信正文、服务账号私钥、API 密钥或签名私钥提交到公开仓库。此 Skill 不携带作者的私人配置。

## 授权

本仓库原创 Skill 文档使用 MIT 许可证。上游应用源码及其他第三方材料适用各自许可证；本许可证不替代上游许可。
