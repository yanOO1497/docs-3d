# 发布到 Mac 平台

## 环境配置

发布原生平台需要配置一些必要的环境，详情请参考 [原生环境配置](setup-native-development.md)。

## 构建选项

一些通用构建参数的配置，请参照 [通用构建参数介绍](build-options.md)。

![native platform](publish-native/native_platform.jpg)

部分原生平台通用的选项配置，请参照 [原生平台构建参数介绍](native-options.md)。

![Mac 平台选项](publish-native/mac_options.png)

### Bundle Identifier 包名

（也称作 Package Name 或 Bundle Identifier），通常以产品网站 url 倒序排列，如 `com.mycompany.myproduct`。

**注意**：包名中只能包含数字、字母和下划线，此外包名最后一部分必须以字母开头，不能以下划线或数字开头。

### 渲染后端

目前支持 metal、gles3、gles2 三个选项，默认选择 metal，其中 metal 和 gles 相关选项是互斥的。gles2、gles3 是可以多选的。

## 编译/运行工程

### 使用构建面板上的编译、运行按钮

支持编译的平台构建，默认情况下会全部执行完毕。如果有需要单独执行编译任务的。可以在构建完成后，单独点击编译按钮即可。编译运行输出的 log 可以打开构建调试工具查看。

点击运行就能直接打开执行编译好的 win32 工程。

### 使用 Xcode 编译运行

使用 Xcode 打开构建目录下的 `mac\proj` 文件夹后即可编译运行。

要了解如何在原生平台上调试，请参考 [原生平台 JavaScript 调试](debug-jsb.md)。
