# Day23 thread02
  
## 作业
1. 用多线程代码来模拟，迅雷用3个线程下载100M资源的的过程。  
   每个线程每次，一次下载1兆(M)资源，直到下载完毕，即剩余的待下载资源大小为0  
   (用一个整数表示资源大小，每次个线程每次下载多少兆(M), 剩余待下载资源就减少多少兆(M)，  
    模拟我们售票的代码实现，考虑多线程的数据安全问题)  
    
  
2. 创建2个线程，打印从0到99这100个数字，要求线程交叉顺序打印。比如：  
    ```
    线程1: 0
    线程2: 1
    线程1: 2
    线程2: 3
    线程1: 4
    线程2: 5
    ```