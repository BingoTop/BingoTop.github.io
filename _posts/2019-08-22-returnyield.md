---
layout: post
title: return과 yield
categories: Python
comments: true
---

# return과 yield를 비교해보자


보통 함수들을 반환할 때 return을 자주 쓰는데요, return 말고도 강력한 메소드가 있습니다.


### __return__ 문
~~~python

def function():
  a,b = 0,1
  while True:
    return b
    a,b = b, a + b

print(function())

'''결과값'''
1
~~~

__return__ 은 키워드는 반환 값을 반환하고 함수를 종료합니다.

하지만 __yield__ 는 코드 실행 중에 예외가 발생할 까지 값을 반환합니다.

### __yield__ 문



~~~python
def function1():
  a,b = 0,1

  while True:
    yield b # yield를 통해 generator로 만들 수 있다.
    a,b = b, a + b

test_fib = function1()
print(next(test_fib)) # 1
print(next(test_fib)) # 1
print(next(test_fib)) # 2
print(next(test_fib)) # 3

'''결과값'''
1
1
2
3
~~~

__yield__ 를 사용하면 값을 함수 바깥으로 전달하면서 코드 실행을 함수 바깥에 양보합니다.
따라서 __yield__ 는 현재 함수를 잠시 중단하고 함수 바깥의 코드가 실행되도록 만듭니다.
next로 다음 값들을 출력할 수 있음을 알 수 있습니다.


---
__참고__
[파이썬 코딩도장](https://dojang.io/mod/page/view.php?id=2412)
