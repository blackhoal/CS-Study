# 퀵 정렬
```python
def quick_sort(array) : 
    if len(array) <= 1 : 
        return array
    
    pivot = array[0]
    tail = array[1:]
    
    left = [x for x in tail if x <= pivot]
    right = [x for x in tail if x > pivot]
    
    return quick_sort(left) + [pivot] + quick_sort(right)
```
- 분할 정복(divide and conquer) 기법을 통해 주어진 배열을 정렬하는 알고리즘
    - 분할 정복 : 문제를 작은 2개의 문제로 분리하고 해결한 후 결과를 모아서 원래의 문제를 해결하는 전략
- 배열을 비균등하게 분할

<br>

# 병합 정렬
```python
def merge(left, right) : 
    sorted_list = []
    i, j = 0, 0
    
    while i < len(left) and j < len(right) : 
        if left[i] <= right[j] : 
            sorted_list.append(left[i])
            i += 1
        else : 
            sorted_list.append(right[j])
            j += 1
            
    while i < len(left) : 
        sorted_list.append(left[i])
        i += 1
        
    while j < len(right) : 
        sorted_list.append(right[j])
        j += 1
        
    return sorted_list

def merge_sort(array) : 
    if len(array) <= 1 : 
        return array
    
    mid = len(array) // 2
    left = merge_sort(array[:mid])
    right = merge_sort(array[mid:])
    
    return merge(left, right)
```
- 분할 정복 (Devide and Conquer) 기법과 재귀 알고리즘을 이용한 정렬 알고리즘
