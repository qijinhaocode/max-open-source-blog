## 前言

在现代网络环境中，使用SSL证书来保护网站的数据传输安全是非常重要的。

没有证书，你的网站就像是这样。
![](https://twodog-1301294154.cos.ap-guangzhou.myqcloud.com/not-security-withoutssl.png?imageSlim)

最近很多人发现，免费SSL证书的有效期从一年缩短到了三个月。眨眼间就要更新证书，忘记更新还会导致浏览器警告，用户打不开网站，真是烦不胜烦。
我们本着能薅就薅的原则，坚决要免费的证书，毕竟省钱才是硬道理。虽然三个月的有效期除了证书本身免费，其他都是成本：申请和更新的时间成本，服务器维护的技术成本，记挂证书有效期的精神成本。果然，免费的东西往往最贵。

免费SSL证书就像一个定期来访的麻烦，每三个月就要耗费我们的时间和资源。

但我们依然选择它，因为免费才是王道。

本文将详细介绍如何在阿里云上获取免费的SSL证书，并配置到Nginx服务器。

## 步骤一：在阿里云申请SSL证书

1.  **登录阿里云控制台：**
    *   访问[阿里云官网](https://www.aliyun.com/)并登录您的阿里云账号。

2.  **进入SSL证书服务：**
    *   在控制台中找到“数字证书管理服务” -> “SSL证书管理”服务，点击进入。
        ![](https://twodog-1301294154.cos.ap-guangzhou.myqcloud.com/ssl-cert-management?imageSlim)
    *   没买的朋友可以立即购买，每年可以免费买一次，一次会发20个。
        ![](https://twodog-1301294154.cos.ap-guangzhou.myqcloud.com/buy-test-cert.png?imageSlim)
    *   买了的朋友可以点击创建，照提示填写域名信息并完成验证。
        ![](https://twodog-1301294154.cos.ap-guangzhou.myqcloud.com/create-ssl-test-cert?imageSlim)
    *   创建完成等待审核通过和证书机构签发成功通知。
        ![](https://twodog-1301294154.cos.ap-guangzhou.myqcloud.com/cert-issued-successfully-sms.jpg?imageSlim)
    *   审核通过后，进入“证书管理”页面，找到刚申请的证书并点击“下载”。择“Nginx”格式，下载证书文件并解压。
        ![](https://twodog-1301294154.cos.ap-guangzhou.myqcloud.com/view-ssl-test-cert.png?imageSlim)
        ![](https://twodog-1301294154.cos.ap-guangzhou.myqcloud.com/download-ssl-test-cert-for-nginx.png?imageSlim)

## 步骤二：配置Nginx使用SSL证书

1.  **上传证书到服务器：**
    *   使用FTP或SCP工具，将解压后的证书文件上传到服务器上的指定目录（例如`/etc/nginx/ssl/`）。

2.  **编辑Nginx配置文件：**
    *   打开Nginx的主配置文件或站点配置文件（通常位于`/etc/nginx/nginx.conf`或`/etc/nginx/sites-available/`）。
    *   在`server`块中添加或修改SSL配置，示例如下：

<!---->

       server {
           listen 443 ssl;
           server_name yourdomain.com;

           ssl_certificate /etc/nginx/ssl/qijinhao.com.pem;
           ssl_certificate_key /etc/nginx/ssl/qijinhao.com.key;

           ssl_protocols TLSv1.2 TLSv1.3;
           ssl_ciphers HIGH:!aNULL:!MD5;

           location / {
               root /var/www/html;
               index index.html index.htm;
           }

           # 其他配置...
       }

按ssl\_certificate和ssl\_certificate\_key的路径上传对应的证书，
或者把最新上传证书的路径更新到nginx server的配置当中。

1.  **重新加载Nginx以应用更改：**
    *   保存配置文件并退出编辑器，然后运行以下命令重新加载Nginx：
        ```bash
        sudo nginx -s reload
        ```
        如果不放心nginx的配置，也可以先运行一下  `nginx -t`，来检测配置文件是否正确。

![](https://twodog-1301294154.cos.ap-guangzhou.myqcloud.com/nginx-t-and-response.jpg?imageSlim)

## 步骤三：验证SSL配置

1.  **检查网站：**
    *   打开浏览器，访问您的网站（例如`https://qijinhao.com`）以确保SSL证书已正确应用。
    *   使用在线工具（例如[SSL Labs](https://www.ssllabs.com/ssltest/)）检查您的SSL配置是否安全。

## 额外提示：

*   **监控：** 设置提醒或自动化系统，以在证书过期之前更新您的SSL证书。
*   **安全性：** 始终遵循最佳实践来配置SSL/TLS设置以增强安全性。

## 结语

通过以上步骤，您可以在阿里云上申请免费的SSL证书，并将其配置到Nginx服务器中。定期更新证书以确保网站的持续安全非常重要，可以设置提醒或使用自动化脚本来简化更新过程。

如果您对本文有任何疑问或需要进一步的帮助，请在评论区留言。希望这篇教程对您有所帮助！
