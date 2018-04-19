## GC相关 ##

**三色标记清除算法**

go 1.8之后gc的速度已显著提升了，不需要特别关注，但是如果需要追求更高的性能，可以增加一些优化措施：
减少 string, map, slice, *Type 对象的创建

## 并发相关 ##

**go的map不是线程安全的**

在需要考虑性能的高并发环境下，可以使用sync.Map代替map。

**Mutex or channel**

通过沟通来共享内存，不要用共享内存来沟通。

|    mutex  |    chan    |
| :-: | :-: | 
| 缓存，状态 | 分配工作，传递数据，交流异步结果 |

sync.WaitGroup 是一个非常有用的工具

## 性能相关 ##

**go的原生json性能较差**

可以使用 [jsoniter](https://github.com/json-iterator/go) 代替原生json
