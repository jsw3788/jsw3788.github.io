## Python 기초

- 식별자(Identifiers)

  - 변수, 함수, 모듈, 클래스 등을 식별하는데 사용되는 이름

  - 영문알파벳, 언더스코어, 숫자로 구성됨

  - 첫 글자에 숫자가 올 수 없다.

  - 길이에 제한이 없다.

  - 대소문자를 구별한다.

  - 아래의 키워드는 사용 불가

    ```python
    import keyword
    print(keyword.kwlist)
    
    # ['False', 'None', 'True', 'and', 'as', 'assert', 'async', 'await', 'break', 'class', 'continue', 'def', 'del', 'elif', 'else', 'except', 'finally', 'for', 'from', 'global', 'if', 'import', 'in', 'is', 'lambda', 'nonlocal', 'not', 'or', 'pass', 'raise', 'return', 'try', 'while', 'with', 'yield']
    
    ```

  - [표기법](https://cishome.tistory.com/199)

    - 스네이크 케이스(snake case)

    > var snake_case;

    언더바(_) 가 들어 있는 표현 방식을 뱀처럼 생겼다고 하여 스네이크 케이스라고 한다.

    

    - 파스칼 케이스(pascal case)

    > var PascalCase;

    첫 글자와 중간 글자들이 대문자인 경우 파스칼 언어의 표기법과 유사하다고 하여 파스칼 케이스라고 한다.

    

    - 카멜 케이스 (camel case)

    > var camelCase;

    중간 글자들은 대문자로 시작하지만 첫 글자가 소문자인 경우에는 낙타와 모양이 비슷하다 하여 카멜 케이스라고 한다.

    

- 실수의 연산

  ```python
  a = 0.38
  b = 3.5-3.12
  print(a == b) // false
  ```

  1. 기본적인 처리방법

     ```python
     print(abs(a-b)<=1e-10)
     ```

  2. sys 모듈

     - 'epsilon` 은 부동소수점 연산에서 반올림을 함으로써 발생하는 오차 상환

     ```python
     import sys
     abs(a - b) <= sys.float_info.epsilon
     ```

  3. math 모듈

     ```python
     import math
     math.isclose(a, b)
     ```

- 객체 비교 연산자

  - is : 객체 아이덴티티가 동일한지 확인
  - is not : 객체 아이덴티티가 동일하지 않은지 확인

  

- 표현식과 문장

  - 표현식(Expression)
    - 하나의 값(value)으로 평가(evaluate)될 수 있는 문장을 의미함

    - 표현식은 식별자, 값(리터럴), 연산자로 구성됨

    - 표현식을 만드는 문법(은 일반적인 (중위표기) 수식의 규칙과 유사함

      

  - 문장(Statement)

    - 파이썬이 실행 가능한 최소한의 코드 단위
    - 표현식도 문장이 될 수 있다.

    

## 컨테이너

- 컨테이너 : 여러 개의 값을 저장할 수 있는 것

  - 시퀀스형 컨테이너

    - 순서를 가질 수 있다.(정렬되었다는 뜻은 아니다.)
    - 특정 위치의 데이터를 가리킬 수 있다.
    - list, tuple, range, string, binary가 해당된다.

  - 비 시퀀스형 컨테이너

    - 순서가 없다.
    - set, dictionary가 해당된다.

  - 컨테이너형 형변환

    |            | string | list        | tuple       | range | set         | dictionary |
    | ---------- | ------ | ----------- | ----------- | ----- | ----------- | ---------- |
    | string     |        | O           | O           | X     | O           | X          |
    | list       | O      |             | O           | X     | O           | X          |
    | tuple      | O      | O           |             | X     | O           | X          |
    | range      | O      | O           | O           |       | O           | X          |
    | set        | O      | O           | O           | X     |             | X          |
    | dictionary | O      | O(only key) | O(only key) | X     | O(only key) |            |

    

- 데이터의 분류

  - 변경 가능한(mutable) 데이터 : list, dict, set
  - 변경 불가능한(immutable) 데이터 : literal(number/string/bool), range, tuple, frozenset

  

## 제어문

- for-else, while-else문

  - for-else문 : 반복에서 리스트가 소진된 경우 실행됨

  - while-else문 : 조건이 거짓이 되서 종료될 때 실행됨

  - 반복문이 break문으로 종료될 때는 실행되지 않는다.

    ```python
    for i in range(3):
        print(i)
        if i == 3:
            print(f'{i}에서 break')
            break
    else:
        print("no break")
        
     // 0 1 no break
        
    for i in range(3):
        print(i)
        if i == 1:
            print(f'{i}에서 break')
            break
    else:
        print('no break')
        
    # 0 1 1에서 break
    ```



## 함수

- 내장함수 목록 확인

  ```python
  dir(__builtins__)
  ```

- return

  - 오직 한 개의 객체만 반환한다.
  - return 이 없거나 return 뒤에 값을 적지 않은 경우 : None를 반환
  - return에 콤마를 통해 여러 개의 값을 적은 경우 : tuple로 감싸서 하나의 객체를 반환

  

- 매개변수와 전달인자

  - 매개변수(parameter)

    ```python
    def func(x):
        return x*3
    ```

    - 위에서 `x`는 매개변수이다.
    - 입력을 받아 함수 내부에서 활용할 변수를 의미
    - 함수를 정의할 때

    

  - 전달인자(argument)

    ```python
    func(1)
    ```

    - 1은 (전달)인자이다.
    - 실제로 전달되는 입력값을 의미
    - 함수를 호출할 때
    
    

- 인자(argument)

  - 위치인자(positional arguments) 

    기본적으로 인자는 위치에 따라 함수 내에 전달된다.

    

  - 기본 인자 값 (default argument values)

    - **함수를 정의할 때** 기본값을 지정
    - 기본 인자값을 가지는 인자 다음에 기본 값이 없는 인자를 사용할 수 없다.

    

  - 키워드 인자(keyword arguments)

    - **함수를 호출할 때** 키워드 인자를 활용하여 직접 변수의 이름으로 특정 인자를 전달
    - 키워드 인자를 활용한 다음에 위치 인자를 활용할 수 없다.

    

  - 가변(임의) 인자 리스트(arbitrary argument lists)

    - 개수가 정해지지 않은 인자를 받기 위해서 **함수를 정의할 때** 가변 인자 리스트 `*args`를 활용
    - 가변 인자 리스트는 tuple 형태로 처리되며 매개변수에 *로 표현한다.

    

  - 가변(임의) 키워드 인자

    - 정해지지 않은 키워드 인자들은 **함수를 정의할 때** 가변 키워드 인자 `**kwargs`를 활용
    - 가변 키워드 인자는 dict 형태로 처리되며 매개변수에 **로 표현한다.

    

- 스코프(scope)

  - 지역 스코프
    - 함수가 만든 스코프로 함수 내부에서만 참조 가능
    - 함수가 호출될 때 생성되고 함수가 종료될 때까지 유지된다.
    - 로컬 스코프에 정의된 변수를 지역 변수라고 한다.
    
  - 전역 스코프
    - 모듈 내 어디에서든 참조 가능
    - 모듈이 호출된 시점 이후 혹은 선언된 이후부터 인터프리터가 끝날 때까지 유지된다.
    - 전역 스코프에 정의된 변수를 전역 변수라고 한다.
    
  - 이름 검색 규칙
    - 파이썬에서 사용되는 이름(식별자)를 찾아가는 순서
    - LEGB Rule : Local Scope -> Enclosed Scope -> Global Scope -> Built-in Scope
      - Enclosed Scope : 특정 함수의 상위 함수
      - Built-in Scope : 파이썬안에 내장되어 있는 함수 또는 속성
      
    
  - 상위 스코프에 있는 변수를 수정하고 싶다면 global, nonlocal 키워드를 활용 가능하나 권장하지 않는다.
  
  

## 예외처리

- 복수의 예외 처리

  ```python
  try:
      <코드 블럭 1>
  except (예외1, 예외2):
      <코드 블럭 2>
  
  
  try:
      <코드 블럭 1>
  except 예외1:
      <코드 블럭 2>
  except 예외2:
      <코드 블럭 3>
  ```

- else

  - 에러가 발생되지 않는 경우 수행되는 문장에 이용한다.
  - `try`절이 예외를 일으키지 않을 때 실행되어야하는 코드에 적절하다.
  - 모든 `except`절 뒤에 와야한다.

  ```python
  try:
      <코드 블럭 1>
  except 예외:
      <코드 블럭 2>
  else:
      <코드 블럭 3>
  ```

- finally

  - 반드시 수행해야하는 문장에 활용한다.
  - 모든 상황에 실행되어야만 하는 코드를 정의하는데 활용한다.
  - 예외의 발생 여부와 관계없이 실행한다.

  ```python
  try:
      <코드 블럭 1>
  except 예외:
      <코드 블럭 2>
  finally:
      <코드 블럭 3>
  ```

- as

  - 에러 메시지도 alias가 가능하다.

  ```python
  try:
      <코드 블럭 1>
  except 예외 as err:
      <코드 블럭 2>
  ```

- raise

  - raise를 통해 강제로 예외를 발생시킬 수 있다.

  ```python
  raise <error> ('message')
  ```

- assert

  - 상태를 검증하는데 사용되며 무조건 AssertionError를 발생시킨다.
  - 일반적으로 디버깅용도로 사용된다.

  ```python
  assert Boolean expression, error message
  
  assert len([1, 2]) == 1, '길이가 1이 아닙니다.'
  '''
  $ python code.py
  Traceback (most recent call last):
    File "code.py", line 1, in <module>
      assert len([1, 2]) == 1, '길이가 1이 아닙니다.'
  AssertionError: 길이가 1이 아닙니다.
  '''
  ```



## 데이터 구조

- String

  - `.find(x)` : x의 첫번째 위치를 반환, 없으면 -1을 반환
  - `.index(x)` : x의 첫번째 위치를 반환, 없으면 오류가 발생
  - `.capitalize()` : 앞글자를 대문자로 만들어 반환
  - `.title()` : `(어포스트로피)나 공백 이후를 대문자로 만들어 반환
  - `.swapcase()` : 대문자와 소문자를 서로 변경하여 반환

  

- List

  - `.extend(iterable)` : 리스트에 iterable(list, range, tuple, string) 값을 붙일 수 있다.
  - `.clear()` : 리스트의 모든 항목을 삭제한다.

  

- Set

  - `.update(*iterable)` : 여러 값을 추가, 인자로는 반드시 iterable를 전달해야함
  - `.remove(elem)` : elem을 set에서 삭제하고 없으면 KeyError가 발생
  - `.discard(elem)` : elem을 set에서 삭제하고 없어도 에러가 발생하지 않음
  - `.pop()` : 임의의 원소를 제거해 반환함

  

- Dictionary

  - `.get(key[, default])` 
    - key를 통해 value를 가져옴
    - KeyError가 발생하지 않음
    - default는 기본적으로 None
  - `.pop(key[, default])`
    - key가 dict에 있으면 제거하고 그 값을 반환
    - 그렇지 않으면 인자로 설정한 default를 반환
    - default도 없으면 KeyError가 발생
  - `.update()` : 값을 제공하는 key, value를 덮어씀
  - `.items()` : dict의 key와 value를 튜플 형태로 반환

  

- filter(function, iterable)

  - iterable에서 function의 반환된 결과가 `True`인 것들만 구성하여 반환
  - filter object를 반환



## 모듈

- 모듈 : 모듈은 특정 기능을 하는 코드를 담고 있는 파일이나 스크립트

  | 용어                        | 정의                                                         |
  | --------------------------- | ------------------------------------------------------------ |
  | 모듈                        | 특정 기능을 .py 파일 단위로 작성한 것                        |
  | 패키지                      | 특정 기능과 관련된 여러 모듈들의 집함. 패키지 안에 서브 패키지를 포함 가능 |
  | 파이썬 표준 라이브러리(PSL) | 파이썬에 기본적으로 설치된 모듈과 내장함수를 묶은 것을 지칭  |
  | 패키지 관리자(pip)          | PyPI에 저장된 외부 패키지들을 설치하도록 도와주는 패키지     |



- `__init__.py` 

  python3.3부터는 `__init__.py` 파일이 없어도 패키지로 인식하지만 하위 버전 호환 및 일부 프레임워크에서의 올바른 동작을 위해 `__init__.py`파일을 생성하는 것이 권장된다.

  ```python
  my_package/
      __init__.py
      math/
          __init__.py
          tools.py  
  ```

  

- 패키지 활용

  - `from <패키지> import <모듈>` 
  - `from <패키지.모듈> import <데이터>` : 특정한 함수나 데이터만 활용하고 싶을 때
  - `from <모듈> import *` : 해당하는 모듈 내의 모든 변수, 함수, 클래스를 가져옴



## 객체지향 프로그래밍(OOP)

- 인스턴스(Instance)
  - 파이썬에서 모든 것은 객체이고 모든 객체는 특정 타입의 인스턴스이다.
- 속성(Attribute)
  - 속성은 객체의 상태/데이터를 의미

- 클래스(Class)

  - 클래스의 이름은 PascalCase로 정의

  - 클래스 내부에서 정의된 데이터를 속성, 함수를 메서드라고 부른다.

  - docstring

    - 클래스나 함수의 설명을 정의할 때 사용

      ```python
      class Dog:
          """
          This is Dog class
          """
      dog = Dog()
      print(dog.__doc__)
      // This is Dog Class
      ```

- self
  - 인스턴스 자신을 의미
  - Python에서 인스턴스 메서드는 호출 시 첫번째 인자로 인스턴스 자신이 전달되게 설계됨
  - 보통 매개변수명으로 `self`를 첫번째 인자로 정의

- 생성자(Constructor)

  - 인스턴스 객체가 생성될 때 호출되는 함수. `__init__`이라는 이름으로 정의

    ```python
    class Dog:
        def __init__(self,name):
            self.name = name
            
        def bark(self):
            print(f'멍멍, 나는 {self.name}')
            
    dog = Dog('개똥이')
    dog.bark() # 멍멍, 나는 개똥이
    ```

- 소멸자(destructor)
  - 인스턴스 객체가 소멸되기 직전에 호출되는 함수. `__del__`이라는 이름으로 정의
  - 프로그램이 종료될 때(인스턴스 소멸)도 호출됨

- 매직(스페셜) 메서드
  - 더블언더스코어(`__`)가 있는 메서드는 특별한 목적을 위해 만들어진 메서드이고 매직 메서드라 불린다.

```python
'__str__(self)',
'__len__(self)',
'__repr__(self)',
'__lt__(self, other)',
'__le__(self, other)',
'__eq__(self, other)',
'__ne__(self, other)',
'__gt__(self, other)',
'__ge__(self, other)'
```

- namespace 탐색 순서

  - 클래스를 정의하면 클래스가 생성됨과 동시에 namespace가 생성된다.
  - 인스턴스를 만들게되면, 인스턴스 객체가 생성되고 해당되는 namespace가 생성된다.
  - 이 때, 클래스와 인스턴스는 서로 다른 namespace를 가지게 된다.
  - 인스턴스의 속성이 변경되면, 변경된 데이터를 인스턴스 객체 이름 공간에 저장한다.
  - 인스턴스에서 특정한 어트리뷰트에 접근하게 되면 **인스턴스 -> 클래스** 순으로 탐색한다.

  

- 메서드의 종류

  - 인스턴스 메서드

    - 인스턴스가 사용할 메서드
    - 호출시, 첫번째 인자로 `self`가 전달됨

    

  - 클래스 메서드

    - 클래스가 사용할 메서드
    - `@classmethod` 데코레이터를 사용하여 정의
    - 호출시, 첫번째 인자로 `cls`가 전달됨
    - 클래스 자체(`cls`)와 그 속성에 접근할 필요가 있다면 클래스 메서드로 정의

    

  - 스태틱 메서드

    - 클래스가 사용할 메서드
    - `@staticmethod` 데코레이터를 사용하여 정의
    - 호출시, 어떠한 인자도 (필수적으로) 전달되지 않음

    

    ```python
    class Puppy:
        population = 0
        
        def __init__(self,name,breed):
            self.name = name
            self.breed = breed
            Puppy.population += 1
        
        def __del__(self):
            Puppy.population -= 1
        
        def bark(self):
            print(f'왈왈! 나는{self.name}, {self.breed}(이)야')
            
        @classmethod
        def get_population(cls):
            print(f'현재 강아지 마리수:{cls.population}')
            return cls.population
    
        @staticmethod
        def info():
            return '이것은 Puppy 클래스입니다!'
    ```

- 상속

  - 부모 클래스의 모든 속성이 자식 클래스에게 상속되므로 코드 재사용성이 높아짐

  - 사용

    ```python
    class ChildClass(ParentClass):
        <code block>
    ```

  - 관련 함수

    - `issubclass(class, classinfo)` : class가 classinfo의 subclass면 True를 반환

    - `isinstance(object, classinfo)` : object가 classinfo의 인스턴스이거나 subclass인 경우 True를 반환

    - `super()`

      - 자식 클래스에 메서드를 추가로 구현할 때 사용

      - 중복되는 부분은 `super()`을 사용하여 처리하고 필요한 부분을 구현

      - 부모 클래스의 내용을 사용하고자 할 때, `super()`를 사용

        ```python
        class ChildClass(ParentClass):
            def method(self, arg):
                super().method(arg) 
        ```

  - 메서드 오버라이딩

    - 자식 클래스에서 부모 클래스에서 상속 받은 메서드를 재정의하는 것을 말한다.
    - 상속 받은 클래스에서 같은 이름의 메서드로 덮어쓴다.

    

  - 상속관계에서의 namespace 탐색 순서

    - 인스턴스 -> 자식 클래스 -> 부모 클래스

    

  - 다중 상속

    - 두개 이상의 클래스를 상속받는 경우, 다중 상속됨

    - 상속 받은 모든 클래스의 요소를 활용 가능

    - 중복된 속성이나 메서드가 있는 경우 상속 순서에 의해 결정됨(앞선 것이 우선됨)

    - 사용

      ```python
      class ChildClass(ParentClass1, ParentClass2):
          <code block>
      ```

      