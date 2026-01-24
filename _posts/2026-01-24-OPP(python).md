---
layout: post
title: "객체지향 프로그래밍 (OOP)  정리 - Python"
date: 2024-01-24
categories: [Python, Programming]
tags: [OOP, 객체지향, Python, 클래스, 상속, 캡슐화, 다형성, 추상화]
toc: true
toc_sticky: true
---

# 객체지향 프로그래밍 (OOP)  정리

## 📌 객체지향이란?

**데이터(속성)와 기능(메서드)을 하나의 "객체"로 묶어서 프로그래밍하는 방식**

> 현실 세계의 사물을 프로그램으로 모델링하는 패러다임입니다.

> ### 🎯 객체 (Object)
> 
> **속성 (Attributes)**
> - 이름, 나이, 색상 등
> 
> **메서드 (Methods)**
> - 행동, 기능, 동작 등



---

## 🔑 4가지 핵심 개념

### 1. 캡슐화 (Encapsulation)

> 데이터를 보호하고 외부 접근을 제한

**접근 제어자:**
- `__변수명`: private 변수 (외부 접근 차단)
- `_변수명`: protected 변수 (관례적 보호)
- `변수명`: public 변수 (자유로운 접근)

```python
class BankAccount:
    def __init__(self, owner, balance):
        self.owner = owner          # public: 자유롭게 접근 가능
        self._bank = "신한은행"      # protected: 관례적 보호
        self.__balance = balance    # private: 외부 접근 차단
    
    def deposit(self, amount):
        """입금 메서드"""
        if amount > 0:
            self.__balance += amount
            return f"{amount}원 입금 완료"
        return "입금 실패"
    
    def withdraw(self, amount):
        """출금 메서드"""
        if 0 < amount <= self.__balance:
            self.__balance -= amount
            return f"{amount}원 출금 완료"
        return "잔액 부족"
    
    def get_balance(self):
        """잔액 조회 (getter)"""
        return self.__balance
    
    @property
    def balance(self):
        """property를 이용한 getter"""
        return self.__balance

# 사용 예시
account = BankAccount("홍길동", 10000)

# public 접근
print(account.owner)           # 홍길동 ✅

# protected 접근 (가능하지만 권장하지 않음)
print(account._bank)           # 신한은행 ⚠️

# private 접근 시도
# print(account.__balance)     # AttributeError ❌

# 올바른 접근 방법
print(account.get_balance())   # 10000 ✅
print(account.balance)         # 10000 ✅ (property 사용)

# 메서드를 통한 안전한 조작
print(account.deposit(5000))   # 5000원 입금 완료
print(account.withdraw(3000))  # 3000원 출금 완료
print(account.get_balance())   # 12000
```

**💡 캡슐화를 사용하는 이유:**

- 데이터 무결성 보장
- 내부 구현 변경 시 외부 코드에 영향 없음
- 유효성 검사 가능



### 2. 상속 (Inheritance)
>부모 클래스의 속성/메서드를 자식이 물려받음

상속 관계도:



         Animal (부모/슈퍼 클래스)
              │
        ┌─────┴─────┐
        ▼           ▼
        Dog         Cat (자식/서브 클래스)
        │
        ▼
     Bulldog (손자 클래스)


```python
class Animal:
    """부모 클래스 (슈퍼 클래스)"""
    
    def __init__(self, name, age):
        self.name = name
        self.age = age
    
    def speak(self):
        pass
    
    def info(self):
        return f"이름: {self.name}, 나이: {self.age}살"

class Dog(Animal):
    """자식 클래스 - Animal 상속"""
    
    def __init__(self, name, age, breed):
        super().__init__(name, age)  # 부모 생성자 호출
        self.breed = breed           # 자식만의 속성 추가
    
    def speak(self):
        return f"{self.name}: 멍멍!"
    
    def fetch(self):
        return f"{self.name}가 공을 가져옵니다!"

class Cat(Animal):
    """자식 클래스 - Animal 상속"""
    
    def __init__(self, name, age, color):
        super().__init__(name, age)
        self.color = color
    
    def speak(self):
        return f"{self.name}: 야옹~"
    
    def scratch(self):
        return f"{self.name}가 스크래처를 긁습니다!"

class Bulldog(Dog):
    """손자 클래스 - Dog 상속 (다단계 상속)"""
    
    def __init__(self, name, age):
        super().__init__(name, age, "불독")
    
    def speak(self):
        return f"{self.name}: 컹컹!"

# 사용 예시
dog = Dog("바둑이", 3, "진돗개")
cat = Cat("나비", 2, "흰색")
bulldog = Bulldog("불리", 4)

print(dog.info())      # 이름: 바둑이, 나이: 3살 (상속받은 메서드)
print(dog.speak())     # 바둑이: 멍멍! (오버라이딩된 메서드)
print(dog.fetch())     # 바둑이가 공을 가져옵니다! (자식만의 메서드)

print(cat.speak())     # 나비: 야옹~
print(bulldog.speak()) # 불리: 컹컹!

# 상속 관계 확인
print(isinstance(dog, Animal))  # True
print(isinstance(dog, Dog))     # True
print(issubclass(Dog, Animal))  # True
```

