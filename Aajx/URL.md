`URI包括URL和URN`
- URI : 统一资源标识符
- URL ： 统一资源定位符
- URN : 统一资源名称


> ###### URL的组成

<html>
比如：https://www.baidu.com:80/item/url/110640?fr=aladdin
</html>

---


第一部分协议（scheme）

```
该URL的协议部分为http
如果是http协议，网站可以进行省略；
常用的有http、https、ftp、mailto、file
```
第二部分域名

```
一个URL中，也可以使用IP地址作为域名使用
顶级域名:
www.baidu.com
二级域名:
baike.baidu.com
三级域名:
baike.tieba.baidu.com
```
第三部分端口

```
范围是0-65535
http默认端口为80
跟在域名后面的是端口，域名和端口之间使用“:”作为分隔符。
端口不是一个URL必须的部分
如果省略端口部分，将采用默认端口
```
第四部分虚拟目录（路径）

```
从 域名后的第一个“/”开始到最后一个“/”为止，是虚拟目录部分。
虚拟目录也不是一个URL必须的部分
/ 可能是目录也可能是接口或者二次封装之前的目录
```
第五部分文件名

```
域名后最后一个/到？的部分。如果没有？则是从域名后的最后一个'/'到 #
？代表查询信息。
window.location.search包含问号不包含#号
即可以读也可以写，写了查询信息是会刷新页面

```
第六部分锚信息

```
#号之后的都是锚信息
window.loaction.hash可以获取#
即可以读也可以写，写了锚信息是不会刷新页面
window.onhashchange  当hash值发生变化的时候才会触发 
```

