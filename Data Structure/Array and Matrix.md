# 数组
## 一、知识点
### 1. 概述
(1) 数组是存放在**连续内存空间**上的**相同类型**数据的集合。可通过下标索引(**0-based**)获取到下标下对应的数据。

(2) 因为数组的在内存空间的地址是连续的，所以在删除或者增添元素的时候，就难免要**移动其他元素的地址**。

(3) **数组的元素是不能删的，只能覆盖**。(?!)

(4) 数组本身是引用数据类型，它的元素相当于类的成员变量。

### 2. 一维数组
(1) 声明和初始化
```java
// 声明:声明时不能指定长度
int[] a1;
int a[];
String[] c; // 引用类型变量数组
// 初始化
// 静态初始化
int arr[] = new int[]{ 3, 9, 8};
// 动态初始化
String names[] = new String[3];
```
<1> **数组一旦初始化，其长度(arr.length)是不可变的。**

<2> 对于基本数据类型，默认初始化值各有不同(byte, short, int: 0; long: 0L; float: 0.0F; double: 0.0; char: 0 或写为:’\u0000’(表现为空))。对于引用数据类型，默认初始化值为null。
    
### 3. 二维数组

(1) 二维数组的空间地址

**Java是没有指针的，同时也不对程序员暴露其元素的地址，寻址操作完全交给虚拟机。所以看不到每个元素的地址情况。**
```java
public static void test_arr() {
    int[][] arr = {{1, 2, 3}, {3, 4, 5}, {6, 7, 8}, {9,9,9}};
    System.out.println(arr[0]);
    System.out.println(arr[1]);
    System.out.println(arr[2]);
    System.out.println(arr[3]);
}
```
```html
[I@7852e922
[I@4e25154f
[I@70dea4e
[I@5c647e05
```
不是真正的地址，而是经过处理过后的数值。二维数组的每一行头结点的地址是没有规则的，更谈不上连续。

Java的二维数组可能是如下排列的方式：
<div align="center"> <img src="https://img-blog.csdnimg.cn/20201214111631844.png" width="350px"> </div><br>

(2) 二维数组转稀疏数组(sparsearray)

<1> 遍历原始二维数组，得到有效数据的个数N。

<2> 创建稀疏数组 sparseArr int[N + 1][3]。sparseArr[0][0] = row, sparseArr[0][1] = col, sparseArr[0][2] = sum。

<3> 遍历原始二维数组，将有效数据的行、列、值存入稀疏数组。

## 二、题目

