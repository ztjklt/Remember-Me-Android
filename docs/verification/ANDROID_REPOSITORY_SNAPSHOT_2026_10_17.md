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

待本轮构建和来源完整性校验完成后补充实际结果。本轮不部署后端、不安装实体设备、不复测线上通话。既有语音质量、时延和设备验收的通过及失败边界见 [2026-10-17 语音核验记录](voice-reviewed-latency-20261017.md)。
