# 1. 삽입 정렬(Insertion Sort)
![6-3](https://user-images.githubusercontent.com/48504392/128068486-0f1b8d23-79d2-4291-9567-fce2ed798269.png)
```python
def insertion_sort(array) : 
    for i in range(1, len(array)) : 
        for j in range(i, 0, -1) : 
            if array[j-1] > array[j] : 
                array[j-1], array[j] = array[j], array[j-1]
                
    return array
```
- 정렬 범위를 1칸씩 확장하며 새롭게 범위에 들어온 값을 기존 값들과 비교하여 알맞은 자리에 보내는 정렬 알고리즘

<br>

# 2. 선택 정렬(Selection Sort)
![6-4](https://user-images.githubusercontent.com/48504392/128068489-d9c6835a-62a2-46a9-919f-b5e53417e026.png)
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

<br>

# 3. 거품 정렬(Bubble Sort)
![6-1](https://user-images.githubusercontent.com/48504392/128068477-f3f458aa-2e6c-4a4c-a389-2af02976affe.png)
![6-2](https://user-images.githubusercontent.com/48504392/128068484-0e522dc6-4f86-4736-9d17-8d67467b08a0.png)
```python
def bubble_sort(array) : 
    for i in range(len(array)-1, 0, -1) : 
        for j in range(i) : 
            if array[j] > array[j+1] : 
                array[j], array[j+1] = array[j+1], array[j] 
                
    return array
```
- 서로 인접한 값을 비교 후 자리를 교환하는 정렬 알고리즘







 
