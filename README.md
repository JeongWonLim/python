# Python
파이썬 공부

## datetime 패키지
```python
import datetime #패키지 파일 
```

## 스페셜 메소드(Special method)
특정 상황에서 자동 호출 되는 미리 약속되어 있는 메소드.

## \_\_init\_\_(self, ...)
파이썬 클래스의 생성자, 객체를 생성할 때 초기 설정 값들을 설정한다.

```python
class init:
    def __init__(self):
        print('생성자입니다.)

cla = init()

#출력
생성자입니다.
```

## \_\_call\_\_(self,...)
객체는 함수가 아니다. 하지만 파이썬에서 제공하는 스페셜 메소드 \_\_call\_\_를 사용하면 객체를 함수처럼 사용할 수 있다.
```python
class cal:
    def __init__(self, a):
        self.a = a
        
    def __call__(self, other):
        return self.a * other

a = cal(10)

print(a.__call__( 10)) # 100 출력 
print(a(10)) #100출력 __call__ 메소드가 있으므로 cal 클래스의 인스턴스인 a도 함수처럼 사용 가능하다
```

## 데코레이터(Decorator)
데코레이터는 파이썬에서 함수에 추가 기능을 구현할 때 사용한다. 특정 방식이 여러군데 쓰인다면 데코레이터를 쓰는 것이 편리하다.

### 데코레이터의 편리함
```python
def hi():
    print('hi')
    
def hi_kr():
    print("안녕하세요")
    
hi()
hi_kr()

#출력
hi
안녕하세요
```
만약 hi와 안녕하세요를 같이 여러면 출력해야한다면 우리는 아래와 같이 새로운 함수에 hi와 hi_kr 함수를 호출할 것 이다.
```python
...(생략)
def all():
    hi()
    hi_kr()
```
하지만 hi는 그대로 출력하고 다른 인사법도 추가한다면?? 
```python
def hi_fr():
    print("봉쥬르~")

def all2():
    hi()
    hi_fr()
```
이런 식으로 작성하게되면 코드는 모든 언어에 대해서 hi 함수를 중복적으로 작성하게된다. 이를 막기 위해서 일급함수 특징을 사용한다
```python
def decorator(func):
    def wrap():
        hi()
        func()
    
    return wrap
fr = decorator(hi_fr) ## c언어처럼 함수 이름을 넘겨주면 함수포인터로 설정된다.
fr()

#출력
hi
봉쥬르~
```
fr=decorator(hi_fr) 이것 또한 명시적으로 보이지는 않는다
```python
@decorator
def hi_fr():
  print("봉쥬르~")

hi_fr()

#출력
hi
봉쥬르~
```
위와 같이 뒤에 나와야될 함수명 위에 @'데코레이터함수'로 설정하면 decorator(hi_fr)과 같은 효과를 볼 수 있다.
```python
def decorator(func):
    def wrap():
        hi()
        print("봉쥬르~")
    
    return wrap
```

