---
layout: post
title:  "C++ :: Learning C++ by following"
date:   2019-04-26
categories: C++
permalink: FollowCpp
tags: C++

# author
author: Kipoong Kim
---

## 

<!-- more -->

## 따라하며 배우는 C++

<https://www.youtube.com/channel/UCg6IlhycdYiK_nWB3spjIqA/playlists>

## 프로그래밍 과정

1. Define: 풀어야 할 문제를 정의한다.

2. Design: 해법을 설계한다.

3. Write: 해법을 구현하는 프로그램을 작성한다.

4. Compile: 프로그램을 컴파일한다.

   - g++ -c file1.cpp file2.cpp file3.cpp : 소스파일(.cpp) --> compile --> object file(.o)

5. Linking: 오브젝트 파일들을 링킹한다.

   - object1, object2, object3 --> Linker (Runtime Support[^footnote]) --> Executable file

     [^footnote]: 오픈소스를 사용하기 위함. 

6. Debuging: 테스트해보고 문제가 있으면 고친다.



## 연산자 (operator)

```C++
#include <iostream>

using namespace std;

int main()
{
    int x = 2; // x is a variable, 2 is a literal
    // "="는 assignment, "=="가 eqaul to
    
    cout << x + 2 << endl; // -, + : 연산자, 2 : 피연산자; -2에서 (-) 역시 연산자임
    int y = (x > 0) ? 1 : 2; // 3항 연산자
    // ( 조건 ) 참이면 : 왼쪽을 진행, 거짓이면 : 오른쪽을 진행
    
    cout << "My Home" << endl;
    cout << y << endl;
    
    return 0;
}
```



## Header

- 목적: 불러오고자 하는 함수의 길이가 너무 길어서 main 함수에 포함시키기 어려우므로, 헤더파일을 만들어 불러옴으로써 가독성을 높이기 위함.

- 사용방법: 

  1. Create header file ".h"

  2. Calling the function with #include "function.h"

  3. If you want to group many of header files into a folder, 
     include the folder name as "Folder / function.h"

     ㅁㄴ

  4. 기본 라이브러리(iostream 등)는 compiler 설치할 때 같이 설치되므로 #include <iostream> 만으로도 불러와짐.

- 기본 라이브러리(iostream 등)는 compiler 설치할 때 같이 설치되므로 #include <iostream> 만으로도 불러와짐.

- 익숙해지면 헤더파일을 먼저 만들어서 함수를 만들고 main에 추가함.
  cpp 파일에는 바디, 헤더파일에는 헤더로 구분하는 것이 좋음.

## Header guard

- 목적: 헤더가드가 있으면 헤더에 바디가 있어도 문제가 되지 않기 때문에 사용함.
  또는, 헤더가 중복으로 calling되는 경우에 에러가 발생하므로 그런 경우를 처리함.
- For example,

```C++
//#pragma once :: ifndef define endif와 같으므로 pragma once만 쓰면 됨

#ifndef MY_ADD // MY_ADD라는게 이미 정의되어있거나 include되어있으면 define/include 하지마라
#define MY_ADD

int add(int a, int b)
{
    return a + b;
}

#endif
```



## Namespace

- 네임스페이스는 
  어떤 함수를 하나 만들었습니다.
  어떻게 하다보니, 이름도 같고 parameter도 같으나 결과값이 다른 함수가 생길 수가 있다.
  그러면 에러가 날 것이다.
  경우에 따라 두 함수의 이름을 유지하고자 할 것이다.
  그러면 아래와 같이 한다.

  ```C++
  #include <iostream>
  
  namespace MySpace1
  {
      int doSomething(int a, int b)
      {
          return a + b;
      }
  }
  
  //namespace MySpace2
  //{
  int doSomething(int a, int b)
  {
      return a * b;
  }
  //}
  
  using namespace std;
  
  int main()
  {
      cout << MySpace::doSomething(3, 4) << endl;
      cout << doSomething(3, 4) << endl;
      
      return 0;
  }
  ```

  - 매번 "MySpace::"로 namespace를 불러오기 번거롭다.
    그런 경우 `using namespace MySpace`로 간편하게 사용할 수 있다.

  - namespace 안에 또다른 namespace를 넣을 수 있다.

    `MySpace::InnerSpace::my_function();` 
    마찬가지로 `using namespace MySpace::InnerSpace`로 간편하게 사용 가능하다.

    

  

  ## Preprocessor

  ```c++
  #include <iostream>
  using namespace std;
  
  #define MY_NUMBER 9 // #define MY_NUMBER "Hello, World!"
  // 매크로라고도 부르는데 대문자로 사용함; 코드에서 MY_NUMBER 값을 9로 대체함
  
  #define MAX(a, b) ( ( (a) > (b) ) ? (a) : (b) )
  // a에 1+2가 들어올 수 있으므로 모든 변수에 괄호를 씌우는 것이 좋다.
  // 요즘은 매크로 많이 사용하지 않음
  // 참고로 algorithm에서 std::max가 있다.
  
  int main()
  {
      cout << MY_NUBMER << endl;
      cout << MAX(1, 2) << endl;
      return 0;
  }
  ```

  





```C++
#include <iostream>
using namespace std;

#define LIKE_APPLE // 해당 파일안에서만 효력범위가 해당된다.

int main()
{
#ifdef LIKE_APPLE // 전처리기; 전처리기 안에서는 매크로로 작동 안함. LIKE_APPLE가 정의되어 있는 경우
    cout << "Apple " << endl;
// #endif
#else    
// #ifndef LIKE_APPLE // LIKE_APPLE가 정의되지 않은 경우
    cout << "Orange " << endl;
#endif
    return 0;
}

// 많이 쓰는 경우는 리눅스인지, 윈도우인지에 따라 다른 함수를 적용하고자 하는 경우.
```

