---
title: "Unleashing the Power of Python Data Classes"
date: "2023-01-28T23:46:37.121Z"
---
## Why and Where to Use Them
In Python, when we think of a _class_, a structure like this comes to our mind

```python
class Student():
    def __init__(self, first_name, last_name, age, grade):
        self.first_name = first_name
        self.last_name = last_name
        self.age = age
        self.grade = grade

    def function1(arg1, arg2):
        pass
```

which is not so intuitive.
What if we could have a more intuitive & less repeatitive way of doing the same thing?

```python
from dataclasses import dataclass
@dataclass
class Person:
     first_name: str  # attribute 1
     last_name: str   # attribute 2
     age: int         # attribute 3
     job: int         # attribute 4

    def function1(arg1, arg2): # functionality 1
        pass
```

Yes, we are talking about the [Data Classes](https://docs.python.org/3/library/dataclasses.html).

- Less biolerplate code
- More intuitive
- Type Annotations, to help you & your text editor with better type checking & linting

---

_In this blog_, we'll focus on **why** you should use DataClasses & **where** you should use them.
For a deeper dive in DataClasses, I'll attach resources.

#### **REQUIREMENTS**

- Python 3.7+ 🐍
- A zeal to learn 😊

When working with data-oriented classes, that behave like data-containers, you might want to create many instances of them ,compare, sort the objects and do a lot of data centric operations. Such functionalities are not provided right out of the box by regular classes. Data classes can hlp you achieve those functionalities with a more compact & intuitive code. 

### 1. **Less code to define a class**

```python
from dataclasses import dataclass
@dataclass
class Student:
     first_name: str  # attribute 1
     last_name: str   # attribute 2
     age: int         # attribute 3
     grade: int       # attribute 4
```

The dataclass decorator `@dataclass` is actually a code generator that automatically adds other methods under the hood. It adds methods like `__init__` , `__eq__` and `__repr__` and many more to your class. These methods are responsible for setting the attribute values, testing for equality and representing objects in a nice string format.

### 2. **Default values for attributes**

```python
from dataclasses import dataclass
@dataclass
class Student:
    first_name: str = "John"
    last_name: str = "Smith"
    age: int = 12
    grade: int = 5
```

As a general python convention, _"fields without default values cannot appear after fields with default values."_

### 3. **Customised representation of the objects**

The `__repr__()` can be modified to ovverride the default presentation of objects in the console

```python

from dataclasses import dataclass


@dataclass
class Student:
    first_name: str = "John"
    last_name: str = "Smith"
    age: int = 12
    grade: int = 5

    def __repr__(self):
        return f"{self.first_name} {self.last_name} age {self.age}, studies in grade {self.grade}"


john = Student()
print(john)
# John Smith age 12, studies in grade 5
# instead of
# Student(first_name='John', last_name='Smith', age=12, grade=5)
```

The same can be achieved by using a `__str__()` which works with regular classes as well.

```python
def __str__(self) -> str:
    return f"{self.first_name} {self.last_name} age {self.age}, studies in grade {self.grade}"

# John Smith age 12, studies in grade 5
```

### 4. **Easy conversion to a tuple or a dictionary**

Always converting objects to `dict` or `tuple` when interacting with other programs that expect these formats? Objects can be easily serialized into dicts or tuples using Data Classes.

```python
from dataclasses import dataclass
from dataclasses import asdict, astuple


@dataclass
class Student:
    first_name: str = "John"
    last_name: str = "Smith"
    age: int = 12
    grade: int = 5

    # def __repr__(self):
    #     return f"{self.first_name} {self.last_name} age {self.age}, studies in grade {self.grade}"


john = Student()
print(john)
print(astuple(john))
# ('John', 'Smith', 12, 5)

print(asdict(john))
# {'first_name': 'John', 'last_name': 'Smith', 'age': 12, 'grade': 5}

```

### 5. **Create READ-only objects**

There are times when you want to prevent anyone from modifying the values of the attributes once the object is instantiated i.e. you want a `frozen` or an _immutable_ instance.

This can be achieved by using `@dataclass(frozen=True)`

```python
from dataclasses import dataclass

@dataclass(frozen=True)
class Student:
    first_name: str = "John"
    last_name: str = "Smith"
    age: int = 12
    grade: int = 5

john = Student()
print(john)

john.age = 13
```

If you set a `frozen` object’s attribute to a new value, a `FrozenInstanceError` error will be raised.

```sh
Student(first_name='John', last_name='Smith', age=12, grade=5)
Traceback (most recent call last):
  File "student.py", line 15, in <module>
    john.age = 13
  File "<string>", line 4, in __setattr__
dataclasses.FrozenInstanceError: cannot assign to field 'age'
```

Also a `__hash__()` will be automatically created by Python when `frozen=True`

### 6. **Comparison of objects**

```python
john_again = Student(first_name='John', last_name='Smith', age=12, grade=5)

print(john == john_again)
```

In order to compare two objects `john` & `john_again`, you’d have to implement the `__eq__` method yourself.
This method should first check that the two objects are instances of the same class and then test the equality between tuples of attributes.

```python
def __eq__(self, other):
    if other.__class__ is not self.__class__:
        return NotImplemented
    return (self.first_name,
            self.last_name,
            self.age,
            self.grade) == (other.first_name,
                            other.last_name,
                            other.age,
                            other.grade)

```

Now if you decide to add new attributes to your class, you’d have to update the `__eq__()` method again. Then do the same for `__ge__()` , `__gt__()` ,`__le__()` and `__lt__()` if they’re used.

But, with DataClass, it works out of the box

```python
print(john == john_again)
# True
```

### **ADVANCED FEATURE: SORTING**

It's also possible to **sort** & **compare** objects based on an attribute as a default.

```python
from dataclasses import dataclass
from dataclasses import field  # import field


@dataclass(order=True)  # tells dataclass that we want to compare objects
class Student:
    first_name: str = "John"
    last_name: str = "Smith"
    age: int = 12
    grade: int = 5
    sort_index: int = field(init=False, repr=False)
    # init = False, means sort_index will be initialised after 'age' is initialised
    # repr = False, means sort_index will not be printed

    def __post_init__(self):
        self.sort_index = self.age


john = Student(age=15)
print(john)
# Student(first_name='John', last_name='Smith', age=15, grade=5)

sam = Student(age=20)
print(sam)
# Student(first_name='John', last_name='Smith', age=20, grade=5)

print(sam > john)
# True
```

Here, `__gt__()` considers `age` attribute for comparison.

**QUESTION**❓**How'd you sort a frozen set?**

Using the `__setattr__()`

```python
from dataclasses import dataclass
from dataclasses import field  # import field


@dataclass(order=True, frozen=True)  # enable order=True
class Student:
    first_name: str = "John"
    last_name: str = "Smith"
    age: int = 12
    grade: int = 5
    sort_index: int = field(init=False, repr=False)
    # init = False, means sort_index will be initialised after 'age' is initialised
    # repr = False, means sort_index will not be printed

    def __post_init__(self):
        object.__setattr__(self,'sort_index', self.age)


john = Student(age=15)
print(john)
# Student(first_name='John', last_name='Smith', age=15, grade=5)

sam = Student(age=20)
print(sam)
# Student(first_name='John', last_name='Smith', age=20, grade=5)

print(sam > john)
# True
```

### ⭐**BONUS**⭐

If you've read this far, here's an advanced functionality of DataClasses

### 7. **Initialize internal attributes**

In some situations, you may need to create an attribute that is only defined internally, not when the class is instantiated. The attribute has a value that can depend on previously-set attributes.  
The `__post_init__()` is called right after the `__init__()` method is called.
By using this function and setting its `init=False` and `repr=True` we can create a new field called `full_name`

```python
from dataclasses import dataclass
from dataclasses import field  # import field

@dataclass
class Student:
    first_name: str = "John"
    last_name: str = "Smith"
    age: int = 12
    grade: int = 5
    full_name: str = field(init=False, repr=True)

    def __post_init__(self):
         self.full_name = self.first_name + " " + self.last_name

john = Student()
print(john)
# Student(first_name='John', last_name='Smith', age=12, grade=5, full_name='John Smith')

print(john.full_name)
# John Smith
```

We can still instantiate the `Student` class without setting the `full_name` attribute.
The `repr=True` makes it visible when the object is printed.

### It's all great but where do I use it?

- If you have Python 3.7+ 😛
- You're working on a data-centric scenario
  - It's a data-driven scenario & less behaviour-driven
  - You need to create many data-centric classes
  - Optionally, you need to ensure a degree of _immutability_ on the objects of this class.

Example from `artifacts.py` showing how Data Classes are used for data-centric classes in production code. 

```python
from dataclasses import dataclass


@dataclass
class DataIngestionArtifact:
    trained_file_path: str
    test_file_path: str


@dataclass
class DataValidationArtifact:
    validation_status: bool
    valid_train_file_path: str
    valid_test_file_path: str
    invalid_train_file_path: str
    invalid_test_file_path: str
    drift_report_file_path: str


@dataclass
class DataTransformationArtifact:
    transformed_object_file_path: str
    transformed_train_file_path: str
    transformed_test_file_path: str

```

#### Resources

While learning about dataclasses, I went through many resources, here’s a list of the best I’ve found.

- [9 Reasons Why You Should Start Using Python Dataclasses](https://towardsdatascience.com/9-reasons-why-you-should-start-using-python-dataclasses-98271adadc66)

- [If you're not using Python DATA CLASSES yet, you should 🚀](https://youtu.be/vRVVyl9uaZc)

- [Data Classes in Python 3.7+ (Guide)](www.realpython.com/python-data-classes/)

- [Data Classes Python documentation](https://docs.python.org/3/library/dataclasses.html)

_I have tried to be thorough and cover most of the use cases, yet, no man is perfect. Reach out if you find mistakes, or want me to pay attention to relevant use cases._

---

Thanks for reading 🙏
If you’ve made it this far, I really thank you for your time 😊
That’ll be all for me today. Until next time 👋
