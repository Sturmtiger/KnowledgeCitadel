#SOLID #LSP

LSP states:
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

Technically, it seems correct to derive a Square class from a Rectangle class. But let's see how it breaks LSP in practice.

```
class Rectangle:
	def __init__(self, width, height):
		self._width = width
		self._height = height

	def get_area(self):
		return self.width * self.height

	@property.setter
	def width(self, value):
		self._width = value

	@property.setter
	def height(self, value):
		self._height = value


class Square(Rectangle):
	def __init__(self, size):
		super().__init__(width=size, height=size)

	@property.setter
	def width(self, value):
		self._width = self._height = value

	@property.setter
	def height(self, value):
		self._height = self._width = value
```