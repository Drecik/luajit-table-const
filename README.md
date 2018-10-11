# luajit-table-const
在LuaJIT-2.1.0-beta3基础上，增加对于常驻内存table的常量化处理。这样就可以避免lua gc对于这些特殊table的遍历。   
常驻内存的table一般用在游戏配置表中，这种表可能会有几万个元素，而且会一直常驻内存，供游戏运行时使用。但是lua的gc是不会知道这些，每次gc的时候都会一次次去遍历这些table，导致gc不必要的开销，所以做了一个优化，给个接口指定这是一个常驻内存的table，在第一次被gc的时候，将这些table从全局gc链表中移除，加入到一个新的constant链表。避免每次gc的遍历。   
对于lua gc分析可以参考我博客：[lua gc分析](https://drecik.top/2018/06/17/12/)

## 编译
直接采用luajit编辑方式即可，直接make

## 使用方式
直接使用table.constant设置table即可，建议在
``` lua
local t = {1,2,3,4,5,6}
table.constant(t)
```

## 注意
- 从给table设置constant之后完整的一次gc之前，不能主动调用full gc否则会导致table子元素没有被标记，这样就会被误删除，导致访问的时候出现内存问题
- table不能设置weak
- table元素只能是table、string、number、function，不能有线程
