---
title: "파이썬 초보 탈출을 위한 클래스(Class) 핵심 가이드 (feat. 던더 메서드, 상속)"
seoTitle: "파이썬 클래스 초보 가이드"
seoDescription: "파이썬의 클래스, 던더 메서드, 상속 등 객체지향 프로그래밍 기초 가이드를 통해 초보 탈출에 도전하세요"
datePublished: Wed Jun 11 2025 13:10:25 GMT+0000 (Coordinated Universal Time)
cuid: cmbrywhyt000902l4en6bhz1c
slug: class-feat
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/dC6Pb2JdAqs/upload/cd8dcd3a2a8c8280de65f9f8ea502128.jpeg
tags: python, python3

---

이번 문서에서는 파이썬 객체 지향 프로그래밍(OOP)의 근간을 이루는 \*\*클래스(Class)\*\*에 대해 정리해보고자 한다. 클래스의 기본적인 개념과 용어를 정의하고, 실제 예제를 통해 클래스를 설계하고 활용하는 방법을 알아본다.

나아가 지난 '리스트(List) 심층 분석'에서 잠시 다루었던 \*\*던더 메서드(Dunder Method)\*\*가 클래스 내에서 어떻게 연산자 오버로딩과 객체 표현을 가능하게 하는지, 그리고 클래스 변수와 상속의 개념까지 단계적으로 분석한다.

## 1\. 클래스의 기본 개념

클래스 학습에 앞서, 반드시 숙지해야 할 핵심 용어들은 다음과 같다.

* **클래스 (Class)**: 객체를 만들기 위한 '설계도' 또는 '틀'.
    
* **인스턴스 (Instance)**: 클래스라는 설계도를 바탕으로 만들어진 '생산품'. `x = 10` 에서 `x`는 `int` 클래스의 인스턴스이다. 파이썬의 모든 것은 클래스의 인스턴스, 즉 객체다.
    
* **인스턴스 변수 (Instance Variable)**: 각 인스턴스가 고유하게 가지는 데이터.
    
* **클래스 변수 (Class Variable)**: 해당 클래스로 만들어진 모든 인스턴스가 공유하는 데이터.
    
* **메서드 (Method)**: 클래스 내에 정의된 함수.
    
* **상속 (Inheritance)**: 부모 클래스의 모든 속성과 메서드를 자식 클래스가 물려받아 사용하는 것.
    
* **메서드 오버라이딩 (Method Overriding)**: 상속받은 부모 클래스의 메서드를 자식 클래스에서 재정의하는 것.
    

## 2\. 클래스 설계와 구현: `Point` 클래스 예제

클래스는 특정 데이터를 표현하고, 그와 관련된 기능을 함께 묶어 관리하기 위해 사용된다. 예를 들어 좌표 평면 위의 '점'을 표현해야 한다고 가정해보자.

* **필요한 데이터**: 점은 x좌표와 y좌표를 가진다.
    
* **필요한 기능**: 점과 점 사이의 거리를 구하는 기능이 필요할 수 있다.
    

### 2-1. 클래스 정의하기

위 요구사항을 바탕으로, x, y 좌표를 데이터로 갖고 거리 계산 기능을 메서드로 갖는 `Point` 클래스를 설계할 수 있다.

```python
class Point:
  # 인스턴스가 생성될 때 호출되는 초기화 메서드 (생성자)
  def __init__(self, x, y):
    # self.x, self.y는 인스턴스 변수
    self.x = x
    self.y = y

  # 다른 점(other)과의 거리를 계산하는 인스턴스 메서드
  def distance(self, other):
    return ((self.x - other.x) ** 2 + (self.y - other.y) ** 2) ** 0.5
```

`init` 은 인스턴스가 생성되는 시점에 단 한 번 호출되는 특별한 던더 메서드로, 인스턴스가 가질 초기 데이터를 설정하는 역할을 한다.

### 2-2. 인스턴스 생성 및 활용

정의된 `Point` 클래스라는 설계도를 바탕으로, 실제 데이터를 가진 인스턴스(객체)를 만들고 활용할 수 있다.

