---
layout: single
title: "Approximation"
date: 2021-05-20 16:25
categorise: jekyll update
---

### Approximation(근사 알고리즘)

#### problem 

> 작업스케줄링(job scheduling) : m개의 기계로 주어진 n개의 작업을 해결하는 데 걸리는 작업시간의 해를 구하여라
>
> greedy algorithm으로 구한 해와 brute force로 구한 해의 근사비율과 시간복잡도를 포함하도록. 



greedy로 구현한 코드는 다음과 같다. 

```java
import java.util.Scanner;

public class JobSch { //greedy algorithm

    public void getMaxValue(int machine[]) {
        int max = machine[0];

        for(int i = 0; i < 1; i++) {
            for(int j = i+1; j < machine.length; j++) {
                if(max < machine[j])
                    max = machine[j]; //더 큰 값을 max 에 저장
            }
        }
        System.out.print("기계에 할당되는 가장 긴 시간은 : "+ max);
    }

    public void JobScheduling(int m, int t[]) {
        int machine[] = new int[m];

        for(int i = 0; i < m; i++) {
            machine[i] = 0; //처음에 기계들에 할당된 시간 0
        }

        for(int i = 0; i < t.length; i++) {
            int least = 0; //min 에 해당하는 인덱스의 요소가 가장 작을 것이라 가정
            for(int j = 1; j < m; j++) {
                if(machine[least] > machine[j])
                    least = j; //더 작은 요소의 인덱스 값으로 min 값을 변경.
            }
            machine[least] = machine[least] + t[i]; // 할당된 시간이 적은 기계에 다음 작업 할당해줌.
        }

        getMaxValue(machine);
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in); //scanner 객체 생성
        System.out.print("기계개수: ");
        int m = scanner.nextInt();

        JobSch job = new JobSch();

        System.out.print("입력개수: ");
        int num = scanner.nextInt(); //할당될 작업의 개수

        int time[] = new int[num];
        System.out.print("작업시간을 입력하시오: ");
        for(int i = 0; i<time.length;i++) {
            time[i] = scanner.nextInt(); //할당된 각 작업들의 작업시간
        }

        job.JobScheduling(m, time);
    }
}
```



**구현한 함수** 

- **getMaxValue()** : 기계들에 할당된 작업 시간 중 가장 긴 작업시간 값을 구해내는 함수 

- **jobScheduling()** : 주어진 기계에 모든 작업을 분배함으로써 jobScheduling을 실질적으로 실행시키는 함수

  (이때 **가장 수행시간이 짧게 남은 기계를 기준으로 다음 작업을 할당**해준다. 최대한 작업시간은 10h)



- **getMaxValue()** 

```
public void getMaxValue(int machine[]) {
        int max = machine[0];

        for(int i = 0; i < 1; i++) {
            for(int j = i+1; j < machine.length; j++) {
                if(max < machine[j])
                    max = machine[j]; //더 큰 값을 max 에 저장
            }
        } 
        System.out.print("기계에 할당되는 가장 긴 시간은 : "+ max);
}
```

: 각 기계에 할당된 전체적인 시간값을 가지고 있는 배열(machine)을 매개변수로 받아온다

: 배열의 첫 번째 요소를 max의 값으로 초기화

: for,if문을 통해 배열의 모든 요소들과 비교하면서 배열 요소들 중 가장 큰 값을 찾아 max값을 출력시킨다. 



- **jobScheduling()**

```
public void JobScheduling(int m, int t[]) {
        int machine[] = new int[m];

        for(int i = 0; i < m; i++) {
            machine[i] = 0; //처음에 기계들에 할당된 시간 0
        }

        for(int i = 0; i < t.length; i++) {
            int least = 0; //least 에 해당하는 인덱스의 요소가 가장 작을 것이라 가정
            for(int j = 1; j < m; j++) {
                if(machine[least] > machine[j])
                    least = j; //더 작은 요소의 인덱스 값으로 least 값을 변경.
            }
            machine[least] = machine[least] + t[i]; // 할당된 시간이 적은 기계에 다음 작업 할당해줌.
        }

        getMaxValue(machine);
}
```

: 기계들에 할당된 시간을 저장할 배열 machine의 값을 모두 0으로 초기화

: 배열의 요소에 하나씩 접근하면서 **least가 인덱스 번호인 요소와 현재 j가 인덱스 번호인 요소를 비교**

: least보다 j에 해당하는 요소의 값이 더 작으면  least값을 j의 값으로 갱신. 

(이를 반복하여 가장 할당된 시간이 적은 인덱스번호를 찾아낸다.)

: 가장 작은 값을 가진 인덱스 요소에 다음 실행할 작업의 시간을 더해준다.

**(가장 작은 값을 가졌다는 것 == 가장 빨리 작업이 끝난다는 것)**



- 시간복잡도

: 주어진 n개의 작업을 기계에 할당시킨다(가장 빨리 끝나는 기계 찾기) -> **n(바깥 쪽 for문) x O(m-1) (안쪽 for문)**

: 할당되었던 값이 가장 큰 요소를 찾는다(배열 machine요소들에 한 번씩 접근) -> **O(m)**

=> **n x O(m) + O(m) = O(nm)**



시간복잡도는 n이 증가함에 따라 다음과 같이 나타나게 된다. 

![time](https://user-images.githubusercontent.com/80511175/118948130-f6170680-b992-11eb-9970-bf91f1de4561.png)



<p>brute force로 해 구하기</p>

<image src= "https://user-images.githubusercontent.com/80511175/118942814-07114900-b98e-11eb-942f-f565ee004492.png" alt= "appro1">

<image src="https://user-images.githubusercontent.com/80511175/118942825-0973a300-b98e-11eb-81c6-48291fe6433d.png" alt="appro2">



<p>( n = 4,5일 경우는 그림으로 나타낸 case가 기계 A와 B가 서로 바뀐 경우로도 존재 )</p>

<image src="https://user-images.githubusercontent.com/80511175/118942850-10021a80-b98e-11eb-9901-96c8a90a6093.png" alt="appro3">

<image src="https://user-images.githubusercontent.com/80511175/118942871-15f7fb80-b98e-11eb-8036-c1d4eac95a83.png" alt="appro4">

<image src="https://user-images.githubusercontent.com/80511175/118942893-1abcaf80-b98e-11eb-939e-e357058583ef.png" alt="appro5">

<image src="https://user-images.githubusercontent.com/80511175/118942916-1e503680-b98e-11eb-97c5-8ba4b7147ad3.png" alt="appro6">

<p>n = 2 )  7 / 7 <= 2  (근사비율 = 1)</p>

<p>n = 3 )  9 / 8 <= 2  (근사비율 = 1.125)

<p>n = 4 ) 11 / 10 <= 2  (근사비율 = 1.1)

<p>n = 5 ) 14 / 13 <= 2  (근사비율 = 1.07)

