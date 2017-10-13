# 环境搭建

1. 操作系统：Ubuntu 14.04

2. NDK:

   * 下载<https://developer.android.google.cn/ndk/downloads/index.html>  (版本：[android-ndk-r14b-linux-x86_64.zip](https://dl.google.com/android/repository/android-ndk-r14b-linux-x86_64.zip)),下载版本根据操作系统来定

   * 解压到：/opt/android-ndk-r14b

3. 环境变量配置：`export PATH=/opt/android-ndk-r14b:$PATH`

# 源码下载


1. Libevent: https://github.com/nmathewson/Libevent.git
2. Jansson: <https://github.com/akheron/jansson.git>
3. Openssl: <https://github.com/openssl/openssl.git>
4. Curl: https://github.com/curl/curl.git
5. Zlib: <https://github.com/madler/zlib.git>
# 交叉编译库文件
1. 编写交叉编译脚本build_all.sh及build.sh
2. 脚本使用方法：
   * 分别将build_all.sh和build.sh拷贝到源码文件夹


   * 修改build.sh:
     ANDROID_NDK_ROOT=ndk根目录（脚本57行）
     PREFIX：修改PATH到你想安装的目录（201行）
     请先编译Openssl库再编译Curl库，编译Curl库时修改--with-ssl=SSL_APTH配置（219行,SSL_PATH为步骤2中PATH路径），编译其他库时请删除该行（219行）
     终端切换到源码目录执行脚本：./build_all.sh

# 使用库文件
在Android Studio（2.3）下新建项目,将上面生成的库文件拷贝使用即可，具体参考项目源码

# 编译库文件前的准备工作

### Libevent  

先确保安装了libtool, autoconf

```sh
sudo apt-get install libtool
sudo apt-get install autoconf
```

再执行./autogen.sh

### jansson

源码是通过zip下载的，可以直接用，用git clone就不行

### openssl  

要把build.sh中的

```sh
./configure \
--prefix=$PREFIX \
--host=$TOOLNAME_BASE
```
改成

``````sh
./config \
--prefix=$PREFIX \
--openssldir=$PREFIX no-asm shared
``````

并删除源文件中所有的`-m64`

### zlib

去掉build.sh中第218行的`--host=$TOOLNAME_BASE`

### curl

要先执行`./buildconf`

