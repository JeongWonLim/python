# Python
파이썬 공부

## datetime 패키지
```python
import datetime #패키지 파일 
```

## Dict

* **dict.pop(__key:str, __default:str)**
    + key의 해당되는 원소를 삭제하고 value를 반환한다.
    + 

## 스페셜 메소드(Special method)
특정 상황에서 자동 호출 되는 미리 약속되어 있는 메소드.

### \_\_init\_\_(self, ...)
객체를 생성할 때 초기 설정 값들을 설정한다. **생성자가 아니다!!**
```python
class init:
    def __init__(self):
        print('생성자입니다.)

cla = init()

#출력
생성자입니다.
```

### \_\_call\_\_(self,...)
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

### \_\_new\_\_(self,...)
파이썬의 생성자. 대부분 생성자를 init으로 알고있는데 init은 생성된 변수들을 초기화만 할 뿐 객체를 생성하지는 않는다.  
* 생성자의 정의 : 클래스로부터 **객체를 만들어내는** 메소드 
* \_\_init\_\_과 \_\_new\_\_의 호출 순서
    + new가 먼저 호출되어 해당 클래스의 인스턴스를 생성하고 init함수를 호출하여 인스턴의 내의 변수가 초기화가 된다.





## 메타 클래스(Metaclass)

### 객체와 클래스
클래스로 만든 객체 == 인스턴스라고 한다. 객체와 인스턴스의 관계가 중요시 되냐 마냐의 차이다. student = People() 이렇게 만든 student는 객체라고도 하고 인스턴스라고도 한다. 무엇이 맞을까? 사실 둘다 맞다. 하지만 관계를 중요시 여긴다면 People 객체의 인스턴스이다 라고 말하는 것이 더 명확할 것 이다. 

### 파이썬에서 클래스
자바, C++에서의 클래스는 객체를 생성해주는 하나의 틀이였다. 하지만 파이썬에서는 클래스 마저 객체이다. 즉 클래스를 만들어 주는 또 다른 클래스가 있다. 파이썬에서는 객체를 **메타 클래스(Meta Class)** 라고 부른다. 클래스를 만들 때 특별한 규칙을 생성하고 싶다면 메타클래스를 재정의 하면 된다.

### type
파이썬에 type은 클래스이다. type은 대부분 자료형의 종류를 알아 낼 때 쓰지만 또 다른 기능이 존재한다. type클래스로 새로운 클래스를 만들수 있다. 
```python
type(클래스명, (상속받을 클래스), (멤버변수, 멤버함수...))

#예시
Tor = type ('Tor', (), {})
```
위와 같이 type으로도 클래스를 만들 수 있다. 기존에는 class 클래스명:과 같은 방식으로 프로그램이 실행하기전 만들어 놓고 사용했다면 type클래스로 런타임 중에도 다양한 요소들의 조합으로 클래스를 동적으로 만들 수 있다.

타입을 상속받아서 사용하기
```python

import tornado


class Avengers(type):
    def __new__(cla, name, bases, namespace):
        return super().__new__(cla, name, bases, namespace)

def Attack():
    print("망치 뿅뿅")

age = 10

Tor = Avengers ('Tor', (),{'Attack':Attack, 'age' : age} )

Tor.Attack()
print(Tor.age)


##출력
망치 뿅뿅
10
```
### 메타클래스의 생성
메타클래스는 클래스를 만들어주는 클래스라고 앞에서 말했다. 그럼 메타클래스는 미리 코드가 실행되어 있어야한다. 그래야 다른 클래스를 생성할 수 있기 때문이다.
```python
class Avensers(type):
    def __new__(cls, *args, **kwargs):
        print('Avensers __new__')
        return super().__new__(cls, *args, **kwargs)

    def __init__(cls, *args, **kwargs):
        print('Avensers __init__')
        super().__init__(*args, **kwargs)

    def __call__(cls, *args, **kwargs):
        print('Avensers __call__')
        return super().__call__(*args, **kwargs)

class Tor(metaclass=Avensers):
    def __init__(self):
        print('Tor __init__')

    def __call__(self):
        print('Tor __call__')
        
#출력
Avensers __new__
Avensers __init__
```
위의 코드를 실행하면 우리는 tor클래스의 인스턴스를 생성하지 않았음에도 불구하고 이미 Avensers가 호출된것을 볼 수 있다. 이유인 즉 메타클래스는 클래스를 만드는 클래스이기 때문에 미리 생성되어 클래스를 만들 준비를 하고 있어야한다.
```python
a = Tor()

#출력
Avensers __call__
Tor __init__
```
Tor객체의 인스턴스인 a를 만들게 되면 Avensers의 call 메소드가 호출되어 avensers의 인스턴스인 Tor가 생성되고 Tor의 인스턴스인 a가 생성되게 된다.

