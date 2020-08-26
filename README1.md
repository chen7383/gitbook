# lock锁源码分析
## 二级标题


## 有序列表

1. ada
2. adadadad啊a大
3. a ad啊是大
4. a a的a 啊


1. asda
2. ada
3. as
4. da
5. da

## 引用
> 这就是引用
> 大于号加空格
> 

## 代码
今天学了一个很牛逼的API ``getxxx()``
三个点加语言名称加代码加三个点
```java
lock()
通过cas操作偏移量
true操作成功,将锁赋值给该线程
flase操作失败进入方法：
    public final void acquire(int arg) {
        if (!tryAcquire(arg) &&
            acquireQueued(addWaiter(Node.EXCLUSIVE), arg))
            selfInterrupt();
    }


final boolean nonfairTryAcquire(int acquires) {  
    final Thread current = Thread.currentThread();  
    int c = getState();  
    if (c == 0) {  
        if (compareAndSetState(0, acquires)) {  
            setExclusiveOwnerThread(current);  
            return true;  
        }  
    }  
    else if (current == getExclusiveOwnerThread()) {  
        int nextc = c + acquires;  
        if (nextc < 0) // overflow  
            throw new Error("Maximum lock count exceeded");  
        setState(nextc);  
        return true;  
    }  
    return false;  
};
tryAcquire(arg)作用：
获取state值;
c==0 没有线程竞争锁;
通过CAS设置该状态值为acquires的初始调用值1，每次线程重入该锁都会+1，每次unlock都会-1，为0时释放锁;
CAS设置成功，则可以预计其他任何线程调用CAS都不会再成功，也就认为当前线程得到了该锁，也作为Running线程，很显然这个Running线程并未进入等待队列;
c!=0 先判断是否是当前线程获取此锁，如果已经持有只是修改了state的值


private Node addWaiter(Node mode) {  
    Node node = new Node(Thread.currentThread(), mode);  
    // Try the fast path of enq; backup to full enq on failure  
    Node pred = tail;  
    if (pred != null) {  
        node.prev = pred;  
        if (compareAndSetTail(pred, node)) {  
            pred.next = node;  
            return node;  
        }  
    }  
    enq(node);  
    return node;  
};

addWaite()作用：
将当前线程追加到队列队尾处
参数mode是独占锁还是共享锁,默认为null独占锁,
当队尾pred!=null 使用cas把当前线程更新为



```
### 加粗
**加粗**
< font  color="red">asas</font>
[点我去百度](http://www.baidu.com)

![这是一张图片](daomei.png)
![这是一张图片](图片地址)

图床
把本地的图片上传到网络，把网络地址复制到md文件中

七牛
ctrl+alt+v上传网络并且返回图片地址
[toc] 目录