### 3. 다형성 (Polymorphism)
>같은 메서드가 객체에 따라 다르게 동작

```python
class Shape:
    """도형 기본 클래스"""
    
    def area(self):
        pass
    
    def perimeter(self):
        pass

class Rectangle(Shape):
    def __init__(self, width, height):
        self.width = width
        self.height = height
    
    def area(self):
        return self.width * self.height
    
    def perimeter(self):
        return 2 * (self.width + self.height)

class Circle(Shape):
    def __init__(self, radius):
        self.radius = radius
    
    def area(self):
        return 3.14159 * self.radius ** 2
    
    def perimeter(self):
        return 2 * 3.14159 * self.radius

class Triangle(Shape):
    def __init__(self, base, height, side1, side2, side3):
        self.base = base
        self.height = height
        self.sides = [side1, side2, side3]
    
    def area(self):
        return 0.5 * self.base * self.height
    
    def perimeter(self):
        return sum(self.sides)

# 다형성 활용 함수
def print_shape_info(shape):
    """어떤 도형이든 동일한 인터페이스로 처리"""
    print(f"넓이: {shape.area():.2f}")
    print(f"둘레: {shape.perimeter():.2f}")
    print("-" * 20)

# 다양한 도형 객체 생성
shapes = [
    Rectangle(10, 5),
    Circle(7),
    Triangle(6, 4, 5, 5, 6)
]

# 동일한 방식으로 처리 (다형성)
for shape in shapes:
    print(f"도형: {type(shape).__name__}")
    print_shape_info(shape)

# 출력:
# 도형: Rectangle
# 넓이: 50.00
# 둘레: 30.00
# --------------------
# 도형: Circle
# 넓이: 153.94
# 둘레: 43.98
# --------------------
# 도형: Triangle
# 넓이: 12.00
# 둘레: 16.00
# --------------------

```
**💡 다형성의 장점:**

- 코드 유연성 증가
- 확장성 용이
- 인터페이스 통일
- 유지보수 편리


### 4. 추상화 (Abstraction)
>복잡한 내부 구현을 숨기고 필요한 기능만 노출

```python
from abc import ABC, abstractmethod

class Vehicle(ABC):
    """추상 클래스 - 직접 인스턴스 생성 불가"""
    
    def __init__(self, brand, model):
        self.brand = brand
        self.model = model
        self._fuel = 100
    
    @abstractmethod
    def start(self):
        """시동 걸기 - 반드시 구현해야 함"""
        pass
    
    @abstractmethod
    def stop(self):
        """정지하기 - 반드시 구현해야 함"""
        pass
    
    @abstractmethod
    def fuel_type(self):
        """연료 타입 - 반드시 구현해야 함"""
        pass
    
    def get_info(self):
        """일반 메서드 - 공통 기능"""
        return f"{self.brand} {self.model}"

class Car(Vehicle):
    """구체적인 클래스 - 추상 메서드 모두 구현"""
    
    def start(self):
        return f"{self.get_info()} 자동차 시동 ON 🚗"
    
    def stop(self):
        return f"{self.get_info()} 자동차 정지"
    
    def fuel_type(self):
        return "가솔린"

class ElectricCar(Vehicle):
    def start(self):
        return f"{self.get_info()} 전기차 시동 ON ⚡"
    
    def stop(self):
        return f"{self.get_info()} 전기차 정지"
    
    def fuel_type(self):
        return "전기"

class Motorcycle(Vehicle):
    def start(self):
        return f"{self.get_info()} 오토바이 시동 ON 🏍️"
    
    def stop(self):
        return f"{self.get_info()} 오토바이 정지"
    
    def fuel_type(self):
        return "가솔린"

# 사용 예시
# vehicle = Vehicle("현대", "소나타")  # TypeError! 추상 클래스는 인스턴스 생성 불가

car = Car("현대", "소나타")
electric = ElectricCar("테슬라", "모델3")
motorcycle = Motorcycle("할리데이비슨", "스포스터")

vehicles = [car, electric, motorcycle]

for v in vehicles:
    print(v.start())
    print(f"연료: {v.fuel_type()}")
    print("-" * 30)

# 출력:
# 현대 소나타 자동차 시동 ON 🚗
# 연료: 가솔린
# ------------------------------
# 테슬라 모델3 전기차 시동 ON ⚡
# 연료: 전기
# ------------------------------
# 할리데이비슨 스포스터 오토바이 시동 ON 🏍️
# 연료: 가솔린
# ------------------------------

```

