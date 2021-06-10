# 체육복 / 탐욕법 / Level 1
## ① 문제
[링크](https://programmers.co.kr/learn/courses/30/lessons/42862)
## ② 구현
```python
def solution(n, lost, reserve):
    for i in reserve :
        if i in lost :
            lost.remove(i)
            reserve.remove(i)
            
    answer = n - len(lost)
    for i in lost :
        if i in reserve :
            reserve.remove(i)
            answer += 1
        elif i-1 in reserve :
            reserve.remove(i-1)
            answer += 1
        elif i+1 in reserve :
            reserve.remove(i+1)
            answer += 1
    
    return answer
```
```java
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

class Solution {
    public int solution(int n, int[] lost, int[] reserve) {
        List<Integer> lost_list = new ArrayList<Integer>();
		for (int i : lost) {
			lost_list.add(i);
		}
		
		List<Integer> reserve_list = new ArrayList<Integer>();
		for (int i : reserve) {
			reserve_list.add(i);
		}
		
		for (Integer i : reserve) {
			// Arrays.asList(lost_list).contains(i)
			if (lost_list.contains(i)) {
				lost_list.remove(i);
				reserve_list.remove(i);
			}
		}
		
		int answer = n - lost_list.size();
		
		for (Integer i : lost_list) {
			if (reserve_list.contains(i)) {
				int index = reserve_list.indexOf(i);
				reserve_list.remove(index);
				answer += 1;
			}
			else if (reserve_list.contains(i - 1)) {
				int index = reserve_list.indexOf(i - 1);
				reserve_list.remove(index);
				answer += 1;
			}
			else if (reserve_list.contains(i + 1)) {
				int index = reserve_list.indexOf(i + 1);
				reserve_list.remove(index);
				answer += 1;
			}
		}
        return answer;
    }
}
```
## ③ 결과
|Python|Result|
|:--:|:--:|
|테스트 01|통과 (0.01ms, 10.2MB)|
|테스트 02|통과 (0.01ms, 10.3MB)|
|테스트 03|통과 (0.01ms, 10.2MB)|
|테스트 04|통과 (0.01ms, 10.3MB)|
|테스트 05|통과 (0.01ms, 10.2MB)|
|테스트 06|통과 (0.01ms, 10.2MB)|
|테스트 07|통과 (0.02ms, 10.1MB)|
|테스트 08|통과 (0.01ms, 10.2MB)|
|테스트 09|통과 (0.00ms, 10.4MB)|
|테스트 10|통과 (0.01ms, 10.2MB)|
|테스트 11|통과 (0.01ms, 10.3MB)|
|테스트 12|통과 (0.00ms, 10.2MB)|

|Java|Result|
|:--:|:--:|
|테스트 01|통과 (0.08ms, 51.7MB)|
|테스트 02|통과 (0.13ms, 53.1MB)|
|테스트 03|통과 (0.11ms, 52.2MB)|
|테스트 04|통과 (0.08ms, 52.4MB)|
|테스트 05|통과 (0.12ms, 52.9MB)|
|테스트 06|통과 (0.10ms, 51.8MB)|
|테스트 07|통과 (0.16ms, 52.2MB)|
|테스트 08|통과 (0.23ms, 53.4MB)|
|테스트 09|통과 (0.07ms, 52.6MB)|
|테스트 10|통과 (0.17ms, 52.8MB)|
|테스트 11|통과 (0.08ms, 52MB)|
|테스트 12|통과 (0.05ms, 52.1MB)|
#

# 모의고사 / 완전탐색 / Level 1
## ① 문제
[링크](https://programmers.co.kr/learn/courses/30/lessons/42840)
## ② 구현
```python
def solution(answers):
    a = [1,2,3,4,5]
    b = [2,1,2,3,2,4,2,5]
    c = [3,3,1,1,2,2,4,4,5,5]
    cnt = [0,0,0]

    for i in range(len(answers)) :
        if answers[i] == a[i%5] :
            cnt[0] += 1
        if answers[i] == b[i%8] :
            cnt[1] += 1
        if answers[i] == c[i%10] :
            cnt[2] += 1

    answer = []
    for i in range(3) :
        if cnt[i] == max(cnt) :
            answer.append(i+1)
    return answer
```
```java
import java.util.ArrayList;
import java.util.List;

class Solution {
    public int[] solution(int[] answers) {
        int[] a = {1,2,3,4,5};
	    int[] b = {2,1,2,3,2,4,2,5};
	    int[] c = {3,3,1,1,2,2,4,4,5,5};
	    int[] cnt = {0,0,0};
	    
	    for (int i = 0; i < answers.length; i++) {
	    	if (answers[i] == a[i % 5]) {
	    		cnt[0] += 1;
	    	}
	    	if (answers[i] == b[i % 8]) {
	    		cnt[1] += 1;
	    	}
	    	if (answers[i] == c[i % 10]) {
	    		cnt[2] += 1;
	    	}
	    }
	    
	    int max_value = 0;
	    for (int i : cnt) {
			if (max_value < i) {
				max_value = i;
			}
		}
	    
	    List<Integer> list = new ArrayList<Integer>();
	    for (int i = 0; i < 3; i++) {
			if (cnt[i] == max_value) {
				list.add(i + 1);
			}
		}
	    int[] answer = list.stream().mapToInt(i->i).toArray();
        
        return answer;
    }
}
```
## ③ 결과
|Python|Result|
|:--:|:--:|
|테스트 01|통과 (0.01ms, 10.2MB)|
|테스트 02|통과 (0.01ms, 10.3MB)|
|테스트 03|통과 (0.01ms, 10.3MB)|
|테스트 04|통과 (0.01ms, 10.2MB)|
|테스트 05|통과 (0.03ms, 10.3MB)|
|테스트 06|통과 (0.03ms, 10.2MB)|
|테스트 07|통과 (2.93ms, 10.4MB)|
|테스트 08|통과 (0.70ms, 10.3MB)|
|테스트 09|통과 (4.62ms, 10.3MB)|
|테스트 10|통과 (1.50ms, 10.1MB)|
|테스트 11|통과 (2.74ms, 10.3MB)|
|테스트 12|통과 (2.52ms, 10.1MB)|
|테스트 13|통과 (0.20ms, 10.2MB)|
|테스트 14|통과 (2.75ms, 10.2MB)|

|Java|Result|
|:--:|:--:|
|테스트 01|통과 (8.93ms, 52.1MB)|
|테스트 02|통과 (6.78ms, 52.4MB)|
|테스트 03|통과 (15.61ms, 52.6MB)|
|테스트 04|통과 (2.70ms, 52.7MB)|
|테스트 05|통과 (2.56ms, 51.5MB)|
|테스트 06|통과 (2.63ms, 53.4MB)|
|테스트 07|통과 (3.12ms, 52.7MB)|
|테스트 08|통과 (5.20ms, 55.1MB)|
|테스트 09|통과 (5.21ms, 52.8MB)|
|테스트 10|통과 (3.55ms, 52.1MB)|
|테스트 11|통과 (5.84ms, 52.3MB)|
|테스트 12|통과 (3.29ms, 52.4MB)|
|테스트 13|통과 (2.83ms, 53.5MB)|
|테스트 14|통과 (4.81ms, 53.2MB)|
#

