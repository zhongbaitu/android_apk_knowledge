# 使用 Bundletool 將 AAB 轉換為 APK 的完整指南

Android App Bundle (AAB) 是 Google Play 商店使用的一種發布格式，它包含了應用程式的所有程式碼和資源，但將 APK 的生成推遲到使用者下載時。這樣可以根據使用者設備的配置生成最佳化的 APK，減小下載大小。

## 方法一、使用線上網站工具（**推薦**）

優點：方便快捷、無需搭建環境、無需使用命令列

網址：[aab - apk 線上轉換器](https://aabtoapk.online/zh-TW)


## 方法二、使用命令列轉換工具：bundletool

bundletool 是一個官方命令列工具，用於：
- 建構 Android App Bundle 檔案
- 將 AAB 轉換為可安裝的 APK
- 測試 AAB 在不同設備配置下的行為

### 基本用法

1. 下載 bundletool

確保系統已安裝 Java 環境。

從 GitHub 程式碼庫下載最新版本 bundletool：https://github.com/google/bundletool/releases

或使用 wget 下載特定版本 bundletool:
```
wget https://github.com/google/bundletool/releases/download/1.15.6/bundletool-all-1.15.6.jar
```

2. 準備簽名檔案
- 使用已有的簽名檔案(.keystore或.jks)
- 或使用 debug 簽名檔案(位於 ~/.android/debug.keystore)（調試簽名的 APK 僅用於測試，不能上傳到應用商店）

3. 轉換步驟
生成 APK Set 檔案:
```
使用自定義簽名:
java -jar bundletool.jar build-apks \\
  --bundle=/path/to/app.aab \\
  --output=/path/to/output.apks \\
  --ks=/path/to/keystore.jks \\
  --ks-pass=pass:keystorePassword \\
  --ks-key-alias=keyAlias \\
  --key-pass=pass:keyPassword

使用系統debug簽名:
java -jar bundletool.jar build-apks \\
  --bundle=/path/to/app.aab \\
  --output=/path/to/output.apks 
```

從 APK Set 提取設備相關的 APK:
```
針對已連接設備:
java -jar bundletool.jar install-apks --apks=/path/to/output.apks

提取通用 APK(支援所有設備)，可以用這種方式提取apk:
java -jar bundletool.jar build-apks \\
  --bundle=/path/to/app.aab \\
  --output=/path/to/output.apks \\
  --mode=universal
```


## 重要參數說明

- `--bundle`: AAB 檔案路徑
- `--output`: 輸出的 APK 集合路徑
- `--ks`: 金鑰庫檔案路徑
- `--ks-pass`: 金鑰庫密碼
- `--ks-key-alias`: 金鑰別名
- `--key-pass`: 金鑰密碼
- `--mode=universal`: 生成通用 APK（包含所有程式碼和資源）

## 注意事項

1. **簽名驗證**
   - 確保使用與上傳 Google Play 相同的金鑰簽名
   - 不要使用調試金鑰發布應用程式

2. **檔案大小**
   - 通用 APK 會比針對特定設備最佳化的 APK 更大
   - 建議僅在測試時使用通用 APK

3. **相容性**
   - 確保測試不同 Android 版本的相容性
   - 驗證在各種設備配置下的行為

## 參考資料

- [Android 開發者文件 - bundletool](https://developer.android.com/tools/bundletool)
