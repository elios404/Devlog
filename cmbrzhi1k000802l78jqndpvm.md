---
title: "Understanding Python Classes: A Beginner's Guide to Object-Oriented Programming"
seoTitle: "Beginner's Guide to Python Classes"
seoDescription: "Learn the basics of Python classes, object-oriented programming, and more with this beginner's guide, featuring practical examples and key concepts"
datePublished: Wed Jun 11 2025 13:26:45 GMT+0000 (Coordinated Universal Time)
cuid: cmbrzhi1k000802l78jqndpvm
slug: understanding-python-classes-a-beginners-guide-to-object-oriented-programming
cover: https://cdn.hashnode.com/res/hashnode/image/stock/unsplash/npxXWgQ33ZQ/upload/a560ec1f7ac68714bd3693419cf9bd16.jpeg
tags: python, python3, python-beginner, oop-in-python

---

In this document, I aim to organize my thoughts on the **Class**, the foundation of object-oriented programming (OOP) in Python. I will define the basic concepts and terminology of classes and learn how to design and utilize them through practical examples. Furthermore, building on the **Dunder Methods** briefly covered in the previous "In-Depth Analysis of Lists," I will analyze step-by-step how they enable operator overloading and object representation within classes, as well as the concepts of class variables and inheritance.

## 1\. Basic Concepts of a Class

Before diving into classes, here are the core terms that must be understood:

* **Class**: A 'blueprint' or 'template' for creating objects.
    
* **Instance**: A 'product' created from a class blueprint. In `x = 10`, `x` is an instance of the `int` class. Everything in Python is an instance of a class, i.e., an object.
    
* **Instance Variable**: Data that is unique to each instance.
    
* **Class Variable**: Data that is shared by all instances created from a class.
    
* **Method**: A function defined within a class.
    
* **Inheritance**: Allows a child class to inherit all attributes and methods from a parent class.
    
* **Method Overriding**: Redefining a method inherited from a parent class in the child class.
    

## 2\. Class Design and Implementation: The `Point` Class Example

Classes are used to represent specific data and to bundle related functionalities together. For example, let's assume we need to represent a 'point' on a coordinate plane.

* **Required Data**: A point has an x-coordinate and a y-coordinate.
    
* **Required Functionality**: We might need a function to calculate the distance between two points.
    

### 2-1. Defining a Class

Based on these requirements, we can design a `Point` class that holds x and y coordinates as data and has a distance calculation method.

```python
class Point:
  # The initialization method (constructor) called when an instance is created
  def __init__(self, x, y):
    # self.x and self.y are instance variables
    self.x = x
    self.y = y

  # An instance method to calculate the distance to another point (other)
  def distance(self, other):
    return ((self.x - other.x) ** 2 + (self.y - other.y) ** 2) ** 0.5
```

`__init__` is a special dunder method that is called only once when an instance is created. It serves to set the initial data for the instance.

### 2-2. Creating and Using Instances

Using the defined `Point` class as a blueprint, we can create and use instances (objects) with actual data.

````python
```python
# Create instances p1 and p2 of the Point class
p1 = Point(0, 0)
p2 = Point(3, 4)

# Accessing instance variables
print(f"p1's x-coordinate: {p1.x}") # Output: p1's x-coordinate: 0

# Calling an instance method
dist = p1.distance(p2)
print(f"The distance between p1 and p2 is: {dist}") # Output: The distance between p1 and p2 is: 5.0
````

## 3\. Extending Classes with Dunder Methods

Using dunder methods, we can make instances of our custom classes behave like Python's built-in data types.

### 3-1. Operator Overloading

To enable arithmetic operations like `+`, `-`, and `==` between `Point` instances, we need to define dunder methods such as `__add__`, `__sub__`, and `__eq__` within the class.

```python
class Point:
  def __init__(self, x, y):
    self.x = x
    self.y = y

  # Method for the + operator
  def __add__(self, other):
    return Point(self.x + other.x, self.y + other.y)

  # Method for the - operator
  def __sub__(self, other):
    return Point(self.x - other.x, self.y - other.y)

  # Method for the == operator
  def __eq__(self, other):
    return self.x == other.x and self.y == other.y

  # Method for object representation
  def __repr__(self):
    return f'<Point: {self.x}, {self.y}>'
```

Now, `Point` instances can perform arithmetic and comparison operations.

```python
p1 = Point(1, 2)
p2 = Point(3, 4)

# The __add__ method is called
p3 = p1 + p2
print(f"Result of p1 + p2: {p3}") # Output: Result of p1 + p2: <Point: 4, 6>

