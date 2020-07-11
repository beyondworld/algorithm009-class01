# 学习笔记

## 冒泡排序
## 快速排序
## 简单插入排序
## 希尔排序
## 简单选择排序
## 堆排序
## 二路归并排序
## 多路归并排序
## 计数排序
````
// 计数排序
func ContSort(data []int) []int {
	var temp = make([]int, 1000)
	var result = []int{}
	for _, item := range data {
		temp[item] = temp[item] + 1
	}
	// fmt.Println(temp)
	for key, val := range temp {
		for i := val; i > 0; i-- {
			result = append(result, key)
		}
	}
	return result
}
````
## 桶排序
````
// 桶排序
func BuckSort(data []int) []int {
	var buck = make(map[int]([]int))
	var result = []int{}
	for _, item := range data {
		var index = item % 10
		buck[index] = append(buck[index], item)
	}
	// fmt.Println(buck)
	for i := 0; i < 10; i++ {
		sortedItem := ContSort(buck[i])
		for _, k := range sortedItem {
			result = append(result, k)
		}
	}
	return result
}
````
## 基数排序
````
// 基数排序
func BaseSort(data []int) []int {
	for index := 0; index <= 1; index++ {
		for i := 0; i < len(data); i++ {
			for j := 0; j < len(data)-1-i; j++ {
				if BaseCmp(data[i], data[j], index) {
					var temp = data[i]
					data[i] = data[j]
					data[j] = temp
				}
			}
		}
		fmt.Println(data)
	}
	return data
}

func BaseCmp(a, b int, index int) bool {
	// fmt.Println("before", a, b, index)
	for i := 0; i < index; i++ {
		a = a / 10
		b = b / 10
	}
	//fmt.Println("after", a, b, index)
	if (a % 10) > (b % 10) {
		return true
	} else {
		return false
	}
}
````