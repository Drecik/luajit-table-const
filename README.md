# luajit-table-const
在LuaJIT-2.1.0-beta3基础上，增加对于常驻内存table的常量化处理。这样就可以避免lua gc对于这些特殊table的遍历。
