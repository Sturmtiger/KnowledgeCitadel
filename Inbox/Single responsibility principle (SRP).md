#SOLID #SRP

>A class should have one and only one reason to change. - Robert Martin

Ask yourself a question "What the class is responsible for?", if you find yourself saying "and" that indicates your class breaks SRP.

## Frequency and Effects of Changes
Requirements may change over time.
The more responsibilities the class has the more often you'll need to modify it.
If your class implements multiple responsibilities, they are not independent, but - [[Coupling|coupled]].

Having multiple responsibilities brings another issue - whenever you need to change your class, you might need to update all its dependencies, even though they are not directly affected by your change.
The dependencies may use only one of the class' responsibilities, but you need to update them anyway.

In the end, you need to modify you class and its dependencies more often, and each modification is more complicated because multiple responsibilities are coupled within one class and the usage of the class in its dependencies becomes confusing.

## Easier to understand
Classes, software components and microservices that have only one responsibility are much easier to explain, understand and implement.
There's much less space for bugs, it improves your developments speed and testing.

# An example of SRP

Entity transformer. It takes an entity DTO object and transform it into an ORM object.

```
from abc import ABC, abstactmethod


class EntityTransformer(ABC):
	"""Transforms an entity into an ORM object."""

	@abstactmethod
	def transform(self) -> None:
		pass


class ItemTransformer(EntityTransformer):
	"""Transforms an item into an ORM object."""
	def transform(self) -> None:
		# do stuff


class OrderTransformer(EntityTransformer):
	"""Transforms an order into an ORM object."""
	def transform(self) -> None:
		# do stuff
```

It has only one responsibility - it transforms some entity into an ORM equivalent.
It does not validate the entity, neither it saves it into the DB. It only transform it into an ORM object.
Thus, the EntityTransformer class is easy to grasp, implement, support and cover with tests.

Avoid oversimplifying your code and taking the SRP to the extreme through.
For example, say, the Order has transaction data within it, and we'd like to parse the object. If parsing logic is quite simple and fits into one private function it is completely fine to keep it in Transformer class, other then having multiple classes that just contain one function. Having these multiple classes lead to another problem, when writing some actual code, you would have to inject all those dependencies which makes the code less readable and confusing.

The SRP is an important rule but don't overuse it.