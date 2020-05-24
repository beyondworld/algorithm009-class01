## 学习笔记
### 1、用 add first 或 add last 这套新的 API 改写 Deque 的代码
    使用数组模拟存储，headIndex指向头指针，tailIndex指向尾指针，两个指针指向的位置均为null，方便直接使用以及判断空满。
  * NewDeque    生成新的双端队列
  * AddFirst    头部添加元素
  * RemoveFirst 头部移除元素
  * AddLast     尾部添加元素
  * RemoveLast  尾部移除元素
  * isFull      判断队列是否满
  * isEmpty     判断队列是否空
  * extendDeque 内部方法扩展队列存储容量，并且调整指针位置

```
var localDeque = Array()
var initSize = 10
var extendSize = 10
var headIndex = 0
var tailIndex = 1

function NewDeque() {
  localDeque = Array()
  for(let i = 0; i < initSize; i++){
    localDeque[i] = null
  }
}

function extendDeque() {
  console.log('extend',headIndex,tailIndex)
  var newDeque = Array()
  tailIndex = localDeque.length
  for(let i = 0; i < localDeque.length + extendSize; i++){
    newDeque[i] = null
  }
  for(let i = 0; i < localDeque.length; i++){
    newDeque[i] = localDeque[headIndex]
    console.log(headIndex,newDeque[i])
    headIndex = (headIndex + 1) % localDeque.length
  }
  headIndex = 0
  localDeque = newDeque
}

function isFull() {
  if (headIndex === tailIndex) {
    return true
  }
  return false
}

function isEmpty() {
  if ((headIndex +1 + 2*localDeque.length) % localDeque.length === tailIndex) {
    return true
  }
  return false
}

function AddFirst(data) {
  if(isFull()){
    extendDeque()
  }
  localDeque[headIndex] = data
  headIndex = (headIndex - 1 + 2*localDeque.length) % localDeque.length
}

function AddLast(data) {
  if(isFull()){
    extendDeque()
  }
  localDeque[tailIndex] = data
  tailIndex = (tailIndex + 1) % localDeque.length
}

function RemoveFirst() {
  if(isEmpty()){
    return false
  }
  res = localDeque[headIndex = (headIndex + 1 )%localDeque.length]
  localDeque[headIndex] = null
  return res
}

function RemoveLast() {
  if(isEmpty()){
    return false
  }
  res = localDeque[tailIndex = (tailIndex - 1 + 2*localDeque.length) % localDeque.length]
  localDeque[tailIndex] = null
  return res
}

function Test(){
  NewDeque()
  for(let i = 0; i < 10; i++){
    AddFirst(i)
  }
  for(let i = 0; i < 10; i++){
    RemoveFirst()
  }
  for(let i = 0; i < 10; i++){
    AddLast(i)
  }
  for(let i = 0; i < 10; i++){
    RemoveLast()
  }
  return localDeque
}
```

### 2、分析 Queue 和 Priority Queue 的源码
  * 首先，Queue和Priority Queue在抽象思维上其实一模一样。Priority Queue有一个comparator，这个comparator可以根据自然顺序或者你自己实现的比较逻辑来比较两个节点的大小。而Queue可以理解为自然顺序就是入队的顺序，给每个入队的节点一个顺序编号，出队的时候编号小的优先出队。
  * 其次，两个Queue在实现上都不关心具体的节点内容，不管这个节点是整数、字母亦或者是对象，一视同仁都是一个抽象的节点，Queue只是对这个节点进行了一个排序存储。
  * 最后，可以将Priority Queue实现成一个Queue，只需要比较Queue入队的时间顺序即可。
