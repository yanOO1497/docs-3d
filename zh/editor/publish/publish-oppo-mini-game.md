# 发布到 OPPO 小游戏

## 环境配置

- 下载 [OPPO 小游戏调试器](https://cdofs.oppomobile.com/cdo-activity/static/201810/26/quickgame/documentation/#/games/use)，并安装到 Android 设备上（建议 Android Phone 6.0 或以上版本）

- 全局安装 [nodejs-8.1.4](https://nodejs.org/zh-cn/download/) 或以上版本

- 根据用户自己的开发需求判断是否需要安装 [调试工具](https://cdofs.oppomobile.com/cdo-activity/static/201810/26/quickgame/documentation/games/use.html)。

## 发布流程

一、使用 Cocos Creator 打开需要发布的项目工程，在 **构建发布** 面板的 **发布平台** 中选择 **OPPO Mini Game**。

![](./oppo-mini-game/build_options.jpg)

**必填参数项**：根据用户的需求及参数输入框的提示信息进行填写，包括：**游戏包名**、**游戏名称**、**桌面图标**、**游戏版本名称**、**游戏版本号**、**支持的最小平台版本号**。

**密钥库** 以及两个签名文件（**certificate.pem 路径** 和 **private.pem 路径**），需要根据用户需求选择勾选 **密钥库** 或者填写两个路径。

**OPPO 小游戏支持分包，可以在主包配置类型里选择分包类型使用。**

关于通用构建参数的详细说明可以参考文档 [构建面板内的通用参数](./build-options)

平台相关参数配置具体的填写规则如下：

- **初始场景分包**

  该项为可选项。<br>
  勾选后，首场景及其相关的依赖资源会被构建到发布包目录 assets 下的内置 Asset Bundle — [start-scene](../asset-manager/bundle.md#%E5%86%85%E7%BD%AE-asset-bundle) 中，提高初始场景的资源加载速度。具体内容可参考文档 [初始场景的资源加载](publish-wechatgame.md#微信小游戏的资源管理)。

- **资源服务器地址**

  该项为选填项，用于填写资源存放在服务器上的地址。

  - 若 **不填写** 该项，则发布包目录下的 `build/oppo-mini-game/remote` 文件夹将会被打包到构建出来的 rpk 包中。

  - 若 **填写** 该项，则 remote 文件夹不会被打包到 rpk 包中。开发者需要在构建后手动将 remote 文件夹上传到所填写的资源服务器地址上。

  具体的资源管理细节，请参考文档下方的[资源管理](#OPPO-小游戏环境的资源管理)部分。

- **游戏包名**

  该项为必填项，根据用户的需求进行填写。

- **桌面图标**

  **桌面图标** 为必填项。点击输入框后面的 **...** 按钮选择所需的图标。构建时，图标将会被构建到 OPPO 小游戏的工程中。桌面图标建议使用 **png** 图片。

- **游戏版本名称**

  该项为必填项，根据用户的需求进行填写。

- **游戏版本号**

  该项为必填项，根据用户的需求进行填写。

- **支持的最小平台版本号**

  该项为必填项。根据 OPPO 的要求目前这个值必须大于或等于 **1031**。

- **密钥库**

  勾选 **密钥库** 时，表示默认用的是 Creator 自带的证书构建 rpk 包，仅用于 **调试** 时使用。
  > **注意**：若 rpk 包要用于提交审核，则构建时不要勾选该项。

  如果不勾选 **密钥库**，则需要配置签名文件 **certificate.pem 路径** 和 **private.pem 路径**，此时构建出的是可以 **直接发布** 的 rpk 包。用户可通过输入框右边的 **...** 按钮来配置两个签名文件。<br>
  用户可以通过以下两种方式生成签名文件，如下：
  - 通过 **构建发布** 面板 **certificate.pem 路径** 后的 **新建** 按钮生成。

  - 通过命令行生成 release 签名。

      用户需要通过 openssl 命令等工具生成签名文件 private.pem、certificate.pem。

      ```bash
      # 通过 openssl 命令工具生成签名文件
      openssl req -newkey rsa:2048 -nodes -keyout private.pem -x509 -days 3650 -out certificate.pem
      ```

      > **注意**：openssl 工具在 linux 或 Mac 环境下可在终端直接打开。而在 Windows 环境下则需要安装 openssl 工具并且配置系统环境变量，配置完成后需重启 Creator。

二、**构建发布** 面板的相关参数设置完成后，点击 **构建**。构建完成后点击对应构建任务上的 **在文件夹中显示** 打开构建发布包位置，可以看到在默认发布路径 build 目录下生成了与构建任务名称相同的目录，该目录就是导出的 OPPO 小游戏工程目录和 rpk，rpk 包在对应目录的 dist 文件夹下。

![](./oppo-mini-game/package.jpg)

三、将构建出来的 rpk 运行到手机上。

将构建生成的小游戏 rpk 包（ dist 目录中）拷贝到手机 SD 卡的 **/sdcard/games/** 目录。然后在 Android 设备上打开之前已经安装完成的 **OPPO 小游戏调试器**，点击 **OPPO 小游戏** 栏目，然后找到填写游戏名相对应的图标即可，如没有发现，可点击右上角的更多按钮-刷新按钮进行刷新。

  **注意：OPPO 小游戏调试器为 V3.2.0 及以上的需要将准备好的 rpk 拷贝到手机 sdcard 的 Android/data/com.nearme.instant.platform/files/games 中, 无 games 目录则需新建**

![](./oppo-mini-game/rpk_games.jpg)

四、分包 rpk

分包加载，即把游戏内容按一定规则拆分成几个包，在首次启动的时候只下载必要的包，这个必要的包称为 **主包**，开发者可以在主包内触发下载其他子包，这样可以有效降低首次启动的消耗时间。开发者可以将需要分包的内容划分成多个 Asset Bundle，这些 Asset Bundle 会被构建成小游戏的分包。在启动游戏时只会下载必要的主包，不会加载这些分包，而是由开发者在游戏过程中手动加载，从而有效降低游戏启动的时间。设置完成后构建时就会自动根据 Asset Bundle 的配置也会按照规则自动生成到对应小游戏发布包。

构建完成后，分包 rpk 在 dist 目录下。<br>
这时需要在 Android 设备的 **sdcard** 目录下，新建一个 **subPkg** 目录，然后把 dist 目录下的 **.rpk** 文件拷贝到 subPkg 目录中。<br>
然后切换到 **OPPO 小游戏调试器** 的 **分包加载** 栏目，点击右上方的刷新即可看到分包的游戏名称，点击 **秒开** 即可跟正常打包的 rpk 一样使用。

![](./oppo-mini-game/run_subpackage.jpg)

**注意**：

1. 分包 rpk 需要拷贝到 Android 设备的 **/sdcard/subPkg/** 目录，未分包的 rpk 需要拷贝到 Android 设备的 **/sdcard/games/** 目录，两者不可混用。

> 注意：OPPO 小游戏调试器为 V3.2.0 及以上的需要将准备好的 rpk 拷贝到手机 sdcard 的 Android/data/com.nearme.instant.platform/files/subPkg 中, 无 subPkg 目录则需新建

2. **快应用 & vivo 小游戏调试器** 从 **1051** 版本开始支持 vivo 小游戏分包加载。低于 1051 的版本虽然不支持分包加载，但是也做了兼容处理，如果使用了分包也不会影响游戏正常运行。具体可参考 [vivo 分包加载-运行时兼容](https://minigame.vivo.com.cn/documents/#/lesson/base/subpackage?id=%e8%bf%90%e8%a1%8c%e6%97%b6%e5%85%bc%e5%ae%b9)。

## OPPO 小游戏环境的资源管理

OPPO 小游戏与微信小游戏类似，都存在着包体限制。不过 OPPO 的主包包体限制是 **10MB**，超过的部分必须通过网络请求下载。

Cocos Creator 已经帮开发者做好了远程资源的下载、缓存和版本管理。具体的实现逻辑和操作步骤都与微信小游戏类似，请参考 [微信小游戏资源的管理](./publish-wechatgame.md#资源管理)。

## 相关参考链接

- [OPPO 小游戏调试说明](https://cdofs.oppomobile.com/cdo-activity/static/201810/26/quickgame/documentation/games/debug.html)
- [OPPO 小游戏教程](https://cdofs.oppomobile.com/cdo-activity/static/201810/26/quickgame/documentation/games/quickgame.html)
- [OPPO 小游戏 API 文档](https://cdofs.oppomobile.com/cdo-activity/static/201810/26/quickgame/documentation/feature/account.html)
- [OPPO 小游戏工具下载](https://cdofs.oppomobile.com/cdo-activity/static/201810/26/quickgame/documentation/games/use.html)
