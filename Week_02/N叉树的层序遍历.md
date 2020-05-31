# 429. N叉树的层序遍历
*解题思路*
使用队列或者使用两个堆栈模拟队列  
下面代码使用两个堆栈模拟了队列  
firstStack是当前需要处理的堆栈，要将堆栈中的每个节点弹出，访问该节点，并且将子节点压入到secondStack中  
secondStack是子节点，当firstStack处理完毕后，则将secondStack中的值放到firstStack中，清空secondStack，如此往复  

````
/**
 * Definition for a Node.
 * type Node struct {
 *     Val int
 *     Children []*Node
 * }
 */

func levelOrder(root *Node) [][]int {
    if root == nil {
        return nil
    }
	var result = [][]int{}
	var firstLevel = []*Node{}
	var secondLevel = []*Node{}
	firstLevel = append(firstLevel, root)
	for {
		if len(firstLevel) == 0 && len(secondLevel) == 0 {
			break
		}

		if len(firstLevel) == 0 {
			firstLevel = secondLevel
			secondLevel = []*Node{}
		}
        temp := []int{}
        for _, first := range firstLevel {
            if first != nil {
                temp = append(temp, first.Val)
                for _, second := range first.Children {
                    if second != nil {
                        secondLevel = append(secondLevel, second)
                    }
                }
            }
        }
		firstLevel = []*Node{}
        result = append(result, temp)
	}
	return result
}
````