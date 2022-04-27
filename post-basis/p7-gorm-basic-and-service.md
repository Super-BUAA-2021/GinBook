# 第七章：初识GROM使用GORM进行数据库操作

ORM（对象关系一行社）可以在关系型数据库与对象之间建立映射，可以想操纵对象一样操作数据库。而在Go中，我所指的有Gorm与Beego ORM两种ORM库用一操作数据库。而本Book中选择Gorm。

### 为什么选择Gorm

Gorm是Go语言中的一个性能较好的ORM库。对开发人员友好，功能强大，调用方便。许多大厂都在使用[1]。

此外还有一些特点：

* 支持关联（Has one Has Many….)
* 支持预加载
* 支持Hook（增删改查前后进行操作）
* 支持事务处理。（很重要）



### Gorm的安装

```
go get -u github.com/gorm.io/gorm
```





## Gorm的具体使用



TODO





# 参考

1. 极客时间：Go项目开发实战：30
2. [Gorm](https://github.com/go-gorm/gorm)
