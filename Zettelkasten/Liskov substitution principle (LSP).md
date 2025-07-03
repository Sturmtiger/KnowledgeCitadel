#SOLID #LSP

LSP states:
> Let Φ(x) be a property provable about objects x of type T. Then Φ(y) should be true for objects y of type S where S is a subtype of T.

The principle states that objects of a superclass shall be replaceable with objects of its subclasses without breaking the application.
That requires subclass objects to behave in the same way as superclass objects.

That means you may implement less restrictive validation rules, but must not enforce stricter ones in a subclass. Otherwise, code that expects a superclass instance may raise an exception when it interacts with a subclass instance.

Similar rules apply to method return values.
A method in the subclass must return values compatible with those returned by the superclass method.
You can only decide to apply even stricter rules by returning a specific subclass of the original return value, or by returning a subset of the valid return values defined by the superclass.
## Code examples
### Rectangle and Square
A rectangle's height and width may be any values.
A square is a rectangle in which the height and width are equal.

Technically, it may seem correct to derive a `Square` class from a `Rectangle` class, but let's see how it breaks LSP in practice.
```python
class Rectangle:
	def __init__(self, width: float, height: float) -> None:
		self._width = width
		self._height = height

	def get_area(self) -> float:
		return self._width * self._height

	@property.setter
	def width(self, value: float) -> None:
		self._width = value

	@property.setter
	def height(self, value: float) -> None:
		self._height = value


class Square(Rectangle):
    @property.setter
    def width(self, value: float) -> None:
        self._width = self._height = value

    @property.setter
    def height(self, value: float) -> None:
	    self._height = self._width = value
```
#### ❌ Problem
Code that works with `Rectangle` assumes the width and height are independent. But if you pass `Square`, changing one affects both.
```python
def resize_and_calc_area(rect: Rectangle) -> float:
	rect.width = 5
	rect.height = 10
    
    return rect.get_area()


resize_and_calc_area(Rectangle(1, 1))  # 50 ✅
resize_and_calc_area(Square(1, 1))  # 100 ❌
```
#### ✅ Solution
Avoid inheritance when it doesn't represent "[is-a](https://en.wikipedia.org/wiki/Is-a)".
```python
from abc import ABC, abstractmethod


class Shape(ABC):
    @abstractmethod
    def get_area(self) -> float:
        pass


class Rectangle(Shape):
	def __init__(self, width: float, height: float) -> None:
		self._width = width
		self._height = height

	def get_area(self) -> float:
		return self._width * self._height

	@property.setter
	def width(self, value: float) -> None:
		self._width = value

	@property.setter
	def height(self, value: float) -> None:
		self._height = value


class Square(Shape):
	def __init__(self, size: float) -> None:
		self._size = size

    def get_area(self) -> float:
        return self._size ** 2

    @property.setter
    def size(self, value: float) -> None:
        self._size = value
```
Now, the inheritance is correct!
### Birds
```python
from abc import ABC, abstractmethod


class Bird(ABC):
    @abstractmethod
    def fly(self):
        pass


class Sparrow(Bird):
    def fly(self):
        print("Sparrow is flying.")


class Penguin(Bird):
    def fly(self):
        raise Exception("Penguins can't fly!")

```
#### ❌ Problem
```python
def make_bird_fly(bird: Bird):
    bird.fly()

make_bird_fly(Sparrow())  # OK
make_bird_fly(Penguin())  # ❌ Runtime exception!
```
- Code expects the `Bird` and calling `fly()` will crash when the `Penguin` is passed.
- The `Penguin` is not **substitutable** for the `Bird` - it breaks LSP.
#### ✅ Solution
Restructure the class hierarchy. Not all birds fly. Ergo, the `fly()` method should not be defined in `Bird`.

```python
from abc import ABC, abstractmethod


class Bird(ABC):
    def lay_eggs(self):
        print("Lays eggs.")


class FlyingBird(Bird, ABC):
    @abstractmethod
    def fly(self):
        pass


class Sparrow(FlyingBird):
    def fly(self):
        print("Sparrow is flying.")


class Penguin(Bird):
    def swim(self):
        print("Penguin is swimming.")

```
- No `Bird` is **forced** to have  the `fly()` method.
- Now, you can safely substitute `Sparrow` where `FlyingBird` is expected, and `Penguin` where  `Bird` is expected - **no surprises**.