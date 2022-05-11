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
### 데코레이터의 매개변수
인사말을 매개변수로 받아서 출력하고 싶다. 아래와 같이 인사말을 받아 hi와 같이 출력하도록 데코레이터를 지정해주면 에러가난다. 
```python
def hi(func):
    def wrap():
        print('hi')
        func()
        
    return wrap
    
@hi
def hi_lang(인사말):
    print(인사말)
    
hi_lang("봉쥬르~")

#출력
hi.<locals>.wrap() takes 0 positional arguments but 1 was given   
```
에러가 나는 이유는 사실 우리는 hi_lang을 실행시키는 것이 아닌 hi_lang을 실행시키는 wrap 함수를 실행시키는 것이다. 그러므로 wrap함수에 인자를 줘야되고 wrap함수의 인자는 func에 전달해줘야한다.
```python
def hi(func):
    def wrap(인사말):
        print('hi')
        func(인사말)
        
    return wrap
    
@hi
def hi_lang(인사말):
    print(인사말)
    
hi_lang("봉쥬르~")

#출력
hi
봉쥬르
```
쉽게 생각하는 법, 저는 데코레이터는 '알을 낳는다'라고 생각하는데 hi함수는 자신의 갖고있는 wrap함수를 반환하기 때문에 @hi를 지정함으로써 wrap이라는 알을 낳는다. 이때 알 안에 동물은 
꾸밈을 받는 함수, hi_lang과 같은 다양한 매개변수를 집어 넣을 수 있다. 그리고 최종적으로 사용을 선언함으로써 사용하게된다. 

### Django에서 데코레이터
Django에서는 흔히 쓰는 login_required가 데코레이터가 있다. login_requried 함수는 데코레이터가 2중으로 들어가 있어 상당히 복잡해보인데 하지만 이 원칙만 안다면 이해는 쉽다. 
데코레이터 1개는 1번 알을 낳고 1번 실행한다. 그럼 2중 데코레이터는? 2번 알을 낳고 1번 실행한다.
```python
# 장고 login_requried

def user_passes_test(
    test_func, login_url=None, redirect_field_name=REDIRECT_FIELD_NAME
):
    """
    Decorator for views that checks that the user passes the given test,
    redirecting to the log-in page if necessary. The test should be a callable
    that takes the user object and returns True if the user passes.
    """

    def decorator(view_func):
        @wraps(view_func)
        def _wrapped_view(request, *args, **kwargs):
            if test_func(request.user):
                return view_func(request, *args, **kwargs)
            path = request.build_absolute_uri()
            resolved_login_url = resolve_url(login_url or settings.LOGIN_URL)
            # If the login url is the same scheme and net location then just
            # use the path as the "next" url.
            login_scheme, login_netloc = urlparse(resolved_login_url)[:2]
            current_scheme, current_netloc = urlparse(path)[:2]
            if (not login_scheme or login_scheme == current_scheme) and (
                not login_netloc or login_netloc == current_netloc
            ):
                path = request.get_full_path()
            from django.contrib.auth.views import redirect_to_login

            return redirect_to_login(path, resolved_login_url, redirect_field_name)

        return _wrapped_view

    return decorator


def login_required(
    function=None, redirect_field_name=REDIRECT_FIELD_NAME, login_url=None
):
    """
    Decorator for views that checks that the user is logged in, redirecting
    to the log-in page if necessary.
    """
    actual_decorator = user_passes_test(
        lambda u: u.is_authenticated,
        login_url=login_url,
        redirect_field_name=redirect_field_name,
    )
    if function:
        return actual_decorator(function)
    return actual_decorator
    
...(생략)
```
Django의 login_required는 위와 같이 구현되어 있다. 우리는 이 함수를 사용할 때 아래와 같이 사용한다. 호출 순서를 살펴보자
```python
@login_requried
def mainview(request):
    ...
    return render(...)
```
일단 mainview에서 사용된 login_required 함수는 실은 실질적인 작업보다는 user_passes_test 인자를 세팅 후 user_passes_test 함수가 가진 데코레이터를 반환해주는 것 뿐이다. 일단 login_required 함수에 들어가서 actual_decorator를 따라가보면 user_passes함수가 보이고 이 함수는 데코레이터를 두개 반환하는 것을 볼수 있다(decorator, \_\_wrapper). 그럼 총 2번 알을 낳고 1번 실행해야한 다는 것을 알 수 있다. 일단 acture_decorator가 user_passes_test를 호출하므로써 알을 한번 낳았다. 그리고 if function을 들어가면 한번 호출하므로써 2번 알을 낳고 마지막은 사용하므로써 실행 시켜야한다. 하지만 우리는 뷰에 대한 함수를 정의했지 사용은 어디서 하는 걸까? 바로 url 호출이 들어올 시 실행되는 path(re_path, url)함수 이다. 이로써 총 2번의 알 낳기 작업과 1번의 실행작업이 완료되었다. 


