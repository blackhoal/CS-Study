# 삽입 정렬
```python
def insertion_sort(array) : 
    for i in range(1, len(array)) : 
        for j in range(i, 0, -1) : 
            if array[j-1] > array[j] : 
                array[j-1], array[j] = array[j], array[j-1]
                
    return array
```
- 정렬 범위를 1칸씩 확장하며 새롭게 범위에 들어온 값을 기존 값들과 비교하여 알맞은 자리에 보내는 정렬 알고리즘

# 선택 정렬
```python
def selection_sort(array) :
    for i in range(len(array)) : 
        min_idx = i
        for j in range(i+1, len(array)) : 
            if array[min_idx] > array[j] : 
                min_idx = j
        array[min_idx], array[i] = array[i], array[min_idx]
        
    return array
```
- 배열 내 가장 작은 값을 선택하여 앞으로 보내는 정렬 알고리즘

# 버블 정렬
```python
def bubble_sort(array) : 
    for i in range(len(array)-1, 0, -1) : 
        for j in range(i) : 
            if array[j] > array[j+1] : 
                array[j], array[j+1] = array[j+1], array[j] 
                
    return array
```
- 서로 인접한 값을 비교 후 자리를 교환하는 정렬 알고리즘