```python
# Point 클래스의 인스턴스 p1, p2 생성
p1 = Point(0, 0)
p2 = Point(3, 4)

# 인스턴스 변수 접근
print(f"p1의 x좌표: {p1.x}") # 출력: p1의 x좌표: 0

# 인스턴스 메서드 호출
dist = p1.distance(p2)
print(f"p1과 p2 사이의 거리: {dist}") # 출력: p1과 p2 사이의 거리: 5.0
```

## 3\. 던더 메서드를 활용한 클래스 확장

던더 메서드를 활용하면 우리가 직접 만든 클래스의 인스턴스를 파이썬의 내장 자료형처럼 자연스럽게 다룰 수 있다.

### 3-1. 연산자 오버로딩 (Operator Overloading)

`Point` 인스턴스끼리 `+`, `-`, `==` 와 같은 연산을 가능하게 하려면 `__add__`, `__sub__`, `__eq__` 같은 던더 메서드를 클래스 내에 정의하면 된다.

```python
class Point:
  def __init__(self, x, y):
    self.x = x
    self.y = y

  # + 연산자를 위한 메서드
  def __add__(self, other):
    return Point(self.x + other.x, self.y + other.y)

  # - 연산자를 위한 메서드
  def __sub__(self, other):
    return Point(self.x - other.x, self.y - other.y)

  # == 연산자를 위한 메서드
  def __eq__(self, other):
    return self.x == other.x and self.y == other.y

  # 객체 표현을 위한 메서드
  def __repr__(self):
    return f'<Point: {self.x}, {self.y}>'
```

이제 `Point` 인스턴스는 산술 및 비교 연산이 가능해진다.

```python
p1 = Point(1, 2)
p2 = Point(3, 4)

# __add__ 메서드가 호출됨
p3 = p1 + p2
print(f"p1 + p2 결과: {p3}") # 출력: p1 + p2 결과: <Point: 4, 6>

p4 = Point(1, 2)
# __eq__ 메서드가 호출됨
print(f"p1과 p4는 같은가? {p1 == p4}") # 출력: p1과 p4는 같은가? True
```

`p1 + p2` 코드가 실행되면, 파이썬 인터프리터는 `p1`의 `__add__` 메서드를 호출하면서 `p2`를 인자로 넘겨준다. 이처럼 객체가 특정 연산자에 반응하여 약속된 동작을 수행하게 하는 것을 **연산자 오버로딩**이라 한다.

### 3-2. 객체의 문자열 표현

`__str__` vs `__repr__` 객체를 `print()` 함수로 출력하거나, 인터랙티브 셸에서 변수명만 입력했을 때 보이는 결과를 제어하는 것도 던더 메서드의 역할이다.

`__str__`: `print()` 함수처럼 '사람이 읽기 좋은' 비공식적 문자열 표현을 위해 사용된다.

`__repr__`: 객체를 명확히 식별할 수 있는 '공식적'인 문자열 표현을 위해 사용된다. 디버깅이나 개발 과정에서 객체의 상태를 정확히 파악하는 데 목적이 있다.

```python
class Point:
     def __init__(self, x, y):
         self.x = x
         self.y = y 

     def __str__(self):
         return f'점 좌표는 [{self.x}, {self.y}] 입니다.'

     def __repr__(self):
         return f'Point(x={self.x}, y={self.y})'
```

두 메서드의 차이는 실제 사용 시 명확하게 드러난다.

```python
p = Point(1, 2)

# __str__ 호출
print(p) # 출력: 점 좌표는 [1, 2] 입니다.

# __repr__ 호출 (Jupyter/Colab 환경에서 변수만 실행 시)
# p  -> Point(x=1, y=2)

# 컨테이너(리스트) 안의 객체들은 __repr__로 표현됨
points = [Point(1, 2), Point(3, 4)]
print(points) # 출력: [Point(x=1, y=2), Point(x=3, y=4)]
```

리스트와 같은 컨테이너 안의 요소들을 출력할 때는, 각 요소를 대표하는 공식적인 표현인 `__repr__`이 사용된다는 점이 중요하다.

## 4\. 클래스 활용 연습

