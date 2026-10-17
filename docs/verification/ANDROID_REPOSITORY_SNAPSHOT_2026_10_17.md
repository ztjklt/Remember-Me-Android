# Android 独立仓库快照 — 2026-10-17

新库：`ztjklt/Remember-Me-Android`，公开（按本人的明确选择）。Android 版本 `0.16.4-device-internal`，versionCode `20261034`。

来源为 `ztjklt/Remember-Me` 的 `codex/work3200-device-20261016`，完整提交 `7e72c7779207dae28e1333368d332acdc91d3fcc`。已从远端拉取后锁定该提交，未使用当前 iOS 工作区的旧 Android 目录，也未拷入工作区未提交文件。新库独立起始历史，原提交与 PR 继续在原库追溯。

## 范围

保留原目录布局和该提交的全部受版本控制文件，包括 Android、配套 Backend / AI Core / Voice、Contract、工具和历史交接文档。历史 iOS 文件只作参考；本轮不移植、整合或发布 iOS。两个 README 添加新库入口，运行代码和 Contract 内容不变。来源文件及迁移前 SHA256 见 [来源清单](ANDROID_REPOSITORY_SOURCE_2026_10_17.json)。

不复制本机 `.env`、签名私钥、未提交内容、构建产物或厂商私有 SDK。普通构建走现有无 SDK 适配器；实物录音豆功能需要私下取得厂商 AAR。没有将 Windows 机器上的已签名 APK 冒充本次构建或上传。

## 构建与协作

JDK 17、Android SDK 35，Android Studio 打开 `apps/android`。命令：

```bash
cd apps/android
./gradlew testDebugUnitTest assembleDebug -PrememberServiceUrl=http://10.0.2.2:8891
```

模拟器访问本机服务 `10.0.2.2:8891`；账号与后端初始化见[本地启动说明](../agent-loop/TWIN_LOCAL_SETUP_2026-10-12.md)。录音豆构建在上述命令中补充 `-PrememberSoundcoreSdk=<私有 AAR 绝对路径> -PrememberRequireHardwareSdk=true`。供应商密钥仅配置在服务端。

新库建立同起点 `main` 与 `develop`。邀请原库三位 Write 协作者：刘修贤 `shuziyuxingxing-stack`、康欣 `centraler`、王昊宇 `qingtian-4`，均沿用 Write 权限；邀请接受状态以 GitHub 为准。协作规范和原库 Issue 的历史背景仍保留，旧文档中要求继续旧分支的描述只适用于当时交接，不是新库的开发入口。

## 本轮验证

本轮在新库目录实际执行：

- 来源完整性：1771 个来源文件全部存在；除两个 README 的入口说明外，1769 个文件的 Git blob 与模式均和来源提交完全一致，运行代码及 Contract 没有额外变化。
- 导入前检查：无本机环境文件、签名私钥、厂商私有 SDK 或匹配到的真实令牌格式；只导入来源提交的文件与两份新库溯源文档。
- `cd packages/contracts && npm ci --ignore-scripts && npm test`：**15 项通过，0 失败，0 跳过**。
- `cd apps/android && ./gradlew --no-daemon testDebugUnitTest assembleDebug -PrememberServiceUrl=http://10.0.2.2:8891`：JDK 17 / Android SDK 35，**BUILD SUCCESSFUL**，耗时 3m04s；**153 项单元测试通过，0 失败、0 错误、0 跳过**。
- 构建未传厂商 AAR，使用现有 noSoundcore 降级入口。Debug APK 为本机开发服务包，大小 175029747 bytes，SHA256 `dc9cdb17e41995acb547e956d706966daa36dadd6993fcc0c7e5b00d90539c0a`，保留在 `apps/android/app/build/outputs/apk/debug/app-debug.apk`，不进入 Git，也不冒充云端签名版。
- 构建提示 Kotlin / 图标 API 弃用、SDK XML 版本及既有 Manifest 合并警告；未因这些警告修改运行源码。新库新增说明文件的空白检查通过；整仓初次导入的 `gradlew.bat` 保留来源 CRLF，通用空白检查会将其显示为既有尾部空白。
- 三位队友的 Write 邀请均已由 GitHub 返回邀请 ID，且在新库待接受邀请列表中读回确认。

本轮不部署后端、不安装实体设备、不复测线上通话；不将无 SDK 构建扩展为硬件验收。既有语音质量、时延和设备验收的通过及失败边界见 [2026-10-17 语音核验记录](voice-reviewed-latency-20261017.md)。
