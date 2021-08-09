# Class

## 1. 클래스 기본

### 클래스와 메서드 만들기

```python
class Person:           # 클래스 이름은 대문자로 시작
    def greeting(self): # 클래스의 메서드의 첫 번째 매개변수는 반드시 self이어야 함
        print('Hello')
```

```python
# 인스턴스를 만들어서 클래스를 사용할 수 있다
james = Person()
james.greeting()
# 결과 : 
Hello
```

```python
# 클래스 내에서 메서드 호출하기 : self.메서드명()
class Person:
    def greeting(self):
        print('Hello')

    def hello(self):
        self.greeting()
```

### 속성(Attribute)

```python
# 클래스 속성: 모든 인스턴스가 공유. 인스턴스 전체가 사용해야 하는 값을 저장할 때 사용
# 인스턴스 속성: 인스턴스별로 독립되어 있음. 각 인스턴스가 값을 따로 저장해야 할 때 사용

class Person:
    bag = ['수학','과학']                   # 클래스 속성, 인스턴스없이 접근 가능
    def __init__(self):                     # 인스턴스 속성, 속성을 정의할 때는 __init__(self)함수에 해야한다.
        self.hello = '안녕하세요'           # hello 속성을 갖게 됨

    def greeting(self):
        print(self.hello)
```

### 비공개 속성

```python
class Person:
    def __init__(self, name, age, addr, wallet):
        self.name = name        
        self.age = age
        self.addr = addr
        self.__wallet = wallet      # 비공개 속성 :  self.__속성명

    def greeting(self):
        print(f'안녕하세요? 저는 {self.name}입니다.')

    def pay(self, amount):          # 비공개 속성은 method를 통해서만 값을 바꿀 수 있다.
        if self.__wallet - amount < 0:
            print('지갑에 돈이 부족합니다.')
            return
        self.__wallet -= amount
        print(f'지갑에 남은 돈은 {self.__wallet}입니다.')

    # JAVA의 toString() method : 객체가 가지고 있는 정보나 값들을 문자열로 만들어 리턴하는 메소드
    def __str__(self):
        return f'name: {self.name}, age: {self.age}, addr: {self.addr}, wallet: {self.__wallet}'
```

## 2. 정적 메서드

```python
# 정적 메서드 : 클래스를 인스턴스로 만들지 않고 사용할 수 있는 메서드 
class Calc:
    @staticmethod
    def add(a,b):       # self가 없음
        print(a + b)
```

## 3. 상속

### 기본 상속

```python
class Person:
    def greeting(self):
        print('안녕하세요?')

class Student(Person):      # Student class(파생 클래스)가 Person class(기반 클래스)를 상속 받음
    def study(self):
        print('공부하기')
```

### 기반 클래스의 속성 사용하기

```python
class Person:
    def __init__(self):
        self.hello = '안녕하세요?'
        print('Person.__init__')

class Student(Person):
    def __init__(self):
        self.school = '파이썬'              # 부모 클래스와 자식 클래스에서 모두 __init__을 정의하여 overriding가 일어남
        print('Student.__init__')           # => 자식 클래스에서 __init__을 정의하지 않거나 super().__init__()이용
```

- ### super()로 기반 클래스 초기화

  ```python
  class Person:
      def __init__(self):
          self.hello = '안녕하세요?'
          print('Person.__init__')
  
  class Student(Person):
      def __init__(self):
          super().__init__()
          self.school = '파이썬'
          print('Student.__init__')
  ```

### Method Overriding

```python
class Person:                   # class Person(object) => object 클래스 : 모든 클래스의 조상
    def greeting(self):
        print('안녕하세요?')

class Student(Person):
    def greeting(self):
        super().greeting()       # 같은 메서드명을 갖는 부모 클래스의 메서드를 사용하는 방법, 이 코드가 없는 경우 student에서 정의한 메소드를 사용하게 됨.(overriding)
        print('안녕하세요? 저는 파이썬을 공부하는 학생입니다.')
```

### 추상 클래스

```python
# 추상 클래스 : 메서드의 목록만 가진 클래스, interface 역할
from abc import *

class StudentBase(metaclass=ABCMeta):
    @abstractmethod                     # decorator
    def study(self):
        pass
    @abstractmethod
    def go_to_school(self):
        pass
```

