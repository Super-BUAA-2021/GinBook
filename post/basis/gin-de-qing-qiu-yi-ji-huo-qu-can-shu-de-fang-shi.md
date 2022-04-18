# 第二章 gin的请求以及获取参数的方式

一个简单的Web服务器代码如下,在下面的代码中，通过访问 127.0.0.1:8081

```
package main

import "github.com/gin-gonic/gin"

func main() {
	r := gin.Default()
	r.GET("/ping", func(c *gin.Context) {
		c.JSON(200, gin.H{"message": "HelloGin"})
	})
	r.Run()
}

```
