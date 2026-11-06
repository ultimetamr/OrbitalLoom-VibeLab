# Orbital Loom 星轨织机 — PICO Vibe Lab

在独立 MR 空间中观察遗迹、水晶与星光，利用转向、分流、符文门、转换与联动机关，解开四个星域的 24 道谜题。包含逐步教学、错误复盘与节拍演出。

本仓库是用于 Vibe Lab GitHub + Commit 提交的独立 Android Gradle 导出工程。
完整 Unity C# 源工程另存于 https://github.com/ultimetamr/OrbitalLoom 。

## 构建信息

- 应用版本：0.9.9，versionCode 27
- 图标：应用、圆形图标和启动入口均指定原星轨 PNG；PICO 真机已确认应用列表显示星轨图标。
- 应用包名：`com.orbitalloom.puzzle`
- Unity：6000.6.8f1，IL2CPP，ARM64，Release
- Android：minSdk29，target/compileSdk36，NDK27.2.12479018(r27c)
- Unity 模板版本：26；Gradle9.3.1；AGP9.1.1；Java17；CMake3.22.1
- 唯一 application 模块：launcher；其余是 library 模块

## Vibe Lab 提交

使用本仓库 URL 与完整 40 位 Commit ID。工程根目录包含 settings.gradle、build.gradle、launcher、unityLibrary、shared、gradle。
保留 Unity 官方模板的工具路径和 release 使用 debug signingConfig 的默认行为；未提交自定义密钥、密码、local.properties 或 APK。平台负责最终签名。
此 Unity 导出模板不含 gradlew/gradlew.bat/gradle-wrapper.jar；未额外拼装其他版本的 wrapper。

ARM64 IL2CPP 已在 Windows 使用 Unity 自带 NDK 实际编译。`jniLibs/arm64-v8a/libil2cpp.so` 与 global-metadata.dat 的 SHA256 见 `.vibelab-prebuilt.json`。
保留原 buildIl2Cpp 任务体并禁用执行，`jni.srcDirs=[]`；中间 Il2CppOutputProject 已移除。禁止使用 skipIl2CppBuild 生成假库。

本地构建命令为 `:launcher:assembleRelease --no-daemon`，通过 Unity 配套 Gradle 启动器执行。SDK/JDK/NDK 应使用配套版本。Windows Java17 若遇到 UnixDomainSockets Invalid argument，可仅对本地进程设置 `-Djdk.net.unixdomain.tmpdir=C:/OrbitalLoomVibeLab/JavaTemp`。隐藏 staging 目录通过外部 init 脚本指定工程名，未修改交付工程模板。

## 验证范围

- Unity6 编译成功；144 项 EditMode 测试通过，0 失败，0 跳过
- 24 关参考解法、场景引用与缺失脚本检查通过
- Editor 完整 UI 操作回归通过，24 关实际回调通关
- 10 段教学仅按当前高亮操作可通关；DSP 节拍暂停、继续、停止验证通过
- Release APK 构建成功；全部原生库为 ARM64 ELF64
- APK 本地签名验证、zipalign 检查通过
- 构建号 27 已覆盖安装到 PICO 真机；安装包 SHA256：`4a1acd3fca06dd17dce6122335e533381b9f834b5d15a994ee4451a2896b0bab`
- 移除中间代码及缓存后再次构建成功，buildIl2Cpp SKIPPED，所有 APK 原生库名称/ABI/哈希与移除前一致

上述结果不等同于 Unity6 真机验收或 Vibe Lab 平台审核通过。此前“安装包签名失败”的完整平台日志尚未提供，不能据此断定根因。
部分第三方/PICO 预编译原生库未满足16KB ELF LOAD对齐，详细清单见 Validation/native-final.json；当前策略为报告警告，不宣称完整16KB兼容。

## 权利说明

游戏源码与项目资源来源说明请见源工程仓库。Unity、PICO SDK 与其余依赖保留各自许可；公开本构建工程不改变第三方许可。
