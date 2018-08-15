背景： 
        Chromium因为开发版的原因，account sync功能并没有集成进来，导致多台电脑之间不能同步访问过的url,书签，扩展等信息。结合开源和Google本身支持API的特点，通过编译即可满足工作需求。

  另外：通过Google sync API可以通过环境变量的形式使Chromium支持account sync功能，但是通过Launchpad或Dock等方式打开GUI程序是读取不到设置的环境变量的，除非通过命令行运行能读取到，网上也有相应的解决方案，比如通过plist，/etc/launch.conf，~/.launch.conf， /etc/profile，/etc/.bash_profile等方案在MacOS 10.13/14版本中均不可用，苹果对这块在新版本MACOS系统中控制的更加严格，而且从操作便捷性来看，并没有通过Launchpad或放到Dock启动方便。

1.安装Xcode
   可以通过ApppStore安装，也可以单独从Apple官方下载安装。
    验证
```
[admin@localhost chromium_build]$ls `xcode-select -p`/Platforms/MacOSX.platform/Developer/SDKs
MacOSX.sdk      MacOSX10.13.sdk
```
2.目录准备
```
mkdir -p chromium_build/chromium
```
4.安装depot_tools
```
cd chromium_build
git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
export PATH=$PATH:/Users/admin/Downloads/chromium_build/depot_tools   --添加环境变量
```
3.配置代理，这块需要自备木弟子，Server/Client 请参考各种实现：https://github.com/shadow#socks     //链接去掉#

   a.http/https代理
```
 export https_proxy=http://127.0.0.1:1087
 export http_proxy=http://127.0.0.1:1087
```
   b.boto代理
```
cat ~/.boto <>
[Boto]
proxy = 127.0.0.1
proxy_port = 1087
EOF

export NO_AUTH_BOTO_CONFIG=~/.boto
```
   4.获取源码
 ```
cd ../chromium
git config --global core.precomposeUnicode true
fetch --no-history chromium
或
gclient sync
```
  5.编译配置
```
cd src
gn gen out/Relase

cat chromium_build/chromium/src/out/Relase/args.gn
# Build arguments go here.
# See "gn args <out_dir> --list" for available build arguments.
is_official_build = true
google_api_key = "xxxxxxxxxxxxxx"
google_default_client_id = "yyyyyyyyyyyyyyyy"
google_default_client_secret = "zzzzzzzzzzzzzzzzzz"
use_official_google_api_keys = false
chrome_pgo_phase = 0
current_cpu = "x64"
enable_google_now = false
enable_hotwording = false
enable_iterator_debugging = false
enable_nacl = true
ffmpeg_branding = "Chrome"
is_component_build = false
is_debug = false
proprietary_codecs = true
symbol_level = 0
target_cpu = "x64"
exclude_unwind_tables = true
remove_webcore_debug_symbols = true
enable_webrtc = true
rtc_use_h264 = true
rtc_use_lto = true
use_openh264 = true
enable_hangout_services_extension = true
enable_widevine = true
#icu_use_data_file = true
enable_hevc_demuxing = true
enable_ac3_eac3_audio_demuxing = true
enable_mse_mpeg2ts_stream_parser = true
```
加快编译
```
is_debug = false
is_component_build = true
symbol_level = 0
```
重要部分
```
google_api_key = "xxxxxxxxxxxxxx"
google_default_client_id = "yyyyyyyyyyyyyyyy"
google_default_client_secret = "zzzzzzzzzzzzzzzzzz"
```
此处三个值需到 https://console.developers.google.com/  "凭证API服务"生成（具体操作自行Google），sync功能是根据此部分做鉴权。

args.gn 参数文件，参考：https://github.com/macchrome/chromium/blob/master/args.sync.gn.txt

6.编译
```
ninja -C out/Release chrome
```
编译完成后生成chromium_build/chromium/src/out/Release/Chromium.app即为所要浏览器文件。
编译时长根据计算机硬件而定，楼主MacBook Pro 2017 2c Intel Core i5 ，8G内存，256PCIe SSD硬盘 编译近6个小时，供参考。

7.更新编译
chromiume代码更新较快，每天都会有多版本提交，想用最新版本更新代码重复以上编译过程即可。
```
cd chromium_build/chromium/src
git rebase-update
gclient sync
```

原文参考：https://chromium.googlesource.com/chromium/src/+/master/docs/mac_build_instructions.md
最新各平台版本（已编译）：https://chromium.woolyss.com/
最新源码：https://github.com/chromium/chromium/releases
木弟 子：https://console.uk.to

