## 数据结构

![](https://images2015.cnblogs.com/blog/616953/201603/616953-20160322185210151-491440543.png)

## 属性

```
// 版本号
private static final long serialVersionUID = 8683452581122892189L;
// 缺省容量
private static final int DEFAULT_CAPACITY = 10;
// 空对象数组
private static final Object[] EMPTY_ELEMENTDATA = {};
// 缺省空对象数组
private static final Object[] DEFAULTCAPACITY_EMPTY_ELEMENTDATA = {};
// 元素数组
transient Object[] elementData;
// 实际元素大小，默认为0
private int size;
// 最大数组容量
private static final int MAX_ARRAY_SIZE = Integer.MAX_VALUE - 8;
```
## 核心构造器

## 核心函数
- add
- addAll
- remove

## 重要第三方函数
Arrays.copyOf
System.arraycopy

> [【Java集合源码剖析】ArrayList源码剖析](https://blog.csdn.net/ns_code/article/details/35568011)