## Django의 메타클래스
Django에는 메타클래스로 쓰이는 ModelBase 클래스가 있다. 이 클래스는 모든 모델을 위한 메타클래스이다
```python
class ModelBase(type):
    """Metaclass for all models."""

    def __new__(cls, name, bases, attrs, **kwargs):
        super_new = super().__new__     #type __new__를 함수포인터로 가르킨다.

        # Also ensure initialization is only performed for subclasses of Model
        # (excluding Model class itself).
        parents = [b for b in bases if isinstance(b, ModelBase)] 
        if not parents: #부모클래스가 없으면 새로운 객체 생성
            return super_new(cls, name, bases, attrs)

        # Create the class.
        module = attrs.pop("__module__")
        new_attrs = {"__module__": module} #새로운 속성추가
        classcell = attrs.pop("__classcell__", None)
        if classcell is not None:
            new_attrs["__classcell__"] = classcell
        attr_meta = attrs.pop("Meta", None) # Model 작성할때 class Meta:를 작성하는 이유
        # Pass all attrs without a (Django-specific) contribute_to_class()
        # method to type.__new__() so that they're properly initialized
        # (i.e. __set_name__()).
        contributable_attrs = {}
        for obj_name, obj in attrs.items():
            if _has_contribute_to_class(obj):
                contributable_attrs[obj_name] = obj
            else:
                new_attrs[obj_name] = obj
        new_class = super_new(cls, name, bases, new_attrs, **kwargs)

        abstract = getattr(attr_meta, "abstract", False)
        meta = attr_meta or getattr(new_class, "Meta", None)
        base_meta = getattr(new_class, "_meta", None)

        app_label = None

        # Look for an application configuration to attach the model to.
        app_config = apps.get_containing_app_config(module)

        if getattr(meta, "app_label", None) is None:
            if app_config is None:
                if not abstract:
                    raise RuntimeError(
                        "Model class %s.%s doesn't declare an explicit "
                        "app_label and isn't in an application in "
                        "INSTALLED_APPS." % (module, name)
                    )

            else:
                app_label = app_config.label

        new_class.add_to_class("_meta", Options(meta, app_label))
        if not abstract:
            new_class.add_to_class(
                "DoesNotExist",
                subclass_exception(
                    "DoesNotExist",
                    tuple(
                        x.DoesNotExist
                        for x in parents
                        if hasattr(x, "_meta") and not x._meta.abstract
                    )
                    or (ObjectDoesNotExist,),
                    module,
                    attached_to=new_class,
                ),
            )
            new_class.add_to_class(
                "MultipleObjectsReturned",
                subclass_exception(
                    "MultipleObjectsReturned",
                    tuple(
                        x.MultipleObjectsReturned
                        for x in parents
                        if hasattr(x, "_meta") and not x._meta.abstract
                    )
                    or (MultipleObjectsReturned,),
                    module,
                    attached_to=new_class,
                ),
            )
            if base_meta and not base_meta.abstract:
                # Non-abstract child classes inherit some attributes from their
                # non-abstract parent (unless an ABC comes before it in the
                # method resolution order).
                if not hasattr(meta, "ordering"):
                    new_class._meta.ordering = base_meta.ordering
                if not hasattr(meta, "get_latest_by"):
                    new_class._meta.get_latest_by = base_meta.get_latest_by

        is_proxy = new_class._meta.proxy

        # If the model is a proxy, ensure that the base class
        # hasn't been swapped out.
        if is_proxy and base_meta and base_meta.swapped:
            raise TypeError(
                "%s cannot proxy the swapped model '%s'." % (name, base_meta.swapped)
            )

        # Add remaining attributes (those with a contribute_to_class() method)
        # to the class.
        for obj_name, obj in contributable_attrs.items():
            new_class.add_to_class(obj_name, obj)

        # All the fields of any type declared on this model
        new_fields = chain(
            new_class._meta.local_fields,
            new_class._meta.local_many_to_many,
            new_class._meta.private_fields,
        )
        field_names = {f.name for f in new_fields}

        # Basic setup for proxy models.
        if is_proxy:
            base = None
            for parent in [kls for kls in parents if hasattr(kls, "_meta")]:
                if parent._meta.abstract:
                    if parent._meta.fields:
                        raise TypeError(
                            "Abstract base class containing model fields not "
                            "permitted for proxy model '%s'." % name
                        )
                    else:
                        continue
                if base is None:
                    base = parent
                elif parent._meta.concrete_model is not base._meta.concrete_model:
                    raise TypeError(
                        "Proxy model '%s' has more than one non-abstract model base "
                        "class." % name
                    )
            if base is None:
                raise TypeError(
                    "Proxy model '%s' has no non-abstract model base class." % name
                )
            new_class._meta.setup_proxy(base)
            new_class._meta.concrete_model = base._meta.concrete_model
        else:
            new_class._meta.concrete_model = new_class

        # Collect the parent links for multi-table inheritance.
        parent_links = {}
        for base in reversed([new_class] + parents):
            # Conceptually equivalent to `if base is Model`.
            if not hasattr(base, "_meta"):
                continue
            # Skip concrete parent classes.
            if base != new_class and not base._meta.abstract:
                continue
            # Locate OneToOneField instances.
            for field in base._meta.local_fields:
                if isinstance(field, OneToOneField) and field.remote_field.parent_link:
                    related = resolve_relation(new_class, field.remote_field.model)
                    parent_links[make_model_tuple(related)] = field

        # Track fields inherited from base models.
        inherited_attributes = set()
        # Do the appropriate setup for any model parents.
        for base in new_class.mro():
            if base not in parents or not hasattr(base, "_meta"):
                # Things without _meta aren't functional models, so they're
                # uninteresting parents.
                inherited_attributes.update(base.__dict__)
                continue

            parent_fields = base._meta.local_fields + base._meta.local_many_to_many
            if not base._meta.abstract:
                # Check for clashes between locally declared fields and those
                # on the base classes.
                for field in parent_fields:
                    if field.name in field_names:
                        raise FieldError(
                            "Local field %r in class %r clashes with field of "
                            "the same name from base class %r."
                            % (
                                field.name,
                                name,
                                base.__name__,
                            )
                        )
                    else:
                        inherited_attributes.add(field.name)

                # Concrete classes...
                base = base._meta.concrete_model
                base_key = make_model_tuple(base)
                if base_key in parent_links:
                    field = parent_links[base_key]
                elif not is_proxy:
                    attr_name = "%s_ptr" % base._meta.model_name
                    field = OneToOneField(
                        base,
                        on_delete=CASCADE,
                        name=attr_name,
                        auto_created=True,
                        parent_link=True,
                    )

                    if attr_name in field_names:
                        raise FieldError(
                            "Auto-generated field '%s' in class %r for "
                            "parent_link to base class %r clashes with "
                            "declared field of the same name."
                            % (
                                attr_name,
                                name,
                                base.__name__,
                            )
                        )

                    # Only add the ptr field if it's not already present;
                    # e.g. migrations will already have it specified
                    if not hasattr(new_class, attr_name):
                        new_class.add_to_class(attr_name, field)
                else:
                    field = None
                new_class._meta.parents[base] = field
            else:
                base_parents = base._meta.parents.copy()

                # Add fields from abstract base class if it wasn't overridden.
                for field in parent_fields:
                    if (
                        field.name not in field_names
                        and field.name not in new_class.__dict__
                        and field.name not in inherited_attributes
                    ):
                        new_field = copy.deepcopy(field)
                        new_class.add_to_class(field.name, new_field)
                        # Replace parent links defined on this base by the new
                        # field. It will be appropriately resolved if required.
                        if field.one_to_one:
                            for parent, parent_link in base_parents.items():
                                if field == parent_link:
                                    base_parents[parent] = new_field

                # Pass any non-abstract parent classes onto child.
                new_class._meta.parents.update(base_parents)

            # Inherit private fields (like GenericForeignKey) from the parent
            # class
            for field in base._meta.private_fields:
                if field.name in field_names:
                    if not base._meta.abstract:
                        raise FieldError(
                            "Local field %r in class %r clashes with field of "
                            "the same name from base class %r."
                            % (
                                field.name,
                                name,
                                base.__name__,
                            )
                        )
                else:
                    field = copy.deepcopy(field)
                    if not base._meta.abstract:
                        field.mti_inherited = True
                    new_class.add_to_class(field.name, field)

        # Copy indexes so that index names are unique when models extend an
        # abstract model.
        new_class._meta.indexes = [
            copy.deepcopy(idx) for idx in new_class._meta.indexes
        ]

        if abstract:
            # Abstract base models can't be instantiated and don't appear in
            # the list of models for an app. We do the final setup for them a
            # little differently from normal models.
            attr_meta.abstract = False
            new_class.Meta = attr_meta
            return new_class

        new_class._prepare()
        new_class._meta.apps.register_model(new_class._meta.app_label, new_class)
        return new_class
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


