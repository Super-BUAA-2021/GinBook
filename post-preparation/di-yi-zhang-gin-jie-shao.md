# Gin介绍

### Gin 是什么

* Gin是一个golang的微框架，封装比较优雅，API友好，源码注释比较明确，具有快速灵活，容错方便等特点
* 对于golang而言，web框架的依赖要远比Python，Java之类的要小。自身的`net/http`足够简单，性能也非常不错
* 借助框架开发，不仅可以省去很多常用的封装带来的时间，也有助于团队的编码风格和形成规范



### 为什么选择Gin

国外如Google、AWS、Cloudflare、CoreOS等，国内如[七牛](https://www.zhihu.com/search?q=%E4%B8%83%E7%89%9B\&search\_source=Entity\&hybrid\_search\_source=Entity\&hybrid\_search\_extra=%7B%22sourceType%22%3A%22answer%22%2C%22sourceId%22%3A47481548%7D)、阿里、字节等都已经开始大规模使用Golang开发其云计算相关产品。

相较于Springboot，Gin的上手难度低，Go语言总体语法上与C类似，很快就能学会。并且在内存、qps、部署后包大小上相较于Springboot均有不小的提升\[1]。go使用mod管理包极为方便，且目前个人已知下包与平台无关，参见pywin32。

相较于Django，go语言轻快的优势体现的较明显。go在速度上比起python快上很多，通常高一个数量级左右。并且在包管理上比起django还要方便。Go的旗号是为并发而生，django最大的并发量也不过300，而go的并发量在一般功能下可以破万。此外Gin的配置简单，有一个基本的结构后便可轻松上手，而个人觉得Django配置相对繁琐，例如跨域问题等。部署上相对Django也简单很多，打包运行即可，而Django仍要进行一定配置。

不过Gin相对于Django的缺点也比较明显，Go语言相对年轻，语法糖并不成熟，泛型也是在2022年的1.18版本中刚刚添加，很多代码可能会写的相对丑陋。



### 参考&#x20;

1. [go gin框架和springboot框架WEB接口性能对比](https://www.cnblogs.com/zhouqinxiong/p/14730217.html)




<script src="https://utteranc.es/client.js"
        repo="Super-BUAA-2021/GinBook"
        issue-term="pathname"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>