### 📋 클래스 vs 인스턴스
```python
class Person:
    # 클래스 변수 (모든 인스턴스가 공유)
    species = "호모 사피엔스"
    count = 0
    
    def __init__(self, name, age):
        # 인스턴스 변수 (각 인스턴스가 개별 소유)
        self.name = name
        self.age = age
        Person.count += 1  # 클래스 변수 접근
    
    def introduce(self):
        """인스턴스 메서드"""
        return f"안녕하세요, {self.age}살 {self.name}입니다."
    
    @classmethod
    def get_count(cls):
        """클래스 메서드 - 클래스 자체에서 호출"""
        return f"현재 {cls.count}명의 사람이 있습니다."
    
    @classmethod
    def from_birth_year(cls, name, birth_year):
        """클래스 메서드 - 팩토리 메서드로 활용"""
        age = 2024 - birth_year
        return cls(name, age)
    
    @staticmethod
    def is_adult(age):
        """정적 메서드 - 인스턴스/클래스와 무관한 기능"""
        return age >= 18

# 인스턴스 생성
p1 = Person("철수", 20)
p2 = Person("영희", 25)
p3 = Person.from_birth_year("민수", 2000)  # 팩토리 메서드 사용

# 클래스 변수 접근
print(Person.species)    # 호모 사피엔스
print(p1.species)        # 호모 사피엔스 (인스턴스로도 접근 가능)

# 인스턴스 변수 접근
print(p1.name)           # 철수
print(p2.name)           # 영희

# 메서드 호출
print(p1.introduce())         # 안녕하세요, 20살 철수입니다.
print(Person.get_count())     # 현재 3명의 사람이 있습니다.
print(Person.is_adult(20))    # True
print(p3.age)                 # 24

```


### 🛠 자주 쓰는 특수 메서드 (Magic Methods)
```python
class Book:
    def __init__(self, title, author, price, pages):
        self.title = title
        self.author = author
        self.price = price
        self.pages = pages
    
    def __str__(self):
        """print() 시 사용자 친화적 출력"""
        return f"📖 {self.title} - {self.author}"
    
    def __repr__(self):
        """개발자용 상세 표현"""
        return f"Book('{self.title}', '{self.author}', {self.price}, {self.pages})"
    
    def __len__(self):
        """len() 함수 지원"""
        return self.pages
    
    def __eq__(self, other):
        """== 동등 비교"""
        if isinstance(other, Book):
            return self.title == other.title and self.author == other.author
        return False
    
    def __lt__(self, other):
        """< 비교 (정렬에 사용)"""
        return self.price < other.price
    
    def __le__(self, other):
        """<= 비교"""
        return self.price <= other.price
    
    def __add__(self, other):
        """+ 연산자"""
        if isinstance(other, Book):
            return self.price + other.price
        return self.price + other
    
    def __contains__(self, keyword):
        """in 연산자"""
        return keyword.lower() in self.title.lower()
    
    def __getitem__(self, key):
        """인덱싱/키 접근 지원"""
        if key == "title":
            return self.title
        elif key == "author":
            return self.author
        elif key == "price":
            return self.price
        raise KeyError(f"'{key}' not found")
    
    def __call__(self, discount=0):
        """객체를 함수처럼 호출"""
        return self.price * (1 - discount / 100)

# 사용 예시
book1 = Book("파이썬 마스터", "김파이", 30000, 500)
book2 = Book("자바의 정석", "남궁성", 35000, 800)
book3 = Book("파이썬 마스터", "김파이", 30000, 500)

# __str__
print(book1)                    # 📖 파이썬 마스터 - 김파이

# __repr__
print(repr(book1))              # Book('파이썬 마스터', '김파이', 30000, 500)

# __len__
print(f"페이지 수: {len(book1)}")  # 페이지 수: 500

# __eq__
print(book1 == book3)           # True
print(book1 == book2)           # False

# __lt__, __le__ (정렬 가능)
books = [book2, book1]
books.sort()  # 가격순 정렬
print([str(b) for b in books])  # ['📖 파이썬 마스터 - 김파이', '📖 자바의 정석 - 남궁성']

# __add__
print(f"총 가격: {book1 + book2}원")  # 총 가격: 65000원

# __contains__
print("파이썬" in book1)        # True
print("자바" in book1)          # False

# __getitem__
print(book1["title"])           # 파이썬 마스터

# __call__
print(f"10% 할인가: {book1(10)}원")  # 10% 할인가: 27000.0원
```

