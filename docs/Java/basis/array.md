## 数组概念

数组的概念：相同数据类型的数据元素的有序（物理地址有序非元素有序）集合。通过一个整型下标就可以访问数组中的每一个元素。  

特点：  
- 存储多个数据元素。

- 这多个数据元素的数据类型必须一致。

可以存储的数据类型：  
- 基本类型数据。  

- 引用类型数据。

数组的定义格式：  
- 格式1：
  ```java
  数据类型[] 数组名;
  ```

- 格式2：
  ```java
  数据类型 数组名[];
  ```

建议使用格式1，避免造成歧义。

## 数组的初始化

数组的初始化：为数组中的数组元素分配内存空间，并为每个数组元素赋初值。Java 中的数组必须先初始化，然后才能使用。  

数组的初始化方式：  
- 动态初始化（指定长度）。格式：
  ```java
  数据类型[] 数组名 = new 数据类型[数组长度];
  ```  
  示例：  
  ```java
  int[] arr = new int[3];
  ```  

- 静态初始化（指定内容）。格式：
  ```java
  数据类型[] 数组名 = new 数据类型[]{元素1, 元素2, …};
  ```  
  示例：  
  ```java
  int[] arr1 = new int[]{1, 2, 3};

  int[] arr2 = {1, 2, 3}; // 简化写法
  ```
  注：简化写法只在数组定义时有效！

注：
- 一旦创建了数组就不能再改变它的大小。如果经常需要在运行中拓展数组的大小，就应该使用另外一种数据结构 —— 数组列表（Arraylist），之后会详细介绍。  

- 字符串类型的数据不是基本数据类型的数据，我们所说的一个字符串，其实是一个字符串对象。字符串对象存储在数组中，数组中其实存储的是对象的地址值，并未直接存储字符串中的字符序列数据，而同一个 JVM 中的引用值（地址值）占用的内存大小相同。


## 数组拷贝

在 Java 中，允许将一个数组拷贝给另一个数组变量。这时两个变量将引用同一个数组。  
示例：
```java
int[] arr2 = arr1;
arr2[3] = 5; // 这时 arr1[3] 也等于 5
```

如果希望将一个数组的所有值拷贝到一个新的数组中去，可以使用 Arrays 类中的 copyOf 方法。  
示例：    
```java
int[] arr2 = Arrays.copyOf(arr1, arr1.length);
```
参数2 确定新数组的长度，因此这个方法常用来调整数组的长度。如果参数2 小于原始长度，则只拷贝最前面的数据元素。

## 二维数组

相对于一维数组单行多列的结构，二维数组就是一张多行多列的数据表结构。  

定义格式：  
- 动态初始化：
  ```java
  数据类型[][] 数组名 = new 数据类型[行的长度][列的长度];
  ```  
  示例：  
  ```java
  int[][] arr = new int[10][10]
  ```

- 静态初始化：
  ```java
  数据类型[][] 数组名 = {
      {}, 
      {}, 
      {}
  };
  ```  
  示例：  
  ```java
  int[][] arr = {
      {1, 2, 3},
      {3, 4, 6},
      {7, 8, 9}
  };
  ```

注：for each 循环语句不能自动处理二维数组的每一个元素。它是按行，也就是一位数组处理的。想要访问二维数组的所有元素，需要使用两个嵌套的循环。  
示例：
```java
for (int[] row : arr) {
    for (int value : row) {
        循环语句
    }
}
```
 
- Java 实际上没有多维数组，只有一维数组。多维数组实际上是「数组的数组」。因此 Java 中可以构建不规则数组，即数组的每一行有不同的长度。   

Java 中允许定义不规则的多维数组。  
不规则数组动态定义格式：`数据类型[][] 数组名 = new 数据类型[行的长度][];`。  
示例：
```java
int[][] arr = new int[3][];
for (int i = 0; i < arr.length; i++) {
  int[i] = new int[i + 1];
}
/*
构建了不规则数组：
{
  {长度为 1},
  {长度为 2},
  {长度为 3}
}
*/
```


## 数组常见报错

`ArrayIndexOutOfBoundsException`：数组索引越界。  

`NullPointerException`：空指针异常。 