---
layout: post
title: "유클리드 알고리즘 Euclidean algorithm"
date: 2024-03-23 10:00:00
description: "유클리드 알고리즘이란?"
tags: euclidean algorithm
categories: algorithm
thumbnail: assets/img/post240323/thumbnail.jpg
featured: false
comments: true
---

유클리드 알고리즘은 두 정수의 최대공약수(Greatest Common Divisor, GCD)를 찾기 위한 고대 알고리즘이다. 이 방법은 고대 그리스의 수학자로, 기하학 분야에 큰 업적을 남긴 유클리드에 의해 고안되었으며, 매우 효율적이어서 컴퓨터 프로그래밍 분야에서 널리 활용되고 있다.

### #유클리드 알고리즘의 단계
1. 두 수 a와 b를 준비(a > b인 경우가 일반적)
2. a를 b로 나눈 나머지를 구한다. 나머지를 r이라 한다.
3. 만약 r이 0이면, b가 최대 공약수가 된다.
4. r이 0이 아니면, a에 b를 대입하고, b에는 r을 대입한 후, 2단계로 돌아간다.

이 알고리즘은 나머지를 계속해서 새로운 분모로 사용하는 과정을 반복함으로써 작동한다.
나머지가 0이 되는 순간이 오는데, 이 때의 분모가 두 수의 최대공약수가 된다.

### #예시
```c++
// 최대공약수를 계산하는 함수
int gcd(int a, int b) {
	int r = a % b;

    while (r != 0) {
		a = b;
        b = r;
		r = a % b;
    }

    return b;
}
```
