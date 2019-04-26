---
layout: post
title:  "C++ :: Introduction to C++"
date:   2019-04-26
categories: C++
permalink: Introduction_to_Cpp
tags: C++

# author
author: Kipoong Kim
---

## 동빈나 실전 알고리즘 강좌

<!-- more -->

<https://www.youtube.com/channel/UChflhu32f5EUHlY7_SetNWw/featured>

## 정렬 알고리즘

### 선택 정렬

- 알고리즘은 먼저 손으로 실습한 뒤에 프로그래밍하는 것이 좋다.
- Problem: 	1	10	5	8	7	6	4	3	2	9
- 선택정렬방법: " 가장 작은 것을 선택해서 제일 앞으로 보낸다! "
- 시간 복잡도 (big-O): N*(N-1)/2 --> O(N^2)
- 구현코드

```c++
#include <stdio.h>

int main(void) {
    int i, j, min, index, temp;
    int array[10] = {1, 10, 5, 8, 7, 6, 4, 3, 2, 9};
    for(i = 0; i < 10; i++){
        min = 9999;
        for(j=i; j<10; j++){
            if(min > array[j]) {
                min = array[j];
                index = j;
            }
        }
        temp = array[i];
        array[i] = array[index];
        array[index] = temp;
    }
    
    for(i=0; i<10; i++){
        printf("%d ", array[i]);
    }
    
    return 0;
}
```

- 강의 영상

  <iframe width="560" height="315" src="https://www.youtube.com/embed/8ZiSzteFRYc?start=8" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

  ## 

### 버블 정렬

- " 두 원소쌍을 비교하여 작은 것을 앞으로 보낸다! "
- 가장 큰 값이 맨뒤로 보내짐.
- 시간복잡도 = O(N^2)
- 그러나 매번 원소의 교체가 이루어지므로 실제로는 <u>버블정렬이 선택정렬보다 훨씬 느리다.</u>
- **정렬 알고리즘 중에서 가장 느린 방법**
- 구현코드

```c++
#include <stdio.h>

int main(void) {
    int i, j, temp;
    int array[10] = {10, 5, 8, 7, 6, 4, 3, 2, 9, 11};
    for(i = 0; i < 10; i++)
    {
        for(j=0; j<9-i; j++)
        {
            if(array[j] > array[j+1]) {
                temp = array[j];
                array[j] = array[j+1];
                array[j+1] = temp;
            }
        }
    }

    for(i=0; i<10; i++){
        printf("%d ", array[i]);
    }

    return 0;
}
```

- 강의 영상

  <iframe width="560" height="315" src="https://www.youtube.com/embed/EZN0Irp2aPs?start=8" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

  

### 삽입정렬

- " 각 숫자를 적절한 위치에 삽입하는 방법! "
- 필요할 때만 위치를 바꾸기 때문에 *선택정렬*과 *버블정렬*보다 빠르다.
- 시간 복잡도 = O(N^2)
- 구현코드

```c++
#include <stdio.h>

int main(void) {
    int i, j, temp;
    int array[10] = {10, 8, 7, 5, 6, 4, 3, 2, 9, 11};

    for(i=0; i<9; i++){
        j = i;
        while( j>=0 && array[j] > array[j+1] ){
            temp = array[j];
            array[j] = array[j+1];
            array[j+1] = temp;
            j--;
            // if(j < 0){ break; }
        }
    }

    for(i=0; i<10; i++){
        printf("%d ", array[i]);
    }
    
    return 0;
}
```

- 강의 영상

  <iframe width="560" height="315" src="https://www.youtube.com/embed/16I9Z7bS1iM?start=8" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

  

### 퀵정렬

- " 특정한 값(<u>피벗</u>)을 기준으로 큰 숫자와 작은 숫자로 나누면 어떨까? "
- 가장 앞에 있는 값을 피벗값으로 많이 선택한다.
- 이미 정렬이 잘 되어있는 경우, 최대 O(N^2) 시간 복잡도를 갖는다.
- 시간 복잡도 = O(N*logN)
- 구현코드

```C++
#include <stdio.h>
int number = 10;
int data[10] = {1, 10, 5, 8, 7, 6, 4, 3, 2, 9};

void QuickSort(int *data, int start, int end) {
    if (start >= end) { // 원소가 1개인 경우
        return;
    }

    int key = start; // 키는 첫번째 원소
    int i = start + 1;
    int j = end;
    int temp;

    while( i <= j ) { // 엇갈릴 때까지 반복
        while( data[i] <= data[key] ){ // 키 값보다 큰 값을 만날 때까지
            i++;
        }
        while( data[j] >= data[key] && j > start ){ // 키 값보다 작은 값을 만날 때까지
            j--;
        }
        if( i > j ) { // 엇갈린 경우
            temp = data[j];
            data[j] = data[key];
            data[key] = temp;
        } else {
            temp = data[j];
            data[j] = data[i];
            data[i] = temp;
        }
    }

    QuickSort(data, start, j-1);
    QuickSort(data, j+1, end);
}

int main(void) {
    QuickSort(data, 0, number - 1);
    for( int i=0; i<number; i++ ){
        printf("%d", data[i]);
    }
}
```

- 강의 영상 (I)

  <iframe width="560" height="315" src="https://www.youtube.com/embed/O-O-90zX-U4?start=8" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

- 강의 영상 (II)

  <iframe width="560" height="315" src="https://www.youtube.com/embed/gBcUO_6JXIA?start=8" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

  