리스트, 튜플, 딕셔너리로 저장할 수 있는 데이터를 의도적으로 클래스로 구현해보는 작업은 실력 향상에 큰 도움이 된다. 예를 들어, 딕셔너리의 리스트로 표현된 책 정보 데이터가 있다고 가정해보자.

```python
books_data = [
    {"title": "파이썬 완전정복", "author": "김파이썬", "price": 28000},
    {"title": "자바스크립트 마스터하기", "author": "박자바", "price": 32000}
]
```

이를 `Book` 클래스로 변환하면 데이터의 구조를 더욱 명확하게 만들고, 책과 관련된 기능(예: 할인율 적용)을 메서드로 추가하는 등 확장성 있는 코드를 작성할 수 있다.

```python
class Book:
    def __init__(self, title, author, price):
        self.title = title
        self.author = author
        self.price = price

    def __repr__(self):
      return f'<Book: {self.title}, {self.author}>'

books = [
    Book(b['title'], b['author'], b['price']) for b in books_data
]

print(books)
# 출력: [<Book: 파이썬 완전정복, 김파이썬>, <Book: 자바스크립트 마스터하기, 박자바>]
```

## 5\. 클래스 변수와 인스턴스 변수

* **인스턴스 변수**: `self.x` 와 같이 `self`를 통해 접근하며, 각 인스턴스에 독립적으로 귀속된다.
    
* **클래스 변수**: 클래스에 직접 귀속되며, 해당 클래스로부터 생성된 모든 인스턴스가 공유한다.
    

지금까지 생성된 `Point` 인스턴스의 개수를 세고 싶다면 클래스 변수를 활용할 수 있다.

```python
class Point:
  count = 0 # 클래스 변수

  def __init__(self, x, y):
      self.x = x
      self.y = y
      Point.count += 1 # 인스턴스가 생성될 때마다 클래스 변수 1 증가

p1 = Point(1, 1)
p2 = Point(3, 4)

print(f"p1의 x좌표: {p1.x}")      # p1의 인스턴스 변수 -> 출력: 1
print(f"p2의 x좌표: {p2.x}")      # p2의 인스턴스 변수 -> 출력: 3

print(f"p1의 count: {p1.count}")    # 공유되는 클래스 변수 -> 출력: 2
print(f"p2의 count: {p2.count}")    # 공유되는 클래스 변수 -> 출력: 2
print(f"Point 클래스의 count: {Point.count}") # 클래스 변수 직접 접근 -> 출력: 2
```

`p1.x` 는 인스턴스 고유의 변수이지만, `count`는 `Point` 클래스로 생성된 모든 인스턴스가 공유하는 변수이다.

## 6\. 상속과 메서드 오버라이딩

**상속**은 기존 클래스의 기능을 그대로 물려받아 새로운 클래스를 만들 때 사용된다.

```python
class Animal:
  def bark(self):
    print("동물이 웁니다.")

class Dog(Animal): # Animal 클래스를 상속
  pass

class Cat(Animal):
  # bark 메서드를 재정의 (메서드 오버라이딩)
  def bark(self):
    print("야옹!")

dog = Dog()
dog.bark() # 출력: 동물이 웁니다.

cat = Cat()
cat.bark() # 출력: 야옹!
```

`Dog` 클래스는 `Animal`의 `bark` 메서드를 그대로 물려받았지만, `Cat` 클래스는 자신만의 `bark` 메서드를 **재정의(오버라이딩)**하여 부모 클래스와 다른 동작을 수행한다.

## 결론

파이썬 클래스의 기본 개념부터 상속까지, 객체 지향 프로그래밍의 핵심 요소들을 정리해보았다.

단순히 데이터를 딕셔너리나 리스트로 다루는 것을 넘어, 클래스라는 설계도를 통해 데이터와 기능을 함께 캡슐화하는 것은 코드의 재사용성을 높이고 유지보수를 용이하게 만드는 핵심적인 패러다임이다. 특히 객체의 메모리 참조 방식과 던더 메서드를 통한 연산자 오버로딩 개념은 파이썬의 객체 지향적 특성을 깊이 이해하는 데 중요한 부분이라고 생각한다.