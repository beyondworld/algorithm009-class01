# 学习笔记
### 分析GC递归处理算法

标记清除方式  
代码在书写过程中生成对象，对象在内存中存放，如果有其他对象引用该对象，则在该对象的标记上+1，如果取消对该对象对应用，则标记-1.
当不断申请，释放对象时会有两部分内容。一个是从root开始对树形结构，树中的每个节点都有被引用。剩下的是游离节点，没有被引用。  
在进行垃圾回收的时候直接从root节点遍历，没有被访问的所有节点都会被清除。 
以下代码是模拟gc过程

````
 function TreeNode() {
    this.flag = 0
    this.mmIndex = null
    this.children = Array()
  }

  let myMemoryLength = 100
  let myMemory = Array()
  let myRoot = new TreeNode()

  function Allocate(root) {
    for(let i = 0; i < myMemoryLength; i++){
      if(myMemory[i] === undefined || myMemory[i] === null) {
        let newNode = new TreeNode(0)
        newNode.flag = newNode.flag + 1
        root.children.push(newNode)
        newNode.mmIndex = i
        myMemory[i] = newNode
        return newNode
      }
    }
    return null
  }

  function Free(node) {
    if (node) {
      node.flag = node.flag - 1
    }
  }

  function GC(root) {
    for(let i = 0; i < root.children.length; i++){
      if(root.children[i]) {
        if (root.children[i].flag === 0) {
          GCAll(root.children[i])
          myMemory[root.children[i].mmIndex] = null
          root.children[i] = null
        }
      }
    }
  }
  
  function GCAll(root) {
    for(let i = 0; i < root.children; i++) {
      myMemory[root.children[i].mmIndex] = null
      GCAll(root.children[i])
    }
  }

  function test() {
    for(let i = 0; i < 200; i++ ){
      let t = Allocate(myRoot)
      if ( t === null ) {
        console.log('allocate failed,', i ,t)
      }
      Free(t)
      GC(myRoot)
    }
  }
````