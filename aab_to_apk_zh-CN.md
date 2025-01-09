# 使用 Bundletool 将 AAB 转换为 APK 的完整指南

Android App Bundle (AAB) 是 Google Play 商店使用的一种发布格式，它包含了应用的所有代码和资源，但将 APK 的生成推迟到用户下载时。这样可以根据用户设备的配置生成优化的 APK，减小下载大小。

## 方法一、使用在线网站工具（**推荐**）

优点：方便快捷、无需搭建环境、无需使用命令行

地址：[aab - apk 在线转换器](https://aabtoapk.online/zh-CN)


## 方法二、使用命令行转换工具：bundletool

bundletool 是一个官方命令行工具，用于：
- 构建 Android App Bundle 文件
- 将 AAB 转换为可安装的 APK
- 测试 AAB 在不同设备配置下的行为

### 基本用法

1. 下载 bundletool

确保系统已安装 Java 环境。

从 GitHub 代码库下载最新版本 bundletool：https://github.com/google/bundletool/releases

或使用 wget 下载特定版本 bundletool:
```
wget https://github.com/google/bundletool/releases/download/1.15.6/bundletool-all-1.15.6.jar
```

2. 准备签名文件
- 使用已有的签名文件(.keystore或.jks)
- 或使用 debug 签名文件(位于 ~/.android/debug.keystore)（调试签名的 APK 仅用于测试，不能上传到应用商店）

3. 转换步骤
生成 APK Set 文件:
```
使用自定义签名:
java -jar bundletool.jar build-apks \
  --bundle=/path/to/app.aab \
  --output=/path/to/output.apks \
  --ks=/path/to/keystore.jks \
  --ks-pass=pass:keystorePassword \
  --ks-key-alias=keyAlias \
  --key-pass=pass:keyPassword

使用系统debug签名:
java -jar bundletool.jar build-apks \
  --bundle=/path/to/app.aab \
  --output=/path/to/output.apks 
```

从 APK Set 提取设备相关的 APK:
```
针对已连接设备:
java -jar bundletool.jar install-apks --apks=/path/to/output.apks

提取通用 APK(支持所有设备)，可以用这种方式提取apk:
java -jar bundletool.jar build-apks \
  --bundle=/path/to/app.aab \
  --output=/path/to/output.apks \
  --mode=universal
```


## 重要参数说明

- `--bundle`: AAB 文件路径
- `--output`: 输出的 APK 集合路径
- `--ks`: 密钥库文件路径
- `--ks-pass`: 密钥库密码
- `--ks-key-alias`: 密钥别名
- `--key-pass`: 密钥密码
- `--mode=universal`: 生成通用 APK（包含所有代码和资源）

## 注意事项

1. **签名验证**
   - 确保使用与上传 Google Play 相同的密钥签名
   - 不要使用调试密钥发布应用

2. **文件大小**
   - 通用 APK 会比针对特定设备优化的 APK 更大
   - 建议仅在测试时使用通用 APK

3. **兼容性**
   - 确保测试不同 Android 版本的兼容性
   - 验证在各种设备配置下的行为

## 参考资料

- [Android 开发者文档 - bundletool](https://developer.android.com/tools/bundletool)
