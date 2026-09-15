# 宝贝学习乐园 APK 打包说明

## 应用信息

- 应用名：宝贝学习乐园
- 包名：com.badi.learningpark
- 版本：1.0（versionCode 1）
- 特性：纯离线应用，无 INTERNET 权限，无云端同步、无登录；语音全部为内置 MP3

## 目录结构

| 位置 | 说明 |
|---|---|
| `C:\badi-pack\jdk-21.0.12.1+1\` | JDK 21（Temurin，便携版） |
| `C:\badi-pack\android-sdk\` | Android SDK（platform-35 + build-tools 35.0.0 + platform-tools） |
| `C:\badi-pack\app\` | Capacitor 安卓工程（webDir=www） |
| `C:\badi-pack\app\www\index.html` | 页面源（脱云改造版，源自本目录 offline_work\src\7.html） |
| `C:\badi-pack\app\www\audio\` | 114 个内置 MP3（26 字母 + 32 拼读 + 8 古诗 + 48 单词） |
| `C:\badi-pack\downloads\` | 安装包留底（jdk21.zip、cmdline-tools.zip） |

## 页面脱云改造记录（相对线上 7 岁版）

1. 删除 Database SDK 整块（DATABASE_ID / db.addRecord / pullStudyRecords 等）
2. 学习进度只存 localStorage（wb_baby_ 前缀）
3. 自然拼读卡片、听音选图、古诗卡片朗读 → 播放本地 audio/*.mp3（按 speech.json 索引映射）
4. 删除字母配对 Correct/Try again 朗读、古诗问答答对朗读、乘除算式朗读
5. AndroidManifest：锁定竖屏、删除 INTERNET 权限

## 重新打包步骤（页面改动后）

1. 把改好的页面覆盖到 `C:\badi-pack\app\www\index.html`（音频变动则同步 www\audio\）
2. 打开命令行执行：

```
cd /d C:\badi-pack\app
set JAVA_HOME=C:\badi-pack\jdk-21.0.12.1+1
npx cap copy android
cd android
gradlew.bat assembleDebug
```

3. 产物在 `C:\badi-pack\app\android\app\build\outputs\apk\debug\app-debug.apk`

## 环境要求

- 首次构建已下载 Gradle 发行版与依赖，之后构建很快
- 构建路径必须保持纯英文（C:\badi-pack），中文路径会导致部分构建工具崩溃
