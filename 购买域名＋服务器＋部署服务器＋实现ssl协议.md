---
title: 购买域名+服务器+部署服务器+实现ssl协议
date: 
tags:
layout: draft
---

纪录部署busihacker.com的一些问题
<!-- more -->

[两条a记录主要是为了负载均衡吧？](https://www.google.com.hk/search?ie=utf-8&oe=UTF-8&hl=zh-CN&q=%E4%B8%A4%E6%9D%A1+a%E8%AE%B0%E5%BD%95&gws_rd=ssl)
[常用域名记录解释：A记录、MX记录、CNAME记录、TXT记录、AAAA记录、NS记录](https://www.ezloo.com/2011/04/a_mx_cname_txt_aaaa_ns.html)
[如何建立个人网站？](https://www.zhihu.com/question/19774219)
[新手教程：建立网站的全套流程与详细解释](http://yihui.name/cn/2009/06/how-to-build-a  -website-as-a-dummy/)
[2014年网站SEO常见作弊方法详细解析](https://www.douban.com/group/topic/61689932/)
[Godaddy注册商域名修改DNS地址](https://support.dnspod.cn/Kb/showarticle/tsid/42/)
[管理DNS](https://sg.godaddy.com/zh/help/dns-680)
[个人godaddy域名备案解决方案](http://www.ixirong.com/2015/04/15/how-blog-record-by-aliyun/)
godaddy 对域名的所有权以及备案问题。
免费的是最贵的[免费域名](https://www.zhihu.com/question/19835955)
[如何抢注一个刚刚过期的域名](https://www.zhihu.com/question/20845371)

SSL协议与加密
[SSL/TLS协议运行机制的概述](http://www.ruanyifeng.com/blog/2014/02/ssl_tls.html)
[HTTPS 升级指南](http://www.ruanyifeng.com/blog/2016/08/migrate-from-http-to-https.html)
[PGP加密原理](http://www.asiapeak.com/PGPTheory.php)
[GPG入门教程](http://www.ruanyifeng.com/blog/2013/07/gpg.html)
[关于pgp](http://pgp.sourceforge.net/pgpintro.php)
[RSA算法原理](http://www.ruanyifeng.com/blog/2013/06/rsa_algorithm_part_one.html)
[RSA算法原理](http://www.ruanyifeng.com/blog/2013/07/rsa_algorithm_part_two.html)
http协议与ssl协议
[[TCP三次握手&Render Tree页面渲染=>从输入URL到页面显示的过程？](https://segmentfault.com/a/1190000006921322)]
[不同format的certificate和private/public key](https://www.sslshopper.com/ssl-converter.html)
# 使用startssl认证证书
[站点启用https替换http完整步骤（申请StartSSL免费证书，nginx配置）](https://www.zghhome.cn/?p=310)
[怎样申请StartSSL免费ssl证书](https://amon.org/how-to-apply-for-startssl-free-ssl-ca.html)
[SSL Certificate Reviews](https://www.sslshopper.com/certificate-authority-reviews.html)
[StartSSL申请全过程 让网站拥有免费SSL证书](http://www.laozuo.org/2823.html)
[现在就启用 HTTPS，免费的！](https://www.oschina.net/translate/switch-to-https-now-for-free)
[how to obtain and install an SSL/TLS certificate for free](http://arstechnica.com/security/2009/12/how-to-get-set-with-a-secure-sertificate-for-free/2/)
[openssl、x509、crt、cer、key、csr、ssl、tls 这些都是什么鬼?](http://www.cnblogs.com/yjmyzz/p/openssl-tutorial.html)
[crt csr 谷歌搜索](https://www.google.com.hk/search?q=crt++csr&ie=utf-8&oe=utf-8&gws_rd=cr,ssl)
[伪造 证书 谷歌搜索](https://www.google.com.hk/search?q=%E4%BC%AA%E9%80%A0++%E8%AF%81%E4%B9%A6&ie=utf-8&oe=utf-8&gws_rd=cr,ssl)
[tornado使用nginx作为反向代理](http://docs.pythontab.com/tornado/introduction-to-tornado/ch8.html#ch8-2-2)
[公钥 私钥 原理 谷歌搜索](https://www.google.com.hk/search?q=%E5%85%AC%E9%92%A5+%E7%A7%81%E9%92%A5+%E5%8E%9F%E7%90%86&ie=utf-8&oe=utf-8&gws_rd=cr,ssl)
[rsa pop关系 谷歌搜索](https://www.google.com.hk/search?newwindow=1&safe=strict&q=rsa+pgp+%E5%85%B3%E7%B3%BB&oq=rsa+pgp+%E5%85%B3%E7%B3%BB&gs_l=serp.3...495206.496328.0.496470.8.6.0.0.0.0.0.0..0.0....0...1c.1.64.serp..8.0.0.nVv5C48oNSk)
[使用Tornado搭建HTTPS网站](http://www.yeolar.com/note/2015/04/30/tornado-ssl-https/)
[How to create HTTPS tornado server](stackoverflow.com/questions/18307131/how-to-create-https-tornado-server)
[SSL和SSH的区别](http://blog.csdn.net/simanstar/article/details/40592057)
[SSL SSH 谷歌搜索](https://www.google.com.hk/search?q=ssh+ssl&ie=utf-8&oe=utf-8&gws_rd=cr,ssl)
[generating-a-gpg-key](https://help.github.com/articles/generating-a-gpg-key/)
[generating-an-ssh-key](https://help.github.com/articles/generating-an-ssh-key/)

---
ipython notebook
When using a password, it is a good idea to also use SSL, so that your password is not sent unencrypted by your browser. You can start the notebook to communicate via a secure protocol mode using a self-signed certificate with the command:

>$ ipython notebook --certfile=mycert.pem

A self-signed certificate can be generated with openssl
. For example, the following command will create a certificate valid for 365 days with both the key and certificate data written to the same file:
>$ openssl req -x509 -nodes -days 365 -newkey rsa:1024 -keyout mycert.pem -out mycert.pem


---
# 远程服务

下一步：
部署到域名进行访问[busihacker.com](https://busihacker.com:9999)
认证ssl/tls证书
`https://<your-domain>:9999`