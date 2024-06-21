# 腾讯云SSL证书申请

## 前言

文章记录腾讯云SSL证书申请，网站启用HTTPS对于网站安全性以及易用性是有实质性帮助。

在部署HTTPS之前，先申请并下载HTTPS证书，下面以腾讯云SSL证书为例，介绍腾讯云SSL证书申请详细教程：

## 操作流程

**1、进入腾讯云控制台**

腾讯云控制台地址：[总览 - 控制台 - 腾讯云 (tencent.com)](https://console.cloud.tencent.com/)

![image-20230613204212181](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/06/13/64886442ba7dd.png)

**2、进入SSL证书功能界面**

SSL证书界面https://console.cloud.tencent.com/certoverview

**3、在左侧菜单，找到【我的证书]】，然后点击右侧的【免费证书】，在选择【申请免费证书】，在弹窗中确定【申请免费证书】**

> 文章以免费的SSL证书为例

![image-20230613205208533](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/06/13/6488667ace2dd.png)

**腾讯云证书说明：**

腾讯云SSL证书，分为付费证书、以及不收费的证书：

- 不收费的证书：不收费的证书单个账号总共能申请50个证书，其证书类型为DV SSL证书。

  > 关于：DV SSL证书（Domain ValidationSSL），只验证域名权限，并颁发，网站的信息从用户浏览器到服务器之间的传输是加密传输的，信息不会被非法窃取和非法篡改。

- 付费证书：付费证书有四种类型可供选择

  - **TrustAsia DV 泛域名证书**
    - TrustAsia DV 泛域名证书经济实惠，保护网站数据安全，适合个人，中小企业应用。一个证书保护多个子域，性价比高
    - TrustAsia DV 泛域名证书由Sectigo 根证书签发，也有利网站设置 HTTPS 后的访问速度。
  - **DNSPod DV 泛域名证书**
    - 支持 SM2 国密算法和国密安全协议，适合对国密合规性有要求的网站。
    - 一个证书保护多个子域，支持双证书部署以强化兼容性。
  - **GlobalSign OV 单域名证书**
    - 企业级加密SSL证书，互联网、电商、政企服务的选择。Globalsign是一家CA 中心和SSL数字证书提供商
  - **WoTrus OV 泛域名证书**
    - WoTrus 自有数字证书产品**，**针对市场的需求而设计，从不同用户需求出发，支持浏览器和服务器。

**4、填写参数，然后提交**

- 证书绑定域名：填写您申请SSL证书的域名，免费证书不支持泛域名，即您只能在此处填写一个域名

- 申请邮箱：填写您当前的QQ邮箱地址就可以
- 域名验证方式：自动DNS验证
- 算法选择：RSA算法、ECC算法；（此项保持默认的选择就可）

![image-20230613220655966](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/06/13/648878021e293.png)

**5、等待生效**

点击页面左侧[我的证书]，您会发现，在右侧**证书列表会显示证书状态[已签发]**，点击右侧操作下方的[下载]，完成腾讯云SSL证书的申请。

![image-20230613221205561](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/06/13/648879379b23b.png)

## 部署到宝塔面板

**1、下载【Nginx（适用大部分场景）（pem文件、crt文件、key文件）】**

![image-20230613222806079](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/06/13/64887d1109f92.png)

**2、进入宝塔面板，点击SSL证书，并打开下载下来的证书文件，将`域名.key`文件用记事本打开，并复制内容到【密钥(KEY)】，将`域名.pem`文件用记事本打开，并复制内容到【证书(PEM格式)】中，然后选择【保持并启用证书】**

![image-20230613223629747](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/06/13/64887eefe858c.png)

**3、部署完成后，可以看见SSL证书的剩余有效期**

![image-20230613223729307](https://lskypro-1309218011.cos.ap-shanghai.myqcloud.com/2023/06/13/64887f2aebed7.png)