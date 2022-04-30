# Hello,Gin!



一个简单的Web服务器代码如下,在下面的代码中，使用go run main.go运行，即可通过访问 `127.0.0.1:8081/ping` 即可查到看HelloGin的提示。

```go
package main

import "github.com/gin-gonic/gin"

func main() {
	r := gin.Default()
	r.GET("/ping", func(c *gin.Context) {
		c.JSON(200, gin.H{"message": "HelloGin"})
	})
    r.Run(":8081")
}
```

从上面代码中我们可以发现几个部分

* 路由注册：通过`r.GET` 使用Get方法，（其他的POST、DELETE、PUT方法同理）来注册路由`/ping` ,并紧接着对应调用的方法。
* 运行信息，本次后端监听了8081端口，而默认设置为8080。
* 默认Engine实例，此处指Engine实例，表示使用官方的Logger，Recovery中间件来创建实例，后续我们会使用Logrus来作为日志中间件。





<script src="https://utteranc.es/client.js"
        repo="Super-BUAA-2021/GinBook"
        issue-term="pathname"
        theme="github-light"
        crossorigin="anonymous"
        async>
</script>