**转载请标注原创地址：[https://blog.csdn.net/lsyz0021/article/details/96499543](https://blog.csdn.net/lsyz0021/article/details/96499543)**

[GitHub地址 https://github.com/lsyz0021/androidSigner](https://github.com/lsyz0021/androidSigner)

一提到给apk签名，大家或许想这还不简单，打开终端配置好“apksigner”命令一行不就搞定了，但是如果让你给100个apk签名，这样的签名方式还简单吗？
因为最近有经常要给apk签名的需要，并且有时候可能要同时给十几个apk签名，所以就想到了写个批量v2签名的shell脚本，写完之后使用感觉起来感觉也特别方便。想到或许其他人也可能会有这种需求，于是将他开源出来供大家使用。废话不多说先介绍功能！先使用
**特点：**

1、一键批量签名

2、可选配置项丰富

3、安全（担心配置文件泄露密码，密码为空即手动输入）

4、长期维护



**注意使用前请先配置buildTools的路径**



## 1、使用方式
```shell
#打开终端，克隆项目
git clone https://github.com/lsyz0021/androidSigner.git
#进入目录
cd androidSigner
#运行脚本
./signer.sh
```

## 2、设置配置文件
`signerConfig.ini`配置文件可以不使用，默认在当前目录下搜索，这时就需要手动输入所有的内容，并且不支持指定搜索目录和输出目录。
这里需要注意：由于`apksigner`命令是在25.0.0版本才出现的，所以`buildTools`配置的**build-tools**版本至少要是25.0.0。

```shell
#签名文件的位置
storeFile=./appkey.jks
#签名文件的密码
storePassword=123456
#key的别名
keyAlias=appkey
#key的密码
keyPassword=12345678
#sdk中build-tools的路径，apksigner命令25.0.0版本才出现
buildTools=/Users/wuge/sdk/build-tools/29.0.0/
#是否自动搜索指定目录，true:自动搜索，false:手动输入，no:需要选择获取方式
auto=no
#是否重复签名，已签名的apk名字上会加个“signer-”，true：如果搜索到会对该apk再次签名，false：不会再签名
repeatSigner=false
#指定搜索的apk的关键字(支持通配符*)
searchKey=app-release*.apk
#指定搜索目录，"."代表当前目录，不设置此值默认为当前目录
searchPath=.
#指定的apk文件
sourceApkPath=apk/app-release-01.apk
#指定输出目录，不设置默认为当前目录下的out目录
outPath=./out
```
## 3、执行脚本，选择“y”
配置好上面的内容之后，就可以执行`signer.sh`脚本了。配置文件默认`auto=no`，所以需要手动选择输入方式，下面是输入的`y`，开始在指定目录(`searchPath=.`表示当前目录)下开始自动搜索所有的apk，并且进行签名。
**注意：如果`auto=true`，相当于自动输入了`y`，就自动进行搜索指定目录(`searchPath=.`)下的apk**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190719205719878.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xzeXowMDIx,size_16,color_FFFFFF,t_70)

## 4、生成获取签名apk
执行完`signer.sh`脚本，在指定的输出目录(`outPath=./out`)下生成了签名后的apk，每个签名成功的apk名字前面都会增加"signer-"前缀。**默认情况下如果搜索到以signer-开头的apk不会重复签名，如果想重复签名则设置`repeatSigner=true`**
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190719205757300.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xzeXowMDIx,size_16,color_FFFFFF,t_70)

## 5、执行脚本，选择“n”
运行`signer.sh`，我们输入`n`，这时就需要我们手动指定apk的路径了
**注意**：如果`auto=false`，相当于自动输入了`n`
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190719205807907.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xzeXowMDIx,size_16,color_FFFFFF,t_70)
根据自己的需求配置`signerConfig.ini`，即可灵活使用了。

## 6、不配置密码
如果你担心配置文件中的密码被泄露，完全可以手动输入，这样很好的避免了密码外泄。**（甚至你都可以删除配置文件）**
配置文件中设置`storePassword=`、`keyPassword=`都是设置为空，这样就可以手动输入密码了。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190721184510999.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xzeXowMDIx,size_16,color_FFFFFF,t_70)

## 7、删除配置文件
如果你认为使用配置文件太累赘，也可以删除他，然后手动配置选项。
![在这里插入图片描述](https://img-blog.csdnimg.cn/20190721185914724.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L2xzeXowMDIx,size_16,color_FFFFFF,t_70)
[GitHub地址 https://github.com/lsyz0021/androidSigner](https://github.com/lsyz0021/androidSigner)

## 8、新增v1、v2签名校验

```shell
                echo V2签名成功:$signerApk
                echo -e "---------------签名验证提示开始----------------\n"
                #echo -e `apksigner verify -v $signerApk`
                result=$(echo `apksigner verify -v $signerApk`)
                signVerifyStr="Verifies Verified using v1 scheme (JAR signing): true Verified using v2 scheme (APK Signature Scheme v2): true"
                echo $result
                echo -e "---------------签名验证提示结束----------------\n"
                if [[ $result == *$signVerifyStr* ]]
                    then
                        echo "v1、v2 签名验证成功"
                    else
                        echo "v1、v2 签名验证失败"
                    fi
            
```

## 9、注意

使用时需注意`signerConfig.ini`配置文件中的build-tools版本号，需要与环境变量一致，否则验证签名时会有如下报错

```shell
DOES NOT VERIFY
ERROR: APK Signature Scheme v2 signer #1: Malformed additional attribute #1
WARNING: APK Signature Scheme v2 signer #1: Unknown signature algorithm: 0x421
```

环境变量地址（Mac）： export AAPT_HOME=/Users/xxxx/Library/Android/sdk/build-tools/25.0.2 









