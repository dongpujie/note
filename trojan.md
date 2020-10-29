## Trojan搭建代理流程

**一文看懂Trojan搭建代理流程 含一键脚本及Trojan客户端下载**

相比其它几款主流的科学上网工具，比如Shadowsocks/SSR/V2Ray/WireGuard等，Trojan搭建流程稍有不同，因为它多了一个搭建伪装网站的步骤。

为了方便一些刚接触的用户，下面我们对Trojan的搭建流程，做一个总体的概括介绍，算是Trojan搭建目录或大纲吧。

 

### Trojan搭建流程一、VPS服务器

搭建Trojan代理的第一步，当然是获得一台国外的VPS服务器。

关于VPS服务器的选择，这里推荐搬瓦工或Vultr，多年来的口碑和性价比都不错：

- 搬瓦工：[点此进入官方网站](https://ssr.tools/goto/banwagonhost)，搬瓦工购买及使用教程[点这里了解](https://ssr.tools/208)。
- Vultr：[点此进入官方网站](https://ssr.tools/goto/vultr)，Vultr购买及使用教程[点这里了解](https://ssr.tools/216)。

注意：在购买VPS服务器时，推荐选择Debian 9系统，方便各种一键脚本的安装。搬瓦工或Vultr的VPS购买成功后，都可以在后台更换系统版本。

更多关于VPS服务器的介绍，可以参考：[适合搭建代理的国外VPS服务器推荐汇总](https://ssr.tools/55)

如果你觉得搭建流程太过繁琐，也可以考虑使用Justmysocks的代理服务，免搭建，速度和性价比也很不错。

- Just My Socks：[点此进入官方网站](https://ssr.tools/goto/justmysocks)，Justmysocks购买及使用教程[点这里了解](https://ssr.tools/999)。



### Trojan搭建流程二、域名

由于涉及到伪装网站的搭建，以及SSL证书的申请，我们需要提前购买一个域名，指向你的VPS服务器IP地址。

购买域名时，可以考虑国外的Namesilo或Godaddy，都支持支付宝付款，随便选购一个便宜的域名就可以。

当然国内的域名网站也可以，但是需要额外做实名认证，相对不推荐。

- Namesilo官方网站：[https://www.namesilo.com](https://www.namesilo.com/)
- Godaddy官方网站：[https://www.godaddy.com](https://www.godaddy.com/)

域名购买成功后，在后台DNS设置里，将域名指向你的VPS服务器IP。

 

### Trojan搭建流程三、安装BBR

这一步非必需，但是非常推荐。在为VPS安装BBR后，可以大幅提高网络连接速度，包括Trojan的连接速度。

我们上一步介绍的域名DNS生效需要一定时间，正好可以利用这段时间安装BBR。

BBR可以通过一键脚本安装，以下脚本任选其一：

- [BBR一键安装脚本](https://ssr.tools/199)
- [魔改BBR一键安装脚本](https://ssr.tools/550)
- [BBR Plus一键安装脚本](https://ssr.tools/1217)

 

 

### Trojan搭建流程四、一键脚本安装Trojan

Trojan一键安装脚本：

| 1    | curl -O https://raw.githubusercontent.com/atrandys/trojan/master/trojan_mult.sh && chmod +x trojan_mult.sh && ./trojan_mult.sh |
| ---- | ------------------------------------------------------------ |
|      |                                                              |

 

此脚本可以自动完成nginx安装、网站搭建、SSL证书申请，非常方便。

更具体的安装流程可以参考：

[Trojan一键安装脚本 傻瓜全自动 搭建伪装网站、续签证书、客户端参数配置](https://ssr.tools/1265)

 

注意：Trojan服务器端安装完成后，会提供Trojan客户端配置文件供下载，建议下载备用。

 

 

### Trojan搭建流程五：下载Trojan客户端连接服务器

根据系统平台的不同，Trojan客户端也有所不同。具体可以分为：

- Trojan Windows客户端
- Trojan Mac客户端
- Trojan 安卓客户端
- Trojan iOS客户端
- Trojan Linux客户端
- Trojan OpenWrt 路由器客户端

 

以上各平台Trojan客户端下载，以及使用教程，可以参考：

[Trojan各平台客户端下载汇总 Windows/Mac/安卓/iOS/Linux/路由器](https://ssr.tools/1413)

 

Trojan客户端成功连接服务器后，即可为所在设备提供科学上网服务，Trojan的代理环境，就算搭建完成了。

 

### **其它常用代理工具 一键搭建脚本**

[Shadowsocks一键安装脚本使用教程](https://ssr.tools/373)

[SSR一键安装脚本使用教程](https://ssr.tools/31)

[V2ray一键安装脚本使用教程](https://ssr.tools/1358)

[WireGuard一键安装脚本使用教程](https://ssr.tools/1492)