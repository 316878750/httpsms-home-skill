# 故障诊断

| 表现 | 检查与处理 |
|---|---|
| 手机 EHOSTUNREACH、连接超时 | 检查电脑家庭网卡为 Up；旧 IP 可留在已断开网卡。USB 网络共享不能代替家庭 Wi-Fi。 |
| TLS 错误 | 检查公开根证书、地址与证书 SAN、电脑和手机信任范围。不能跳过验证。 |
| Windows curl 报吊销检查错误 | 自建 CA 可能无 CRL；可用 `--ssl-revoke-best-effort` 做诊断，保留链与主机名验证，不全局关闭安全设置。 |
| 403 | 检查路径、方法及代理实际源地址；Docker NAT 问题先以宿主机防火墙约束，再调整代理。 |
| 401 | 未认证请求应如此；检查手机使用自己的用户密钥及正确账号。 |
| Firebase 或 FCM 失败 | 检查 Google 网络、自有项目一致性、包名和配置；不靠恢复 Analytics 修复。 |
| 网页不更新 | 检查登录账号、API 返回和轮询；避免输出短信正文。 |
| Android native memory / daemon OOM | 降低 worker/JVM 并发，Kotlin 进程内编译；只处理此工作区构建进程。 |
| APK DUPLICATE_PERMISSION | 检查旧新签名与 rotation lineage 的签名权限继承；不直接卸载丢数据。 |
| 重启后未启动 | 查询计划任务触发、LastTaskResult、恢复记录和 Docker 就绪；手动启动之后不能再声称本次自动恢复成功。 |
| PowerShell 把 Docker 进度当错误 | 在原生命令局部调整错误处理并检查 LASTEXITCODE；不要全局忽略真实失败。 |
| 手机重启后收不到 | 先首次解锁，再检查接收器、后台权限和持久任务；验收时不得手动打开应用掩盖问题。 |

排障后移除临时诊断响应头、测试 APK 和含凭据的临时 provisioning 文件。保留正式签名密钥、数据库卷与必要配置。修改地址或网络规则后重新验证链路；修复自动恢复后需要重新做真实重启测试。
