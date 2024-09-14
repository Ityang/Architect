

## 一.设置HOME按键的默认launcher

在 Android 设备上拥有 `root` 权限后，可以通过以下方法将某个应用设置为默认的 `Launcher` 应用。

#### 第一步：获取应用包名和 `Launcher` Activity 名称

1. **获取应用的包名**： 使用以下命令获取所有安装应用的包名：

   ```
   adb shell pm list packages
   ```

   或者使用 `grep` 来筛选某个具体的应用：

   ```
   adb shell pm list packages | grep <app-name>
   ```

**获取 `Launcher` Activity 名称**： 使用以下命令列出应用中带有 `MAIN` 和 `LAUNCHER` intent 的 `Activity`，找到你要设置的应用的启动 Activity：

```
adb shell dumpsys package <app-package-name> | grep 'android.intent.action.MAIN'
```

输出的结果中会包含类似于 `<app-package-name>/<launcher-activity-name>` 的内容。例如：

```
com.example.app/.MainActivity
```

#### 第二步：设置默认 `Launcher`

1. 使用 `pm` 命令设置应用为默认启动器：

   ```
   adb shell cmd package set-home-activity <app-package-name>/<launcher-activity-name>
   ```

   例如：

   ```
   adb shell cmd package set-home-activity com.dongmayinzhang.hdcamera/com.dongmayinzhang.hdcamera.MainActivity
   ```

2. 成功设置后，按设备的主页键应该会启动该应用。

#### 第三步：确认设置是否生效

可以通过按设备的 Home 键查看是否成功切换默认 `Launcher`。如果操作无误，按下 Home 键会打开你指定的 `Launcher` 应用。



## 二.手动修改 `build.prop` 文件

通过修改系统的 `build.prop` 文件，可以永久地设置某个应用为默认的启动器。

#### 第一步：编辑 `build.prop` 文件

1. 打开具有 `root` 权限的文件管理器，进入 `/system` 目录。
2. 找到 `build.prop` 文件并以文本编辑器打开。

要在 Android 上修改系统文件（例如 `/system/build.prop`），你可以使用以下方法：

### 1. 使用 `adb shell` 和 `echo` 命令编辑文件

即使没有 `vi`，你仍然可以使用 `adb` 命令来修改文件内容。你可以通过 `echo` 命令直接向文件中写入内容：

#### 第一步：获得文件写入权限

首先，通过 `adb shell` 挂载 `/system` 目录为可读写：

```
adb root
adb remount
```

#### 第二步：编辑 `build.prop`

使用 `echo` 命令将新的内容写入文件：

```
adb shell
echo 'ro.config.home_app=com.dongmayinzhang.hdcamera' >> /system/build.prop
```

#### 第三步：重启设备

编辑完成后，重启设备以使更改生效：

```
adb reboot
```