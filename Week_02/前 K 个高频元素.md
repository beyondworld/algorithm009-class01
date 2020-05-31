# 347. 前 K 个高频元素
解题思路  

求解频率，求解前k个数，典型的大根堆或者小根堆解决。  
堆中节点在比较时，其比较的值可以不存放在堆中，只需要能够比较两个节点的顺序即可。  
因此本解法中使用numsMap结构存放两个节点之间的大小值关系，在比较的时候使用。  
而在堆中则存放的时节点的下标索引，不具有比较大小的功能。  

- 节点插入堆算法
  - 将节点插入到堆中最后位置
  - 将节点与父亲节点比较，如果有大小之分则调整位置
  - 如果没有则插入完毕
- 删除堆顶节点算法
  - 将堆顶节点删除
  - 将堆最后一个节点放到堆顶位置
  - 从堆顶向下调整，如果当前节点和子节点中的最大或者最小值有分别，则调整
  - 若无分别，不需要调整，则调整结束

````
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {number[]}
 */
var topKFrequent = function(nums, k) {
    let numsMap = Array()
    let numsExist = Array()
    let myHeap = Array()

    for(let i = 0; i < nums.length; i++) {
        if( numsMap[nums[i]] === null || numsMap[nums[i]] === undefined) {
            numsMap[nums[i]] = 1
            numsExist.push(nums[i])
        } else {
            numsMap[nums[i]] = numsMap[nums[i]] + 1
        }
    }
    for(let i = 0; i < numsExist.length; i++) {
        myHeap.push(numsExist[i])
        heapfiyUp(myHeap, numsMap)
    }

    //console.log(myHeap, numsMap)
    let result = Array()
    for(let i = 0; i < k; i++){
        result.push(heapfiyDown(myHeap, numsMap))
    }
    return result
};

function heapfiyUp(myHeap, numsMap) {
    // console.log('before', myHeap, numsMap)
    let index = myHeap.length
    for(;;) {
        if (index <= 0) {
            break
        }
        let parent = parseInt(index / 2)
        // console.log('beforeWap index', index, myHeap[index-1], numsMap[myHeap[index-1]])
        // console.log('beforeWap parent', parent, myHeap[parent-1], numsMap[myHeap[parent-1]])
        if ( parent > 0 && numsMap[myHeap[parent - 1]] < numsMap[myHeap[index - 1]]) {
            // console.log('swap', myHeap, numsMap)
            let temp = myHeap[parent - 1]
            myHeap[parent - 1] = myHeap[index - 1]
            myHeap[index - 1] = temp
            index = parent
            continue
        } else {
            break
        }
    }
    // console.log('after', myHeap, numsMap)
}

function heapfiyDown(myHeap, numsMap) {
    //console.log('before', myHeap, numsMap)
    let value = myHeap[0]
    let last = myHeap.pop()
    if( last === null || last === undefined) {
        return value
    }

    myHeap[0] = last
    //console.log('myHeap', myHeap)
    let index = 1
    for(;;) {
        //console.log('beforeSwap parent', index, myHeap.length, myHeap[index-1], numsMap[myHeap[index-1]])
        //console.log('beforeSwap right children', 2*index, myHeap.length, myHeap[2*index], numsMap[myHeap[2*index]])
        //console.log('beforeSwap left children', 2*index - 1, myHeap.length, myHeap[2*index - 1], numsMap[myHeap[2*index - 1]])
        //console.log('condition',2*index,myHeap.length,numsMap[myHeap[2*index]],numsMap[myHeap[2*index - 1]],numsMap[myHeap[index - 1]],numsMap[myHeap[2*index]])
        if (2*index < myHeap.length && numsMap[myHeap[2*index]] > numsMap[myHeap[2*index - 1]] && numsMap[myHeap[index - 1]] < numsMap[myHeap[2*index]]) {
            let temp = myHeap[index - 1]
            myHeap[index - 1] = myHeap[2*index]
            myHeap[2*index] = temp
            index = 2*index + 1
            //console.log('swap a', myHeap[index - 1], myHeap[2*index])
        } else if (2*index - 1 < myHeap.length && numsMap[myHeap[index - 1]] < numsMap[myHeap[2*index - 1]]) {
            let temp = myHeap[index - 1]
            myHeap[index - 1] = myHeap[2*index - 1]
            myHeap[2*index - 1] = temp
            //console.log('swap b', myHeap[index - 1], myHeap[2*index - 1])
            index = 2*index
        } else {
            break
        }
    }
    //console.log('after', myHeap, numsMap)
    return value
}
````