p4 = Point(1, 2)
# The __eq__ method is called
print(f"Are p1 and p4 equal? {p1 == p4}") # Output: Are p1 and p4 equal? True
```

When the code `p1 + p2` is executed, the Python interpreter calls the `__add__` method of `p1`, passing `p2` as an argument. This mechanism, where an object performs a predefined action in response to an operator, is called **Operator Overloading**.

### 3-2. String Representation of Objects: `__str__` vs `__repr__`

Dunder methods also control what is displayed when an object is printed with the `print()` function or when just the variable name is entered in an interactive shell.

* `__str__`: Used for an informal, 'human-readable' string representation, like with the `print()` function.
    
* `__repr__`: Used for an 'official' string representation that can unambiguously identify the object. Its purpose is to accurately convey the object's state, primarily during development and debugging.
    

```python
class Point:
     def __init__(self, x, y):
         self.x = x
         self.y = y

     def __str__(self):
         return f'The coordinates are [{self.x}, {self.y}].'

     def __repr__(self):
         return f'Point(x={self.x}, y={self.y})'
```

The difference between the two methods becomes clear in actual use.

```python
p = Point(1, 2)

# __str__ is called
print(p) # Output: The coordinates are [1, 2].

# __repr__ is called (when only the variable is run in a Jupyter/Colab environment)
# p  -> Point(x=1, y=2)

# Objects inside a container (list) are represented by __repr__
points = [Point(1, 2), Point(3, 4)]
print(points) # Output: [Point(x=1, y=2), Point(x=3, y=4)]
```

It is important to note that when printing elements within a container like a list, their official representation, `__repr__`, is used.

## 4\. Class Usage Practice

Intentionally implementing data that could be stored in lists, tuples, or dictionaries as a class is a great way to improve your skills. For example, let's consider book information data represented as a list of dictionaries.

```python
books_data = [
    {"title": "Python Mastery", "author": "Kim Python", "price": 28000},
    {"title": "JavaScript Master", "author": "Park Java", "price": 32000}
]
```

By converting this to a `Book` class, you can make the data structure clearer and write more extensible code by adding related functionalities (e.g., applying a discount) as methods.

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
# Output: [<Book: Python Mastery, Kim Python>, <Book: JavaScript Master, Park Java>]
```

## 5\. Class Variables and Instance Variables

* **Instance Variable**: Accessed via `self`, such as `self.x`, and belongs independently to each instance.
    
* **Class Variable**: Belongs directly to the class and is shared by all instances created from that class.
    

If you want to count the number of `Point` instances created so far, you can use a class variable.

```python
class Point:
  count = 0 # Class variable

  def __init__(self, x, y):
      self.x = x
      self.y = y
      Point.count += 1 # Increment the class variable each time an instance is created
```

`p1.x` is a variable unique to the `p1` instance, whereas `count` is a variable shared by all instances created from the `Point` class.

```python
p1 = Point(1, 1)
p2 = Point(3, 4)

print(f"p1's x-coordinate: {p1.x}")      # p1's instance variable -> Output: 1
print(f"p2's x-coordinate: {p2.x}")      # p2's instance variable -> Output: 3

print(f"p1's count: {p1.count}")    # Shared class variable -> Output: 2
print(f"p2's count: {p2.count}")    # Shared class variable -> Output: 2
print(f"Point class's count: {Point.count}") # Accessing the class variable directly -> Output: 2
```

## 6\. Inheritance and Method Overriding

Inheritance is used to create a new class by inheriting the functionalities of an existing class.

```python
class Animal:
  def bark(self):
    print("An animal barks.")

class Dog(Animal): # Inherits from the Animal class
  pass

class Cat(Animal):
  # Redefining the bark method (Method Overriding)
  def bark(self):
    print("Meow!")
```

The `Dog` class inherits the `bark` method from `Animal` as is, but the `Cat` class **redefines (overrides)** its own `bark` method to perform a different action from the parent class.

```python
dog = Dog()
dog.bark() # Output: An animal barks.

cat = Cat()
cat.bark() # Output: Meow!
```

## Conclusion

I have summarized the core elements of object-oriented programming, from the basic concepts of Python classes to inheritance. Moving beyond simply handling data with dictionaries or lists, encapsulating data and functionality together through a class blueprint is a core paradigm that enhances code reusability and maintainability. I believe that understanding concepts like object memory referencing and operator overloading through Dunder methods is a crucial part of deeply comprehending Python's object-oriented features.