## 🛠️ 주요 특수 메서드 정리표 (Magic Methods)

### 생성/소멸

| 메서드 | 용도 | 호출 방법 |
|:---:|:---|:---|
| `__init__` | 생성자 (초기화) | `Class()` |
| `__del__` | 소멸자 | 객체 삭제 시 |

### 문자열 표현

| 메서드 | 용도 | 호출 방법 |
|:---:|:---|:---|
| `__str__` | 문자열 표현 (사용자용) | `print(obj)`, `str(obj)` |
| `__repr__` | 문자열 표현 (개발자용) | `repr(obj)` |

### 비교 연산

| 메서드 | 용도 | 호출 방법 |
|:---:|:---|:---|
| `__eq__` | 동등 비교 | `obj1 == obj2` |
| `__ne__` | 불일치 비교 | `obj1 != obj2` |
| `__lt__` | 작다 비교 | `obj1 < obj2` |
| `__le__` | 작거나 같음 | `obj1 <= obj2` |
| `__gt__` | 크다 비교 | `obj1 > obj2` |
| `__ge__` | 크거나 같음 | `obj1 >= obj2` |

### 산술 연산

| 메서드 | 용도 | 호출 방법 |
|:---:|:---|:---|
| `__add__` | 덧셈 | `obj1 + obj2` |
| `__sub__` | 뺄셈 | `obj1 - obj2` |
| `__mul__` | 곱셈 | `obj1 * obj2` |
| `__truediv__` | 나눗셈 | `obj1 / obj2` |

### 컨테이너 연산

| 메서드 | 용도 | 호출 방법 |
|:---:|:---|:---|
| `__len__` | 길이 반환 | `len(obj)` |
| `__getitem__` | 인덱싱 | `obj[key]` |
| `__setitem__` | 인덱싱 할당 | `obj[key] = value` |
| `__contains__` | 포함 여부 | `item in obj` |

### 반복자

| 메서드 | 용도 | 호출 방법 |
|:---:|:---|:---|
| `__iter__` | 반복자 반환 | `for x in obj` |
| `__next__` | 다음 요소 | `next(obj)` |

### 기타

| 메서드 | 용도 | 호출 방법 |
|:---:|:---|:---|
| `__call__` | 호출 가능 | `obj()` |
| `__bool__` | 불리언 변환 | `bool(obj)`, `if obj:` |
| `__hash__` | 해시값 | `hash(obj)` |





## 🏠 Property와 Descriptor
@property 데코레이터
```python
class Temperature:
    def __init__(self, celsius=0):
        self._celsius = celsius
    
    @property
    def celsius(self):
        """getter"""
        return self._celsius
    
    @celsius.setter
    def celsius(self, value):
        """setter"""
        if value < -273.15:
            raise ValueError("절대 영도보다 낮을 수 없습니다!")
        self._celsius = value
    
    @celsius.deleter
    def celsius(self):
        """deleter"""
        print("온도 삭제됨")
        del self._celsius
    
    @property
    def fahrenheit(self):
        """읽기 전용 속성"""
        return self._celsius * 9/5 + 32
    
    @property
    def kelvin(self):
        """읽기 전용 속성"""
        return self._celsius + 273.15

# 사용 예시
temp = Temperature(25)

# getter
print(f"섭씨: {temp.celsius}°C")       # 섭씨: 25°C
print(f"화씨: {temp.fahrenheit}°F")    # 화씨: 77.0°F
print(f"켈빈: {temp.kelvin}K")         # 켈빈: 298.15K

# setter
temp.celsius = 100
print(f"섭씨: {temp.celsius}°C")       # 섭씨: 100°C

# 유효성 검사
try:
    temp.celsius = -300  # ValueError!
except ValueError as e:
    print(e)  # 절대 영도보다 낮을 수 없습니다!

# deleter
del temp.celsius  # 온도 삭제됨

```


## ✅ OOP 장점 요약
| 장점 | 설명 | 예시 |
|:---:|:---|:---|
| **재사용성** | 상속으로 코드 재활용 | 부모 클래스 메서드 상속 |
| **유지보수** | 수정 시 해당 클래스만 변경 | 버그 수정이 한 곳에서 |
| **확장성** | 새 기능 추가가 쉬움 | 새 클래스 추가로 기능 확장 |
| **가독성** | 현실 세계와 유사한 구조 | Car, Dog 등 직관적 |
| **협업** | 역할 분담이 명확 | 클래스별 담당자 지정 |
| **보안성** | 캡슐화로 데이터 보호 | private 변수 |

---


