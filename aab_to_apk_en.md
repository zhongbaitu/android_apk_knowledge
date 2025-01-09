# Complete Guide to Converting AAB to APK Using Bundletool

Android App Bundle (AAB) is a publishing format used by the Google Play Store that contains all the code and resources of an application but defers APK generation until users download it. This allows the generation of optimized APKs based on user device configurations, reducing download sizes.

## Method 1: Using Online Web Tool (**Recommended**)

Advantages: Convenient, no environment setup required, no command line needed

Website: [AAB to APK Online Converter](https://aabtoapk.online/)


## Method 2: Using Command Line Tool: bundletool

bundletool is an official command-line tool used for:
- Building Android App Bundle files
- Converting AAB to installable APKs
- Testing AAB behavior across different device configurations

### Basic Usage

1. Download bundletool

Ensure Java environment is installed on your system.

Download the latest version of bundletool from GitHub repository: https://github.com/google/bundletool/releases

Or use wget to download a specific version of bundletool:
```
wget https://github.com/google/bundletool/releases/download/1.15.6/bundletool-all-1.15.6.jar
```

2. Prepare Signing File
- Use existing signing file (.keystore or .jks)
- Or use debug signing file (located at ~/.android/debug.keystore) (APKs signed with debug key are for testing only and cannot be uploaded to app stores)

3. Conversion Steps
Generate APK Set file:
```
Using custom signature:
java -jar bundletool.jar build-apks \\
  --bundle=/path/to/app.aab \\
  --output=/path/to/output.apks \\
  --ks=/path/to/keystore.jks \\
  --ks-pass=pass:keystorePassword \\
  --ks-key-alias=keyAlias \\
  --key-pass=pass:keyPassword

Using system debug signature:
java -jar bundletool.jar build-apks \\
  --bundle=/path/to/app.aab \\
  --output=/path/to/output.apks 
```

Extract device-specific APKs from APK Set:
```
For connected devices:
java -jar bundletool.jar install-apks --apks=/path/to/output.apks

Extract universal APK (supports all devices), use this method to extract APK:
java -jar bundletool.jar build-apks \\
  --bundle=/path/to/app.aab \\
  --output=/path/to/output.apks \\
  --mode=universal
```


## Important Parameter Description

- `--bundle`: AAB file path
- `--output`: Output APK set path
- `--ks`: Keystore file path
- `--ks-pass`: Keystore password
- `--ks-key-alias`: Key alias
- `--key-pass`: Key password
- `--mode=universal`: Generate universal APK (includes all code and resources)

## Important Notes

1. **Signature Verification**
   - Ensure using the same key signature as Google Play upload
   - Do not use debug key for publishing applications

2. **File Size**
   - Universal APKs will be larger than device-specific optimized APKs
   - Recommended to use universal APKs for testing only

3. **Compatibility**
   - Ensure testing compatibility across different Android versions
   - Verify behavior across various device configurations

## References

- [Android Developer Documentation - bundletool](https://developer.android.com/tools/bundletool)
