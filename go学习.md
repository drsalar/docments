## GC相关 ##

**三色标记清除算法**

go 1.8之后gc的速度已显著提升了，不需要特别关注，但是如果需要追求更高的性能，可以增加一些优化措施：
减少 string, map, slice, *Type 对象的创建

## 并发相关 ##

**go的map不是线程安全的**

在需要考虑性能的高并发环境下，可以使用sync.Map代替map。

## 性能相关 ##

**go的原生json性能较差**

可以使用 [jsoniter](https://github.com/json-iterator/go) 代替原生json
