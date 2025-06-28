#SOLID #LSP

The LSP states:
> Let Φ(x) be a property provable about objects x of type T. Then Φ(y) should be true for objects y of type S where S is a subtype of T.

The principle defines that objects of a superclass shall be replaceable with objects of its subclasses without breaking the application.

That requires the objects of your superclass to behave in the same way as the objects of your subclasses.
That means you can implement less restrictive validation rules, but you are not allowed to enforce stricter ones in your subclass. Otherwise, the code that calls this method on an object of the superclass might cause an exception if it gets called with a object of the subclass.

Similar rules apply to the return value of the method. The return value of a method of the subclass must comply with the same rules as the return value of the method of the superclass.
You can only decide to apply even stricter rules by returning a specific subclass of the defined return value, or by returning a subset of the valid return values of the superclass.


==Showcase different example of the LSP violation: Rectangular and Square, CoffeeMachine, Notification system.==

## Example 1. Rectangle and Square.

A rectangle's height may be any value and width can be any value.
A square is a rectangle with equal width and height.

Technically, it seems correct to derive a Square class from a Rectangle class. But let's see how it breaks the LSP in practice.

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
### ❌ Problem
Code that works with a `Rectangle` assumes width and height are independent. But if you pass a `Square`, changing one affects both.
```python
def resize_and_calc_area(rect: Rectangle) -> float:
	rect.width = 5
	rect.height = 10
    
    return rect.get_area()


resize_and_calc_area(Rectangle(1, 1))  # 50 ✅
resize_and_calc_area(Square(1, 1))  # 100 ❌
```

### ✅ Solution
Avoid inheritance when it doesn't represent "**is-a**".
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


class Square(Rectangle):
	def __init__(self, size: float) -> None:
		self._size = size

    def get_area(self) -> float:
        return self._size ** 2

    @property.setter
    def size(self, value: float) -> None:
        self._size = value
```
Now, the inheritance is correct!
## Example 2. Birds.
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
### ❌ Problem
```python
def make_bird_fly(bird: Bird):
    bird.fly()

make_bird_fly(Sparrow())  # OK
make_bird_fly(Penguin())  # ❌ Runtime exception!
```
- Code expects a `Bird` and calling `fly()` will crash when a `Penguin` is passed.
- `Penguin` is not **substitutable** for `Bird` - it breaks the LSP.

### ✅ Solution
Restructure the class hierarchy. Not all birds fly. Ergo, the `fly()` method should be defined in `Bird`.

```python
from abc import ABC, abstractmethod


class Bird(ABC):
    def lay_eggs(self):  # Just an example of what can be defined in Bird.
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
- No `Bird` is **forced** to have  the `fly()` method unless it makes sense.
- Now, you can safely substitute `Sparrow` where a `FlyingBird` is expected, and `Penguin` where a `Bird` is expected — **no surprises**. ==Take a look==❗